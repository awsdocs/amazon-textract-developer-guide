# Limits in Amazon Textract<a name="limits"></a>

The following is a list of limits in Amazon Textract that you can't change\.

## Amazon Textract<a name="limits-document"></a>
+ The maximum document image \(JPEG/PNG\) size is 10 MB\. 
+ The maximum PDF file size is 500 MB\. 
+ The maximum number of pages in a PDF file is 3000\.
+ The maximum PDF media size for the height and width dimensions is 40 inches or 2880 points\.
+ The minimum height for text to be detected is 15 pixels\. At 150 DPI, this would be equivalent to 8\-pt font\.
+ Documents can be rotated a maximum of \+/\- 10% from the vertical axis\. Text can be text aligned horizontally within the document\. 
+ Amazon Textract only supports English text detection\.
+ Amazon Textract doesn't support the detection of handwriting\.
+ Amazon Textract synchronous operations \(`DetectDocumentText` and `AnalyzeDocument`\) support the PNG and JPEG image formats\. Asynchronous operations \(`StartDocumentTextDetection`, `StartDocumentAnalysis`\) also support the PDF file format\. 

 For information about limits you can change, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_textract)\. To change a limit, see [Create Case](https://console.aws.amazon.com/support/v1#/case/create?issueType=service-limit-increase)\. 