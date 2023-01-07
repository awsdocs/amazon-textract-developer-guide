# GetLendingAnalysis<a name="API_GetLendingAnalysis"></a>

Gets the results for an Amazon Textract asynchronous operation that analyzes text in a lending document\. 

You start asynchronous text analysis by calling `StartLendingAnalysis`, which returns a job identifier \(`JobId`\)\. When the text analysis operation finishes, Amazon Textract publishes a completion status to the Amazon Simple Notification Service \(Amazon SNS\) topic that's registered in the initial call to `StartLendingAnalysis`\. 

To get the results of the text analysis operation, first check that the status value published to the Amazon SNS topic is SUCCEEDED\. If so, call GetLendingAnalysis, and pass the job identifier \(`JobId`\) from the initial call to `StartLendingAnalysis`\.

## Request Syntax<a name="API_GetLendingAnalysis_RequestSyntax"></a>

```
{
   "JobId": "string",
   "MaxResults": number,
   "NextToken": "string"
}
```

## Request Parameters<a name="API_GetLendingAnalysis_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [JobId](#API_GetLendingAnalysis_RequestSyntax) **   <a name="Textract-GetLendingAnalysis-request-JobId"></a>
A unique identifier for the lending or text\-detection job\. The `JobId` is returned from `StartLendingAnalysis`\. A `JobId` value is only valid for 7 days\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `^[a-zA-Z0-9-_]+$`   
Required: Yes

 ** [MaxResults](#API_GetLendingAnalysis_RequestSyntax) **   <a name="Textract-GetLendingAnalysis-request-MaxResults"></a>
The maximum number of results to return per paginated call\. The largest value that you can specify is 30\. If you specify a value greater than 30, a maximum of 30 results is returned\. The default value is 30\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 ** [NextToken](#API_GetLendingAnalysis_RequestSyntax) **   <a name="Textract-GetLendingAnalysis-request-NextToken"></a>
If the previous response was incomplete, Amazon Textract returns a pagination token in the response\. You can use this pagination token to retrieve the next set of lending results\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*`   
Required: No

## Response Syntax<a name="API_GetLendingAnalysis_ResponseSyntax"></a>

```
{
   "AnalyzeLendingModelVersion": "string",
   "DocumentMetadata": { 
      "Pages": number
   },
   "JobStatus": "string",
   "NextToken": "string",
   "Results": [ 
      { 
         "Extractions": [ 
            { 
               "ExpenseDocument": { 
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
               },
               "IdentityDocument": { 
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
               },
               "LendingDocument": { 
                  "LendingFields": [ 
                     { 
                        "KeyDetection": { 
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
                           "SelectionStatus": "string",
                           "Text": "string"
                        },
                        "Type": "string",
                        "ValueDetections": [ 
                           { 
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
                              "SelectionStatus": "string",
                              "Text": "string"
                           }
                        ]
                     }
                  ],
                  "SignatureDetections": [ 
                     { 
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
                        }
                     }
                  ]
               }
            }
         ],
         "Page": number,
         "PageClassification": { 
            "PageNumber": [ 
               { 
                  "Confidence": number,
                  "Value": "string"
               }
            ],
            "PageType": [ 
               { 
                  "Confidence": number,
                  "Value": "string"
               }
            ]
         }
      }
   ],
   "StatusMessage": "string",
   "Warnings": [ 
      { 
         "ErrorCode": "string",
         "Pages": [ number ]
      }
   ]
}
```

## Response Elements<a name="API_GetLendingAnalysis_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AnalyzeLendingModelVersion](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-AnalyzeLendingModelVersion"></a>
 The current model version of the Analyze Lending API\.  
Type: String

 ** [DocumentMetadata](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-DocumentMetadata"></a>
Information about the input document\.  
Type: [DocumentMetadata](API_DocumentMetadata.md) object

 ** [JobStatus](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-JobStatus"></a>
 The current status of the lending analysis job\.  
Type: String  
Valid Values:` IN_PROGRESS | SUCCEEDED | FAILED | PARTIAL_SUCCESS` 

 ** [NextToken](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-NextToken"></a>
If the response is truncated, Amazon Textract returns this token\. You can use this token in the subsequent request to retrieve the next set of lending results\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.*\S.*` 

 ** [Results](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-Results"></a>
 Holds the information returned by one of AmazonTextract's document analysis operations for the pinstripe\.  
Type: Array of [LendingResult](API_LendingResult.md) objects

 ** [StatusMessage](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-StatusMessage"></a>
 Returns if the lending analysis job could not be completed\. Contains explanation for what error occurred\.   
Type: String

 ** [Warnings](#API_GetLendingAnalysis_ResponseSyntax) **   <a name="Textract-GetLendingAnalysis-response-Warnings"></a>
 A list of warnings that occurred during the lending analysis operation\.   
Type: Array of [Warning](API_Warning.md) objects

## Errors<a name="API_GetLendingAnalysis_Errors"></a>

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

## See Also<a name="API_GetLendingAnalysis_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/textract-2018-06-27/GetLendingAnalysis) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/GetLendingAnalysis) 