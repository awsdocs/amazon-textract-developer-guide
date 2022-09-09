# ExpenseDetection<a name="API_ExpenseDetection"></a>

An object used to store information about the Value or Label detected by Amazon Textract\.

## Contents<a name="API_ExpenseDetection_Contents"></a>

 ** Confidence **   <a name="Textract-Type-ExpenseDetection-Confidence"></a>
The confidence in detection, as a percentage  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** Geometry **   <a name="Textract-Type-ExpenseDetection-Geometry"></a>
Information about where the following items are located on a document page: detected page, text, key\-value pairs, tables, table cells, and selection elements\.  
Type: [Geometry](API_Geometry.md) object  
Required: No

 ** Text **   <a name="Textract-Type-ExpenseDetection-Text"></a>
The word or line of text recognized by Amazon Textract  
Type: String  
Required: No

## See Also<a name="API_ExpenseDetection_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/ExpenseDetection) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/ExpenseDetection) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/ExpenseDetection) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/ExpenseDetection) 