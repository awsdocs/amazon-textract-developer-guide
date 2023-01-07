# StartLendingAnalysis<a name="API_StartLendingAnalysis"></a>

Starts the classification and analysis of an input document\. `StartLendingAnalysis` initiates the classification and analysis of a packet of lending documents\. `StartLendingAnalysis` operates on a document file located in an Amazon S3 bucket\.

 `StartLendingAnalysis` can analyze text in documents that are in one of the following formats: JPEG, PNG, TIFF, PDF\. Use `DocumentLocation` to specify the bucket name and the file name of the document\. 

 `StartLendingAnalysis` returns a job identifier \(`JobId`\) that you use to get the results of the operation\. When the text analysis is finished, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that you specify in `NotificationChannel`\. To get the results of the text analysis operation, first check that the status value published to the Amazon SNS topic is SUCCEEDED\. If the status is SUCCEEDED you can call either `GetLendingAnalysis` or `GetLendingAnalysisSummary` and provide the `JobId` to obtain the results of the analysis\.

If using `OutputConfig` to specify an Amazon S3 bucket, the output will be contained within the specified prefix in a directory labeled with the job\-id\. In the directory there are 3 sub\-directories: 
+ detailedResponse \(contains the GetLendingAnalysis response\)
+ summaryResponse \(for the GetLendingAnalysisSummary response\)
+ splitDocuments \(documents split across logical boundaries\)

## Request Syntax<a name="API_StartLendingAnalysis_RequestSyntax"></a>

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

## Request Parameters<a name="API_StartLendingAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ClientRequestToken](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-ClientRequestToken"></a>
The idempotent token that you use to identify the start request\. If you use the same token with multiple `StartLendingAnalysis` requests, the same `JobId` is returned\. Use `ClientRequestToken` to prevent the same job from being accidentally started more than once\. For more information, see [Calling Amazon Textract Asynchronous Operations](https://docs.aws.amazon.com/textract/latest/dg/api-sync.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: No

 ** [DocumentLocation](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-DocumentLocation"></a>
The Amazon S3 bucket that contains the document to be processed\. It's used by asynchronous operations\.  
The input document can be an image file in JPEG or PNG format\. It can also be a file in PDF format\.  
Type: [DocumentLocation](API_DocumentLocation.md) object  
Required: Yes

 ** [JobTag](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-JobTag"></a>
An identifier that you specify to be included in the completion notification published to the Amazon SNS topic\. For example, you can use `JobTag` to identify the type of document that the completion notification corresponds to \(such as a tax form or a receipt\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9_.\-:]+`   
Required: No

 ** [KMSKeyId](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-KMSKeyId"></a>
The KMS key used to encrypt the inference results\. This can be in either Key ID or Key Alias format\. When a KMS key is provided, the KMS key will be used for server\-side encryption of the objects in the customer bucket\. When this parameter is not enabled, the result will be encrypted server side, using SSE\-S3\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `^[A-Za-z0-9][A-Za-z0-9:_/+=,@.-]{0,2048}$`   
Required: No

 ** [NotificationChannel](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-NotificationChannel"></a>
The Amazon Simple Notification Service \(Amazon SNS\) topic to which Amazon Textract publishes the completion status of an asynchronous document operation\.   
Type: [NotificationChannel](API_NotificationChannel.md) object  
Required: No

 ** [OutputConfig](#API_StartLendingAnalysis_RequestSyntax) **   <a name="Textract-StartLendingAnalysis-request-OutputConfig"></a>
Sets whether or not your output will go to a user created bucket\. Used to set the name of the bucket, and the prefix on the output file\.  
 `OutputConfig` is an optional parameter which lets you adjust where your output will be placed\. By default, Amazon Textract will store the results internally and can only be accessed by the Get API operations\. With `OutputConfig` enabled, you can set the name of the bucket the output will be sent to the file prefix of the results where you can download your results\. Additionally, you can set the `KMSKeyID` parameter to a customer master key \(CMK\) to encrypt your output\. Without this parameter set Amazon Textract will encrypt server\-side using the AWS managed CMK for Amazon S3\.  
Decryption of Customer Content is necessary for processing of the documents by Amazon Textract\. If your account is opted out under an AI services opt out policy then all unencrypted Customer Content is immediately and permanently deleted after the Customer Content has been processed by the service\. No copy of of the output is retained by Amazon Textract\. For information about how to opt out, see [ Managing AI services opt\-out policy\. ](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_ai-opt-out.html)   
For more information on data privacy, see the [Data Privacy FAQ](https://aws.amazon.com/compliance/data-privacy-faq/)\.  
Type: [OutputConfig](API_OutputConfig.md) object  
Required: No

## Response Syntax<a name="API_StartLendingAnalysis_ResponseSyntax"></a>

```
{
   "JobId": "string"
}
```

## Response Elements<a name="API_StartLendingAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [JobId](#API_StartLendingAnalysis_ResponseSyntax) **   <a name="Textract-StartLendingAnalysis-response-JobId"></a>
A unique identifier for the lending or text\-detection job\. The `JobId` is returned from `StartLendingAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$` 

## Errors<a name="API_StartLendingAnalysis_Errors"></a>

 ** AccessDeniedException **   
You aren't authorized to perform the action\. Use the Amazon Resource Name \(ARN\) of an authorized user or IAM role to perform the operation\.  
HTTP Status Code: 400

 ** BadDocumentException **   
Amazon Textract isn't able to read the document\. For more information on the document limits in Amazon Textract, see [Quotas in Amazon Textract](limits.md)\.  
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

## See Also<a name="API_StartLendingAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/StartLendingAnalysis) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/StartLendingAnalysis) 