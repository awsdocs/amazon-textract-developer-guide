# Information on Default Quotas in Amazon Textract<a name="limits-quotas-explained"></a>

Your console account has default quotas for Amazon Textract operations\. These are a set of region\-specific quotas that you can modify\. Default quotas can be viewed or changed via the Service quotas console\. You can also view the current Amazon Textract default quotas on the [Amazon Textract endpoints and service quotas](https://docs.aws.amazon.com/general/latest/gr/textract.html)\.

## Types of Quotas<a name="limits-quotas-types"></a>

There are two types of quotas for Amazon Textract\.
+ Transactions Per Second \(TPS\) \(Synchronous and Asynchronous\)
+ Concurrent Jobs \(Asynchronous Only\)

### TPS<a name="limits-quotas-TPS"></a>

TPS quotas determine how often you can request that Textract process a new document\. TPS quotas are unique to API, Synchronous or Asynchrounous operations, and region\.

#### Synchronous TPS<a name="limits-quotas-TPS-sync"></a>

Synchronous operations are used for processing single\-page documents and receiving near real\-time responses for Textract include AnalyzeDocument, DetectDocumentText, AnalyzeExpense, and AnalyzeID\. For more information, see [Processing Documents with Synchronous Operations](sync.md)\. 

For the following Synchronous APIs, Maximum Transactions Per Second \(TPS\) describes the maximum number of transactions \(API calls\) you can perform with your account in a given region: 
+ AnalyzeDocument
+ DetectDocumentText
+ AnalyzeExpense
+ AnalyzeID

#### Asynchronous TPS<a name="limits-quotas-TPS-async"></a>

Amazon Textract provides non\-real time, asynchronous operations for the processing of larger, multipage documents\. There are two types of asynchronous processing operations: Start operations and Get operations\. Start operations are used to request that Textract process a document, while Get operations are used to obtain the status and results of document processing\. Quotas for both Start and Get operations are measured in TPS\. For more information about asynchronous processing, see [Processing Documents with Asynchronous Operations](async.md)\.

For the following, Start and Get APIs, Maximum Transactions Per Second \(TPS\) describes the maximum number of transactions \(API calls\) you can perform with your account in a given region:

##### Start<a name="limits-quotas-TPS-async-start"></a>
+ StartDocumentAnalysis
+ StartDocumentTextDetection
+ StartExpenseAnalysis

##### Get<a name="limits-quotas-TPS-async-get"></a>
+ GetDocumentAnalysis
+ GetDocumentTextDetection
+ GetExpenseAnalysis

### Concurrent Jobs<a name="limits-quotas-concurrent-jobs"></a>

Concurrent job limit defines how many jobs can be run in parallel at a given time\. The Concurrent Job limits apply only to asynchronous operations, and is unique to individual operations\.

The following APIs have quotas defining the Maximum Number of Concurrent Jobs that your account can create in a given region:
+ AnalyzeDocument
+ DetectDocumentText
+ DetectDocumentExpense

## Calculate quota increase<a name="limits-quotas-calculate"></a>

Amazon Textract has a product specific [Service Quotas Calculator](https://console.aws.amazon.com/textract/home#/quotaCalculator) to estimate your quota needs\. You can use the Textract Service Quotas Calculator to estimate the quota values that will satisfy your use case\. It is accessible from the Amazon Textract console\. This section will outline the process of calculating a quota to suit your needs with the Quotas Calculator\. 

**To use the Textract Service Quotas Calculator**

1. **Selecting Regions**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/region-example.png)

   Select your account's region

   The service calculator can estimate quotas in any region, and different regions have different default quotas\. As such, make sure you match your account's region to the region you're calculating for\. If you want to calculate a quota for a different region, change your account's region in the console first\.

1. **Processing type**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/processing-example.png)

   Select *Synchronous* or *Asynchronous*\.

   Synchronous and Asynchronous operations have different limits in Amazon Textract\. Determine which type of operation that you intend to use the most, and select that operation type for calculations\. Generally, synchronous operations are used for single page document processing, where asynchronous processing covers multipage documents\.

   For example, if your use case is processing single page customer receipts, you'll select Synchronous 

1. **Use case type**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/type-example.png)

   Select the operation best suited for your use case\.

   Different operations extract different kinds of data, so select the operation most suited to your use case\. For more information on the data extracted by different operations, see [How Amazon Textract Works](how-it-works.md)\.

   For example, if your use case is primarily related to processing receipts, you'll select *Expense Analysis* from the list of operations\.

1. **Usage values**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/sync-input-example.png)

    Specify your usage values\.

   When determining requirements, keep in mind whether your type of operation\. For Synchronous operations, specify the maximum number of pages you want to process in a given timeframe, either hours or days\. 
**Note**  
For asynchronous operations you also can enter the expected number of hours processing will require, and the average number of pages in the processed documents\.

   For example, if you process an average of a million receipts a day, you'll enter 1,000,000 into the *Documents to be processed tab* to estimate the quota value needed\.

1. **Review results**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/output-example.png)

   Check step 3 of the calculator page and review the calculator's output\. The calculator pulls your current quotas information and compares them to the quotas required for your use case\. This will tell you if your current quotas are too low for your processing needs\. If this is the case, you can click on the link provided by the calculator which will directly link you to a quota increase request for the operation you estimated for in the region you selected in Service quotas\.
**Note**  
If you are calculating for asynchronous, you will see multiple quotas estimated\. You will need to click the link for each quota to request an increase for each one, if an increase is required\.

   Following with our example throughout these steps, you would require a TPS of 12 to process all one million receipts in a day\. As such, you'll need to request an increase from the default TPS of 5\.

## Best Practices for Service Quota Increase Requests<a name="best-quota-practices"></a>

When requesting an increase to a default quota, there are several recommended best practices to follow\. These include smooth spiky traffic, configuring retries, and configuring exponential backoff and jitters\.
+ Estimate your optimal quota values using the Textract Service Quota Calculator\.
+ Smoothening spiky traffic\. Spiky traffic affects throughput\. To get maximum throughput for the allotted transactions per second \(TPS\), use a queueing serverless architecture or another mechanism to “smooth” traffic so it is more consistent\. 
+ Configure retries\. Follow the guidelines at [Error handling](https://docs.aws.amazon.com/textract/latest/dg/handling-errors.html) to configure retries for the errors that allow them
+ Configure exponential backoff and jitter\. By configuring exponential backoff and jitter for retries, you can improve throughput\. See [Error retries and exponential backoff in AWS\.](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)
+ Start with samples that apply best practices, like the [IDP CDK Samples](https://github.com/aws-samples/amazon-textract-idp-cdk-stack-samples) using [CDK Constructs](https://github.com/aws-samples/amazon-textract-idp-cdk-constructs)\.

## Change Default Quota<a name="limits-quotas-change"></a>

As an alternative to raising a request directly from the calculator, you can also use the Service quotas console\. This can be done from the [Service quotas console](https://console.aws.amazon.com/servicequotas), and may be processed automatically\. Under some circumstances, such as a particularly large increase, the request will be processed manually and additional information may be required\.

**To raise a service quota increase request**

1. Log in to Management Console and navigate to the [Service quotas console](https://console.aws.amazon.com/servicequotas) and select “Textract” under services

1. Select the desired quota and click “Request Quota Increase” on the subsequent page

1. Enter in the desired quota value and click “Request”\. If you used the Service Quotas Calculator, you can copy the quota value from your calculations into the request\.

1. After requesting a quota increase, refresh the page to see the quota status update\.

**Note**  
Some increases in quota values may need manual review\.

## Quota Modification Effects<a name="limits-quotas-modifications"></a>

The following table lists the different types of quotas and the effects modifying them will have\.


| Quota Value to Increase | Effect of Increase | 
| --- | --- | 
|  Synchronous operation TPS  |  Increases how often you can request that Textract process a new document using a given synchronous operation, measured in transactions per second  | 
|  Asynchronous Start operation TPS  |  Increases how often you can request that Textract begin the asynchronous processing of an input document, measured in transactions per second  | 
|  Asynchronous Get operation TPS  |  Increases how often you can request that Textract return the results of a given asynchronous analysis job, measured in transactions per second  | 
|  Asynchronous Concurrent jobs  |  Increases the total number of documents that you can have processing in parallel\.  | 