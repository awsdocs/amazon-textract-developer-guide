# Handling Throttled Calls and Dropped Connections<a name="handling-errors"></a>

An Amazon Textract operation might fail due to throttling by the Amazon Textract service or by a dropped connection\. For example, if you make too many calls to Amazon Textract operations in a short period of time, Amazon Textract throttles your calls and you receive a `ProvisionedThroughputExceededException` error in the operation response\. Your calls are throttled if you exceed the maximum number of transactions per second \(TPS\)\. For information about Amazon Textract TPS limits, see [Amazon Textract Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_textract)\.

If you're using an AWS SDK, one approach to managing throttling and dropped connections is to automatically retry the operation a set number of times\. You can specify the number of retries by including the `Config` parameter in the creation of the Amazon Textract client\. We recommend a retry count of five\. The AWS SDK retries an operation the specified number of times before failing and throwing an exception\. For more information, see [Error Retries and Exponential Backoff in AWS](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)\.

**Note**  
The automatic retry of operations is supported by synchronous and asynchronous Amazon Textract operations\. Ensure that you have the most recent version of the AWS SDK installed\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

The following example shows how to automatically retry Amazon Textract operations when you're processing multiple documents\. 

**To automatically retry operations \(API\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonTextractFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

1. Upload multiple document images to your S3 bucket\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Use the following examples to call the `DetectDocumentText` operation on the documents in your Amazon S3 bucket\. In the function `main`, change the value of `bucket` to your Amazon S3 bucket\. Change the value of `documents` to the names of document images that you uploaded in step 2\.

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