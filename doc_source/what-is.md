# What Is Amazon Textract?<a name="what-is"></a>

Amazon Textract makes it easy to add document text detection and analysis to your applications\. The Amazon Textract Text Detection API can detect text in a variety of documents including financial reports, medical records, and tax forms\. For documents with structured data, you can use the Amazon Textract Document Analysis API to extract text, forms and tables\. 

Amazon Textract is based on the same proven, highly scalable, deep\-learning technology that was developed by Amazon’s computer vision scientists to analyze billions of images and videos daily\. You don't need any machine learning expertise to use it\. Amazon Textract includes simple, easy\-to\-use APIs that can analyze image files and PDF files\. Amazon Textract is always learning from new data, and we’re continually adding new features to the service\.

The following are common use cases for using Amazon Textract:
+ **Creating an intelligent search index** – Amazon Textract enables you to create libraries of text that is detected in image and PDF files\.
+ **Using intelligent text extraction for natural language processing \(NLP\)** – You can use Amazon Textract to extract text into words and lines\. It also groups text by table cells if Amazon Textract document table analysis is enabled\. Amazon Textract provides you with control over how text is grouped as an input for NLP\.
+ **Accelerating the capture and normalization of data from different sources** – Amazon Textract enables text and tabular data extraction from a wide variety of documents, such as financial documents, research reports, and medical notes\. With Amazon Textract Analyze Document APIs, you can easily and quickly extract unstructured and structured data from your documents\.
+ **Automating data capture from forms** – Amazon Textract enables structured data to be extracted from forms\. With Amazon Textract Analysis APIs, you can build extraction capabilities into existing business workflows so that user data that's submitted through forms can be extracted into a usable format\.

Some of the benefits of using Amazon Textract include:
+ **Integration of document text detection into your apps** – Amazon Textract removes the complexity of building text detection capabilities into your applications by making powerful and accurate analysis available with a simple API\. You don’t need computer vision or deep learning expertise to use Amazon Textract’s document text detection\. With Amazon Textract Text APIs, you can easily build text detection into any web, mobile, or connected device application\.
+ **Scalable document analysis** – Amazon Textract enables you to analyze and extract data quickly from millions of documents, which can accelerate decision making\.
+ **Low cost** – With Amazon Textract, you only pay for the documents you analyze\. There are no minimum fees or upfront commitments\. You can get started for free, and save more as you grow with Amazon Textract’s tiered pricing model\.

With synchronous processing, Amazon Textract can analyze single\-page documents for applications where latency is critical\. Amazon Textract also provides asynchronous operations to extend support to multipage documents\. 

An example is if you're building a mobile app to capture data from a single\-page document that might require continued user engagement\. If the uploaded image is too dark, the app might suggest photographing the image again with the camera flash turned on\. A synchronous call would be appropriate because a user is waiting on the response\. However, if you're building an application to index documents that have various numbers of pages, you might choose to send the same single\-page form to the asynchronous call so that all of your documents can be processed in the same workflow\.

By using AWS Batch, Amazon Textract is able to process multiple document images in a single operation\. AWS Batch calls the Amazon Textract synchronous operations to process the document images\. Note that PDF documents aren't supported\.

## First\-Time Amazon Textract Users<a name="first-time-user"></a>

If this is your first time using Amazon Textract, we recommend that you read the following sections in order:

1. **[How Amazon Textract Works](how-it-works.md)** – This section introduces the Amazon Textract components and how they work together for an end\-to\-end experience\. 

1. **[Getting Started with Amazon Textract](getting-started.md)** – In this section, you set your account and test the Amazon Textract API\.