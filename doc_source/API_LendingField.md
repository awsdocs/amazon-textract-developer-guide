# LendingField<a name="API_LendingField"></a>

Holds the normalized key\-value pairs returned by AnalyzeDocument, including the document type, detected text, and geometry\.

## Contents<a name="API_LendingField_Contents"></a>

 ** KeyDetection **   <a name="Textract-Type-LendingField-KeyDetection"></a>
The results extracted for a lending document\.  
Type: [LendingDetection](API_LendingDetection.md) object  
Required: No

 ** Type **   <a name="Textract-Type-LendingField-Type"></a>
The type of the lending document\.  
Type: String  
Required: No

 ** ValueDetections **   <a name="Textract-Type-LendingField-ValueDetections"></a>
An array of LendingDetection objects\.  
Type: Array of [LendingDetection](API_LendingDetection.md) objects  
Required: No

## See Also<a name="API_LendingField_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/LendingField) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/LendingField) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/LendingField) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/LendingField) 