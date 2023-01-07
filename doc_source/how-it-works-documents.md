# Input Documents<a name="how-it-works-documents"></a>

A suitable input for an Amazon Textract operation is a single or multipage document\. Some examples are a legal document, a form, an ID, or a letter\. A form is a document with questions or prompts for a user to provide answers\. Some examples are a patient registration form, a tax form, or an insurance claim form\. 

A document can be in JPEG, PNG, PDF or TIFF format\. With PDF and TIFF format files, you can process multipage documents\. For information about how Amazon Textract represents documents as `Block` objects, see [Text Detection and Document Analysis Response Objects](how-it-works-document-layout.md)\.

The following is an acceptable input document example\.

![\[Image of a white piece of paper with a header Employment Application. The next line says Application Information, the next Full Name: Jane Doe, the next Phone Number: 555-0100, the next Home Address: 123 Any Street, AnyTown USA, the next Mailing Address: same as above. Underneath is a table titled Previous Employment History. It has five columns and four rows. The column titles are Start Date, End Date, Employer Name, Position Held, and Reason for leaving. The next row lists 1/15/2009, 6/30/2011, Any Company, Assistant baker, and relocated. The next 7/1/2011, 8/10/2013, Example Corp. Baker, better opp. The next 8/15/2013, Present, AnyCompany, head baker, and N/A, current.\]](http://docs.aws.amazon.com/textract/latest/dg/images/Handwriting%20Sample%203.png)

For information about document limits, see [Quotas in Amazon Textract](limits.md)\.

For Amazon Textract synchronous operations, you can use input documents that are stored in an Amazon S3 bucket, or you can pass base64\-encoded image bytes\. For more information, see [Calling Amazon Textract Synchronous Operations](sync-calling.md)\. For asynchronous operations, you need to supply input documents in an Amazon S3 bucket\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\. 