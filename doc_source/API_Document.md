# Document<a name="API_Document"></a>

The input document, either as bytes or as an S3 object\.

You pass image bytes to an Amazon Textract API operation by using the `Bytes` property\. For example, you would use the `Bytes` property to pass a document loaded from a local file system\. Image bytes passed by using the `Bytes` property must be base64 encoded\. Your code might not need to encode document file bytes if you're using an AWS SDK to call Amazon Textract API operations\. 

You pass images stored in an S3 bucket to an Amazon Textract API operation by using the `S3Object` property\. Documents stored in an S3 bucket don't need to be base64 encoded\.

The AWS Region for the S3 bucket that contains the S3 object must match the AWS Region that you use for Amazon Textract operations\.

If you use the AWS CLI to call Amazon Textract operations, passing image bytes using the Bytes property isn't supported\. You must first upload the document to an Amazon S3 bucket, and then call the operation using the S3Object property\.

For Amazon Textract to process an S3 object, the user must have permission to access the S3 object\. 

## Contents<a name="API_Document_Contents"></a>

 ** Bytes **   <a name="Textract-Type-Document-Bytes"></a>
A blob of base64\-encoded document bytes\. The maximum size of a document that's provided in a blob of bytes is 5 MB\. The document bytes must be in PNG or JPEG format\.  
If you're using an AWS SDK to call Amazon Textract, you might not need to base64\-encode image bytes passed using the `Bytes` field\.   
Type: Base64\-encoded binary data object  
Length Constraints: Minimum length of 1\. Maximum length of 10485760\.  
Required: No

 ** S3Object **   <a name="Textract-Type-Document-S3Object"></a>
Identifies an S3 object as the document source\. The maximum size of a document that's stored in an S3 bucket is 5 MB\.  
Type: [S3Object](API_S3Object.md) object  
Required: No

## See Also<a name="API_Document_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Document) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Document) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Document) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Document) 