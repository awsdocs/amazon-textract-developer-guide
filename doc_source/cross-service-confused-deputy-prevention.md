# Cross\-service confused deputy prevention<a name="cross-service-confused-deputy-prevention"></a>

In AWS, cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to act on another customer's resources even though it shouldn't have the proper permissions, resulting in the confused deputy problem\.

To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in resource policies to limit the permissions that Amazon Textract gives another service to the resource\. 

If the value of `aws:SourceArn` does not contain the account ID, such as an Amazon S3 bucket ARN, you must use both keys to limit permissions\. If you use both keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. 

Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.

The value of `aws:SourceArn` must be the ARN of the resource used by Textract, which is specified with the following format: `arn:aws:rekognition:region:account:resource`\.

The recommended approach to the confused deputy problem is to use the `aws:SourceArn` global condition context key with the full resource ARN\. 

 If you don't know the full ARN of the resource or if you are specifying multiple resources, use the `aws:SourceArn` key with wildcard characters \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:textract:*:111122223333:*`\. 

In order to protect against the confused deputy problem, carry out the following steps:

1. In the navigation pane of the IAM console choose the **Roles** option\. The console will display the roles for your current account\.

1. Choose the name of the role that you want to modify\. The role you modify should have the **AmazonTextractServiceRole** permissions policy\. Select the **Trust relationships** tab\.

1. Choose **Edit trust policy**\.

1. On the **Edit trust policy** page, replace the default JSON policy with a policy that utilizes one or both of the `aws:SourceArn` and `aws:SourceAccount` global condition context keys\. See the following example policies\.

1. Choose **Update policy**\.

The following examples are trust policies that show how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys in Amazon Textract to prevent the confused deputy problem\.

You can specify multiple accounts in both the `SourceAccount` and `SourceArn` condition\. Be sure to specify the ID of any trusted account in both conditions\. 

If you are working with Amazon Textract's asynchronous operations, you could use a policy like the following in your IAM role\. In the example below, replace the red replaceable text with the IDs of the accounts calling the API operations \(your account ID and the IDs of any other trusted accounts\):

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "ConfusedDeputyPreventionExamplePolicy",
    "Effect": "Allow",
    "Principal": {
      "Service": "textract.amazonaws.com"
    },
    "Action": "sts:AssumeRole",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn":["arn:aws:textract:*:123456789012:*","arn:aws:textract:*:111122223333:*"]
      },
      "StringEquals": {
        "aws:SourceAccount": ["123456789012", "111122223333"]
      }
    }
  }
}
```