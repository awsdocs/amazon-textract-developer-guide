# GetDocumentAnalysis<a name="API_GetDocumentAnalysis"></a>

Gets the results for an Amazon Textract asynchronous operation that analyzes text in a document\.

You start asynchronous text analysis by calling [StartDocumentAnalysis](API_StartDocumentAnalysis.md), which returns a job identifier \(`JobId`\)\. When the text analysis operation finishes, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that's registered in the initial call to `StartDocumentAnalysis`\. To get the results of the text\-detection operation, first check that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call `GetDocumentAnalysis`, and pass the job identifier \(`JobId`\) from the initial call to `StartDocumentAnalysis`\.

 `GetDocumentAnalysis` returns an array of [Block](API_Block.md) objects\. The following types of information are returned: 
+ Form data \(key\-value pairs\)\. The related information is returned in two [Block](API_Block.md) objects, each of type `KEY_VALUE_SET`: a KEY `Block` object and a VALUE `Block` object\. For example, *Name: Ana Silva Carolina* contains a key and value\. *Name:* is the key\. *Ana Silva Carolina* is the value\.
+ Table and table cell data\. A TABLE `Block` object contains information about a detected table\. A CELL `Block` object is returned for each cell in a table\.
+ Lines and words of text\. A LINE `Block` object contains one or more WORD `Block` objects\. All lines and words that are detected in the document are returned \(including text that doesn't have a relationship with the value of the `StartDocumentAnalysis` `FeatureTypes` input parameter\)\. 
+ Query\. A QUERY Block object contains the query text, alias and link to the associated Query results block object\.
+ Query Results\. A QUERY\_RESULT Block object contains the answer to the query and an ID that connects it to the query asked\. This Block also contains a confidence score\.

**Note**  
While processing a document with queries, look out for `INVALID_REQUEST_PARAMETERS` output\. This indicates that either the per page query limit has been exceeded or that the operation is trying to query a page in the document which doesn’t exist\. 

Selection elements such as check boxes and option buttons \(radio buttons\) can be detected in form data and in tables\. A SELECTION\_ELEMENT `Block` object contains information about a selection element, including the selection status\.

Use the `MaxResults` parameter to limit the number of blocks that are returned\. If there are more results than specified in `MaxResults`, the value of `NextToken` in the operation response contains a pagination token for getting the next set of results\. To get the next page of results, call `GetDocumentAnalysis`, and populate the `NextToken` request parameter with the token value that's returned from the previous call to `GetDocumentAnalysis`\.

For more information, see [Document Text Analysis](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-analyzing.html)\.

## Request Syntax<a name="API_GetDocumentAnalysis_RequestSyntax"></a>

```
{
   "JobId": "string",
   "MaxResults": number,
   "NextToken": "string"
}
```

## Request Parameters<a name="API_GetDocumentAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [JobId](#API_GetDocumentAnalysis_RequestSyntax) **   <a name="Textract-GetDocumentAnalysis-request-JobId"></a>
A unique identifier for the text\-detection job\. The `JobId` is returned from `StartDocumentAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: Yes

 ** [MaxResults](#API_GetDocumentAnalysis_RequestSyntax) **   <a name="Textract-GetDocumentAnalysis-request-MaxResults"></a>
The maximum number of results to return per paginated call\. The largest value that you can specify is 1,000\. If you specify a value greater than 1,000, a maximum of 1,000 results is returned\. The default value is 1,000\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 ** [NextToken](#API_GetDocumentAnalysis_RequestSyntax) **   <a name="Textract-GetDocumentAnalysis-request-NextToken"></a>
If the previous response was incomplete \(because there are more blocks to retrieve\), Amazon Textract returns a pagination token in the response\. You can use this pagination token to retrieve the next set of blocks\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*`   
Required: No

## Response Syntax<a name="API_GetDocumentAnalysis_ResponseSyntax"></a>

```
{
   "AnalyzeDocumentModelVersion": "string",
   "Blocks": [ 
      { 
         "BlockType": "string",
         "ColumnIndex": number,
         "ColumnSpan": number,
         "Confidence": number,
         "EntityTypes": [ "string" ],
         "Geometry": { 
            "BoundingBox": { 
               "Height": number,
               "Left": number,
               "Top": number,
               "Width": number
            },
            "Polygon": [ 
               { 
                  "X": number,
                  "Y": number
               }
            ]
         },
         "Id": "string",
         "Page": number,
         "Query": { 
            "Alias": "string",
            "Pages": [ "string" ],
            "Text": "string"
         },
         "Relationships": [ 
            { 
               "Ids": [ "string" ],
               "Type": "string"
            }
         ],
         "RowIndex": number,
         "RowSpan": number,
         "SelectionStatus": "string",
         "Text": "string",
         "TextType": "string"
      }
   ],
   "DocumentMetadata": { 
      "Pages": number
   },
   "JobStatus": "string",
   "NextToken": "string",
   "StatusMessage": "string",
   "Warnings": [ 
      { 
         "ErrorCode": "string",
         "Pages": [ number ]
      }
   ]
}
```

## Response Elements<a name="API_GetDocumentAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeDocumentModelVersion](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-AnalyzeDocumentModelVersion"></a>
  
Type: String

 ** [Blocks](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-Blocks"></a>
The results of the text\-analysis operation\.  
Type: Array of [Block](API_Block.md) objects

 ** [DocumentMetadata](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-DocumentMetadata"></a>
Information about a document that Amazon Textract processed\. `DocumentMetadata` is returned in every page of paginated responses from an Amazon Textract video operation\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [JobStatus](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-JobStatus"></a>
The current status of the text detection job\.  
Type: String  
Valid Values:` IN_PROGRESS | SUCCEEDED | FAILED | PARTIAL_SUCCESS` 

 ** [NextToken](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-NextToken"></a>
If the response is truncated, Amazon Textract returns this token\. You can use this token in the subsequent request to retrieve the next set of text detection results\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*` 

 ** [StatusMessage](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-StatusMessage"></a>
Returns if the detection job could not be completed\. Contains explanation for what error occured\.  
Type: String

 ** [Warnings](#API_GetDocumentAnalysis_ResponseSyntax) **   <a name="Textract-GetDocumentAnalysis-response-Warnings"></a>
A list of warnings that occurred during the document\-analysis operation\.  
Type: Array of [Warning](API_Warning.md) objects

## Errors<a name="API_GetDocumentAnalysis_Errors"></a>

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

## See Also<a name="API_GetDocumentAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/GetDocumentAnalysis) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/GetDocumentAnalysis) 