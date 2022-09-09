# LineItemGroup<a name="API_LineItemGroup"></a>

A grouping of tables which contain LineItems, with each table identified by the table's `LineItemGroupIndex`\.

## Contents<a name="API_LineItemGroup_Contents"></a>

 ** LineItemGroupIndex **   <a name="Textract-Type-LineItemGroup-LineItemGroupIndex"></a>
The number used to identify a specific table in a document\. The first table encountered will have a LineItemGroupIndex of 1, the second 2, etc\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** LineItems **   <a name="Textract-Type-LineItemGroup-LineItems"></a>
The breakdown of information on a particular line of a table\.   
Type: Array of [LineItemFields](API_LineItemFields.md) objects  
Required: No

## See Also<a name="API_LineItemGroup_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/LineItemGroup) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/LineItemGroup) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/LineItemGroup) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/LineItemGroup) 