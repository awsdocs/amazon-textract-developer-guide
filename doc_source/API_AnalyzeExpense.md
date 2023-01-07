# AnalyzeExpense<a name="API_AnalyzeExpense"></a>

 `AnalyzeExpense` synchronously analyzes an input document for financially related relationships between text\.

Information is returned as `ExpenseDocuments` and seperated as follows:
+  `LineItemGroups`\- A data set containing `LineItems` which store information about the lines of text, such as an item purchased and its price on a receipt\.
+  `SummaryFields`\- Contains all other information a receipt, such as header information or the vendors name\.

## Request Syntax<a name="API_AnalyzeExpense_RequestSyntax"></a>

```
{
   "Document": { 
      "Bytes": blob,
      "S3Object": { 
         "Bucket": "string",
         "Name": "string",
         "Version": "string"
      }
   }
}
```

## Request Parameters<a name="API_AnalyzeExpense_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [Document](#API_AnalyzeExpense_RequestSyntax) **   <a name="Textract-AnalyzeExpense-request-Document"></a>
The input document, either as bytes or as an S3 object\.  
You pass image bytes to an Amazon Textract API operation by using the `Bytes` property\. For example, you would use the `Bytes` property to pass a document loaded from a local file system\. Image bytes passed by using the `Bytes` property must be base64 encoded\. Your code might not need to encode document file bytes if you're using an AWS SDK to call Amazon Textract API operations\.   
You pass images stored in an S3 bucket to an Amazon Textract API operation by using the `S3Object` property\. Documents stored in an S3 bucket don't need to be base64 encoded\.  
The AWS Region for the S3 bucket that contains the S3 object must match the AWS Region that you use for Amazon Textract operations\.  
If you use the AWS CLI to call Amazon Textract operations, passing image bytes using the Bytes property isn't supported\. You must first upload the document to an Amazon S3 bucket, and then call the operation using the S3Object property\.  
For Amazon Textract to process an S3 object, the user must have permission to access the S3 object\.   
Type: [Document](API_Document.md) object  
Required: Yes

## Response Syntax<a name="API_AnalyzeExpense_ResponseSyntax"></a>

```
{
   "DocumentMetadata": { 
      "Pages": number
   },
   "ExpenseDocuments": [ 
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
         "ExpenseIndex": number,
         "LineItemGroups": [ 
            { 
               "LineItemGroupIndex": number,
               "LineItems": [ 
                  { 
                     "LineItemExpenseFields": [ 
                        { 
                           "Currency": { 
                              "Code": "string",
                              "Confidence": number
                           },
                           "GroupProperties": [ 
                              { 
                                 "Id": "string",
                                 "Types": [ "string" ]
                              }
                           ],
                           "LabelDetection": { 
                              "Confidence": number,
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
                              "Text": "string"
                           },
                           "PageNumber": number,
                           "Type": { 
                              "Confidence": number,
                              "Text": "string"
                           },
                           "ValueDetection": { 
                              "Confidence": number,
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
                              "Text": "string"
                           }
                        }
                     ]
                  }
               ]
            }
         ],
         "SummaryFields": [ 
            { 
               "Currency": { 
                  "Code": "string",
                  "Confidence": number
               },
               "GroupProperties": [ 
                  { 
                     "Id": "string",
                     "Types": [ "string" ]
                  }
               ],
               "LabelDetection": { 
                  "Confidence": number,
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
                  "Text": "string"
               },
               "PageNumber": number,
               "Type": { 
                  "Confidence": number,
                  "Text": "string"
               },
               "ValueDetection": { 
                  "Confidence": number,
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
                  "Text": "string"
               }
            }
         ]
      }
   ]
}
```

## Response Elements<a name="API_AnalyzeExpense_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [DocumentMetadata](#API_AnalyzeExpense_ResponseSyntax) **   <a name="Textract-AnalyzeExpense-response-DocumentMetadata"></a>
Information about the input document\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [ExpenseDocuments](#API_AnalyzeExpense_ResponseSyntax) **   <a name="Textract-AnalyzeExpense-response-ExpenseDocuments"></a>
The expenses detected by Amazon Textract\.  
Type: Array of [ExpenseDocument](API_ExpenseDocument.md) objects

## Errors<a name="API_AnalyzeExpense_Errors"></a>

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

## See Also<a name="API_AnalyzeExpense_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/AnalyzeExpense) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/AnalyzeExpense) 