# Pages<a name="how-it-works-pages"></a>

A document consists of one or more pages\. A [Block](API_Block.md) object of type `PAGE` exists for each page of the document\. A `PAGE` block object contains a list of the child IDs for the lines of text, key\-value pairs, and tables that are detected on the document page\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-pages.png)

The JSON for a `PAGE` block looks similar to the following\.

```
{
    "Geometry": .... 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "2602b0a6-20e3-4e6e-9e46-3be57fd0844b", // Line - Hello, world.
                "82aedd57-187f-43dd-9eb1-4f312ca30042", // Line - How are you?
                "52be1777-53f7-42f6-a7cf-6d09bdc15a30", 
                "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"   
            ]
        }
    ], 
    "BlockType": "PAGE", 
    "Id": "8136b2dc-37c1-4300-a9da-6ed8b276ea97"  // Page identifier
},
```

If you're using asynchronous operations with a multipage document that's in PDF format, you can determine the page that a block is located on by inspecting the `Page` field of the `Block` object\. A scanned image \(an image in JPEG or PNG format\) is considered to be a single\-page document, even if there's more than one document page on the image\. Asynchronous operations always return a `Page` value of 1 for scanned images\.

The total number of pages is returned in the `Pages` field of `DocumentMetadata`\. `DocumentMetadata` is returned with each list of `Block` objects returned by an Amazon Textract operation\.