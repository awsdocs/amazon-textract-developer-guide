# HumanLoopConfig<a name="API_HumanLoopConfig"></a>

Sets up the human review workflow the document will be sent to if one of the conditions is met\. You can also set certain attributes of the image before review\. 

## Contents<a name="API_HumanLoopConfig_Contents"></a>

 **DataAttributes**   <a name="Textract-Type-HumanLoopConfig-DataAttributes"></a>
Sets attributes of the input data\.  
Type: [HumanLoopDataAttributes](API_HumanLoopDataAttributes.md) object  
Required: No

 **FlowDefinitionArn**   <a name="Textract-Type-HumanLoopConfig-FlowDefinitionArn"></a>
The Amazon Resource Name \(ARN\) of the flow definition\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **HumanLoopName**   <a name="Textract-Type-HumanLoopConfig-HumanLoopName"></a>
The name of the human workflow used for this image\. This should be kept unique within a region\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-z0-9](-*[a-z0-9])*`   
Required: Yes

## See Also<a name="API_HumanLoopConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/HumanLoopConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/HumanLoopConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/HumanLoopConfig) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/HumanLoopConfig) 