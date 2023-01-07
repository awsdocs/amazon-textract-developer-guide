# ExpenseDocument<a name="API_ExpenseDocument"></a>

The structure holding all the information returned by AnalyzeExpense

## Contents<a name="API_ExpenseDocument_Contents"></a>

 ** Blocks **   <a name="Textract-Type-ExpenseDocument-Blocks"></a>
This is a block object, the same as reported when DetectDocumentText is run on a document\. It provides word level recognition of text\.  
Type: Array of [Block](API_Block.md) objects  
Required: No

 ** ExpenseIndex **   <a name="Textract-Type-ExpenseDocument-ExpenseIndex"></a>
Denotes which invoice or receipt in the document the information is coming from\. First document will be 1, the second 2, and so on\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** LineItemGroups **   <a name="Textract-Type-ExpenseDocument-LineItemGroups"></a>
Information detected on each table of a document, seperated into `LineItems`\.  
Type: Array of [LineItemGroup](API_LineItemGroup.md) objects  
Required: No

 ** SummaryFields **   <a name="Textract-Type-ExpenseDocument-SummaryFields"></a>
Any information found outside of a table by Amazon Textract\.  
Type: Array of [ExpenseField](API_ExpenseField.md) objects  
Required: No

## See Also<a name="API_ExpenseDocument_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/ExpenseDocument) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/ExpenseDocument) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/ExpenseDocument) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/ExpenseDocument) 