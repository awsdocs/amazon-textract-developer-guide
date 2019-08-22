# Detecting and Analyzing Text in Single\-Page Documents<a name="sync"></a>

Amazon Textract can detect and analyze text in single\-page documents that are provided as images in JPEG or PNG format\. The operations are synchronous and return results in near real time\. For more information about documents, see [Documents and Block Objects](how-it-works-document-layout.md)\.

This section covers how you can use Amazon Textract to detect and analyze text in a single\-page document\. To detect and analyze text in multipage documents or single\-page documents that are in PDF format, see [Detecting and Analyzing Text in Multipage Documents](async.md)\.

You can use Amazon Textract synchronous operations for the following purposes:
+ Text detection – You can detect lines and words on a single\-page document image by using the [DetectDocumentText](API_DetectDocumentText.md) operation\. For more information, see [Detecting Text](how-it-works-detecting.md)\.
+ Text analysis – You can identify relationships between detected text on a single\-page document by using the [AnalyzeDocument](API_AnalyzeDocument.md) operation\. For more information, see [Analyzing Text](how-it-works-analyzing.md)\.

**Topics**
+ [Calling Amazon Textract Synchronous Operations](sync-calling.md)
+ [Detecting Document Text with Amazon Textract](detecting-document-text.md)
+ [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)