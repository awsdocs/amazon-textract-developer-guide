# GetLendingAnalysisSummary<a name="API_GetLendingAnalysisSummary"></a>

Gets summarized results for the `StartLendingAnalysis` operation, which analyzes text in a lending document\. The returned summary consists of information about documents grouped together by a common document type\. Information like detected signatures, page numbers, and split documents is returned with respect to the type of grouped document\. 

You start asynchronous text analysis by calling `StartLendingAnalysis`, which returns a job identifier \(`JobId`\)\. When the text analysis operation finishes, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that's registered in the initial call to `StartLendingAnalysis`\. 

To get the results of the text analysis operation, first check that the status value published to the Amazon SNS topic is SUCCEEDED\. If so, call `GetLendingAnalysisSummary`, and pass the job identifier \(`JobId`\) from the initial call to `StartLendingAnalysis`\.

## Request Syntax<a name="API_GetLendingAnalysisSummary_RequestSyntax"></a>

```
{
   "JobId": "string"
}
```

## Request Parameters<a name="API_GetLendingAnalysisSummary_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [JobId](#API_GetLendingAnalysisSummary_RequestSyntax) **   <a name="Textract-GetLendingAnalysisSummary-request-JobId"></a>
 A unique identifier for the lending or text\-detection job\. The `JobId` is returned from StartLendingAnalysis\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: Yes

## Response Syntax<a name="API_GetLendingAnalysisSummary_ResponseSyntax"></a>

```
{
   "AnalyzeLendingModelVersion": "string",
   "DocumentMetadata": { 
      "Pages": number
   },
   "JobStatus": "string",
   "StatusMessage": "string",
   "Summary": { 
      "DocumentGroups": [ 
         { 
            "DetectedSignatures": [ 
               { 
                  "Page": number
               }
            ],
            "SplitDocuments": [ 
               { 
                  "Index": number,
                  "Pages": [ number ]
               }
            ],
            "Type": "string",
            "UndetectedSignatures": [ 
               { 
                  "Page": number
               }
            ]
         }
      ],
      "UndetectedDocumentTypes": [ "string" ]
   },
   "Warnings": [ 
      { 
         "ErrorCode": "string",
         "Pages": [ number ]
      }
   ]
}
```

## Response Elements<a name="API_GetLendingAnalysisSummary_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeLendingModelVersion](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-AnalyzeLendingModelVersion"></a>
The current model version of the Analyze Lending API\.  
Type: String

 ** [DocumentMetadata](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-DocumentMetadata"></a>
Information about the input document\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [JobStatus](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-JobStatus"></a>
 The current status of the lending analysis job\.   
Type: String  
Valid Values:` IN_PROGRESS | SUCCEEDED | FAILED | PARTIAL_SUCCESS` 

 ** [StatusMessage](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-StatusMessage"></a>
Returns if the lending analysis could not be completed\. Contains explanation for what error occurred\.  
Type: String

 ** [Summary](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-Summary"></a>
 Contains summary information for documents grouped by type\.  
Type: [LendingSummary](API_LendingSummary.md) object

 ** [Warnings](#API_GetLendingAnalysisSummary_ResponseSyntax) **   <a name="Textract-GetLendingAnalysisSummary-response-Warnings"></a>
A list of warnings that occurred during the lending analysis operation\.  
Type: Array of [Warning](API_Warning.md) objects

## Errors<a name="API_GetLendingAnalysisSummary_Errors"></a>

 ** AccessDeniedException **   
You aren't authorized to perform the action\. Use the Amazon Resource Name \(ARN\) of an authorized user or IAM role to perform the operation\.  
HTTP Status Code: 400

 ** InternalServerError **   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 ** InvalidJobIdException **   
An invalid job identifier was passed to an asynchronous analysis operation\.  
HTTP Status Code: 400

 ** InvalidKMSKeyException **   
 Indicates you do not have decrypt permissions with the KMS key entered, or the KMS key was entered incorrectly\.   
HTTP Status Code: 400

 ** InvalidParameterException **   
An input parameter violated a constraint\. For example, in synchronous operations, an `InvalidParameterException` exception occurs when neither of the `S3Object` or `Bytes` values are supplied in the `Document` request parameter\. Validate your parameter before calling the API operation again\.  
HTTP Status Code: 400

 ** InvalidS3ObjectException **   
Amazon Textract is unable to access the S3 object that's specified in the request\. for more information, [Configure Access to Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html) For troubleshooting information, see [Troubleshooting Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/troubleshooting.html)   
HTTP Status Code: 400

 ** ProvisionedThroughputExceededException **   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Textract\.  
HTTP Status Code: 400

 ** ThrottlingException **   
Amazon Textract is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

## See Also<a name="API_GetLendingAnalysisSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/GetLendingAnalysisSummary) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/GetLendingAnalysisSummary) 