# Form Data \(Key\-Value Pairs\)<a name="how-it-works-kvp"></a>

Amazon Textract can extract form data from documents as key\-value pairs\. For example, in the following text, Amazon Textract can identify a key \(*Name:*\) and a value \(*Ana Carolina*\)\.

Name: Ana Carolina

Detected key\-value pairs are returned as [Block](API_Block.md) objects in the responses from [AnalyzeDocument](API_AnalyzeDocument.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. You can use the `FeatureTypes` input parameter to retrieve information about key\-value pairs, tables, or both\. For key\-value pairs only, use the value `FORMS`\. For an example, see [Extracting Key\-Value Pairs from a Form Document](examples-extract-kvp.md)\. For general information about how a document is represented by `Block` objects, see [Documents and Block Objects](how-it-works-document-layout.md)\. 

Block objects with the type KEY\_VALUE\_SET are the containers for KEY or VALUE Block objects that store information about linked text items detected in a document\. You can use the `EntityType` attribute to determine if a block is a KEY or a VALUE\. 
+ A *KEY* object contains information about the key for linked text\. For example, *Name:*\. A KEY block has two relationship lists\. A relationship of type VALUE is a list that contains the ID of the VALUE block that's associated with the key\. A relationship of type CHILD is a list of IDs for the WORD blocks that make up the text of the key\.
+ A *VALUE* object contains information about the text that's associated with a key\. In the preceding example, *Ana Carolina* is the value for the key *Name:*\. A VALUE block has a relationship with a list of CHILD blocks that identify WORD blocks\. Each WORD block contains one of the words that make up the text of the value\. A `VALUE` object can also contain information about selected elements\. For more information, see [Selection Elements](how-it-works-selectables.md)\.

Each instance of a KEY\_VALUE\_SET `Block` object is a child of the PAGE Block object that corresponds to the current page\.

The following diagram shows how the key\-value pair *Name: Ana Carolina* is represented by `Block` objects\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-key-value-set.png)

The following examples show how the key\-value pair *Name: Ana Carolina* is represented by JSON\.

The PAGE block has CHILD blocks of type `KEY_VALUE_SET` for each KEY and VALUE block detected in the document\. 

```
{
    "Geometry": .... 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "2602b0a6-20e3-4e6e-9e46-3be57fd0844b", 
                "82aedd57-187f-43dd-9eb1-4f312ca30042", 
                "52be1777-53f7-42f6-a7cf-6d09bdc15a30", // Key - Name:
                "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"  // Value - Ana Caroline 
            ]
        }
    ], 
    "BlockType": "PAGE", 
    "Id": "8136b2dc-37c1-4300-a9da-6ed8b276ea97"  // Page identifier
},
```

The following JSON shows that the KEY block \(52be1777\-53f7\-42f6\-a7cf\-6d09bdc15a30\) has a relationship with the VALUE block \(7ca7caa6\-00ef\-4cda\-b1aa\-5571dfed1a7c\)\. It also has a CHILD block for the WORD block \(c734fca6\-c4c4\-415c\-b6c1\-30f7510b72ee\) that contains the text for the key \(*Name:*\)\.

```
{
    "Relationships": [
        {
            "Type": "VALUE", 
            "Ids": [
                "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"  // Value identifier
            ]
        }, 
        {
            "Type": "CHILD", 
            "Ids": [
                "c734fca6-c4c4-415c-b6c1-30f7510b72ee"  // Name:
            ]
        }
    ], 
    "Confidence": 51.55965805053711, 
    "Geometry": ...., 
    "BlockType": "KEY_VALUE_SET", 
    "EntityTypes": [
        "KEY"
    ], 
    "Id": "52be1777-53f7-42f6-a7cf-6d09bdc15a30"  //Key identifier
},
```

The following JSON shows that VALUE block 7ca7caa6\-00ef\-4cda\-b1aa\-5571dfed1a7c has a CHILD list of IDs for the WORD blocks that make up the text of the value \(*Ana* and *Carolina*\)\.

```
{
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "db553509-64ef-4ecf-ad3c-bea62cc1cd8a", // Ana
                "e5d7646c-eaa2-413a-95ad-f4ae19f53ef3"  // Carolina
            ]
        }
    ], 
    "Confidence": 51.55965805053711, 
    "Geometry": ...., 
    "BlockType": "KEY_VALUE_SET", 
    "EntityTypes": [
        "VALUE"
    ], 
    "Id": "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c" // Value identifier
}
```

The following JSON shows the `Block` objects for the words *Name:*, *Ana*, and *Carolina*\.

```
{
    "Geometry": {...}, 
    "Text": "Name:", 
    "BlockType": "WORD", 
    "Confidence": 99.56285858154297, 
    "Id": "c734fca6-c4c4-415c-b6c1-30f7510b72ee"
},
 {
    "Geometry": {...}, 
    "Text": "Ana", 
    "BlockType": "WORD", 
    "Confidence": 99.52057647705078, 
    "Id": "db553509-64ef-4ecf-ad3c-bea62cc1cd8a"
}, 
{
    "Geometry": {...}, 
    "Text": "Carolina", 
    "BlockType": "WORD", 
    "Confidence": 99.84207916259766, 
    "Id": "e5d7646c-eaa2-413a-95ad-f4ae19f53ef3"
},
```