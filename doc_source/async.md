# Processing Documents with Asynchronous Operations<a name="async"></a>

Amazon Textract can detect and analyze text in multipage documents that are in PDF or TIFF format\. This includes invoices and receipts\. Multipage document processing is an asynchronous operation\. Asynchronous processing of documents is useful for processing large, multipage documents\. For example, a PDF file with over 1,000 pages takes a while to process\. Processing the PDF file asynchronously allows your application to complete other tasks while it waits for the process to complete\. 

This section covers how you can use Amazon Textract to asynchronously detect and analyze text on a multipage or single\-page document\. Multipage documents must be in PDF or TIFF format\. Single\-page documents processed with asynchronous operations can be in JPEG, PNG, TIFF or PDF format\.

You can use Amazon Textract asynchronous operations for the following purposes:
+ Text detection – You can detect lines and words on a multipage document\. The asynchronous operations are [StartDocumentTextDetection](API_StartDocumentTextDetection.md) and [GetDocumentTextDetection](API_GetDocumentTextDetection.md)\. For more information, see [Detecting Text](how-it-works-detecting.md)\.
+ Text analysis – You can identify relationships between detected text on a multipage document\. The asynchronous operations are [StartDocumentAnalysis](API_StartDocumentAnalysis.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. For more information, see [Analyzing Documents](how-it-works-analyzing.md)\.
+ Expense analysis – You can identify data relationships on multipage invoices and receipts\. Amazon Textract treats each invoice or a receipt page of a multi\-page document as an individual receipt or an invoice\. It does not retain the context from one page to another of a multi\-page document\. The asynchronous operations are [StartExpenseAnalysis](API_StartExpenseAnalysis.md) and [GetExpenseAnalysis](API_GetExpenseAnalysis.md)\. For more information, see [Analyzing Invoices and Receipts](invoices-receipts.md)\.

**Topics**
+ [Calling Amazon Textract Asynchronous Operations](api-async.md)
+ [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)
+ [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md)
+ [Amazon Textract Results Notification](async-notification-payload.md)