# Relationship<a name="API_Relationship"></a>

Information about how blocks are related to each other\. A `Block` object contains 0 or more `Relation` objects in a list, `Relationships`\. For more information, see [Block](API_Block.md)\.

The `Type` element provides the type of the relationship for all blocks in the `IDs` array\. 

## Contents<a name="API_Relationship_Contents"></a>

 **Ids**   <a name="Textract-Type-Relationship-Ids"></a>
An array of IDs for related blocks\. You can get the type of the relationship from the `Type` element\.  
Type: Array of strings  
Pattern: `.*\S.*`   
Required: No

 **Type**   <a name="Textract-Type-Relationship-Type"></a>
The type of relationship that the blocks in the IDs array have with the current block\. The relationship can be `VALUE` or `CHILD`\.  
Type: String  
Valid Values:` VALUE | CHILD`   
Required: No

## See Also<a name="API_Relationship_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Relationship) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Relationship) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/textract-2018-06-27/Relationship) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/Relationship) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/textract-2018-06-27/Relationship) 