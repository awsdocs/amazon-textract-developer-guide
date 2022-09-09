# How Amazon Textract Works<a name="how-it-works"></a>

Amazon Textract enables you to detect and analyze text in single or multipage input documents \(see [Input Documents](how-it-works-documents.md)\)\. 

Amazon Textract provides operations for the following actions\.
+ Detecting text only\. For more information see [Detecting Text](how-it-works-detecting.md)\.
+ Detecting and analyzing relationships between text\. For more information see [Analyzing Documents](how-it-works-analyzing.md)\.
+ Detecting and analyzing text in invoices and receipts\. For more information see [Analyzing Invoices and Receipts](invoices-receipts.md)\.
+ Detecting and analyzing text in government identity documents\. For more information see [Analyzing Identity Documents](how-it-works-identity.md)\.

Amazon Textract provides synchronous operations for processing small, single\-page, documents and with near real\-time responses\. For more information, see [Processing Documents with Synchronous Operations](sync.md)\. Amazon Textract also provides asynchronous operations that you can use to process larger, multipage documents\. Asynchronous responses aren't in real time\. For more information, see [Processing Documents with Asynchronous Operations](async.md)\. 

When an Amazon Textract operation processes a document, the results are returned in an array of [Block](API_Block.md) objects or an array of [ExpenseDocument](API_ExpenseDocument.md) objects\. Both objects contain information that's detected about items, including their location on the document and their relationship to other items on the document\. For more information, see [Amazon Textract Response Objects](how-it-works-document-response.md)\. For examples that show how to use `Block` objects, see [Tutorials](examples-blocks.md)\.

**Topics**
+ [Detecting Text](how-it-works-detecting.md)
+ [Analyzing Documents](how-it-works-analyzing.md)
+ [Analyzing Invoices and Receipts](invoices-receipts.md)
+ [Analyzing Identity Documents](how-it-works-identity.md)
+ [Input Documents](how-it-works-documents.md)
+ [Amazon Textract Response Objects](how-it-works-document-response.md)
+ [Item Location on a Document Page](text-location.md)