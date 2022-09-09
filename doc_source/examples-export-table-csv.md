# Exporting Tables into a CSV File<a name="examples-export-table-csv"></a>

These Python examples show how to export tables from an image of a document into a comma\-separated values \(CSV\) file\.

The example for synchronous document analysis collects table information from a call to [AnalyzeDocument](API_AnalyzeDocument.md)\. The example for asynchronous document analysis makes a call to [StartDocumentAnalysis](API_StartDocumentAnalysis.md) and then retrives the results from [GetDocumentAnalysis](API_GetDocumentAnalysis.md) as `Block` objects\.

Table information is returned as [Block](API_Block.md) objects from a call to [AnalyzeDocument](API_AnalyzeDocument.md)\. For more information, see [Tables](how-it-works-tables.md)\. The `Block` objects are stored in a map structure that's used to export the table data into a CSV file\. 

------
#### [ Synchronous ]

In this example, you will use the functions: 
+ `get_table_csv_results` – Calls [AnalyzeDocument](API_AnalyzeDocument.md), and builds a map of tables that are detected in the document\. Creates a CSV representation of all detected tables\.
+ `generate_table_csv` – Generates the CSV file for an individual table\.
+ `get_rows_columns_map` – Gets the rows and columns from the map\.
+ `get_text` – Gets the text from a cell\.

**To export tables into a CSV file**

1. Configure your environment\. For more information, see [Prerequisites](examples-blocks.md#examples-prerequisites)\.

1. Save the following example code to a file named *textract\_python\_table\_parser\.py*\.

   ```
   import webbrowser, os
   import json
   import boto3
   import io
   from io import BytesIO
   import sys
   from pprint import pprint
   
   
   def get_rows_columns_map(table_result, blocks_map):
       rows = {}
       for relationship in table_result['Relationships']:
           if relationship['Type'] == 'CHILD':
               for child_id in relationship['Ids']:
                   cell = blocks_map[child_id]
                   if cell['BlockType'] == 'CELL':
                       row_index = cell['RowIndex']
                       col_index = cell['ColumnIndex']
                       if row_index not in rows:
                           # create new row
                           rows[row_index] = {}
                           
                       # get the text value
                       rows[row_index][col_index] = get_text(cell, blocks_map)
       return rows
   
   
   def get_text(result, blocks_map):
       text = ''
       if 'Relationships' in result:
           for relationship in result['Relationships']:
               if relationship['Type'] == 'CHILD':
                   for child_id in relationship['Ids']:
                       word = blocks_map[child_id]
                       if word['BlockType'] == 'WORD':
                           text += word['Text'] + ' '
                       if word['BlockType'] == 'SELECTION_ELEMENT':
                           if word['SelectionStatus'] =='SELECTED':
                               text +=  'X '    
       return text
   
   
   def get_table_csv_results(file_name):
   
       with open(file_name, 'rb') as file:
           img_test = file.read()
           bytes_test = bytearray(img_test)
           print('Image loaded', file_name)
   
       # process using image bytes
       # get the results
       client = boto3.client('textract')
   
       response = client.analyze_document(Document={'Bytes': bytes_test}, FeatureTypes=['TABLES'])
   
       # Get the text blocks
       blocks=response['Blocks']
       pprint(blocks)
   
       blocks_map = {}
       table_blocks = []
       for block in blocks:
           blocks_map[block['Id']] = block
           if block['BlockType'] == "TABLE":
               table_blocks.append(block)
   
       if len(table_blocks) <= 0:
           return "<b> NO Table FOUND </b>"
   
       csv = ''
       for index, table in enumerate(table_blocks):
           csv += generate_table_csv(table, blocks_map, index +1)
           csv += '\n\n'
   
       return csv
   
   def generate_table_csv(table_result, blocks_map, table_index):
       rows = get_rows_columns_map(table_result, blocks_map)
   
       table_id = 'Table_' + str(table_index)
       
       # get cells.
       csv = 'Table: {0}\n\n'.format(table_id)
   
       for row_index, cols in rows.items():
           
           for col_index, text in cols.items():
               csv += '{}'.format(text) + ","
           csv += '\n'
           
       csv += '\n\n\n'
       return csv
   
   def main(file_name):
       table_csv = get_table_csv_results(file_name)
   
       output_file = 'output.csv'
   
       # replace content
       with open(output_file, "wt") as fout:
           fout.write(table_csv)
   
       # show the results
       print('CSV OUTPUT FILE: ', output_file)
   
   
   if __name__ == "__main__":
       file_name = sys.argv[1]
       main(file_name)
   ```

1. At the command prompt, enter the following command\. Replace `file` with the name of the document image file that you want to analyze\.

   ```
   python textract_python_table_parser.py file
   ```

When you run the example, the CSV output is saved in a file named `output.csv`\.

------
#### [ Asynchronous ]

In this example, you will use make use of two different scripts\. The first script starts the process of asynchronoulsy analyzing documents with `StartDocumentAnalysis` and gets the `Block` information returned by `GetDocumentAnalysis`\. The second script takes the returned `Block` information for each page, formats the data as a table, and saves the tables to a CSV file\.

**To export tables into a CSV file**

1. Configure your environment\. For more information, see [Prerequisites](examples-blocks.md#examples-prerequisites)\.

1. Ensure that you have followed the instructions given at see [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)\. The process documented on that page enables you to send and receive messages about the completion status of asynchronous jobs\.

1. In the following code example, replace the value of `roleArn` with the Arn assigned to the role that you created in Step 2\. Replace the value of `bucket` with the name of the S3 bucket containing your document\. Replace the value of `document` with the name of the document in your S3 bucket\. Replace the value of `region_name` with the name of your bucket's region\.

   Save the following example code to a file named *start\_doc\_analysis\_for\_table\_extraction\.py\.*\.

   ```
   import boto3
   import time
   
   class DocumentProcessor:
   
       jobId = ''
       region_name = ''
   
       roleArn = ''
       bucket = ''
       document = ''
   
       sqsQueueUrl = ''
       snsTopicArn = ''
       processType = ''
   
       def __init__(self, role, bucket, document, region):
           self.roleArn = role
           self.bucket = bucket
           self.document = document
           self.region_name = region
   
           self.textract = boto3.client('textract', region_name=self.region_name)
           self.sqs = boto3.client('sqs')
           self.sns = boto3.client('sns')
   
       def ProcessDocument(self):
   
           jobFound = False
   
           response = self.textract.start_document_analysis(DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                   FeatureTypes=["TABLES", "FORMS"], NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
           print('Processing type: Analysis')
   
           print('Start Job Id: ' + response['JobId'])
   
           print('Done!')
   
       def CreateTopicandQueue(self):
   
           millis = str(int(round(time.time() * 1000)))
   
           # Create SNS topic
           snsTopicName = "AmazonTextractTopic" + millis
   
           topicResponse = self.sns.create_topic(Name=snsTopicName)
           self.snsTopicArn = topicResponse['TopicArn']
   
           # create SQS queue
           sqsQueueName = "AmazonTextractQueue" + millis
           self.sqs.create_queue(QueueName=sqsQueueName)
           self.sqsQueueUrl = self.sqs.get_queue_url(QueueName=sqsQueueName)['QueueUrl']
   
           attribs = self.sqs.get_queue_attributes(QueueUrl=self.sqsQueueUrl,
                                                   AttributeNames=['QueueArn'])['Attributes']
   
           sqsQueueArn = attribs['QueueArn']
   
           # Subscribe SQS queue to SNS topic
           self.sns.subscribe(TopicArn=self.snsTopicArn, Protocol='sqs', Endpoint=sqsQueueArn)
   
           # Authorize SNS to write SQS queue
           policy = """{{
         "Version":"2012-10-17",
         "Statement":[
           {{
             "Sid":"MyPolicy",
             "Effect":"Allow",
             "Principal" : {{"AWS" : "*"}},
             "Action":"SQS:SendMessage",
             "Resource": "{}",
             "Condition":{{
               "ArnEquals":{{
                 "aws:SourceArn": "{}"
               }}
             }}
           }}
         ]
       }}""".format(sqsQueueArn, self.snsTopicArn)
   
           response = self.sqs.set_queue_attributes(
               QueueUrl=self.sqsQueueUrl,
               Attributes={
                   'Policy': policy
               })
   
   def main():
       roleArn = 'role-arn'
       bucket = 'bucket-name'
       document = 'document-name'
       region_name = 'region-name'
   
       analyzer = DocumentProcessor(roleArn, bucket, document, region_name)
       analyzer.CreateTopicandQueue()
       analyzer.ProcessDocument()
   
   if __name__ == "__main__":
       main()
   ```

1. Run the code\. The code will print a JobId\. Copy this JobId down\.

1.  Wait for your job to finish processing, and after it has finished, copy the following code to a file named *get\_doc\_analysis\_for\_table\_extraction\.py*\. Replace the value of `jobId` with the Job ID you copied down earlier\. Replace the value of `region_name` with the name of the region associated with your Textract role\. Replace the value of `file_name` with the name you want to give the output CSV\.

   ```
   import boto3
   from pprint import pprint
   
   jobId = ''
   region_name = ''
   file_name = ''
   
   textract = boto3.client('textract', region_name=region_name)
   
   # Display information about a block
   def DisplayBlockInfo(block):
       print("Block Id: " + block['Id'])
       print("Type: " + block['BlockType'])
       if 'EntityTypes' in block:
           print('EntityTypes: {}'.format(block['EntityTypes']))
   
       if 'Text' in block:
           print("Text: " + block['Text'])
   
       if block['BlockType'] != 'PAGE':
           print("Confidence: " + "{:.2f}".format(block['Confidence']) + "%")
   
   def GetResults(jobId, file_name):
       maxResults = 1000
       paginationToken = None
       finished = False
   
       while finished == False:
   
           response = None
   
           if paginationToken == None:
               response = textract.get_document_analysis(JobId=jobId, MaxResults=maxResults)
           else:
               response = textract.get_document_analysis(JobId=jobId, MaxResults=maxResults,
                                                              NextToken=paginationToken)
   
           blocks = response['Blocks']
           table_csv = get_table_csv_results(blocks)
           output_file = file_name + ".csv"
           # replace content
           with open(output_file, "at") as fout:
               fout.write(table_csv)
           # show the results
           print('Detected Document Text')
           print('Pages: {}'.format(response['DocumentMetadata']['Pages']))
           print('OUTPUT TO CSV FILE: ', output_file)
   
           # Display block information
           for block in blocks:
               DisplayBlockInfo(block)
               print()
               print()
   
           if 'NextToken' in response:
               paginationToken = response['NextToken']
           else:
               finished = True
   
   
   def get_rows_columns_map(table_result, blocks_map):
       rows = {}
       for relationship in table_result['Relationships']:
           if relationship['Type'] == 'CHILD':
               for child_id in relationship['Ids']:
                   try:
                       cell = blocks_map[child_id]
                       if cell['BlockType'] == 'CELL':
                           row_index = cell['RowIndex']
                           col_index = cell['ColumnIndex']
                           if row_index not in rows:
                               # create new row
                               rows[row_index] = {}
   
                           # get the text value
                           rows[row_index][col_index] = get_text(cell, blocks_map)
                   except KeyError:
                       print("Error extracting Table data - {}:".format(KeyError))
                       pass
       return rows
   
   
   def get_text(result, blocks_map):
       text = ''
       if 'Relationships' in result:
           for relationship in result['Relationships']:
               if relationship['Type'] == 'CHILD':
                   for child_id in relationship['Ids']:
                       try:
                           word = blocks_map[child_id]
                           if word['BlockType'] == 'WORD':
                               text += word['Text'] + ' '
                           if word['BlockType'] == 'SELECTION_ELEMENT':
                               if word['SelectionStatus'] == 'SELECTED':
                                   text += 'X '
                       except KeyError:
                           print("Error extracting Table data - {}:".format(KeyError))
   
       return text
   
   
   def get_table_csv_results(blocks):
   
       pprint(blocks)
   
       blocks_map = {}
       table_blocks = []
       for block in blocks:
           blocks_map[block['Id']] = block
           if block['BlockType'] == "TABLE":
               table_blocks.append(block)
   
       if len(table_blocks) <= 0:
           return "<b> NO Table FOUND </b>"
   
       csv = ''
       for index, table in enumerate(table_blocks):
           csv += generate_table_csv(table, blocks_map, index + 1)
           csv += '\n\n'
           # In order to generate separate CSV file for every table, uncomment code below
           #inner_csv = ''
           #inner_csv += generate_table_csv(table, blocks_map, index + 1)
           #inner_csv += '\n\n'
           #output_file = file_name + "___" + str(index) + ".csv"
           # replace content
           #with open(output_file, "at") as fout:
           #    fout.write(inner_csv)
   
       return csv
   
   
   def generate_table_csv(table_result, blocks_map, table_index):
       rows = get_rows_columns_map(table_result, blocks_map)
   
       table_id = 'Table_' + str(table_index)
   
       # get cells.
       csv = 'Table: {0}\n\n'.format(table_id)
   
       for row_index, cols in rows.items():
   
           for col_index, text in cols.items():
               csv += '{}'.format(text) + ","
           csv += '\n'
   
       csv += '\n\n\n'
       return csv
   
   response_blocks = GetResults(jobId, file_name)
   ```

1. Run the code\.

   After you have obtained you results, be sure to delete the associated SNS and SQS resources, or else you may accrue charges for them\.

------