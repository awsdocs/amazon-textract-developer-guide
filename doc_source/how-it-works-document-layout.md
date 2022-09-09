# Text Detection and Document Analysis Response Objects<a name="how-it-works-document-layout"></a>

When Amazon Textract processes a document, it creates a list of [Block](API_Block.md) objects for the detected or analyzed text\. Each block contains information about a detected item, where it's located, and the confidence that Amazon Textract has in the accuracy of the processing\.

A document is made up from the following types of `Block` objects\.
+ [Pages](how-it-works-pages.md)
+  [Lines and words of text](how-it-works-lines-words.md) 
+  [Form Data \(Key\-value pairs\)](how-it-works-kvp.md) 
+  [Tables and Cells](how-it-works-tables.md) 
+ [Selection elements](how-it-works-selectables.md)
+ [Queries](queryresponse.md)

The contents of a block depend on the operation you call\. If you call one of the text detection operations, the pages, lines, and words of detected text are returned\. For more information, see [Detecting Text](how-it-works-detecting.md)\. If you call one of the document analysis operations, information about detected pages, key\-value pairs, tables, selection elements, and text is returned\. For more information, see [Analyzing Documents](how-it-works-analyzing.md)\.

Some `Block` object fields are common to both types of processing\. For example, each block has a unique identifier\.

For examples that show how to use `Block` objects, see [Tutorials](examples-blocks.md)\.

## Document Layout<a name="hows-it-works-blocks-types.title"></a>

Amazon Textract returns a representation of a document as a list of different types of `Block` objects that are linked in a parent\-to\-child relationship or a key\-value pair\. Metadata that provides the number of pages in a document is also returned\. The following is the JSON for a typical `Block` object of type `PAGE`\.

```
{
    "Blocks": [
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 1.0, 
                    "Top": 0.0, 
                    "Left": 0.0, 
                    "Height": 1.0
                }, 
                "Polygon": [
                    {
                        "Y": 0.0, 
                        "X": 0.0
                    }, 
                    {
                        "Y": 0.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 0.0
                    }
                ]
            }, 
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "2602b0a6-20e3-4e6e-9e46-3be57fd0844b", 
                        "82aedd57-187f-43dd-9eb1-4f312ca30042", 
                        "52be1777-53f7-42f6-a7cf-6d09bdc15a30", 
                        "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"
                    ]
                }
            ], 
            "BlockType": "PAGE", 
            "Id": "8136b2dc-37c1-4300-a9da-6ed8b276ea97"
        }..... 
        
    ], 
    "DocumentMetadata": {
        "Pages": 1
    }
}
```

A document is made from one or more `PAGE` blocks\. Each page contains a list of child blocks for the primary items detected on the page, such as lines of text and tables\. For more information, see [Pages](how-it-works-pages.md)\. 

You can determine the type of a `Block` object by inspecting the `BlockType` field\.

A `Block` object contains a list of related `Block` objects in the `Relationships` field, which is an array of [Relationship](API_Relationship.md) objects\. A `Relationships` array is either of type CHILD or of type VALUE\. An array of type CHILD is used to list the items that are children of the current block\. For example, if the current block is of type LINE, `Relationships` contains a list of IDs for the WORD blocks that make up the line of text\. An array of type VALUE is used to contain key\-value pairs\. You can determine the type of the relationship by inspecting the `Type` field of the `Relationship` object\. 

Child blocks don't have information about their parent Block objects\.

For examples that show `Block` information, see [Processing Documents with Synchronous Operations](sync.md)\.

## Confidence<a name="how-it-works-confidence"></a>

Amazon Textract operations return the percentage confidence that Amazon Textract has in the accuracy of the detected item\. To get the confidence, use the `Confidence` field of the `Block` object\. A higher value indicates a higher confidence\. Depending on the scenario, detections with a low confidence might need visual confirmation by a human\.

## Geometry<a name="how-it-works-geometry"></a>

Amazon Textract operations, with the exception of identity analysis, return location information about the location of detected items on a document page\. To get the location, use the `Geometry` field of the `Block` object\. For more information, see [Item Location on a Document Page](text-location.md) 