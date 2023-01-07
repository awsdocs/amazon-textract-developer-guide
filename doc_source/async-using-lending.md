# Using the Analyze Lending Workflow<a name="async-using-lending"></a>

 To detect text in, or analyze multipage lending documents, using the Analyze Lending workflow, you do the following:

1. Create the Amazon SNS topic and the Amazon SQS queue\.

1. Subscribe the queue the topic\.

1. Give the topic permission to send messages to the queue\.

1. Start processing the document\. Call `StartLendingAnalysis` operation\.

1. Get the completion status from the Amazon SQS queue\. The example code tracks the job identifier \(`JobId`\) that's returned by the `Start` operation\. The example code only gets the results for matching job identifiers that are read from the completion status\. This is important if other applications are using the same queue and topic\. For simplicity, the example code deletes jobs that don't match\. Consider adding the deleted jobs to an Amazon SQS dead\-letter queue for further investigation\.

   The results of the StartLendingAnalysis operation can be sent to an Amazon S3 bucket of your choice by using the OutputConfig feature\. If you use this feature, you may have to do some additional configuration of your User and Service Role\. For information on how to let Amazon Textract send encrypted documents to your Amazon S3 bucket, see [Permissions for Output Configuration](api-async-roles.md#async-output-config)\.

1. Get and display the processing results by calling the `GetLendingAnalysis` operation or the `GetLendingAnalysisSummary` operation\.

1. Once you are finished processing documents, be sure to delete the Amazon SNS topic and the Amazon SQS queue\. If you need to process additional documents, you can leave the Amazon SNS topic and Amazon SQS queue as they are and reuse them for the other documents\.

## Performing Asynchronous Lending Analysis<a name="async-using-lending-analysis"></a>

The example code for this procedure is provided for Python and the AWS CLI\. Before you begin, install the appropriate AWS SDK\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\. 

1. Configure user access to Amazon Textract, and configure Amazon Textract access to Amazon SNS\. For more information, see [Configuring Amazon Textract for Asynchronous Operations](https://docs.aws.amazon.com/en_us/textract/latest/dg/api-async-roles.html)\. To complete this procedure, you need a multipage document ﬁle in PDF format\. You can skip steps 3 – 6 in the configuration instructions, because the example code creates and configures the Amazon SNS topic and Amazon SQS queue\. If completing the CLI example, you don't need to set up an SQS queue\. 

1. Upload a multipage document file in PDF or TIFF format to your Amazon S3 bucket \(you can also process single\-page documents in JPEG, PNG, TIFF, or PDF formats\)\. For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html)in the *Amazon Simple Storage Service User Guide*\. 

1. Use the following AWS SDK for Python \(Boto3\) or AWS CLI code to analyze text in a multipage lending document\. In the main function: 
   + Replace the value of `roleArn` with the IAM role ARN that you saved in [Giving Amazon Textract Access to Your Amazon SNS Topic](https://docs.aws.amazon.com/en_us/textract/latest/dg/api-async-roles.html#api-async-roles-all-topics)\.
   + Replace the values of `bucket` and `document` with the bucket and document file name that you previously specified in the proceeding Step 2\.
   + Replace the value of the `type` input parameter of the `ProcessDocument` function with the type of processing that you want to use\. For example, use `ProcessType.DETECTION` to detect text, or use `ProcessType.ANALYSIS` to analyze text\.
   + For the Python example, replace the value of `region_name` with the region your client is operating in\.

    For the upcoming AWS CLI example code, do the following: 
   + When calling the [StartLendingAnalysis](https://docs.aws.amazon.com/en_us/textract/latest/dg/API_StartLendingAnalysis.html) operation, replace the value of `bucket-name` with the name of your S3 bucket, and replace `FileName` with the name of the file you specified in step 2\. Specify the region of your bucket by replacing `region-name` with the name of your region\. Take note that the CLI example does not make use of SQS\.
   + When calling the [GetLendingAnalysis](https://docs.aws.amazon.com/en_us/textract/latest/dg/API_GetLendingAnalysis.html) operation or the [GetLendingAnalysisSummary](https://docs.aws.amazon.com/en_us/textract/latest/dg/API_GetLendingAnalysisSummary.html) operation, replace `jobId` with the `jobId` returned by [StartLendingAnalysis](https://docs.aws.amazon.com/en_us/textract/latest/dg/API_StartLendingAnalysis.html)\. Specify the region of your bucket by replacing `region-name` with the name of your region\.

1. Run the code for your chosen SDK or the AWS CLI\. 

   The operation might take a while to finish\. After it's finished, a list of blocks for detected or analyzed text is displayed by the follwing examples:

------
#### [ AWS CLI ]

   To start the lending document analysis use the following CLI command\. If you want to see splitted documents, use the `output-config` argument, otherwise you can remove it : 

   ```
   aws textract start-lending-analysis \
   --document-location '{"S3Object":{"Bucket":"S3Bucket","Name":"FileName"}}' \
   --output-config '{"S3Bucket": "S3Bucket",  "S3Prefix": "S3Prefix"}' \
   --kms-key-id '1234abcd-12ab-34cd-56ef-1234567890ab' \
   --region 'region-name'
   ```

   To get the results of the lending document analysis use the following CLI command\. The `max-results` argument is optional, and if you don't want to limit the number of results returned you can remove it:

   ```
   aws textract get-lending-analysis \
   --job-id 'jobId' \
   --region 'us-west-2' \
   --max-results 30
   ```

   To retrieve a summary of the results:

   ```
   aws textract get-lending-analysis-summary \
   --job-id 'jobId' \
   --region 'us-west-2'
   ```

------
#### [ Python ]

   ```
   import boto3
   import json
   import sys
   import time
   
   class DocumentProcessor:
   
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
   
           response = self.textract.start_lending_analysis(
               DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
               NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
           print('Processing type: Analysis')
   
           print('Start Job Id: ' + response['JobId'])
           dotLine = 0
           while jobFound == False:
               sqsResponse = self.sqs.receive_message(QueueUrl=self.sqsQueueUrl, MessageAttributeNames=['ALL'],
                                                      MaxNumberOfMessages=10)
               if sqsResponse:
                   if 'Messages' not in sqsResponse:
                       if dotLine < 40:
                           print('.', end='')
                           dotLine = dotLine + 1
                       else:
                           print()
                           dotLine = 0
                       sys.stdout.flush()
                       time.sleep(5)
                       continue
   
                   for message in sqsResponse['Messages']:
                       notification = json.loads(message['Body'])
                       textMessage = json.loads(notification['Message'])
                       print(textMessage['JobId'])
                       print(textMessage['Status'])
                       if str(textMessage['JobId']) == response['JobId']:
                           print('Matching Job Found:' + textMessage['JobId'])
                           jobFound = True
                           self.GetResults(textMessage['JobId'])
                           self.GetSummary(textMessage['JobId'])
                           self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                                   ReceiptHandle=message['ReceiptHandle'])
                       else:
                           print("Job didn't match:" +
                                 str(textMessage['JobId']) + ' : ' + str(response['JobId']))
                       # Delete the unknown message. Consider sending to dead letter queue
                       self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                               ReceiptHandle=message['ReceiptHandle'])
   
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
           self.sns.subscribe(
               TopicArn=self.snsTopicArn,
               Protocol='sqs',
               Endpoint=sqsQueueArn)
   
           # Authorize SNS to write SQS queue
           policy = """{{
     "Version":"2012-10-17",
     "Statement":[
       {{
         "Sid":"MyPolicy",
         "Effect":"Allow",
         "Principal" : {{"AWS" : "*"}},
         "Action":"sqs:*",
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
   
       def DeleteTopicandQueue(self):
           self.sqs.delete_queue(QueueUrl=self.sqsQueueUrl)
           self.sns.delete_topic(TopicArn=self.snsTopicArn)
   
       # Display information about a block
       def DisplayExtractInfo(self, response):
           results = response['Results']
           for page in results:
               print("Page Classification: {}".format(page["PageClassification"]["PageType"]))
               print("Page Number: {}".format(page["Page"]))
               for extract in page["Extractions"]:
                   for fields, vals in extract['LendingDocument'].items():
                       for val in vals:
                           print("Document Type: {}".format(val['Type']))
                           detections = val['ValueDetections']
                           for i in detections:
                               print(i['Text'])
                               print('Geometry')
                               print('\tBounding Box: {}'.format(i['Geometry']['BoundingBox']))
                               print('\tPolygon: {}'.format(i['Geometry']['Polygon']))
   
       def GetSummary(self, jobId):
           
           maxResults = 1000
           response = self.textract.get_lending_analysis_summary(JobId=jobId, MaxResults=maxResults)
           doc_groups = response['DocumentGroups']
           print("Summary info:")
           for group in doc_groups:
               print("Document type: " + group['Type'])
               split_docs = group['SplitDocuments']
               for doc in split_docs:
                   print(doc)
                   for idx, page in doc.items():
                       print(str(idx) + " - " + str(page))
   
       def GetResults(self, jobId):
   
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
   
               response = None
               if paginationToken == None:
                   response = self.textract.get_lending_analysis(JobId=jobId,
                                                                 MaxResults=maxResults)
               else:
                   response = self.textract.get_lending_analysis(JobId=jobId,
                                                                 MaxResults=maxResults,
                                                                 NextToken=paginationToken)
   
               print('Detected Document Text')
               print('Pages: {}'.format(response['DocumentMetadata']['Pages']))
   
               self.DisplayExtractInfo(response)
   
               if 'NextToken' in response:
                   paginationToken = response['NextToken']
               else:
                   finished = True
   
   def main():
       roleArn = ''
       bucket = ''
       document = ''
       region_name = ''
   
       analyzer = DocumentProcessor(roleArn, bucket, document, region_name)
       analyzer.CreateTopicandQueue()
       analyzer.ProcessDocument()
       analyzer.DeleteTopicandQueue()
   
   
   if __name__ == "__main__":
       main()
   ```

------