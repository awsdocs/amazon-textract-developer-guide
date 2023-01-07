# Analyze Lending Response Objects<a name="lending-response-objects"></a>

When you submit a document to the Analyze Lending workflow, the document is split apart into individual pages and the pages are classified\. The individual pages are then sent to the appropriate Amazon Textract operation for further analysis, depending on their classification\. Amazon Textract analyzes the data and returns the relevant information extracted from the documents, such as detected signatures, identity information, forms, expense values, and queries data\.

After processing a document with [StartLendingAnalysis](API_StartLendingAnalysis.md), you can obtain analysis results for individual pages by using [GetLendingAnalysis](API_GetLendingAnalysis.md), or you can get a summary of the information in the document with [GetLendingAnalysisSummary](API_GetLendingAnalysisSummary.md)\. The returned summary includes information about documents grouped together by a common document type\. 

 The results for the analysis of individual pages follow one general structure, regardless of the class of the document\. The response from `GetLendingAnalysis` contains information regarding the page number and page classification, along with the information extracted by one of Amazon Textract ’s analysis operations\. For the general structure of the analysis results, see the following example : 

```
{
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
         },
   "Extractions": [
      { LendingDocument | ExpenseDocument | IdentityDocument }
   ]
}
```

`GetLendingAnalysis` returns a structure that contains information on the page classification and the relevant information extracted from the given page using the appropriate operation\. The `Page` entity refers to the physical page number, `PageNumber` refers to the detected page number, and `PageClassification` is the class detected for the page\. The information extracted by an analysis operation is stored in the `Extractions` structure, which contains the normalized key\-value pairs from the appropriate operation\. 

 In the following sample response below, the document is a `LendingDocument` and contains extracted information and associated structures: 

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

 Responses from `GetLendingAnalysis` may include the following attributes: 
+ Text – The detected text\.
+ Confidence – The Confidence score for the detected text\.
+ Geometry – Location information for the detected text\.
+ LendingDocument – Holds the structured data returned by Analyze Lending for lending documents\.
+ LendingField – Holds the normalized key\-value pairs returned by Analyze Lending, including the normalized key for the detection, detected text, and geometry\.
+ LendingFields – An array of LendingField objects\.
+ Type – The normalized value associated with a detection\. For a list of all possible document types, click [here](samples/textract_AnalyzeLending_keys.zip)\.
+ ValueDetections – An array of LendingDetection objects\.
+ LendingDetection – The results extracted for a lending document\.
+ SelectionStatus – The selection status of a selection element, such as an option button or check box\.
+ KeyDetection – Object containing informatin about the detected key\.
+ SignatureDetections – An array of SignatureDetection objects, which contain information regarding detected signatures\.
+ SignatureDetection – Information regarding the confidence and geometry for the detected signatures\.

ExpenseDocument extractions contain structures defined in [Invoice and Receipt Response Objects](expensedocuments.md)\. 

IdentityDocument extractions contain structures defined in [Identity Documentation Response Objects](identitydocumentfields.md)\. 

For an example of the summary returned by the `GetLendingAnalysisSummary` operation, see the following: 

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

 The response elements returned by `GetLendingAnalysisSummary` include: 
+ LendingSummary \- Contains information regarding DocumentGroups and UndetectedDocumentTypes\.
+ DocumentGroup \- Contains information about all the documents grouped by the same document type\.
+ DocumentGroups \- Contains an array of all DocumentGroup objects\.
+ Type \- The type of the documents in a DocumentGroup\.
+ SplitDocument \- Contains information about the pages of a document, defined by logical boundary with regard to document type\. 
+ SplitDocuments \- An array of SplitDocument objects\.
+ Index \- The index for a given document in a DocumentGroup of a specific Type\.
+ Pages \- An array of page numbers for a given document, ordered by a logical boundary with regard to document type\.
+ UndetectedDocumentTypes \- An array of strings, in which each string represents an undetected document type\.

For documents that have a signature field, the following structures are included in the response:
+ DetectedSignature – Contains information about the page where a signature was found\.
+ DetectedSignatures –An array of DetectedSignature objects\.
+ Page \(within DetectedSignature and UndetectedSignature objects\) – Physical page number in the document\.
+ UndetectedSignature – Contains information about the page where a signature was expected, but was not found\. Refer this list <add link> to understand where a signature is expected\.
+ UndetectedSignatures – An array of UndetectedSignature objects\.

## Document Types<a name="lending-document-types"></a>

The following table contains a list of all document types recognized by Analyze Lending\. Also indicated is whether the document has a signature field:

## <a name="lending-document-types-list"></a>


**Document Types**  

| Type | Signature | 
| --- | --- | 
| 1003 | YES | 
| 1005 | YES | 
| 1008 | YES | 
| 1040 | YES | 
| 1065 | YES | 
| 1120 | YES | 
| 1040\_SCHEDULE\_C | NO | 
| 1040\_SCHEDULE\_D | NO | 
| 1040\_SCHEDULE\_E | NO | 
| 1099\_DIV | NO | 
| 1099\_G | NO | 
| 1099\_INT | NO | 
| 1099\_MISC | NO | 
| 1099\_NEC | NO | 
| 1099\_R | NO | 
| 1099\_SSA | NO | 
| 1120\_S | YES | 
| BANK\_STATEMENT | NO | 
| CHECKS | YES | 
| CREDIT\_CARD\_STATEMENT | NO | 
| DEMOGRAPHIC\_ADDENDUM | NO | 
| HOA\_STATEMENT | NO | 
| HUD\_92900\_B | YES | 
| IDENTITY\_DOCUMENT | NO | 
| INVESTMENT\_STATEMENT | NO | 
| INVOICES | NO | 
| MORTGAGE\_STATEMENT | NO | 
| PAYOFF\_STATEMENT | NO | 
| PAYSLIPS | NO | 
| RECEIPTS | NO | 
| SSA\_89 | YES | 
| UNCLASSIFIED | NO | 
| UTILITY\_BILLS | NO | 
| VBA\_26\_0551 | YES | 
| W\_2 | NO | 
| W\_9 | YES | 