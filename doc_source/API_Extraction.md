# Extraction<a name="API_Extraction"></a>

Contains information extracted by an analysis operation after using StartLendingAnalysis\.

## Contents<a name="API_Extraction_Contents"></a>

 ** ExpenseDocument **   <a name="Textract-Type-Extraction-ExpenseDocument"></a>
The structure holding all the information returned by AnalyzeExpense  
Type: [ExpenseDocument](API_ExpenseDocument.md) object  
Required: No

 ** IdentityDocument **   <a name="Textract-Type-Extraction-IdentityDocument"></a>
The structure that lists each document processed in an AnalyzeID operation\.  
Type: [IdentityDocument](API_IdentityDocument.md) object  
Required: No

 ** LendingDocument **   <a name="Textract-Type-Extraction-LendingDocument"></a>
Holds the structured data returned by AnalyzeDocument for lending documents\.  
Type: [LendingDocument](API_LendingDocument.md) object  
Required: No

## See Also<a name="API_Extraction_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Extraction) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Extraction) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Extraction) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Extraction) 