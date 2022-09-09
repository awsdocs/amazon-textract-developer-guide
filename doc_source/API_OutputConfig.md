# OutputConfig<a name="API_OutputConfig"></a>

Sets whether or not your output will go to a user created bucket\. Used to set the name of the bucket, and the prefix on the output file\.

 `OutputConfig` is an optional parameter which lets you adjust where your output will be placed\. By default, Amazon Textract will store the results internally and can only be accessed by the Get API operations\. With OutputConfig enabled, you can set the name of the bucket the output will be sent to and the file prefix of the results where you can download your results\. Additionally, you can set the `KMSKeyID` parameter to a customer master key \(CMK\) to encrypt your output\. Without this parameter set Amazon Textract will encrypt server\-side using the AWS managed CMK for Amazon S3\.

Decryption of Customer Content is necessary for processing of the documents by Amazon Textract\. If your account is opted out under an AI services opt out policy then all unencrypted Customer Content is immediately and permanently deleted after the Customer Content has been processed by the service\. No copy of of the output is retained by Amazon Textract\. For information about how to opt out, see [ Managing AI services opt\-out policy\. ](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_ai-opt-out.html) 

For more information on data privacy, see the [Data Privacy FAQ](https://aws.amazon.com/compliance/data-privacy-faq/)\.

## Contents<a name="API_OutputConfig_Contents"></a>

 ** S3Bucket **   <a name="Textract-Type-OutputConfig-S3Bucket"></a>
The name of the bucket your output will go to\.  
Type: String  
Length Constraints: Minimum length of 3\. Maximum length of 255\.  
Pattern: `[0-9A-Za-z\.\-_]*`   
Required: Yes

 ** S3Prefix **   <a name="Textract-Type-OutputConfig-S3Prefix"></a>
The prefix of the object key that the output will be saved to\. When not enabled, the prefix will be “textract\_output"\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `.*\S.*`   
Required: No

## See Also<a name="API_OutputConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/OutputConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/OutputConfig) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/OutputConfig) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/OutputConfig) 