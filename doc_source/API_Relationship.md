# Relationship<a name="API_Relationship"></a>

Information about how blocks are related to each other\. A `Block` object contains 0 or more `Relation` objects in a list, `Relationships`\. For more information, see [Block](API_Block.md)\.

The `Type` element provides the type of the relationship for all blocks in the `IDs` array\. 

## Contents<a name="API_Relationship_Contents"></a>

 ** Ids **   <a name="Textract-Type-Relationship-Ids"></a>
An array of IDs for related blocks\. You can get the type of the relationship from the `Type` element\.  
Type: Array of strings  
Pattern: `.*\S.*`   
Required: No

 ** Type **   <a name="Textract-Type-Relationship-Type"></a>
The type of relationship that the blocks in the IDs array have with the current block\. The relationship can be `VALUE` or `CHILD`\. A relationship of type VALUE is a list that contains the ID of the VALUE block that's associated with the KEY of a key\-value pair\. A relationship of type CHILD is a list of IDs that identify WORD blocks in the case of lines Cell blocks in the case of Tables, and WORD blocks in the case of Selection Elements\.  
Type: String  
Valid Values:` VALUE | CHILD | COMPLEX_FEATURES | MERGED_CELL | TITLE | ANSWER`   
Required: No

## See Also<a name="API_Relationship_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Relationship) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Relationship) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Relationship) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Relationship) 