# Handling Throttled Calls and Dropped Connections<a name="handling-errors"></a>

An Amazon Textract operation can fail if you exceed the maximum number of transactions per second \(TPS\), causing the service to throttle your application, or when your connection drops\. For example, if you make too many calls to Amazon Textract operations in a short period of time, it throttles your calls and sends a `ProvisionedThroughputExceededException` error in the operation response\. For information about Amazon Textract TPS quotas, see [Amazon Textract Quotas](https://docs.aws.amazon.com/general/latest/gr/textract.html)\. To change a limit, you can access the Amazon Textract option in the Service Quotas console\.

You can manage throttling and dropped connections by automatically retrying the operation\. You can specify the number of retries by including the `Config` parameter when you create the Amazon Textract client\. We recommend a retry count of 5\. The AWS SDK retries an operation the specified number of times before failing and throwing an exception\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)\.

**Note**  
Automatic retries work for both synchronous and asynchronous operations\. Before specifying automatic retries, make sure you have the most recent version of the AWS SDK\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

The following example shows how to automatically retry Amazon Textract operations when you're processing multiple documents\. 

**Prerequisites**
+ If you haven't already:

  1. Create or update an IAM user with `AmazonTextractFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

  1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

**To automatically retry operations**

1. Upload multiple document images to your S3 bucket to run the Synchronous example\. Upload a multi\-page document to your S3 bucket and run `StartDocumentTextDetection` on it to run the Asynchronous example\.

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service User Guide*\.

1. The following examples demonstrate how to use the `Config` parameter to automatically retry an operation\. The Synchronous example calls the `DetectDocumentText` operation, while the Asynchronous example calls the `GetDocumentTextDetection` operation\. 

------
#### [ Sync Example ]

   Use the following examples to call the `DetectDocumentText` operation on the documents in your Amazon S3 bucket\. In `main`, change the value of `bucket` to your S3 bucket\. Change the value of `documents` to the names of the document images that you uploaded in step 2\.

   ```
   import boto3
   from botocore.client import Config
   # Documents
   
   def process_multiple_documents(bucket, documents):
       
       config = Config(retries = dict(max_attempts = 5))
    
       # Amazon Textract client
       textract = boto3.client('textract', config=config)
    
       for documentName in documents:
    
           print("\nProcessing: {}\n==========================================".format(documentName))
    
           # Call Amazon Textract
           response = textract.detect_document_text(
               Document={
                   'S3Object': {
                       'Bucket': bucket,
                       'Name': documentName
                   }
               })
    
           # Print detected text
           for item in response["Blocks"]:
               if item["BlockType"] == "LINE":
                   print ('\033[94m' +  item["Text"] + '\033[0m')
   
   
   def main():
       bucket = ""
       documents = ["document-image-1.png",
       "document-image-2.png", "document-image-3.png",
       "document-image-4.png", "document-image-5.png" ]
       process_multiple_documents(bucket, documents)
   
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Async Example ]

   Use the following examples to call the `GetDocumentTextDetection` operation\. It assumes you have already called `StartDocumentTextDetection` on the documents in your Amazon S3 bucket and obtained a `JobId`\. In `main`, change the value of `bucket` to your S3 bucket and the value of `roleArn` to the Arn assigned to your Textract role\. You'll also need to change the value of `document` to the name of your multi\-page document in your Amazon S3 bucket\. Finally, replace the value of `region_name` with the name of your region and provide the `GetResults` function with the name of your `jobId`\.

   ```
   import boto3
   from botocore.client import Config
   
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
           self.config = Config(retries = dict(max_attempts = 5))
   
           self.textract = boto3.client('textract', region_name=self.region_name, config=self.config)
           self.sqs = boto3.client('sqs')
           self.sns = boto3.client('sns')
   
   # Display information about a block
       def DisplayBlockInfo(self, block):
   
           print("Block Id: " + block['Id'])
           print("Type: " + block['BlockType'])
           if 'EntityTypes' in block:
               print('EntityTypes: {}'.format(block['EntityTypes']))
   
           if 'Text' in block:
               print("Text: " + block['Text'])
   
           if block['BlockType'] != 'PAGE':
               print("Confidence: " + "{:.2f}".format(block['Confidence']) + "%")
   
           print('Page: {}'.format(block['Page']))
   
           if block['BlockType'] == 'CELL':
               print('Cell Information')
               print('\tColumn: {} '.format(block['ColumnIndex']))
               print('\tRow: {}'.format(block['RowIndex']))
               print('\tColumn span: {} '.format(block['ColumnSpan']))
               print('\tRow span: {}'.format(block['RowSpan']))
   
               if 'Relationships' in block:
                   print('\tRelationships: {}'.format(block['Relationships']))
   
           print('Geometry')
           print('\tBounding Box: {}'.format(block['Geometry']['BoundingBox']))
           print('\tPolygon: {}'.format(block['Geometry']['Polygon']))
   
           if block['BlockType'] == 'SELECTION_ELEMENT':
               print('    Selection element detected: ', end='')
               if block['SelectionStatus'] == 'SELECTED':
                   print('Selected')
               else:
                   print('Not selected')
   
       def GetResults(self, jobId):
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
   
               response = None
   
               if paginationToken == None:
                   response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults)
               else:
                   response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults,
                                                                            NextToken=paginationToken)
   
               blocks = response['Blocks']
               print('Detected Document Text')
               print('Pages: {}'.format(response['DocumentMetadata']['Pages']))
   
               # Display block information
               for block in blocks:
                   self.DisplayBlockInfo(block)
                   print()
                   print()
   
               if 'NextToken' in response:
                   paginationToken = response['NextToken']
               else:
                   finished = True
   
   def main():
       roleArn = 'role-arn'
       bucket = 'bucket-name'
       document = 'document-name'
       region_name = 'region-name'
       analyzer = DocumentProcessor(roleArn, bucket, document, region_name)
       analyzer.GetResults("job-id")
   
   if __name__ == "__main__":
       main()
   ```

------