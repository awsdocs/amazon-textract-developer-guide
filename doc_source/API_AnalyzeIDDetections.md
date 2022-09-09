# AnalyzeIDDetections<a name="API_AnalyzeIDDetections"></a>

Used to contain the information detected by an AnalyzeID operation\.

## Contents<a name="API_AnalyzeIDDetections_Contents"></a>

 ** Confidence **   <a name="Textract-Type-AnalyzeIDDetections-Confidence"></a>
The confidence score of the detected text\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** NormalizedValue **   <a name="Textract-Type-AnalyzeIDDetections-NormalizedValue"></a>
Only returned for dates, returns the type of value detected and the date written in a more machine readable way\.  
Type: [NormalizedValue](API_NormalizedValue.md) object  
Required: No

 ** Text **   <a name="Textract-Type-AnalyzeIDDetections-Text"></a>
Text of either the normalized field or value associated with it\.  
Type: String  
Required: Yes

## See Also<a name="API_AnalyzeIDDetections_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/AnalyzeIDDetections) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/AnalyzeIDDetections) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/AnalyzeIDDetections) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/AnalyzeIDDetections) 