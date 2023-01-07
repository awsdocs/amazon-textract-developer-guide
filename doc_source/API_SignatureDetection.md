# SignatureDetection<a name="API_SignatureDetection"></a>

Information regarding a detected signature on a page\.

## Contents<a name="API_SignatureDetection_Contents"></a>

 ** Confidence **   <a name="Textract-Type-SignatureDetection-Confidence"></a>
The confidence, from 0 to 100, in the predicted values for a detected signature\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** Geometry **   <a name="Textract-Type-SignatureDetection-Geometry"></a>
Information about where the following items are located on a document page: detected page, text, key\-value pairs, tables, table cells, and selection elements\.  
Type: [Geometry](API_Geometry.md) object  
Required: No

## See Also<a name="API_SignatureDetection_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/SignatureDetection) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/SignatureDetection) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/SignatureDetection) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/SignatureDetection) 