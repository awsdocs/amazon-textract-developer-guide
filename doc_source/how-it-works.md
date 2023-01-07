# How Amazon Textract Works<a name="how-it-works"></a>

Amazon Textract enables you to detect and analyze text in single or multipage input documents \(see [Input Documents](how-it-works-documents.md)\)\. 

Amazon Textract provides operations for you to perform the following actions:
+ Detecting text only\. For more information see [Detecting Text](how-it-works-detecting.md)\.
+ Detecting and analyzing relationships between text\. For more information see [Analyzing Documents](how-it-works-analyzing.md)\.
+ Detecting and analyzing text in invoices and receipts\. For more information see [Analyzing Invoices and Receipts](invoices-receipts.md)\.
+ Detecting and analyzing text in government identity documents\. For more information see [Analyzing Identity Documents](how-it-works-identity.md)\.
+ Detecting and analyzing text in lending documents\. For more information see [Using Analyze Lending for Document Classification and Extraction](lending-document-classification-extraction.md)\.

Amazon Textract provides you with synchronous operations for processing single\-page documents with near real\-time responses\. For more information, see [Processing Documents with Synchronous Operations](sync.md)\. Amazon Textract also provides asynchronous operations that you can use to process larger, multipage documents\. Asynchronous responses aren't in real time\. For more information, see [Processing Documents with Asynchronous Operations](async.md)\. 

Amazon Textract provides you with a workflow to automatically classify lending document pages and route them to existing solutions\. For more information see [Using Analyze Lending for Document Classification and Extraction](lending-document-classification-extraction.md)\.

When an Amazon Textract operation processes a document, the results are returned in an array of [Block](API_Block.md) objects or an array of [ExpenseDocument](API_ExpenseDocument.md) objects\. Both objects contain information that's detected about items, including their location on the document and their relationship to other items on the document\. For more information, see [Amazon Textract Response Objects](how-it-works-document-response.md)\. For examples that show how to use `Block` objects, see [Tutorials](examples-blocks.md)\.

 For information regarding the results returned by Analyze Lending, see [Analyze Lending Response Objects ](lending-response-objects.md)\.

**Topics**
+ [Detecting Text](how-it-works-detecting.md)
+ [Analyzing Documents](how-it-works-analyzing.md)
+ [Analyzing Invoices and Receipts](invoices-receipts.md)
+ [Analyzing Identity Documents](how-it-works-identity.md)
+ [Input Documents](how-it-works-documents.md)
+ [Amazon Textract Response Objects](how-it-works-document-response.md)
+ [Item Location on a Document Page](text-location.md)
+ [Using Analyze Lending for Document Classification and Extraction](lending-document-classification-extraction.md)