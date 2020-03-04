# DetectDocumentText<a name="API_DetectDocumentText"></a>

Detects text in the input document\. Amazon Textract can detect lines of text and the words that make up a line of text\. The input document must be an image in JPEG or PNG format\. `DetectDocumentText` returns the detected text in an array of [Block](API_Block.md) objects\. 

Each document page has as an associated `Block` of type PAGE\. Each PAGE `Block` object is the parent of LINE `Block` objects that represent the lines of detected text on a page\. A LINE `Block` object is a parent for each word that makes up the line\. Words are represented by `Block` objects of type WORD\.

 `DetectDocumentText` is a synchronous operation\. To analyze documents asynchronously, use [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\.

For more information, see [Document Text Detection](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-detecting.html)\.

## Request Syntax<a name="API_DetectDocumentText_RequestSyntax"></a>

```
{
   "[Document](#Textract-DetectDocumentText-request-Document)": { 
      "[Bytes](API_Document.md#Textract-Type-Document-Bytes)": blob,
      "[S3Object](API_Document.md#Textract-Type-Document-S3Object)": { 
         "[Bucket](API_S3Object.md#Textract-Type-S3Object-Bucket)": "string",
         "[Name](API_S3Object.md#Textract-Type-S3Object-Name)": "string",
         "[Version](API_S3Object.md#Textract-Type-S3Object-Version)": "string"
      }
   }
}
```

## Request Parameters<a name="API_DetectDocumentText_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [Document](#API_DetectDocumentText_RequestSyntax) **   <a name="Textract-DetectDocumentText-request-Document"></a>
The input document as base64\-encoded bytes or an Amazon S3 object\. If you use the AWS CLI to call Amazon Textract operations, you can't pass image bytes\. The document must be an image in JPEG or PNG format\.  
If you're using an AWS SDK to call Amazon Textract, you might not need to base64\-encode image bytes that are passed using the `Bytes` field\.   
Type: [Document](API_Document.md) object  
Required: Yes

## Response Syntax<a name="API_DetectDocumentText_ResponseSyntax"></a>

```
{
   "[Blocks](#Textract-DetectDocumentText-response-Blocks)": [ 
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
   "[DetectDocumentTextModelVersion](#Textract-DetectDocumentText-response-DetectDocumentTextModelVersion)": "string",
   "[DocumentMetadata](#Textract-DetectDocumentText-response-DocumentMetadata)": { 
      "[Pages](API_DocumentMetadata.md#Textract-Type-DocumentMetadata-Pages)": number
   }
}
```

## Response Elements<a name="API_DetectDocumentText_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Blocks](#API_DetectDocumentText_ResponseSyntax) **   <a name="Textract-DetectDocumentText-response-Blocks"></a>
An array of `Block` objects that contain the text that's detected in the document\.  
Type: Array of [Block](API_Block.md) objects

 ** [DetectDocumentTextModelVersion](#API_DetectDocumentText_ResponseSyntax) **   <a name="Textract-DetectDocumentText-response-DetectDocumentTextModelVersion"></a>
Type: String

 ** [DocumentMetadata](#API_DetectDocumentText_ResponseSyntax) **   <a name="Textract-DetectDocumentText-response-DocumentMetadata"></a>
Metadata about the document\. It contains the number of pages that are detected in the document\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

## Errors<a name="API_DetectDocumentText_Errors"></a>

 **AccessDeniedException**   
You aren't authorized to perform the action\.  
HTTP Status Code: 400

 **BadDocumentException**   
Amazon Textract isn't able to read the document\.  
HTTP Status Code: 400

 **DocumentTooLargeException**   
The document can't be processed because it's too large\. The maximum document size for synchronous operations 5 MB\. The maximum document size for asynchronous operations is 500 MB for PDF files\.  
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

 **ProvisionedThroughputExceededException**   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Textract\.  
HTTP Status Code: 400

 **ThrottlingException**   
Amazon Textract is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

 **UnsupportedDocumentException**   
The format of the input document isn't supported\. Documents for synchronous operations can be in PNG or JPEG format\. Documents for asynchronous operations can also be in PDF format\.  
HTTP Status Code: 400

## See Also<a name="API_DetectDocumentText_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/DetectDocumentText) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/DetectDocumentText) 