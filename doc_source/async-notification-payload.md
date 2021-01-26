# Amazon Textract Results Notification<a name="async-notification-payload"></a>

Amazon Textract publishes the results of an Amazon Textract analysis request, including completion status, to an Amazon Simple Notification Service \(Amazon SNS\) topic\. To get the notification from an Amazon SNS topic, use an Amazon SQS queue or an AWS Lambda function\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\. For an example, see [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md)\.

The payload is in the following JSON format:

```
{
  "JobId": "String",
  "Status": "String",
  "API": "String",
  "JobTag": "String",
  "Timestamp": Number,
  "DocumentLocation": {
    "S3ObjectName": "String",
    "S3Bucket": "String"
  }
}
```


| Name | Description | 
| --- | --- | 
|  JobId  |  The unique identifier that Amazon Textract assigns to the job\. It matches a job identifier that's returned from a `Start` operation, such as [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\.  | 
|  Status  |  The status of the job\. Valid values are SUCCEEDED, FAILED, or ERROR\.  | 
|  API  |  The Amazon Textract operation used to analyze the input document\.  | 
|  JobTag  |  The user\-specified identifier for the job\. You specify `JobTag` in a call to the `Start` operation, such as [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\.  | 
|  Timestamp  |  The Unix timestamp for when the job finished\.  | 
|  DocumentLocation  |  Details about the document that was processed\. Includes the file name and the Amazon S3 bucket that the file is stored in\.  | 
