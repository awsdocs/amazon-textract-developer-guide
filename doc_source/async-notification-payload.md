# Amazon Textract Results Notification<a name="async-notification-payload"></a>

Amazon Textract publishes the results of an Amazon Textract analysis request, including completion status, to an Amazon Simple Notification Service \(Amazon SNS\) topic\. To get the notification from an Amazon SNS topic, use an Amazon SQS queue or an AWS Lambda function\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\. For an example, see [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md)\.

The results have the following JSON format:

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

This table describes the different parameters within an Amazon Textract response\.


| Parameter | Description | 
| --- | --- | 
|  JobId  |  The unique identifier that Amazon Textract assigns to the job\. It matches a job identifier that's returned from a `Start` operation, such as [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\.  | 
|  Status  |  The status of the job\. Valid values are Succeeded, Failed, or Error\.  | 
|  API  |  The Amazon Textract operation used to analyze the input document, such as [StartDocumentTextDetection](API_StartDocumentTextDetection.md) or [StartDocumentAnalysis](API_StartDocumentAnalysis.md)\.  | 
|  JobTag  |  The user\-specified identifier for the job\. You specify `JobTag` in a call to the `Start` operation, such as [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\.  | 
|  Timestamp  |  The Unix timestamp that indicates when the job finished, returned in milliseconds\.  | 
|  DocumentLocation  |  Details about the document that was processed\. Includes the file name and the Amazon S3 bucket that the file is stored in\.  | 