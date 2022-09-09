# Invoice and Receipt Response Objects<a name="expensedocuments"></a>

When you submit an invoice or a receipt to the AnalyzeExpense API, it returns a series of ExpenseDocuments objects\. Each ExpenseDocument is further separated into `LineItemGroups` and `SummaryFields`\. Most invoices and receipts contain information such as the vendor name, receipt number, receipt date, or total amount\. AnalyzeExpense returns this information under `SummaryFields`\. Receipts and invoices also contain details about the items purchased\. The AnalyzeExpense API returns this information under `LineItemGroups`\. The `ExpenseIndex` field uniquely identifies the expense, and associates the appropriate `SummaryFields` and `LineItemGroups` detected in that expense\.

The most granular level of data in the AnalyzeExpense response consists of `Type`, `ValueDetection`, and `LabelDetection` \(Optional\)\. The individual entities are:
+ [Type](how-it-works-type.md): Refers to what kind of information is detected on a high level\.
+ [LabelDetection](how-it-works-labeldetection.md): Refers to the label of an associated value within the text of the document\. `LabelDetection` is optional and only returned if the label is written\.
+ [ValueDetection](how-it-works-valuedetection.md): Refers to the value of the label or type returned\.

The AnalyzeExpense API also detects `ITEM`, `QUANTITY`, and `PRICE` within line items as normalized fields\. If there is other text in a line item on the receipt image such as SKU or detailed description, it will be included in the JSON as `EXPENSE_ROW` as shown in the below example:

```
               {
                                    "Type": {
                                        "Text": "EXPENSE_ROW",
                                        "Confidence": 99.95216369628906
                                    },
                                    "ValueDetection": {
                                        "Text": "Banana 5 $2.5",
                                        "Geometry": {
                                          …
                                        },
                                        "Confidence": 98.11214447021484
                                    }
```

The example above shows how the AnalyzeExpense API returns the entire row on a receipt that contains line item information about 5 bananas sold for $2\.5\. 