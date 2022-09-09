# Block<a name="API_Block"></a>

A `Block` represents items that are recognized in a document within a group of pixels close to each other\. The information returned in a `Block` object depends on the type of operation\. In text detection for documents \(for example [DetectDocumentText](API_DetectDocumentText.md)\), you get information about the detected words and lines of text\. In text analysis \(for example [AnalyzeDocument](API_AnalyzeDocument.md)\), you can also get information about the fields, tables, and selection elements that are detected in the document\.

An array of `Block` objects is returned by both synchronous and asynchronous operations\. In synchronous operations, such as [DetectDocumentText](API_DetectDocumentText.md), the array of `Block` objects is the entire set of results\. In asynchronous operations, such as [GetDocumentAnalysis](API_GetDocumentAnalysis.md), the array is returned over one or more responses\.

For more information, see [How Amazon Textract Works](https://docs.aws.amazon.com/textract/latest/dg/how-it-works.html)\.

## Contents<a name="API_Block_Contents"></a>

 ** BlockType **   <a name="Textract-Type-Block-BlockType"></a>
The type of text item that's recognized\. In operations for text detection, the following types are returned:  
+  *PAGE* \- Contains a list of the LINE `Block` objects that are detected on a document page\.
+  *WORD* \- A word detected on a document page\. A word is one or more ISO basic Latin script characters that aren't separated by spaces\.
+  *LINE* \- A string of tab\-delimited, contiguous words that are detected on a document page\.
In text analysis operations, the following types are returned:  
+  *PAGE* \- Contains a list of child `Block` objects that are detected on a document page\.
+  *KEY\_VALUE\_SET* \- Stores the KEY and VALUE `Block` objects for linked text that's detected on a document page\. Use the `EntityType` field to determine if a KEY\_VALUE\_SET object is a KEY `Block` object or a VALUE `Block` object\. 
+  *WORD* \- A word that's detected on a document page\. A word is one or more ISO basic Latin script characters that aren't separated by spaces\.
+  *LINE* \- A string of tab\-delimited, contiguous words that are detected on a document page\.
+  *TABLE* \- A table that's detected on a document page\. A table is grid\-based information with two or more rows or columns, with a cell span of one row and one column each\. 
+  *CELL* \- A cell within a detected table\. The cell is the parent of the block that contains the text in the cell\.
+  *SELECTION\_ELEMENT* \- A selection element such as an option button \(radio button\) or a check box that's detected on a document page\. Use the value of `SelectionStatus` to determine the status of the selection element\.
+  *QUERY* \- A question asked during the call of AnalyzeDocument\. Contains an alias and an ID that attaches it to its answer\.
+  *QUERY\_RESULT* \- A response to a question asked during the call of analyze document\. Comes with an alias and ID for ease of locating in a response\. Also contains location and confidence score\.
Type: String  
Valid Values:` KEY_VALUE_SET | PAGE | LINE | WORD | TABLE | CELL | SELECTION_ELEMENT | MERGED_CELL | TITLE | QUERY | QUERY_RESULT`   
Required: No

 ** ColumnIndex **   <a name="Textract-Type-Block-ColumnIndex"></a>
The column in which a table cell appears\. The first column position is 1\. `ColumnIndex` isn't returned by `DetectDocumentText` and `GetDocumentTextDetection`\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** ColumnSpan **   <a name="Textract-Type-Block-ColumnSpan"></a>
The number of columns that a table cell spans\. Currently this value is always 1, even if the number of columns spanned is greater than 1\. `ColumnSpan` isn't returned by `DetectDocumentText` and `GetDocumentTextDetection`\.   
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** Confidence **   <a name="Textract-Type-Block-Confidence"></a>
The confidence score that Amazon Textract has in the accuracy of the recognized text and the accuracy of the geometry points around the recognized text\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** EntityTypes **   <a name="Textract-Type-Block-EntityTypes"></a>
The type of entity\. The following can be returned:  
+  *KEY* \- An identifier for a field on the document\.
+  *VALUE* \- The field text\.
 `EntityTypes` isn't returned by `DetectDocumentText` and `GetDocumentTextDetection`\.  
Type: Array of strings  
Valid Values:` KEY | VALUE | COLUMN_HEADER`   
Required: No

 ** Geometry **   <a name="Textract-Type-Block-Geometry"></a>
The location of the recognized text on the image\. It includes an axis\-aligned, coarse bounding box that surrounds the text, and a finer\-grain polygon for more accurate spatial information\.   
Type: [Geometry](API_Geometry.md) object  
Required: No

 ** Id **   <a name="Textract-Type-Block-Id"></a>
The identifier for the recognized text\. The identifier is only unique for a single operation\.   
Type: String  
Pattern: `.*\S.*`   
Required: No

 ** Page **   <a name="Textract-Type-Block-Page"></a>
The page on which a block was detected\. `Page` is returned by synchronous and asynchronous operations\. Page values greater than 1 are only returned for multipage documents that are in PDF or TIFF format\. A scanned image \(JPEG/PNG\) provided to an asynchronous operation, even if it contains multiple document pages, is considered a single\-page document\. This means that for scanned images the value of `Page` is always 1\. Synchronous operations operations will also return a `Page` value of 1 because every input document is considered to be a single\-page document\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** Query **   <a name="Textract-Type-Block-Query"></a>
  
Type: [Query](API_Query.md) object  
Required: No

 ** Relationships **   <a name="Textract-Type-Block-Relationships"></a>
A list of child blocks of the current block\. For example, a LINE object has child blocks for each WORD block that's part of the line of text\. There aren't Relationship objects in the list for relationships that don't exist, such as when the current block has no child blocks\. The list size can be the following:  
+ 0 \- The block has no child blocks\.
+ 1 \- The block has child blocks\.
Type: Array of [Relationship](API_Relationship.md) objects  
Required: No

 ** RowIndex **   <a name="Textract-Type-Block-RowIndex"></a>
The row in which a table cell is located\. The first row position is 1\. `RowIndex` isn't returned by `DetectDocumentText` and `GetDocumentTextDetection`\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** RowSpan **   <a name="Textract-Type-Block-RowSpan"></a>
The number of rows that a table cell spans\. Currently this value is always 1, even if the number of rows spanned is greater than 1\. `RowSpan` isn't returned by `DetectDocumentText` and `GetDocumentTextDetection`\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** SelectionStatus **   <a name="Textract-Type-Block-SelectionStatus"></a>
The selection status of a selection element, such as an option button or check box\.   
Type: String  
Valid Values:` SELECTED | NOT_SELECTED`   
Required: No

 ** Text **   <a name="Textract-Type-Block-Text"></a>
The word or line of text that's recognized by Amazon Textract\.   
Type: String  
Required: No

 ** TextType **   <a name="Textract-Type-Block-TextType"></a>
The kind of text that Amazon Textract has detected\. Can check for handwritten text and printed text\.  
Type: String  
Valid Values:` HANDWRITING | PRINTED`   
Required: No

## See Also<a name="API_Block_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Block) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Block) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Block) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Block) 