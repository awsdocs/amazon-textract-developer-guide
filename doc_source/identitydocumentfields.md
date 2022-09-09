# Identity Documentation Response Objects<a name="identitydocumentfields"></a>

 When you submit an identity document to the AnalyzeID API, it returns a series of `IdentityDocumentField` objects\. Each of these objects contains `Type`, and `Value`\. `Type` records the normalized field that Amazon Textract detects, and `Value` records the text associated with the normalized field\. 

 Below is an example of an `IdentityDocumentField`, shortened for brevity\. 

```
{
    "DocumentMetadata": {
        "Pages": 1
    }, 
    "IdentityDocumentFields": [
        {
            "Type": {
                "Text": "first name"
            }, 
            "ValueDetection": {
                "Text": "jennifer", 
                "Confidence": 99.99908447265625
            }
        }, 
        {
            "Type": {
                "Text": "last name"
            }, 
            "ValueDetection": {
                "Text": "sample", 
                "Confidence": 99.99758911132812
            }
        },
```

 These are two examples of IdentityDocumentFields cut from a longer response\. There is a seperation between the type detected and the the value for that type\. Here, it is the first and last name respectively\. This structure repeats with all contained information\. If a type is not recognized as a normalized field, it will be listed as "other"\. 

Following is a list of normalized fields for Driver's Licenses:
+  first name 
+  last name 
+  middle name 
+  suffix 
+  city in address 
+  zip code in address 
+  state in address 
+  county 
+  document number 
+  expiration date 
+  date of birth 
+  state name 
+  date of issue 
+  class 
+  restrictions 
+  endorsements 
+  id type 
+  veteran 
+  address 

Following is a list of normalized fields for U\.S Passports:
+  first name 
+  last name 
+  middle name 
+  document number 
+  expiration date 
+  date of birth 
+ place of birth
+  date of issue 
+  id type 