# Using Analyze Lending for Document Classification and Extraction<a name="lending-document-classification-extraction"></a>

Analyze Lending is a document processing API for mortgage documents\. Analyze Lending allows you to automatically extract, classify, and validate information in mortgage\-related documents\. Analyze Lending receives a loan document and then splits it into pages, classifying them according to the type of document\. The document pages are then automatically routed to Amazon Textract text processing operations for accurate data extraction and analysis\.

[StartLendingAnalysis](API_StartLendingAnalysis.md) initiates the classification and analysis of a packet of lending documents\. StartLendingAnalysis operates on a document file located in an Amazon S3 bucket\.

After processing, you can retreive the results by using [GetLendingAnalysis](API_GetLendingAnalysis.md) while a summary can be retrieved with [GetLendingAnalysisSummary](API_GetLendingAnalysisSummary.md)\. Note that Analyze Lending document analysis is for asynchronous processing only\.

For a sample of the output for the GetLendingAnalysis operation, see the following\. The return includes information about the document classification type for a page, the page number, and the fields extracted by Analyze Lending: 

```
 {
    "DocumentMetadata": {
        "Pages": 1
    },
    "JobStatus": "SUCCEEDED",
    "Results": [
        {
            "Page": 1,
            "PageClassification": {
                "PageType": [
                    {
                        "Value": "1005",
                        "Confidence": 99.99947357177734
                    }
                ],
                "PageNumber": [
                    {
                        "Value": "undetected",
                        "Confidence": 100.0
                    }
                ]
            },
            "Extractions": [
                {
                    "LendingDocument": {
                        "LendingFields": [
                            {
                                "Type": "OVERTIME_CONTINUANCE_LIKELY",
                                "ValueDetections": [
                                    {
                                        "Text": "Yes",
                                        "Geometry": {
                                            "BoundingBox": {
                                                "Width": 0.019448408856987953,
                                                "Height": 0.007367494981735945,
                                                "Left": 0.8211431503295898,
                                                "Top": 0.485835462808609
                                            },
                                            "Polygon": [
                                                {
                                                    "X": 0.8211431503295898,
                                                    "Y": 0.485835462808609
                                                },
                                                {
                                                    "X": 0.8405909538269043,
                                                    "Y": 0.4858577847480774
                                                },
                                                {
                                                    "X": 0.840591549873352,
                                                    "Y": 0.49320295453071594
                                                },
                                                {
                                                    "X": 0.8211436867713928,
                                                    "Y": 0.4931805729866028
                                                }
                                            ]
                                        },
                                        "Confidence": 95.0
                                    }
                                ]
                            },
                            {
                                "Type": "CURRENT_GROSS_PAY_WEEKLY",
                                "KeyDetection": {
                                    "Text": "Weekly",
                                    "Geometry": {
                                        "BoundingBox": {
                                            "Width": 0.039741966873407364,
                                            "Height": 0.009058262221515179,
                                            "Left": 0.17564243078231812,
                                            "Top": 0.5004485845565796
                                        },
                                        "Polygon": [
                                            {
                                                "X": 0.17564436793327332,
                                                "Y": 0.5004485845565796
                                            },
                                            {
                                                "X": 0.21538439393043518,
                                                "Y": 0.5004944205284119
                                            },
                                            {
                                                "X": 0.2153826206922531,
                                                "Y": 0.5095068216323853
                                            },
                                            {
                                                "X": 0.17564243078231812,
                                                "Y": 0.5094608664512634
                                            }
                                        ]
                                    },
                                    "Confidence": 99.98104858398438
                                },
                                "ValueDetections": [
                                    {
                                        "SelectionStatus": "NOT_SELECTED",
                                        "Geometry": {
                                            "BoundingBox": {
                                                "Width": 0.010146399028599262,
                                                "Height": 0.00771764200180769,
                                                "Left": 0.1600940227508545,
                                                "Top": 0.5003445148468018
                                            },
                                            "Polygon": [
                                                {
                                                    "X": 0.16009573638439178,
                                                    "Y": 0.5003445148468018
                                                },
                                                {
                                                    "X": 0.17024043202400208,
                                                    "Y": 0.5003561973571777
                                                },
                                                {
                                                    "X": 0.17023874819278717,
                                                    "Y": 0.5080621242523193
                                                },
                                                {
                                                    "X": 0.1600940227508545,
                                                    "Y": 0.5080504417419434
                                                }
                                            ]
                                        },
                                        "Confidence": 99.88064575195312
                                    }
                                ]
                            }
                        ],
                        "SignatureDetections": [
                            {
                                "Confidence": 98.95830535888672,
                                "Geometry": {
                                    "BoundingBox": {
                                        "Width": 0.1505945473909378,
                                        "Height": 0.019163239747285843,
                                        "Left": 0.1145595833659172,
                                        "Top": 0.8886017799377441
                                    },
                                    "Polygon": [
                                        {
                                            "X": 0.11456418037414551,
                                            "Y": 0.8886017799377441
                                        },
                                        {
                                            "X": 0.2651541233062744,
                                            "Y": 0.8887989521026611
                                        },
                                        {
                                            "X": 0.2651508152484894,
                                            "Y": 0.9077650308609009
                                        },
                                        {
                                            "X": 0.1145595833659172,
                                            "Y": 0.9075667262077332
                                        }
                                    ]
                                }
                            }
                        ]
                    }
                }
            ]
        }
    ],
    "AnalyzeLendingModelVersion": "1.0"
}
```

For a sample of the output for a GetLendingAnalysisSummary operation, see the following\. The return includes information about all the documents grouped by the same document type, which are stored in DocumentGroups:

```
{
    "DocumentMetadata": {
        "Pages": 1
    },
    "JobStatus": "SUCCEEDED",
    "Summary": {
        "DocumentGroups": [
            {
                "Type": "1005",
                "SplitDocuments": [
                    {
                        "Index": 1,
                        "Pages": [
                            1
                        ]
                    }
                ],
                "DetectedSignatures": [
                    {
                        "Page": 1
                    }
                ],
                "UndetectedSignatures": []
            }
        ],
        "UndetectedDocumentTypes": [
            "1040_SCHEDULE_C",
            "1099_INT",
            "1099_SSA",
            "DEMOGRAPHIC_ADDENDUM",
            "1065",
            "1040",
            "1120_S",
            "IDENTITY_DOCUMENT",
            "SSA_89",
            "MORTGAGE_STATEMENT",
            "1099_MISC",
            "CHECKS",
            "HOA_STATEMENT",
            "INVESTMENT_STATEMENT",
            "1120",
            "1003",
            "VBA_26_0551",
            "1099_R",
            "PAYSLIPS",
            "1008",
            "W_2",
            "1099_NEC",
            "BANK_STATEMENT",
            "1040_SCHEDULE_E",
            "UTILITY_BILLS",
            "W_9",
            "UNCLASSIFIED",
            "HUD_92900_B",
            "PAYOFF_STATEMENT",
            "1099_G",
            "CREDIT_CARD_STATEMENT",
            "INVOICES",
            "RECEIPTS",
            "1040_SCHEDULE_D",
            "1099_DIV"
        ]
    },
    "AnalyzeLendingModelVersion": "1.0"
}
```

For descriptions of the response objects, see [Analyze Lending Response Objects ](lending-response-objects.md)\.

Consult the file included with the assets folder for a list of all possible recognized classes\. 