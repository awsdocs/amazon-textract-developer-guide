# GetExpenseAnalysis<a name="API_GetExpenseAnalysis"></a>

Gets the results for an Amazon Textract asynchronous operation that analyzes invoices and receipts\. Amazon Textract finds contact information, items purchased, and vendor name, from input invoices and receipts\.

You start asynchronous invoice/receipt analysis by calling [StartExpenseAnalysis](API_StartExpenseAnalysis.md), which returns a job identifier \(`JobId`\)\. Upon completion of the invoice/receipt analysis, Amazon Textract publishes the completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic\. This topic must be registered in the initial call to `StartExpenseAnalysis`\. To get the results of the invoice/receipt analysis operation, first ensure that the status value published to the Amazon SNS topic is `SUCCEEDED`\. If so, call `GetExpenseAnalysis`, and pass the job identifier \(`JobId`\) from the initial call to `StartExpenseAnalysis`\.

Use the MaxResults parameter to limit the number of blocks that are returned\. If there are more results than specified in `MaxResults`, the value of `NextToken` in the operation response contains a pagination token for getting the next set of results\. To get the next page of results, call `GetExpenseAnalysis`, and populate the `NextToken` request parameter with the token value that's returned from the previous call to `GetExpenseAnalysis`\.

For more information, see [Analyzing Invoices and Receipts](https://docs.aws.amazon.com/textract/latest/dg/invoices-receipts.html)\.

## Request Syntax<a name="API_GetExpenseAnalysis_RequestSyntax"></a>

```
{
   "JobId": "string",
   "MaxResults": number,
   "NextToken": "string"
}
```

## Request Parameters<a name="API_GetExpenseAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [JobId](#API_GetExpenseAnalysis_RequestSyntax) **   <a name="Textract-GetExpenseAnalysis-request-JobId"></a>
A unique identifier for the text detection job\. The `JobId` is returned from `StartExpenseAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: Yes

 ** [MaxResults](#API_GetExpenseAnalysis_RequestSyntax) **   <a name="Textract-GetExpenseAnalysis-request-MaxResults"></a>
The maximum number of results to return per paginated call\. The largest value you can specify is 20\. If you specify a value greater than 20, a maximum of 20 results is returned\. The default value is 20\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 ** [NextToken](#API_GetExpenseAnalysis_RequestSyntax) **   <a name="Textract-GetExpenseAnalysis-request-NextToken"></a>
If the previous response was incomplete \(because there are more blocks to retrieve\), Amazon Textract returns a pagination token in the response\. You can use this pagination token to retrieve the next set of blocks\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*`   
Required: No

## Response Syntax<a name="API_GetExpenseAnalysis_ResponseSyntax"></a>

```
{
   "AnalyzeExpenseModelVersion": "string",
   "DocumentMetadata": { 
      "Pages": number
   },
   "ExpenseDocuments": [ 
      { 
         "ExpenseIndex": number,
         "LineItemGroups": [ 
            { 
               "LineItemGroupIndex": number,
               "LineItems": [ 
                  { 
                     "LineItemExpenseFields": [ 
                        { 
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
   ],
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

## Response Elements<a name="API_GetExpenseAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeExpenseModelVersion](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-AnalyzeExpenseModelVersion"></a>
The current model version of AnalyzeExpense\.  
Type: String

 ** [DocumentMetadata](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-DocumentMetadata"></a>
Information about a document that Amazon Textract processed\. `DocumentMetadata` is returned in every page of paginated responses from an Amazon Textract operation\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [ExpenseDocuments](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-ExpenseDocuments"></a>
The expenses detected by Amazon Textract\.  
Type: Array of [ExpenseDocument](API_ExpenseDocument.md) objects

 ** [JobStatus](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-JobStatus"></a>
The current status of the text detection job\.  
Type: String  
Valid Values:` IN_PROGRESS | SUCCEEDED | FAILED | PARTIAL_SUCCESS` 

 ** [NextToken](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-NextToken"></a>
If the response is truncated, Amazon Textract returns this token\. You can use this token in the subsequent request to retrieve the next set of text\-detection results\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*` 

 ** [StatusMessage](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-StatusMessage"></a>
Returns if the detection job could not be completed\. Contains explanation for what error occured\.   
Type: String

 ** [Warnings](#API_GetExpenseAnalysis_ResponseSyntax) **   <a name="Textract-GetExpenseAnalysis-response-Warnings"></a>
A list of warnings that occurred during the text\-detection operation for the document\.  
Type: Array of [Warning](API_Warning.md) objects

## Errors<a name="API_GetExpenseAnalysis_Errors"></a>

 ** AccessDeniedException **   
You aren't authorized to perform the action\. Use the Amazon Resource Name \(ARN\) of an authorized user or IAM role to perform the operation\.  
HTTP Status Code: 400

 ** InternalServerError **   
Amazon Textract experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 ** InvalidJobIdException **   
An invalid job identifier was passed to [GetDocumentAnalysis](API_GetDocumentAnalysis.md) or to [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\.  
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

## See Also<a name="API_GetExpenseAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/GetExpenseAnalysis) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/GetExpenseAnalysis) 