# LendingDetection<a name="API_LendingDetection"></a>

The results extracted for a lending document\.

## Contents<a name="API_LendingDetection_Contents"></a>

 ** Confidence **   <a name="Textract-Type-LendingDetection-Confidence"></a>
The confidence level for the text of a detected value in a lending document\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** Geometry **   <a name="Textract-Type-LendingDetection-Geometry"></a>
Information about where the following items are located on a document page: detected page, text, key\-value pairs, tables, table cells, and selection elements\.  
Type: [Geometry](API_Geometry.md) object  
Required: No

 ** SelectionStatus **   <a name="Textract-Type-LendingDetection-SelectionStatus"></a>
The selection status of a selection element, such as an option button or check box\.  
Type: String  
Valid Values:` SELECTED | NOT_SELECTED`   
Required: No

 ** Text **   <a name="Textract-Type-LendingDetection-Text"></a>
The text extracted for a detected value in a lending document\.  
Type: String  
Required: No

## See Also<a name="API_LendingDetection_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/LendingDetection) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/LendingDetection) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/LendingDetection) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/LendingDetection) 