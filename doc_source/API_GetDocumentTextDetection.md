# GetDocumentTextDetection<a name="API_GetDocumentTextDetection"></a>

Gets the results for an Amazon Textract asynchronous operation that detects text in a document\. Amazon Textract can detect lines of text and the words that make up a line of text\.

You start asynchronous text detection by calling [StartDocumentTextDetection](API_StartDocumentTextDetection.md), which returns a job identifier \(`JobId`\)\. When the text detection operation finishes, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that's registered in the initial call to `StartDocumentTextDetection`\. To get the results of the text\-detection operation, first check that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call `GetDocumentTextDetection`, and pass the job identifier \(`JobId`\) from the initial call to `StartDocumentTextDetection`\.

 `GetDocumentTextDetection` returns an array of [Block](API_Block.md) objects\. 

Each document page has as an associated `Block` of type PAGE\. Each PAGE `Block` object is the parent of LINE `Block` objects that represent the lines of detected text on a page\. A LINE `Block` object is a parent for each word that makes up the line\. Words are represented by `Block` objects of type WORD\.

Use the MaxResults parameter to limit the number of blocks that are returned\. If there are more results than specified in `MaxResults`, the value of `NextToken` in the operation response contains a pagination token for getting the next set of results\. To get the next page of results, call `GetDocumentTextDetection`, and populate the `NextToken` request parameter with the token value that's returned from the previous call to `GetDocumentTextDetection`\.

For more information, see [Document Text Detection](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-detecting.html)\.

## Request Syntax<a name="API_GetDocumentTextDetection_RequestSyntax"></a>

```
{
   "[JobId](#Textract-GetDocumentTextDetection-request-JobId)": "string",
   "[MaxResults](#Textract-GetDocumentTextDetection-request-MaxResults)": number,
   "[NextToken](#Textract-GetDocumentTextDetection-request-NextToken)": "string"
}
```

## Request Parameters<a name="API_GetDocumentTextDetection_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [JobId](#API_GetDocumentTextDetection_RequestSyntax) **   <a name="Textract-GetDocumentTextDetection-request-JobId"></a>
A unique identifier for the text detection job\. The `JobId` is returned from `StartDocumentTextDetection`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: Yes

 ** [MaxResults](#API_GetDocumentTextDetection_RequestSyntax) **   <a name="Textract-GetDocumentTextDetection-request-MaxResults"></a>
The maximum number of results to return per paginated call\. The largest value you can specify is 1,000\. If you specify a value greater than 1,000, a maximum of 1,000 results is returned\. The default value is 1,000\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 ** [NextToken](#API_GetDocumentTextDetection_RequestSyntax) **   <a name="Textract-GetDocumentTextDetection-request-NextToken"></a>
If the previous response was incomplete \(because there are more blocks to retrieve\), Amazon Textract returns a pagination token in the response\. You can use this pagination token to retrieve the next set of blocks\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*`   
Required: No

## Response Syntax<a name="API_GetDocumentTextDetection_ResponseSyntax"></a>

```
{
   "[Blocks](#Textract-GetDocumentTextDetection-response-Blocks)": [ 
      { 
         "[BlockType](API_Block.md#Textract-Type-Block-BlockType)": "string",
         "[ColumnIndex](API_Block.md#Textract-Type-Block-ColumnIndex)": number,
         "[ColumnSpan](API_Block.md#Textract-Type-Block-ColumnSpan)": number,
         "[Confidence](API_Block.md#Textract-Type-Block-Confidence)": number,
         "[EntityTypes](API_Block.md#Textract-Type-Block-EntityTypes)": [ "string" ],
         "[Geometry](API_Block.md#Textract-Type-Block-Geometry)": { 
            "[BoundingBox](API_Geometry.md#Textract-Type-Geometry-BoundingBox)": { 
               "[Height](API_BoundingBox.md#Textract-Type-BoundingBox-Height)": number,
               "[Left](API_BoundingBox.md#Textract-Type-BoundingBox-Left)": number,
               "[Top](API_BoundingBox.md#Textract-Type-BoundingBox-Top)": number,
               "[Width](API_BoundingBox.md#Textract-Type-BoundingBox-Width)": number
            },
            "[Polygon](API_Geometry.md#Textract-Type-Geometry-Polygon)": [ 
               { 
                  "[X](API_Point.md#Textract-Type-Point-X)": number,
                  "[Y](API_Point.md#Textract-Type-Point-Y)": number
               }
            ]
         },
         "[Id](API_Block.md#Textract-Type-Block-Id)": "string",
         "[Page](API_Block.md#Textract-Type-Block-Page)": number,
         "[Relationships](API_Block.md#Textract-Type-Block-Relationships)": [ 
            { 
               "[Ids](API_Relationship.md#Textract-Type-Relationship-Ids)": [ "string" ],
               "[Type](API_Relationship.md#Textract-Type-Relationship-Type)": "string"
            }
         ],
         "[RowIndex](API_Block.md#Textract-Type-Block-RowIndex)": number,
         "[RowSpan](API_Block.md#Textract-Type-Block-RowSpan)": number,
         "[SelectionStatus](API_Block.md#Textract-Type-Block-SelectionStatus)": "string",
         "[Text](API_Block.md#Textract-Type-Block-Text)": "string"
      }
   ],
   "[DocumentMetadata](#Textract-GetDocumentTextDetection-response-DocumentMetadata)": { 
      "[Pages](API_DocumentMetadata.md#Textract-Type-DocumentMetadata-Pages)": number
   },
   "[JobStatus](#Textract-GetDocumentTextDetection-response-JobStatus)": "string",
   "[NextToken](#Textract-GetDocumentTextDetection-response-NextToken)": "string",
   "[StatusMessage](#Textract-GetDocumentTextDetection-response-StatusMessage)": "string",
   "[Warnings](#Textract-GetDocumentTextDetection-response-Warnings)": [ 
      { 
         "[ErrorCode](API_Warning.md#Textract-Type-Warning-ErrorCode)": "string",
         "[Pages](API_Warning.md#Textract-Type-Warning-Pages)": [ number ]
      }
   ]
}
```

## Response Elements<a name="API_GetDocumentTextDetection_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Blocks](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-Blocks"></a>
The results of the text\-detection operation\.  
Type: Array of [Block](API_Block.md) objects

 ** [DocumentMetadata](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-DocumentMetadata"></a>
Information about a document that Amazon Textract processed\. `DocumentMetadata` is returned in every page of paginated responses from an Amazon Textract video operation\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [JobStatus](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-JobStatus"></a>
The current status of the text detection job\.  
Type: String  
Valid Values:` IN_PROGRESS | SUCCEEDED | FAILED | PARTIAL_SUCCESS` 

 ** [NextToken](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-NextToken"></a>
If the response is truncated, Amazon Textract returns this token\. You can use this token in the subsequent request to retrieve the next set of text\-detection results\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*` 

 ** [StatusMessage](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-StatusMessage"></a>
The current status of an asynchronous text\-detection operation for the document\.   
Type: String

 ** [Warnings](#API_GetDocumentTextDetection_ResponseSyntax) **   <a name="Textract-GetDocumentTextDetection-response-Warnings"></a>
A list of warnings that occurred during the text\-detection operation for the document\.  
Type: Array of [Warning](API_Warning.md) objects

## Errors<a name="API_GetDocumentTextDetection_Errors"></a>

 **AccessDeniedException**   
You aren't authorized to perform the action\.  
HTTP Status Code: 400

 **InternalServerError**   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 **InvalidJobIdException**   
An invalid job identifier was passed to [GetDocumentAnalysis](API_GetDocumentAnalysis.md) or to [GetDocumentTextDetection](API_GetDocumentTextDetection.md)\.  
HTTP Status Code: 400

 **InvalidParameterException**   
An input parameter violated a constraint\. For example, in synchronous operations, an `InvalidParameterException` exception occurs when neither of the `S3Object` or `Bytes` values are supplied in the `Document` request parameter\. Validate your parameter before calling the API operation again\.  
HTTP Status Code: 400

 **ProvisionedThroughputExceededException**   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Textract\.  
HTTP Status Code: 400

 **ThrottlingException**   
Amazon Textract is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

## See Also<a name="API_GetDocumentTextDetection_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/GetDocumentTextDetection) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/textract-2018-06-27/GetDocumentTextDetection) 
