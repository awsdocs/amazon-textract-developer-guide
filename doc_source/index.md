# Amazon Textract Developer Guide

-----
*****Copyright &copy; 2019 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is Amazon Textract?](what-is.md)
+ [How Amazon Textract Works](how-it-works.md)
   + [Detecting Text](how-it-works-detecting.md)
   + [Analyzing Text](how-it-works-analyzing.md)
   + [Input Documents](how-it-works-documents.md)
   + [Documents and Block Objects](how-it-works-document-layout.md)
      + [Pages](how-it-works-pages.md)
      + [Lines and Words of Text](how-it-works-lines-words.md)
      + [Form Data (Key-Value Pairs)](how-it-works-kvp.md)
      + [Tables](how-it-works-tables.md)
      + [Selection Elements](how-it-works-selectables.md)
   + [Item Location on a Document Page](text-location.md)
+ [Getting Started with Amazon Textract](getting-started.md)
   + [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md)
   + [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)
   + [Step 3: Get Started Using the AWS CLI and AWS SDK API](get-started-exercise.md)
+ [Detecting and Analyzing Text in Single-Page Documents](sync.md)
   + [Calling Amazon Textract Synchronous Operations](sync-calling.md)
   + [Detecting Document Text with Amazon Textract](detecting-document-text.md)
   + [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)
+ [Detecting and Analyzing Text in Multipage Documents](async.md)
   + [Calling Amazon Textract Asynchronous Operations](api-async.md)
   + [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)
   + [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md)
   + [Amazon Textract Results Notification](async-notification-payload.md)
+ [Handling Throttled Calls and Dropped Connections](handling-errors.md)
+ [Best Practices for Amazon Textract](textract-best-practices.md)
+ [Examples](examples-blocks.md)
   + [Extracting Key-Value Pairs from a Form Document](examples-extract-kvp.md)
   + [Exporting Tables into a CSV File](examples-export-table-csv.md)
   + [Creating an AWS Lambda Function](lambda.md)
   + [Other Examples](other-examples.md)
+ [Amazon Textract Security](security.md)
   + [Authentication and Access Control for Amazon Textract](authentication-and-access-control.md)
      + [Overview of Managing Access Permissions to Your Amazon Textract Resources](access-control-overview.md)
      + [Using Identity-based Policies (IAM Policies) for Amazon Textract](using-identity-based-policies.md)
      + [Amazon Textract API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)
   + [Logging and Monitoring](textract_monitoring.md)
      + [Monitoring Amazon Textract](textract-monitoring.md)
      + [CloudWatch Metrics for Amazon Textract](cloudwatch-metricsdim.md)
   + [Logging Amazon Textract API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [API Reference](API_Reference.md)
   + [Actions](API_Operations.md)
      + [AnalyzeDocument](API_AnalyzeDocument.md)
      + [DetectDocumentText](API_DetectDocumentText.md)
      + [GetDocumentAnalysis](API_GetDocumentAnalysis.md)
      + [GetDocumentTextDetection](API_GetDocumentTextDetection.md)
      + [StartDocumentAnalysis](API_StartDocumentAnalysis.md)
      + [StartDocumentTextDetection](API_StartDocumentTextDetection.md)
   + [Data Types](API_Types.md)
      + [Block](API_Block.md)
      + [BoundingBox](API_BoundingBox.md)
      + [Document](API_Document.md)
      + [DocumentLocation](API_DocumentLocation.md)
      + [DocumentMetadata](API_DocumentMetadata.md)
      + [Geometry](API_Geometry.md)
      + [NotificationChannel](API_NotificationChannel.md)
      + [Point](API_Point.md)
      + [Relationship](API_Relationship.md)
      + [S3Object](API_S3Object.md)
      + [Warning](API_Warning.md)
+ [Limits in Amazon Textract](limits.md)
+ [Document History for Amazon Textract](document-history.md)
+ [AWS Glossary](glossary.md)