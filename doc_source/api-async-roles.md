# Configuring Amazon Textract for Asynchronous Operations<a name="api-async-roles"></a>

The following procedures show you how to configure Amazon Textract for use with an Amazon SNS topic and an Amazon SQS queue\.

**Note**  
If you're using these instructions to set up the [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md) example, you don't need to do steps 3, 4, 5, and 6\. The example includes code to create and configure the Amazon SNS topic and Amazon SQS queue\.

**To configure Amazon Textract**

1. Set up an AWS account to access Amazon Textract\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md)\.

   Ensure that the user has at least the following permissions:
   + AmazonTextractFullAccess
   + AmazonS3ReadOnlyAccess
   + AmazonSNSFullAccess
   + AmazonSQSFullAccess

1. Install and configure the required AWS SDK\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\. 

1. [Create an Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html)\. Prepend the topic name with *AmazonTextract*\. Note the topic Amazon Resource Name \(ARN\)\. Ensure that the topic is in the same Region as the AWS endpoint that you're using\.

1. [Create an Amazon SQS standard queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html) by using the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\. Note the queue ARN\.

1. [Subscribe the queue to the topic](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-subscribe-queue-sns-topic.html) you created in step 3\.

1. [Give permission to the Amazon SNS topic to send messages to the Amazon SQS queue](https://docs.aws.amazon.com/sns/latest/dg/SendMessageToSQS.html#SendMessageToSQS.sqs.permissions)\.

1. Create an IAM service role to give Amazon Textract access to your Amazon SNS topics\. Note the Amazon Resource Name \(ARN\) of the service role\. For more information, see [Giving Amazon Textract Access to Your Amazon SNS Topic](#api-async-roles-all-topics)\.

1. [ Add the following inline policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#embed-inline-policy-console) to the IAM user that you created in step 1: 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "MySid",
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "arn:Service role ARN from step 7"
           }
       ]
   }
   ```

   Give the inline policy a name\.

1. You can now run the examples in [Detecting or Analyzing Text in a Multipage Document](async-analyzing-with-sqs.md)\.

## Giving Amazon Textract Access to Your Amazon SNS Topic<a name="api-async-roles-all-topics"></a>

Amazon Textract needs permission to send the completion status of an asynchronous operation to your Amazon SNS topic\. You use an IAM service role to give Amazon Textract access to the Amazon SNS topic\. 

 When you create the Amazon SNS topic, you must prepend the topic name with *AmazonTextract*—for example, `AmazonTextractMyTopicName`\. 

1. Sign in to the IAM console \([https://console\.aws\.amazon\.com/iam](https://console.aws.amazon.com/iam)\)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\. 

1. For **Choose the service that will use this role**, choose **EC2**\.

1. Choose **Next: Permissions**\.

1. For **Attach permissions policies**, select the check box next to the policy *AmazonTextractServiceRole*\. To display the policy in the list, enter part of the policy name in the **Filter policies** query filter\.

1. Choose **Next: Tags**\.

1. You don't need to add tags, so choose **Next: Review**\.

1. In the **Review** section, for **Role name**, enter a name for the role \(for example, `TextractRole`\)\. In **Role description**, update the description for the role in **Role description**, and then choose **Create role**\.

1. Choose the new role to open the role's details page\.

1. In the **Summary**, copy the **Role ARN** value and save it\.

1. Choose **Trust relationships**\.

1. Choose **Edit trust relationship**, and update the trust policy as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "textract.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.