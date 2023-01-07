# DocumentGroup<a name="API_DocumentGroup"></a>

Summary information about documents grouped by the same document type\.

## Contents<a name="API_DocumentGroup_Contents"></a>

 ** DetectedSignatures **   <a name="Textract-Type-DocumentGroup-DetectedSignatures"></a>
A list of the detected signatures found in a document group\.  
Type: Array of [DetectedSignature](API_DetectedSignature.md) objects  
Required: No

 ** SplitDocuments **   <a name="Textract-Type-DocumentGroup-SplitDocuments"></a>
An array that contains information about the pages of a document, defined by logical boundary\.  
Type: Array of [SplitDocument](API_SplitDocument.md) objects  
Required: No

 ** Type **   <a name="Textract-Type-DocumentGroup-Type"></a>
The type of document that Amazon Textract has detected\. See LINK for a list of all types returned by Textract\.  
Type: String  
Pattern: `.*\S.*`   
Required: No

 ** UndetectedSignatures **   <a name="Textract-Type-DocumentGroup-UndetectedSignatures"></a>
A list of any expected signatures not found in a document group\.  
Type: Array of [UndetectedSignature](API_UndetectedSignature.md) objects  
Required: No

## See Also<a name="API_DocumentGroup_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/DocumentGroup) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/DocumentGroup) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/DocumentGroup) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/DocumentGroup) 