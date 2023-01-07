# IdentityDocument<a name="API_IdentityDocument"></a>

The structure that lists each document processed in an AnalyzeID operation\.

## Contents<a name="API_IdentityDocument_Contents"></a>

 ** Blocks **   <a name="Textract-Type-IdentityDocument-Blocks"></a>
Individual word recognition, as returned by document detection\.  
Type: Array of [Block](API_Block.md) objects  
Required: No

 ** DocumentIndex **   <a name="Textract-Type-IdentityDocument-DocumentIndex"></a>
Denotes the placement of a document in the IdentityDocument list\. The first document is marked 1, the second 2 and so on\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** IdentityDocumentFields **   <a name="Textract-Type-IdentityDocument-IdentityDocumentFields"></a>
The structure used to record information extracted from identity documents\. Contains both normalized field and value of the extracted text\.  
Type: Array of [IdentityDocumentField](API_IdentityDocumentField.md) objects  
Required: No

## See Also<a name="API_IdentityDocument_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/IdentityDocument) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/IdentityDocument) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/IdentityDocument) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/IdentityDocument) 