# StartDocumentAnalysis<a name="API_StartDocumentAnalysis"></a>

Starts the asynchronous analysis of an input document for relationships between detected items such as key\-value pairs, tables, and selection elements\.

 `StartDocumentAnalysis` can analyze text in documents that are in JPEG, PNG, and PDF format\. The documents are stored in an Amazon S3 bucket\. Use [DocumentLocation](API_DocumentLocation.md) to specify the bucket name and file name of the document\. 

 `StartDocumentAnalysis` returns a job identifier \(`JobId`\) that you use to get the results of the operation\. When text analysis is finished, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that you specify in `NotificationChannel`\. To get the results of the text analysis operation, first check that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call [GetDocumentAnalysis](API_GetDocumentAnalysis.md), and pass the job identifier \(`JobId`\) from the initial call to `StartDocumentAnalysis`\.

For more information, see [Document Text Analysis](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-analyzing.html)\.

## Request Syntax<a name="API_StartDocumentAnalysis_RequestSyntax"></a>

```
{
   "[ClientRequestToken](#Textract-StartDocumentAnalysis-request-ClientRequestToken)": "string",
   "[DocumentLocation](#Textract-StartDocumentAnalysis-request-DocumentLocation)": { 
      "[S3Object](API_DocumentLocation.md#Textract-Type-DocumentLocation-S3Object)": { 
         "[Bucket](API_S3Object.md#Textract-Type-S3Object-Bucket)": "string",
         "[Name](API_S3Object.md#Textract-Type-S3Object-Name)": "string",
         "[Version](API_S3Object.md#Textract-Type-S3Object-Version)": "string"
      }
   },
   "[FeatureTypes](#Textract-StartDocumentAnalysis-request-FeatureTypes)": [ "string" ],
   "[JobTag](#Textract-StartDocumentAnalysis-request-JobTag)": "string",
   "[NotificationChannel](#Textract-StartDocumentAnalysis-request-NotificationChannel)": { 
      "[RoleArn](API_NotificationChannel.md#Textract-Type-NotificationChannel-RoleArn)": "string",
      "[SNSTopicArn](API_NotificationChannel.md#Textract-Type-NotificationChannel-SNSTopicArn)": "string"
   }
}
```

## Request Parameters<a name="API_StartDocumentAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ClientRequestToken](#API_StartDocumentAnalysis_RequestSyntax) **   <a name="Textract-StartDocumentAnalysis-request-ClientRequestToken"></a>
The idempotent token that you use to identify the start request\. If you use the same token with multiple `StartDocumentAnalysis` requests, the same `JobId` is returned\. Use `ClientRequestToken` to prevent the same job from being accidentally started more than once\. For more information, see [Calling Amazon Textract Asynchronous Operations](https://docs.aws.amazon.com/textract/latest/dg/api-async.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: No

 ** [DocumentLocation](#API_StartDocumentAnalysis_RequestSyntax) **   <a name="Textract-StartDocumentAnalysis-request-DocumentLocation"></a>
The location of the document to be processed\.  
Type: [DocumentLocation](API_DocumentLocation.md) object  
Required: Yes

 ** [FeatureTypes](#API_StartDocumentAnalysis_RequestSyntax) **   <a name="Textract-StartDocumentAnalysis-request-FeatureTypes"></a>
A list of the types of analysis to perform\. Add TABLES to the list to return information about the tables that are detected in the input document\. Add FORMS to return detected form data\. To perform both types of analysis, add TABLES and FORMS to `FeatureTypes`\. All lines and words detected in the document are included in the response \(including text that isn't related to the value of `FeatureTypes`\)\.   
Type: Array of strings  
Valid Values:` TABLES | FORMS`   
Required: Yes

 ** [JobTag](#API_StartDocumentAnalysis_RequestSyntax) **   <a name="Textract-StartDocumentAnalysis-request-JobTag"></a>
An identifier that you specify that's included in the completion notification published to the Amazon SNS topic\. For example, you can use `JobTag` to identify the type of document that the completion notification corresponds to \(such as a tax form or a receipt\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[a-zA-Z0-9_.\-:]+`   
Required: No

 ** [NotificationChannel](#API_StartDocumentAnalysis_RequestSyntax) **   <a name="Textract-StartDocumentAnalysis-request-NotificationChannel"></a>
The Amazon SNS topic ARN that you want Amazon Textract to publish the completion status of the operation to\.   
Type: [NotificationChannel](API_NotificationChannel.md) object  
Required: No

## Response Syntax<a name="API_StartDocumentAnalysis_ResponseSyntax"></a>

```
{
   "[JobId](#Textract-StartDocumentAnalysis-response-JobId)": "string"
}
```

## Response Elements<a name="API_StartDocumentAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [JobId](#API_StartDocumentAnalysis_ResponseSyntax) **   <a name="Textract-StartDocumentAnalysis-response-JobId"></a>
The identifier for the document text detection job\. Use `JobId` to identify the job in a subsequent call to `GetDocumentAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$` 

## Errors<a name="API_StartDocumentAnalysis_Errors"></a>

 **AccessDeniedException**   
You aren't authorized to perform the action\.  
HTTP Status Code: 400

 **BadDocumentException**   
Amazon Textract isn't able to read the document\.  
HTTP Status Code: 400

 **DocumentTooLargeException**   
The document can't be processed because it's too large\. The maximum document size for synchronous operations 5 MB\. The maximum document size for asynchronous operations is 500 MB for PDF files\.  
HTTP Status Code: 400

 **IdempotentParameterMismatchException**   
A `ClientRequestToken` input parameter was reused with an operation, but at least one of the other input parameters is different from the previous call to the operation\.   
HTTP Status Code: 400

 **InternalServerError**   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 **InvalidParameterException**   
An input parameter violated a constraint\. For example, in synchronous operations, an `InvalidParameterException` exception occurs when neither of the `S3Object` or `Bytes` values are supplied in the `Document` request parameter\. Validate your parameter before calling the API operation again\.  
HTTP Status Code: 400

 **InvalidS3ObjectException**   
Amazon Textract is unable to access the S3 object that's specified in the request\.  
HTTP Status Code: 400

 **LimitExceededException**   
An Amazon Textract service limit was exceeded\. For example, if you start too many asynchronous jobs concurrently, calls to start operations \(`StartDocumentTextDetection`, for example\) raise a LimitExceededException exception \(HTTP status code: 400\) until the number of concurrently running jobs is below the Amazon Textract service limit\.   
HTTP Status Code: 400

 **ProvisionedThroughputExceededException**   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Textract\.  
HTTP Status Code: 400

 **ThrottlingException**   
Amazon Textract is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

 **UnsupportedDocumentException**   
The format of the input document isn't supported\. Documents for synchronous operations can be in PNG or JPEG format\. Documents for asynchronous operations can also be in PDF format\.  
HTTP Status Code: 400

## See Also<a name="API_StartDocumentAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/StartDocumentAnalysis) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/textract-2018-06-27/StartDocumentAnalysis) 