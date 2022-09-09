# ExpenseField<a name="API_ExpenseField"></a>

Breakdown of detected information, seperated into the catagories Type, LabelDetection, and ValueDetection

## Contents<a name="API_ExpenseField_Contents"></a>

 ** LabelDetection **   <a name="Textract-Type-ExpenseField-LabelDetection"></a>
The explicitly stated label of a detected element\.  
Type: [ExpenseDetection](API_ExpenseDetection.md) object  
Required: No

 ** PageNumber **   <a name="Textract-Type-ExpenseField-PageNumber"></a>
The page number the value was detected on\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** Type **   <a name="Textract-Type-ExpenseField-Type"></a>
The implied label of a detected element\. Present alongside LabelDetection for explicit elements\.  
Type: [ExpenseType](API_ExpenseType.md) object  
Required: No

 ** ValueDetection **   <a name="Textract-Type-ExpenseField-ValueDetection"></a>
The value of a detected element\. Present in explicit and implicit elements\.  
Type: [ExpenseDetection](API_ExpenseDetection.md) object  
Required: No

## See Also<a name="API_ExpenseField_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/ExpenseField) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/ExpenseField) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/ExpenseField) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/ExpenseField) 