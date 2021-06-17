# AnalyzeDocument<a name="API_AnalyzeDocument"></a>

Analyzes an input document for relationships between detected items\. 

The types of information returned are as follows: 
+ Form data \(key\-value pairs\)\. The related information is returned in two [Block](API_Block.md) objects, each of type `KEY_VALUE_SET`: a KEY `Block` object and a VALUE `Block` object\. For example, *Name: Ana Silva Carolina* contains a key and value\. *Name:* is the key\. *Ana Silva Carolina* is the value\.
+ Table and table cell data\. A TABLE `Block` object contains information about a detected table\. A CELL `Block` object is returned for each cell in a table\.
+ Lines and words of text\. A LINE `Block` object contains one or more WORD `Block` objects\. All lines and words that are detected in the document are returned \(including text that doesn't have a relationship with the value of `FeatureTypes`\)\. 

Selection elements such as check boxes and option buttons \(radio buttons\) can be detected in form data and in tables\. A SELECTION\_ELEMENT `Block` object contains information about a selection element, including the selection status\.

You can choose which type of analysis to perform by specifying the `FeatureTypes` list\. 

The output is returned in a list of `Block` objects\.

 `AnalyzeDocument` is a synchronous operation\. To analyze documents asynchronously, use [StartDocumentAnalysis](API_StartDocumentAnalysis.md)\.

For more information, see [Document Text Analysis](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-analyzing.html)\.

## Request Syntax<a name="API_AnalyzeDocument_RequestSyntax"></a>

```
{
   "[Document](#Textract-AnalyzeDocument-request-Document)": { 
      "[Bytes](API_Document.md#Textract-Type-Document-Bytes)": blob,
      "[S3Object](API_Document.md#Textract-Type-Document-S3Object)": { 
         "[Bucket](API_S3Object.md#Textract-Type-S3Object-Bucket)": "string",
         "[Name](API_S3Object.md#Textract-Type-S3Object-Name)": "string",
         "[Version](API_S3Object.md#Textract-Type-S3Object-Version)": "string"
      }
   },
   "[FeatureTypes](#Textract-AnalyzeDocument-request-FeatureTypes)": [ "string" ],
   "[HumanLoopConfig](#Textract-AnalyzeDocument-request-HumanLoopConfig)": { 
      "[DataAttributes](API_HumanLoopConfig.md#Textract-Type-HumanLoopConfig-DataAttributes)": { 
         "[ContentClassifiers](API_HumanLoopDataAttributes.md#Textract-Type-HumanLoopDataAttributes-ContentClassifiers)": [ "string" ]
      },
      "[FlowDefinitionArn](API_HumanLoopConfig.md#Textract-Type-HumanLoopConfig-FlowDefinitionArn)": "string",
      "[HumanLoopName](API_HumanLoopConfig.md#Textract-Type-HumanLoopConfig-HumanLoopName)": "string"
   }
}
```

## Request Parameters<a name="API_AnalyzeDocument_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [Document](#API_AnalyzeDocument_RequestSyntax) **   <a name="Textract-AnalyzeDocument-request-Document"></a>
The input document as base64\-encoded bytes or an Amazon S3 object\. If you use the AWS CLI to call Amazon Textract operations, you can't pass image bytes\. The document must be an image in JPEG or PNG format\.  
If you're using an AWS SDK to call Amazon Textract, you might not need to base64\-encode image bytes that are passed using the `Bytes` field\.   
Type: [Document](API_Document.md) object  
Required: Yes

 ** [FeatureTypes](#API_AnalyzeDocument_RequestSyntax) **   <a name="Textract-AnalyzeDocument-request-FeatureTypes"></a>
A list of the types of analysis to perform\. Add TABLES to the list to return information about the tables that are detected in the input document\. Add FORMS to return detected form data\. To perform both types of analysis, add TABLES and FORMS to `FeatureTypes`\. All lines and words detected in the document are included in the response \(including text that isn't related to the value of `FeatureTypes`\)\.   
Type: Array of strings  
Valid Values:` TABLES | FORMS`   
Required: Yes

 ** [HumanLoopConfig](#API_AnalyzeDocument_RequestSyntax) **   <a name="Textract-AnalyzeDocument-request-HumanLoopConfig"></a>
Sets the configuration for the human in the loop workflow for analyzing documents\.  
Type: [HumanLoopConfig](API_HumanLoopConfig.md) object  
Required: No

## Response Syntax<a name="API_AnalyzeDocument_ResponseSyntax"></a>

```
{
   "[AnalyzeDocumentModelVersion](#Textract-AnalyzeDocument-response-AnalyzeDocumentModelVersion)": "string",
   "[Blocks](#Textract-AnalyzeDocument-response-Blocks)": [ 
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
   "[DocumentMetadata](#Textract-AnalyzeDocument-response-DocumentMetadata)": { 
      "[Pages](API_DocumentMetadata.md#Textract-Type-DocumentMetadata-Pages)": number
   },
   "[HumanLoopActivationOutput](#Textract-AnalyzeDocument-response-HumanLoopActivationOutput)": { 
      "[HumanLoopActivationConditionsEvaluationResults](API_HumanLoopActivationOutput.md#Textract-Type-HumanLoopActivationOutput-HumanLoopActivationConditionsEvaluationResults)": "string",
      "[HumanLoopActivationReasons](API_HumanLoopActivationOutput.md#Textract-Type-HumanLoopActivationOutput-HumanLoopActivationReasons)": [ "string" ],
      "[HumanLoopArn](API_HumanLoopActivationOutput.md#Textract-Type-HumanLoopActivationOutput-HumanLoopArn)": "string"
   }
}
```

## Response Elements<a name="API_AnalyzeDocument_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeDocumentModelVersion](#API_AnalyzeDocument_ResponseSyntax) **   <a name="Textract-AnalyzeDocument-response-AnalyzeDocumentModelVersion"></a>
The version of the model used to analyze the document\.  
Type: String

 ** [Blocks](#API_AnalyzeDocument_ResponseSyntax) **   <a name="Textract-AnalyzeDocument-response-Blocks"></a>
The items that are detected and analyzed by `AnalyzeDocument`\.  
Type: Array of [Block](API_Block.md) objects

 ** [DocumentMetadata](#API_AnalyzeDocument_ResponseSyntax) **   <a name="Textract-AnalyzeDocument-response-DocumentMetadata"></a>
Metadata about the analyzed document\. An example is the number of pages\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [HumanLoopActivationOutput](#API_AnalyzeDocument_ResponseSyntax) **   <a name="Textract-AnalyzeDocument-response-HumanLoopActivationOutput"></a>
Shows the results of the human in the loop evaluation\.  
Type: [HumanLoopActivationOutput](API_HumanLoopActivationOutput.md) object

## Errors<a name="API_AnalyzeDocument_Errors"></a>

 **AccessDeniedException**   
You aren't authorized to perform the action\.  
HTTP Status Code: 400

 **BadDocumentException**   
Amazon Textract isn't able to read the document\.  
HTTP Status Code: 400

 **DocumentTooLargeException**   
The document can't be processed because it's too large\. The maximum document size for synchronous operations 5 MB\. The maximum document size for asynchronous operations is 500 MB for PDF files\.  
HTTP Status Code: 400

 **HumanLoopQuotaExceededException**   
Indicates you have exceeded the maximum number of active human in the loop workflows available  
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

## See Also<a name="API_AnalyzeDocument_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/AnalyzeDocument) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/AnalyzeDocument) 