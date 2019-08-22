# Analyzing Text<a name="how-it-works-analyzing"></a>

Amazon Textract analyzes documents and forms for relationships between detected text\. Amazon Textract analysis operations return 3 categories of text extraction — text, forms, and tables\.

**Text Extraction**  
The raw text extracted from a document\. For more information, see [Lines and words of text](how-it-works-lines-words.md)\.

**Form Extraction**  
Form data is linked text items extracted from a document\. Amazon Textract represents form data as key\-value pairs\. In the following example, one of the lines of text detected by Amazon Textract is *Name: Jane Doe*\. Amazon Textract also identifies a key \(*Name:*\) and a value \(*Jane Doe*\)\. For more information, see [Form data \(Key\-value pairs\)](how-it-works-kvp.md)\.

*Name: Jane Doe*

*Address: 123 Any Street, Anytown, USA*

*Birth date: 12\-26\-1980*

Key\-value pairs are also used to represent check boxes or option buttons \(radio buttons\) that are extracted from forms\.

*Male:* ☑

For more information, see [Selection elements](how-it-works-selectables.md)\.

**Table Extraction**  
Amazon Textract can extract tables, table cells, and the items within table cells\.


| Name | Address | 
| --- | --- | 
|  Ana Carolina  |  123 Any Town  | 

For more information, see [Tables](how-it-works-tables.md)\. Selection elements can also be extracted from tables\. For more information, see [Selection elements](how-it-works-selectables.md)\.

For analyzed items, Amazon Textract returns the following in multiple [Block](API_Block.md) objects:
+ The lines and words of detected text
+ The content of detected items
+ The relationship between detected items
+ The page that the item was detected on
+ The location of the item on the document page

You can use synchronous or asynchronous operations to analyze text in a document\. To analyze text synchronously, use the [AnalyzeDocument](API_AnalyzeDocument.md) operation, and pass a document as input\. `AnalyzeDocument` returns the entire set of results\. For more information, see [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)\. 

To detect text asynchronously, use [StartDocumentAnalysis](API_StartDocumentAnalysis.md) to start processing\. To get the results, call [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. The results are returned in one or more responses from `GetDocumentAnalysis`\. For more information and an example, see [Detecting and Analyzing Text in Multipage Documents](async.md)\. 

To specify which type of analysis to perform, you can use the `FeatureTypes` list input parameter\. Add TABLES to the list to return information about the tables that are detected in the input document—for example, table cells, cell text, and selection elements in cells\. Add FORMS to return word relationships, such as key\-value pairs and selection elements\. To perform both types of analysis, add both TABLES and FORMS to `FeatureTypes`\. 

All lines and words that are detected in the document are included in the response \(including text that's not related to the value of `FeatureTypes`\)\.