# Type<a name="how-it-works-type"></a>

Following is an example of the standard or normalized type of the key\-value pair:

```
               {
                    "PageNumber": 1, 
                    "Type": {
                        "Text": "VENDOR_NAME", 
                        "Confidence": 70.0
                    }, 
                    "ValueDetection": {
                        "Geometry": { ... }, 
                        "Text": "AMAZON", 
                        "Confidence": 87.89806365966797
                    }
                }
```

The receipt did not have “Vendor Name” explicitly listed\. However, the Analyze Expense API recognized the value "AMAZON" as Type `VENDOR_NAME`\. 