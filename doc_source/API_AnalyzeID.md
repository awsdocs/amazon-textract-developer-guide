# AnalyzeID<a name="API_AnalyzeID"></a>

Analyzes identity documents for relevant information\. This information is extracted and returned as `IdentityDocumentFields`, which records both the normalized field and value of the extracted text\. Unlike other Amazon Textract operations, `AnalyzeID` doesn't return any Geometry data\.

## Request Syntax<a name="API_AnalyzeID_RequestSyntax"></a>

```
{
   "DocumentPages": [ 
      { 
         "Bytes": blob,
         "S3Object": { 
            "Bucket": "string",
            "Name": "string",
            "Version": "string"
         }
      }
   ]
}
```

## Request Parameters<a name="API_AnalyzeID_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [DocumentPages](#API_AnalyzeID_RequestSyntax) **   <a name="Textract-AnalyzeID-request-DocumentPages"></a>
The document being passed to AnalyzeID\.  
Type: Array of [Document](API_Document.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 2 items\.  
Required: Yes

## Response Syntax<a name="API_AnalyzeID_ResponseSyntax"></a>

```
{
   "AnalyzeIDModelVersion": "string",
   "DocumentMetadata": { 
      "Pages": number
   },
   "IdentityDocuments": [ 
      { 
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
         "DocumentIndex": number,
         "IdentityDocumentFields": [ 
            { 
               "Type": { 
                  "Confidence": number,
                  "NormalizedValue": { 
                     "Value": "string",
                     "ValueType": "string"
                  },
                  "Text": "string"
               },
               "ValueDetection": { 
                  "Confidence": number,
                  "NormalizedValue": { 
                     "Value": "string",
                     "ValueType": "string"
                  },
                  "Text": "string"
               }
            }
         ]
      }
   ]
}
```

## Response Elements<a name="API_AnalyzeID_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeIDModelVersion](#API_AnalyzeID_ResponseSyntax) **   <a name="Textract-AnalyzeID-response-AnalyzeIDModelVersion"></a>
The version of the AnalyzeIdentity API being used to process documents\.  
Type: String

 ** [DocumentMetadata](#API_AnalyzeID_ResponseSyntax) **   <a name="Textract-AnalyzeID-response-DocumentMetadata"></a>
Information about the input document\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [IdentityDocuments](#API_AnalyzeID_ResponseSyntax) **   <a name="Textract-AnalyzeID-response-IdentityDocuments"></a>
The list of documents processed by AnalyzeID\. Includes a number denoting their place in the list and the response structure for the document\.  
Type: Array of [IdentityDocument](API_IdentityDocument.md) objects

## Errors<a name="API_AnalyzeID_Errors"></a>

 ** AccessDeniedException **   
You aren't authorized to perform the action\. Use the Amazon Resource Name \(ARN\) of an authorized user or IAM role to perform the operation\.  
HTTP Status Code: 400

 ** BadDocumentException **   
Amazon Textract isn't able to read the document\. For more information on the document limits in Amazon Textract, see [Quotas in Amazon Textract](limits.md)\.  
HTTP Status Code: 400

 ** DocumentTooLargeException **   
The document can't be processed because it's too large\. The maximum document size for synchronous operations 10 MB\. The maximum document size for asynchronous operations is 500 MB for PDF files\.  
HTTP Status Code: 400

 ** InternalServerError **   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

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

 ** UnsupportedDocumentException **   
The format of the input document isn't supported\. Documents for operations can be in PNG, JPEG, PDF, or TIFF format\.  
HTTP Status Code: 400

## See Also<a name="API_AnalyzeID_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/AnalyzeID) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/AnalyzeID) 