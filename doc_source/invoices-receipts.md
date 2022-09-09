# Analyzing Invoices and Receipts<a name="invoices-receipts"></a>

Amazon Textract extracts relevant data such as contact information, items purchased, and vendor name, from almost any invoice or receipt without the need for any templates or configuration\. Invoices and receipts often use various layouts, making it difficult and time\-consuming to manually extract data at scale\. Amazon Textract uses ML to understand the context of invoices and receipts and automatically extracts data such as invoice or receipt date, invoice or receipt number, item prices, total amount, and payment terms to suit your business needs\.

Amazon Textract also identifies vendor names that are critical for your workflows but may not be explicitly labeled\. For example, Amazon Textract can find the vendor name on a receipt even if it's only indicated within a logo at the top of the page without an explicit key\-value pair combination\. Amazon Textract also makes it easy for you to consolidate input from diverse receipts and invoices that use different words for the same concept\. For example, Amazon Textract maps relationships between field names in different documents such as customer no\., customer number, and account ID, outputting standard taxonomy as `INVOICE_RECEIPT_ID`\. In this case, Amazon Textract represents data consistently across different document types\. Fields that do not align with the standard taxonomy are categorized as `OTHER`\. 

The following is a list of the standard fields that AnalyzeExpense currently supports:
+ Vendor Name: `VENDOR_NAME`
+ Total: `TOTAL`
+ Receiver Address: `RECEIVER_ADDRESS`
+ Invoice/Receipt Date: `INVOICE_RECEIPT_DATE`
+ Invoice/Receipt ID: `INVOICE_RECEIPT_ID`
+ Payment Terms: `PAYMENT_TERMS`
+ Subtotal: `SUBTOTAL`
+ Due Date: `DUE_DATE`
+ Tax: `TAX`
+ Invoice Tax Payer ID \(SSN/ITIN or EIN\): `TAX_PAYER_ID`
+ Item Name: `ITEM_NAME`
+ Item Price: `PRICE`
+ Item Quantity: `QUANTITY`

The AnalyzeExpense API returns the following elements for a given document page:
+ The number of receipts or invoices within a page represented as `ExpenseIndex`
+ The standardized name for individual fields represented as `Type`
+ The actual name of the field as it appears on the document, represented as `LabelDetection`
+ The value of the corresponding field represented as `ValueDetection`
+ The number of pages within the submitted document represented as `Pages`
+ The page number on which the field, value, or line items was detected, represented as `PageNumber`
+ The geometry, which includes the bounding box and coordinates location of the individual field, value, or line items on the page, represented as `Geometry`
+ The confidence score associated with each piece of data detected on the document, represented as `Confidence`
+ The entire row of individual line items purchased, represented as `EXPENSE_ROW`

The following is a portion of the API output for a receipt processed by AnalyzeExpense that shows the Total: $55\.64 in the document extracted as standard field `TOTAL`, actual text on the document as “Total”, Confidence Score of “97\.1”, Page Number “1”, The total value as “$55\.64” and the bounding box and polygon coordinates: 

```
{
    "Type": {
        "Text": "TOTAL",
        "Confidence": 99.94717407226562
    },
    "LabelDetection": {
        "Text": "Total:",
        "Geometry": {
            "BoundingBox": {
                "Width": 0.09809663146734238,
                "Height": 0.0234375,
                "Left": 0.36822840571403503,
                "Top": 0.8017578125
            },
            "Polygon": [
                {
                    "X": 0.36822840571403503,
                    "Y": 0.8017578125
                },
                {
                    "X": 0.466325044631958,
                    "Y": 0.8017578125
                },
                {
                    "X": 0.466325044631958,
                    "Y": 0.8251953125
                },
                {
                    "X": 0.36822840571403503,
                    "Y": 0.8251953125
                }
        ]
    },
    "Confidence": 97.10792541503906
},
    "ValueDetection": {
        "Text": "$55.64",
        "Geometry": {
            "BoundingBox": {
                "Width": 0.10395314544439316,
                "Height": 0.0244140625,
                "Left": 0.66837477684021,
                "Top": 0.802734375
            },
            "Polygon": [
                {
                    "X": 0.66837477684021,
                    "Y": 0.802734375
                },
                {
                    "X": 0.7723279595375061,
                    "Y": 0.802734375
                },
                {
                    "X": 0.7723279595375061,
                    "Y": 0.8271484375
                },
                {
                    "X": 0.66837477684021,
                    "Y": 0.8271484375
                }
            ]
        },
    "Confidence": 99.85165405273438
},
"PageNumber": 1
}
```

You can use synchronous operations to analyze an invoice or receipt\. To analyze these documents, you use the AnalyzeExpense operation and pass a receipt or invoice to it\. `AnalyzeExpense` returns the entire set of results\. For more information, see [Analyzing Invoices and Receipts with Amazon Textract](analyzing-document-expense.md)\.

To analyze invoices and receipts asynchronously, use [StartExpenseAnalysis](API_StartExpenseAnalysis.md) to start processing an input document file\. To get the results, call [GetExpenseAnalysis](API_GetExpenseAnalysis.md)\. The results for a given call to [StartExpenseAnalysis](API_StartExpenseAnalysis.md) are returned by `GetExpenseAnalysis`\. For more information and an example, see [Processing Documents with Asynchronous Operations](async.md)\. 