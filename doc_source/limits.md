# Hard Limits in Amazon Textract<a name="limits"></a>

The following is a list of hard limits in Amazon Textract, which cannot be changed\. For information about limitations in location and limits you can change see [Amazon Textract Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/textract.html)\. For information about limits you can change, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_textract)\. To change a limit, see [Create Case](https://console.aws.amazon.com/support/v1#/case/create?issueType=service-limit-increase)\. 

## Amazon Textract<a name="limits-document"></a>


| Limit | Description | 
| --- | --- | 
|  Accepted File Formats  |  Operations support JPEG, PNG, PDF, and TIFF files\. \(JPEG 2000\-encoded images within PDFs are supported\)\.  | 
|  File Size and Page Count Limits  |  For synchronous operations, JPEG, PNG, PDF, and TIFF files have a 10MB size limit\. PDF and TIFF files also have a limit of 1 page\. For asynchronous operations, JPEG and PNG files have a 10MB size limit\. PDF and TIFF files have a 500MB limit\. PDF and TIFF files have a limit of 3,000 pages\.  | 
|  PDF Specific Limits  |  The maximum height and width is 40 inches and 2880 points\. PDFs cannot be password protected\. PDFs can contain JPEG 2000 formatted images\.  | 
|  Document Rotation and Image Size  |  Amazon Textract supports all in\-plane document rotations, for example 45 degree in\-plane rotation\. Amazon Textract supports images with a resolution less than or equal to 10000 pixels on all sides\.  | 
| Query Specific Limits |  Amazon Textract supports up to 15 queries per page for synchronous operations and up to 30 queries per page for asynchronous operations\.   | 
|  Text Alignment  |   Text can be text aligned horizontally within the document\. Horizontally arrayed text can be read regardless of the degree of rotation of a document\. Amazon Textract does not support vertical text \(text written vertically, as is common in languages like Japanese and Chinese\) alignment within the document\.  | 
|  Languages  |   Amazon Textract supports English, French, German, Italian, Portuguese and Spanish text detection\. Amazon Textract will not return the language detected in its output\. Query detection is only available in English document detection\.   | 
|  Character Size  |  The minimum height for text to be detected is 15 pixels\. At 150 DPI, this would be the same as 8 point font\.  | 
|  Character Type  |  Amazon Textract supports both handwritten and printed character recognition\.   | 
|  Characters  |  Amazon Textract detects the following characters: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/textract/latest/dg/limits.html)  | 
|  AnalyzeID Specific Limits  |  AnalyzeID only supports U\.S\. Passports, and U\.S\. Driver's Licenses\.  | 