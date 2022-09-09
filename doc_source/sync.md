# Processing Documents with Synchronous Operations<a name="sync"></a>

Amazon Textract can detect and analyze text in single\-page documents that are provided as images in JPEG, PNG, PDF, and TIFF format\. The operations are synchronous and return results in near real time\. For more information about documents, see [Text Detection and Document Analysis Response Objects](how-it-works-document-layout.md)\.

This section covers how you can use Amazon Textract to detect and analyze text in a single\-page document synchronously\. To detect and analyze text in multipage documents, or to detect JPEG and PNG documents asynchronously, see [Processing Documents with Asynchronous Operations](async.md)\.

You can use Amazon Textract synchronous operations for the following purposes:
+ Text detection – You can detect lines and words on a single\-page document image by using the [DetectDocumentText](API_DetectDocumentText.md) operation\. For more information, see [Detecting Text](how-it-works-detecting.md)\.
+ Text analysis – You can identify relationships between detected text on a single\-page document by using the [AnalyzeDocument](API_AnalyzeDocument.md) operation\. For more information, see [Analyzing Documents](how-it-works-analyzing.md)\.
+ Invoice and Receipt Analysis – You can identify financial relationships between detected text on a single\-page invoice or receipt using the AnalyzeExpense operation\. For more information, see [Analyzing Invoices and Receipts](invoices-receipts.md)
+ Identity Document Analysis – You can analyze identity documents issued by the U\.S\. Government and extract information along with common types of information found on identity documents\. For more information see [Analyzing Identity Documents](how-it-works-identity.md)\. 

**Topics**
+ [Calling Amazon Textract Synchronous Operations](sync-calling.md)
+ [Detecting Document Text with Amazon Textract](detecting-document-text.md)
+ [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)
+ [Analyzing Invoices and Receipts with Amazon Textract](analyzing-document-expense.md)
+ [Analyzing Identity Documentation with Amazon Textract](analyzing-document-identity.md)