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

These are two examples of IdentityDocumentFields cut from a longer response\. There is a seperation between the type detected and the value for that type\. Here, it is the first and last name respectively\. This structure repeats with all contained information\. If a type is not recognized as a normalized field, it will be listed as "other"\. Additionally, AnalyzeID returns a Blocks object, the same as document text detection so you can have access to the full text of the document\.

Following is a list of normalized fields for Driver's Licenses:
+ First Name — FIRST\_NAME
+ Last Name — LAST\_NAME 
+ Middle Name — MIDDLE\_NAME 
+ Suffix — SUFFIX
+ City in Address — CITY\_IN\_ADDRESS 
+ Zip Code In Address — ZIP\_CODE\_IN\_ADDRESS
+ State In Address — STATE\_IN\_ADDRESS
+ County — COUNTY
+ Document Number — DOCUMENT\_NUMBER
+ Expiration Date — EXPIRATON\_DATE
+ Date of Birth — DATE\_OF\_BIRTH
+ State Name — STATE\_NAME
+ Date of Issue — DATE\_OF\_ISSUE
+ Class — CLASS
+ Restrictions — RESTRICTIONS
+ Endorsements — ENDORSEMENTS
+ Id Type — ID\_TYPE
+ Veteran — VETERAN
+ Address — ADDRESS

  

Following is a list of normalized fields for U\.S Passports:
+ First Name — FIRST\_NAME
+ Last Name — LAST\_NAME 
+ Middle Name — MIDDLE\_NAME 
+ Document Number — DOCUMENT\_NUMBER
+ Expiration Date — EXPIRATON\_DATE
+ Date of Birth — DATE\_OF\_BIRTH
+ Place of Birth — PLACE\_OF\_BIRTH
+ Date of Issue — DATE\_OF\_ISSUE
+ Id Type — ID\_TYPE
+ MRZ Code — MRZ\_CODE