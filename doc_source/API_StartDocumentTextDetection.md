# StartDocumentTextDetection<a name="API_StartDocumentTextDetection"></a>

Starts the asynchronous detection of text in a document\. Amazon Textract can detect lines of text and the words that make up a line of text\.

 `StartDocumentTextDetection` can analyze text in documents that are in JPEG, PNG, TIFF, and PDF format\. The documents are stored in an Amazon S3 bucket\. Use [DocumentLocation](API_DocumentLocation.md) to specify the bucket name and file name of the document\. 

 `StartTextDetection` returns a job identifier \(`JobId`\) that you use to get the results of the operation\. When text detection is finished, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that you specify in `NotificationChannel`\. To get the results of the text detection operation, first check that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call [GetDocumentTextDetection](API_GetDocumentTextDetection.md), and pass the job identifier \(`JobId`\) from the initial call to `StartDocumentTextDetection`\.

For more information, see [Document Text Detection](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-detecting.html)\.

## Request Syntax<a name="API_StartDocumentTextDetection_RequestSyntax"></a>

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

## Request Parameters<a name="API_StartDocumentTextDetection_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ClientRequestToken](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-ClientRequestToken"></a>
The idempotent token that's used to identify the start request\. If you use the same token with multiple `StartDocumentTextDetection` requests, the same `JobId` is returned\. Use `ClientRequestToken` to prevent the same job from being accidentally started more than once\. For more information, see [Calling Amazon Textract Asynchronous Operations](https://docs.aws.amazon.com/textract/latest/dg/api-async.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: No

 ** [DocumentLocation](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-DocumentLocation"></a>
The location of the document to be processed\.  
Type: [DocumentLocation](API_DocumentLocation.md) object  
Required: Yes

 ** [JobTag](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-JobTag"></a>
An identifier that you specify that's included in the completion notification published to the Amazon SNS topic\. For example, you can use `JobTag` to identify the type of document that the completion notification corresponds to \(such as a tax form or a receipt\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9_.\-:]+`   
Required: No

 ** [KMSKeyId](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-KMSKeyId"></a>
The KMS key used to encrypt the inference results\. This can be in either Key ID or Key Alias format\. When a KMS key is provided, the KMS key will be used for server\-side encryption of the objects in the customer bucket\. When this parameter is not enabled, the result will be encrypted server side,using SSE\-S3\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `^[A-Za-z0-9][A-Za-z0-9:_/+=,@.-]{0,2048}$`   
Required: No

 ** [NotificationChannel](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-NotificationChannel"></a>
The Amazon SNS topic ARN that you want Amazon Textract to publish the completion status of the operation to\.   
Type: [NotificationChannel](API_NotificationChannel.md) object  
Required: No

 ** [OutputConfig](#API_StartDocumentTextDetection_RequestSyntax) **   <a name="Textract-StartDocumentTextDetection-request-OutputConfig"></a>
Sets if the output will go to a customer defined bucket\. By default Amazon Textract will save the results internally to be accessed with the GetDocumentTextDetection operation\.  
Type: [OutputConfig](API_OutputConfig.md) object  
Required: No

## Response Syntax<a name="API_StartDocumentTextDetection_ResponseSyntax"></a>

```
{
   "JobId": "string"
}
```

## Response Elements<a name="API_StartDocumentTextDetection_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [JobId](#API_StartDocumentTextDetection_ResponseSyntax) **   <a name="Textract-StartDocumentTextDetection-response-JobId"></a>
The identifier of the text detection job for the document\. Use `JobId` to identify the job in a subsequent call to `GetDocumentTextDetection`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$` 

## Errors<a name="API_StartDocumentTextDetection_Errors"></a>

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

## See Also<a name="API_StartDocumentTextDetection_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/StartDocumentTextDetection) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/StartDocumentTextDetection) 