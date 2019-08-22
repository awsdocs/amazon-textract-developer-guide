# How Amazon Textract Works<a name="how-it-works"></a>

Amazon Textract enables you to detect and analyze text in single or multipage input documents \(see [Input Documents](how-it-works-documents.md)\)\. 

Amazon Textract provides operations for detecting text only and operations for analyzing text that find deeper relationships, such as form data and tables\. For more information, see [Detecting Text](how-it-works-detecting.md) and [Analyzing Text](how-it-works-analyzing.md)\.

Amazon Textract provides synchronous operations for processing small, single\-page, documents and for getting near real\-time responses\. For more information, see [Detecting and Analyzing Text in Single\-Page Documents](sync.md)\. Amazon Textract also provides asynchronous operations that you can use to process larger, multipage documents\. Asynchronous responses aren't in real time\. For more information, see [Detecting and Analyzing Text in Multipage Documents](async.md)\. 

When an Amazon Textract operation processes a document, the results are returned in an array of [Block](API_Block.md) objects\. A `Block` object contains information that's detected about items, including their location on the document and their relationship to other items on the document\. For more information, see [Documents and Block Objects](how-it-works-document-layout.md)\. For examples that show how to use `Block` objects, see [Examples](examples-blocks.md)\.

**Topics**
+ [Detecting Text](how-it-works-detecting.md)
+ [Analyzing Text](how-it-works-analyzing.md)
+ [Input Documents](how-it-works-documents.md)
+ [Documents and Block Objects](how-it-works-document-layout.md)
+ [Item Location on a Document Page](text-location.md)