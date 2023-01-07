# NotificationChannel<a name="API_NotificationChannel"></a>

The Amazon Simple Notification Service \(Amazon SNS\) topic to which Amazon Textract publishes the completion status of an asynchronous document operation\. 

## Contents<a name="API_NotificationChannel_Contents"></a>

 ** RoleArn **   <a name="Textract-Type-NotificationChannel-RoleArn"></a>
The Amazon Resource Name \(ARN\) of an IAM role that gives Amazon Textract publishing permissions to the Amazon SNS topic\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `arn:([a-z\d-]+):iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+`   
Required: Yes

 ** SNSTopicArn **   <a name="Textract-Type-NotificationChannel-SNSTopicArn"></a>
The Amazon SNS topic that Amazon Textract posts the completion status to\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 1024\.  
Pattern: `(^arn:([a-z\d-]+):sns:[a-zA-Z\d-]{1,20}:\w{12}:.+$)`   
Required: Yes

## See Also<a name="API_NotificationChannel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/NotificationChannel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/NotificationChannel) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/NotificationChannel) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/NotificationChannel) 