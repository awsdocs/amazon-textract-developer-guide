# ValueDetection<a name="how-it-works-valuedetection"></a>

The following is an example that shows the “value” of the key\-value pair\.

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

In the example, the document contained “CASHIER Mina”\. The AnalyzeExpense API detected the Cashier value as Mina and returned it under `ValueDetection`\. 