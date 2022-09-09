# S3Object<a name="API_S3Object"></a>

The S3 bucket name and file name that identifies the document\.

The AWS Region for the S3 bucket that contains the document must match the Region that you use for Amazon Textract operations\.

For Amazon Textract to process a file in an S3 bucket, the user must have permission to access the S3 bucket and file\. 

## Contents<a name="API_S3Object_Contents"></a>

 ** Bucket **   <a name="Textract-Type-S3Object-Bucket"></a>
The name of the S3 bucket\. Note that the \# character is not valid in the file name\.  
Type: String  
Length Constraints: Minimum length of 3\. Maximum length of 255\.  
Pattern: `[0-9A-Za-z\.\-_]*`   
Required: No

 ** Name **   <a name="Textract-Type-S3Object-Name"></a>
The file name of the input document\. Synchronous operations can use image files that are in JPEG or PNG format\. Asynchronous operations also support PDF and TIFF format files\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `.*\S.*`   
Required: No

 ** Version **   <a name="Textract-Type-S3Object-Version"></a>
If the bucket has versioning enabled, you can specify the object version\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `.*\S.*`   
Required: No

## See Also<a name="API_S3Object_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/S3Object) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/S3Object) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/S3Object) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/S3Object) 