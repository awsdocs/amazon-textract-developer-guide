# Input Documents<a name="how-it-works-documents"></a>

A suitable input for an Amazon Textract operation is a single or multipage document\. Some examples are a legal document, a form, or a letter\. A form is a document with questions and/or prompts for a user to provide answers\. Some examples are a patient registration form, a tax form, or an insurance claim form\. 

A document can be in JPEG, PNG or PDF format\. Synchronous operations can process JPEG and PNG format images\. Typically these are images of single\-page documents that you've scanned\. Asynchronous operations can also process documents that are in PDF format\. Using PDF format files enables you to process multipage documents\. For information about how Amazon Textract represents documents as `Block` objects, see [Documents and Block Objects](how-it-works-document-layout.md)\.

For information about document limits, see [Limits in Amazon Textract](limits.md)\.

For Amazon Textract synchronous operations, you can use input documents that are stored in an Amazon S3 bucket, or you can pass base64\-encoded image bytes\. For more information, see [Calling Amazon Textract Synchronous Operations](sync-calling.md)\. For asynchronous operations, you need to supply input documents in an Amazon S3 bucket\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\. 