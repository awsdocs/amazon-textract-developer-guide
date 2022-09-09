# StartExpenseAnalysis<a name="API_StartExpenseAnalysis"></a>

Starts the asynchronous analysis of invoices or receipts for data like contact information, items purchased, and vendor names\.

 `StartExpenseAnalysis` can analyze text in documents that are in JPEG, PNG, and PDF format\. The documents must be stored in an Amazon S3 bucket\. Use the [DocumentLocation](API_DocumentLocation.md) parameter to specify the name of your S3 bucket and the name of the document in that bucket\. 

 `StartExpenseAnalysis` returns a job identifier \(`JobId`\) that you will provide to `GetExpenseAnalysis` to retrieve the results of the operation\. When the analysis of the input invoices/receipts is finished, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that you provide to the `NotificationChannel`\. To obtain the results of the invoice and receipt analysis operation, ensure that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call [GetExpenseAnalysis](API_GetExpenseAnalysis.md), and pass the job identifier \(`JobId`\) that was returned by your call to `StartExpenseAnalysis`\.

For more information, see [Analyzing Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoice-receipts.html)\.

## Request Syntax<a name="API_StartExpenseAnalysis_RequestSyntax"></a>

```
{
   "ClientRequestToken": "string",
   "DocumentLocation": { 
      "S3Object": { 
         "Bucket": "string",
         "Name": "string",
         "Version": "string"
      }
   },
   "JobTag": "string",
   "KMSKeyId": "string",
   "NotificationChannel": { 
      "RoleArn": "string",
      "SNSTopicArn": "string"
   },
   "OutputConfig": { 
      "S3Bucket": "string",
      "S3Prefix": "string"
   }
}
```

## Request Parameters<a name="API_StartExpenseAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ClientRequestToken](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-ClientRequestToken"></a>
The idempotent token that's used to identify the start request\. If you use the same token with multiple `StartDocumentTextDetection` requests, the same `JobId` is returned\. Use `ClientRequestToken` to prevent the same job from being accidentally started more than once\. For more information, see [Calling Amazon Textract Asynchronous Operations](https://docs.aws.amazon.com/textract/latest/dg/api-async.html)   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: No

 ** [DocumentLocation](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-DocumentLocation"></a>
The location of the document to be processed\.  
Type: [DocumentLocation](API_DocumentLocation.md) object  
Required: Yes

 ** [JobTag](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-JobTag"></a>
An identifier you specify that's included in the completion notification published to the Amazon SNS topic\. For example, you can use `JobTag` to identify the type of document that the completion notification corresponds to \(such as a tax form or a receipt\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9_.\-:]+`   
Required: No

 ** [KMSKeyId](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-KMSKeyId"></a>
The KMS key used to encrypt the inference results\. This can be in either Key ID or Key Alias format\. When a KMS key is provided, the KMS key will be used for server\-side encryption of the objects in the customer bucket\. When this parameter is not enabled, the result will be encrypted server side,using SSE\-S3\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `^[A-Za-z0-9][A-Za-z0-9:_/+=,@.-]{0,2048}$`   
Required: No

 ** [NotificationChannel](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-NotificationChannel"></a>
The Amazon SNS topic ARN that you want Amazon Textract to publish the completion status of the operation to\.   
Type: [NotificationChannel](API_NotificationChannel.md) object  
Required: No

 ** [OutputConfig](#API_StartExpenseAnalysis_RequestSyntax) **   <a name="Textract-StartExpenseAnalysis-request-OutputConfig"></a>
Sets if the output will go to a customer defined bucket\. By default, Amazon Textract will save the results internally to be accessed by the `GetExpenseAnalysis` operation\.  
Type: [OutputConfig](API_OutputConfig.md) object  
Required: No

## Response Syntax<a name="API_StartExpenseAnalysis_ResponseSyntax"></a>

```
{
   "JobId": "string"
}
```

## Response Elements<a name="API_StartExpenseAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [JobId](#API_StartExpenseAnalysis_ResponseSyntax) **   <a name="Textract-StartExpenseAnalysis-response-JobId"></a>
A unique identifier for the text detection job\. The `JobId` is returned from `StartExpenseAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$` 

## Errors<a name="API_StartExpenseAnalysis_Errors"></a>

 ** AccessDeniedException **   
You aren't authorized to perform the action\. Use the Amazon Resource Name \(ARN\) of an authorized user or IAM role to perform the operation\.  
HTTP Status Code: 400

 ** BadDocumentException **   
Amazon Textract isn't able to read the document\. For more information on the document limits in Amazon Textract, see [Hard Limits in Amazon Textract](limits.md)\.  
HTTP Status Code: 400

 ** DocumentTooLargeException **   
The document can't be processed because it's too large\. The maximum document size for synchronous operations 10 MB\. The maximum document size for asynchronous operations is 500 MB for PDF files\.  
HTTP Status Code: 400

 ** IdempotentParameterMismatchException **   
A `ClientRequestToken` input parameter was reused with an operation, but at least one of the other input parameters is different from the previous call to the operation\.   
HTTP Status Code: 400

 ** InternalServerError **   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 ** InvalidKMSKeyException **   
 Indicates you do not have decrypt permissions with the KMS key entered, or the KMS key was entered incorrectly\.   
HTTP Status Code: 400

 ** InvalidParameterException **   
An input parameter violated a constraint\. For example, in synchronous operations, an `InvalidParameterException` exception occurs when neither of the `S3Object` or `Bytes` values are supplied in the `Document` request parameter\. Validate your parameter before calling the API operation again\.  
HTTP Status Code: 400

 ** InvalidS3ObjectException **   
Amazon Textract is unable to access the S3 object that's specified in the request\. for more information, [Configure Access to Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html) For troubleshooting information, see [Troubleshooting Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/troubleshooting.html)   
HTTP Status Code: 400

 ** LimitExceededException **   
An Amazon Textract service limit was exceeded\. For example, if you start too many asynchronous jobs concurrently, calls to start operations \(`StartDocumentTextDetection`, for example\) raise a LimitExceededException exception \(HTTP status code: 400\) until the number of concurrently running jobs is below the Amazon Textract service limit\.   
HTTP Status Code: 400

 ** ProvisionedThroughputExceededException **   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Textract\.  
HTTP Status Code: 400

 ** ThrottlingException **   
Amazon Textract is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

 ** UnsupportedDocumentException **   
The format of the input document isn't supported\. Documents for operations can be in PNG, JPEG, PDF, or TIFF format\.  
HTTP Status Code: 400

## See Also<a name="API_StartExpenseAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/StartExpenseAnalysis) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/StartExpenseAnalysis) 