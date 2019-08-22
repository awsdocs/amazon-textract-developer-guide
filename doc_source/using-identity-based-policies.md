# Using Identity\-based Policies \(IAM Policies\) for Amazon Textract<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and then grant permissions to perform operations on Amazon Textract resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your Amazon Textract resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon Textract Resources](access-control-overview.md)\. 

**Topics**
+ [Permissions Required to Use the Amazon Textract Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for Amazon Textract](#access-policy-aws-managed-policies)
+ [Customer Managed Policy Examples](#access-policy-customer-managed-examples)

The following shows an example of a permissions policy\.

```
"Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "textract:DetectDocumentText",
                "textract:AnalyzeDocument"
            ],
            "Resource": "*"
        }
    ]
```

This policy example grants a user access to the synchronous Amazon Textract operations\.

For a table showing all of the Amazon Textract API operations and the resources that they apply to, see [Amazon Textract API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\. 

## Permissions Required to Use the Amazon Textract Console<a name="console-permissions"></a>

Amazon Textract doesn't require any additional permissions when you're working with the Amazon Textract console\.

## AWS Managed \(Predefined\) Policies for Amazon Textract<a name="access-policy-aws-managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon Textract:
+ **AmazonTextractFullAccess** – Grants full access to Amazon Textract\.
+ **AmazonTextractServiceRole** – Allows Amazon Textract to call Amazon SNS services on your behalf\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.  
These policies work when you're using AWS SDKs or the AWS CLI\.

You can also create your own custom IAM policies to allow permissions for Amazon Textract actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

## Customer Managed Policy Examples<a name="access-policy-customer-managed-examples"></a>

In this section, you can find example user policies that grant permissions for various Amazon Textract actions\. These policies work when you're using AWS SDKs or the AWS CLI\. When you're using the console, you need to grant additional permissions that are specific to the console\. For more information, see [Permissions Required to Use the Amazon Textract Console](#console-permissions)\.

**Topics**
+ [Example 1: Allow a User Access to Only Asynchronous Amazon Textract Operations](#access-policy-customer-managed-first-example)
+ [Example 2: Allow a User Full Access to Amazon Textract Operations](#access-policy-customer-managed-second-example)

### Example 1: Allow a User Access to Only Asynchronous Amazon Textract Operations<a name="access-policy-customer-managed-first-example"></a>

The following example grants access to asynchronous Amazon Textract operations\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "textract:StartDocumentTextDetection",
                "textract:StartDocumentAnalysis",
                "textract:GetDocumentTextDetection",
                "textract:GetDocumentAnalysis"
            ],
            "Resource": "*"
        }
    ]
}
```

### Example 2: Allow a User Full Access to Amazon Textract Operations<a name="access-policy-customer-managed-second-example"></a>

The following example grants full access to Amazon Textract operations\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "textract:*"
            ],
            "Resource": "*"
        }
    ]
}
```