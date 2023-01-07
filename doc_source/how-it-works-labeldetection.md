# LabelDetection<a name="how-it-works-labeldetection"></a>

Following is an example of text as it is shown on a customer document page:

```
               {
                    "PageNumber": 1, 
                    "Type": {
                        "Text": "OTHER", 
                        "Confidence": 70.0
                    }, 
                    "LabelDetection": {
                        "Geometry": { ... }, 
                        "Text": "CASHIER", 
                        "Confidence": 88.19171142578125
                    }, 
                    "ValueDetection": {
                        "Geometry": { ... }, 
                        "Text": "Mina", 
                        "Confidence": 87.89806365966797
                    }
                }
```

The example document contained “CASHIER Mina”\. The Analyze Expense API extracted the as\-is value and returns it under `LabelDetection`\. For implied values such as “Invoice Date”, where the “key” is not explicitly shown in the receipt, `LabelDetection` will not be included in the AnalyzeExpense element\. In such cases, the AnalyzeExpense API does not return `LabelDetection`\. 