# Lines and Words of Text<a name="how-it-works-lines-words"></a>

Detected text that's returned by Amazon Textract operations is returned in a list of [Block](API_Block.md) objects\. These objects represent lines of text or textual words that are detected on a document page\. The following text shows two lines of text that are made from multiple words\.

This is text\.

In two separate lines\.

Detected text is returned in the `Text` field of a `Block` object\. The `BlockType` field determines if the text is a line of text \(LINE\) or a word \(WORD\)\. A *WORD* is one or more ISO basic Latin script characters that aren't separated by spaces\. A *LINE* is a string of tab\-delimited and contiguous words\.

 Additionally, Amazon Textract will determine if a piece of text was handwritten or printed using the `TextTypes` field\. These return as HANDWRITING and PRINTED respectively\. 

The other `Block` properties are common to all block types, such as the ID, confidence, and geometry information\. For more information, see [Text Detection and Document Analysis Response Objects](how-it-works-document-layout.md)\. 

To detect only lines and words, you can use [DetectDocumentText](API_DetectDocumentText.md) or [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\. For more information, see [Detecting Text](how-it-works-detecting.md)\. To get the detected text \(lines and words\) and information about how it relates to other parts of the document, such as tables, you can use [AnalyzeDocument](API_AnalyzeDocument.md) or [StartDocumentAnalysis](API_StartDocumentAnalysis.md)\. For more information, see [Analyzing Documents](how-it-works-analyzing.md)\.

`PAGE`, `LINE`, and `WORD` blocks are related to each other in a parent\-to\-child relationship\. A `PAGE` block is the parent for all `LINE` block objects on a document page\. Because a LINE can have one or more words, the `Relationships` array for a LINE block stores the IDs for child WORD blocks that make up the line of text\. 

The following diagram shows how the line *Hello, world\.* in the text *Hello, world\. How are you?* is represented by `Block` objects\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-text-detection.png)



The following is the JSON output from `DetectDocumentText` when the sentence *Hello, world\. How are you?* is detected\. The first example is the JSON for the document page\. Note how the CHILD IDs enable you to navigate through the document\.

```
{
    "Geometry": {...}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "d7fbd604-d609-4d69-857d-247a3f591238", // Line - Hello, world.
                "b6c19a93-6493-4d8e-958f-853c8f7ca055" //  Line - How are you?
            ]
        }
    ], 
    "BlockType": "PAGE", 
    "Id": "56ec1d77-171f-4881-9852-2b5b7e761608"
},
```

The following is the JSON for the LINE blocks that make up the line "Hello, World": 

```
{
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "7f97e2ca-063e-47a8-981c-8beee31afc01", // Word - Hello,
                "4b990aa0-af96-4369-b90f-dbe02538ed21"  // Word - world.
            ]
        }
    ], 
    "Confidence": 99.63229370117188, 
    "Geometry": {...}, 
    "Text": "Hello, world.", 
    "BlockType": "LINE", 
    "Id": "d7fbd604-d609-4d69-857d-247a3f591238"
},
```

The following is the JSON for the WORD block for the word *Hello,*: 

```
{
    "Geometry": {...}, 
    "Text": "Hello,", 
    "TextType": "PRINTED",
    "BlockType": "WORD", 
    "Confidence": 99.74746704101562, 
    "Id": "7f97e2ca-063e-47a8-981c-8beee31afc01"
},
```

The final JSON is the WORD block for the word *world\.*:

```
{
    "Geometry": {...}, 
    "Text": "world.",
    "TextType": "PRINTED",
    "BlockType": "WORD", 
    "Confidence": 99.5171127319336, 
    "Id": "4b990aa0-af96-4369-b90f-dbe02538ed21"
},
```