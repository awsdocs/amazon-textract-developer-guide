# Tutorials<a name="examples-blocks"></a>

[Block](API_Block.md) objects that are returned from Amazon Textract operations contain the results of text detection and text analysis operations, such as [AnalyzeDocument](API_AnalyzeDocument.md)\. The following Python tutorials show some of the different ways that you can use Block objects\. For example, you can export table information to a comma\-separated values \(CSV\) file\.

The tutorials use synchronous Amazon Textract operations that return all results\. If you want to use asynchronous operations such as [StartDocumentAnalysis](API_StartDocumentAnalysis.md), you need to change the example code to accommodate multiple batches of returned `Block` objects\. To make use of the asynchronous operations example, ensure that you have followed the instructions given at [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)\.

For examples that show you other ways to use Amazon Textract, see [Additional Code Samples](other-examples.md)\.

**Topics**
+ [Prerequisites](#examples-prerequisites)
+ [Extracting Key\-Value Pairs from a Form Document](examples-extract-kvp.md)
+ [Exporting Tables into a CSV File](examples-export-table-csv.md)
+ [Creating an AWS Lambda Function](lambda.md)
+ [Extracting and Sending Text to AWS Comprehend for Analysis](textract-to-comprehend.md)
+ [Additional Code Samples](other-examples.md)

## Prerequisites<a name="examples-prerequisites"></a>

Before you can run the examples in this section, you have to configure your environment\. 

**To configure your environment**

1. Create or update an IAM user with `AmazonTextractFullAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.