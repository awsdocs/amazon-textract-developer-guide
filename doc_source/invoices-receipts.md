# Analyzing Invoices and Receipts<a name="invoices-receipts"></a>

Amazon Textract extracts relevant data such as such as vendor and receiver contact information, from almost any invoice or receipt without the need for any templates or configuration\. Invoices and receipts often use various layouts, making it difficult and time\-consuming to manually extract data at scale\. Amazon Textract uses ML to understand the context of invoices and receipts and automatically extracts data such as invoice or receipt date, invoice or receipt number, item prices, total amount, and payment terms to suit your business needs\.

Amazon Textract also identifies vendor names that are critical for your workflows but may not be explicitly labeled\. For example, Amazon Textract can find the vendor name on a receipt even if it's only indicated within a logo at the top of the page without an explicit key\-value pair combination\. Amazon Textract also makes it easy for you to consolidate input from diverse receipts and invoices that use different words for the same concept\. For example, Amazon Textract maps relationships between field names in different documents such as bill number, invoice number, receipt number, outputting standard taxonomy as `INVOICE_RECEIPT_ID`\. In this case, Amazon Textract represents data consistently across different document types\. Fields that do not align with the standard taxonomy are categorized as `OTHER`\. 

Below is a list of standard fields supported by expense analysis operations\.

## List of Expense Analysis Standard Fields<a name="Analyze-Expense-List"></a>
+ Invoice Receipt Date — `INVOICE_RECEIPT_DATE`
+ Invoice Receipt ID — `INVOICE_RECEIPT_ID`
+ Invoice Tax Payer ID — `TAX_PAYER_ID`
+ Customer Number — `CUSTOMER_NUMBER`
+ Account Number — `ACCOUNT_NUMBER`
+ Vendor Name — `VENDOR_NAME`
+ Receiver Name — `RECEIVER_NAME`
+ Vendor Address — `VENDOR_ADDRESS`
+ Receiver Address — `RECEIVER_ADDRESS`
+ Order Date — `ORDER_DATE`
+ Due Date — `DUE_DATE`
+ Delivery Date — `DELIVERY_DATE`
+ PO Number — `PO_NUMBER`
+ Payment Terms — `PAYMENT_TERMS`
+ Total — `TOTAL`
+ Amount Due — `AMOUNT_DUE`
+ Amount Paid — `AMOUNT_PAID`
+ Subtotal — `SUBTOTAL`
+ Tax — `TAX`
+ Service Charge — `SERVICE_CHARGE`
+ Gratuity — `GRATUITY`
+ Prior Balance — `PRIOR_BALANCE`
+ Discount — `DISCOUNT`
+ Shipping and Handling Charge — `SHIPPING_HANDLING_CHARGE`
+ Vendor ABN Number — `VENDOR_ABN_NUMBER`
+ Vendor GST Number — `VENDOR_GST_NUMBER`
+ Vendor PAN Number — `VENDOR_PAN_NUMBER`
+ Vendor VAT Number — `VENDOR_VAT_NUMBER`
+ Receiver ABN Number — `RECEIVER_ABN_NUMBER`
+ Receiver GST Number — `RECEIVER_GST_NUMBER`
+ Receiver PAN Number — `RECEIVER_PAN_NUMBER`
+ Receiver VAT Number — `RECEIVER_VAT_NUMBER`
+ Vendor Phone — `VENDOR_PHONE`
+ Receiver Phone — `RECEIVER_PHONE`
+ Vendor URL — `VENDOR_URL`
+ Line Item/Item Description — `ITEM`
+ Line Item/Quantity — `QUANTITY`
+ Line Item/Total Price — `PRICE`
+ Line Item/Unit Price — `UNIT_PRICE`
+ Line Item/ProductCode — `PRODUCT_CODE`
+ Address \(Bill To, Ship To, Remit To, Supplier\) — `ADDRESS`
+ Name \(Bill To, Ship To, Remit To, Supplier\) — `NAME`
+ Core Address \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `ADDRESS_BLOCK`
+ Street Address \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `STREET_ADDRESS`
+ City \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `CITY`
+ State \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `STATE`
+ Country \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `COUNTRY`
+ ZIP Code \(Vendor, Receiver, Bill To, Ship To, Remit To, Supplier\) — `ZIP_CODE`

The AnalyzeExpense API returns the following elements for a given document page:
+ The number of receipts or invoices within a document represented as `ExpenseIndex`
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
        "Currency": {
            "Code": USD
        }
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