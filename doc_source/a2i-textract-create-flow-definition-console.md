# Create a Human Review Workflow \(Console\)<a name="a2i-textract-create-flow-definition-console"></a>

You can either complete this example using your own document in Amazon S3, or you can download [this sample document](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2020/04/17/sample-document-final.png) and place it in your Amazon S3 bucket\.

Make sure your S3 bucket is in the same AWS Region that you are using Amazon Textract\. To create a bucket, see[Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

**Note**  
The Amazon A2I console is embedded in the SageMaker console\. To use the console, you need permissions to access the SageMaker console, and to create a work team\. To get started, you can use the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) IAM managed policy which includes all of the necessary permissions to perform most actions in SageMaker\. For more information, see [Identity and Access Management for Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam.html) in the *Amazon SageMaker Developer Guide*\.

**Topics**
+ [Step 1: Create a Work Team \(Console\)](#a2i-textract-create-flow-definition-workteam)
+ [Step 2: Create a Human Review Workflow \(Console\)](#a2i-textract-create-flow-definition-console)

## Step 1: Create a Work Team \(Console\)<a name="a2i-textract-create-flow-definition-workteam"></a>

First, create a work team in the Amazon A2I console and add yourself as a worker so that you can preview the human review task in the worker portal, work team members can look at different tasks and documents assigned to them\.

**To create a private workforce using worker emails \(Console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. In the navigation pane, under **Ground Truth**, choose **Labeling workforces**\.

1. Choose **Private**, then choose **Create private team**\.

1. Choose **Invite new workers by email**\.

1. For this example, enter your email address and the email address of any others who you want to be able to preview the worker portal\. You can paste or type a list of up to 50 email addresses, separated by commas, into the **Email Addresses** box\.

1. Enter oneorganization name and contact email\.

1. Choose **Create private team**\.

If you add yourself to a private work team, you receive an email from `no-reply@verificationemail.com` with login information\. Use the link in this email to reset your password and log in to the worker portal\. This is where your human review tasks will appear after you call `AnalyzeDocument`\.

## Step 2: Create a Human Review Workflow \(Console\)<a name="a2i-textract-create-flow-definition-console"></a>

In this step, you create an Amazon Textract human review workflow\. 

**To create a human review workflow \(console\)**

1. Open the Amazon A2I console at [https://console\.aws\.amazon\.com/a2i](https://console.aws.amazon.com/a2i/) to access the **Human review workflows** page\. 

1. Choose **Create human review workflow**\.

1. For **Name**, enter a workflow name\. 

1. For **S3 bucket**, choose the bucket where you want Amazon A2I to store the results of your human review tasks\. If you don't choose a bucket, change that to enter the name of the bucket\.

1. Under **IAM role**, select **Create a new role**\. A window appears with the title **Create an IAM role**\. Use this window to specify the Amazon S3 buckets to which you want this role to have access\. If you do not select **Any S3 bucket**, specify the output bucket that you specified in step 4, and the bucket that contains your input document\.

1. For **Task type**, choose **Textract \- Key\-value pair extraction\.** 

1. In **Amazon Textract form extraction \- Conditions for invoking human review**, specify activation conditions\. We recommend that you set a high confidence score threshold for at least one key in your document to trigger a human review so that you can preview a worker task in the worker portal\.

   If you used the sample document provided in this walkthrough, specify activation conditions as follows: 

   1. Choose **Trigger a human review for specific form keys based on the form key confidence score or when specific form keys are missing\.**

   1. For **Key name**, enter **Mail Address**\. 

   1. Set the **identification confidence** threshold between *0* and *99*\. 

   1. Set the **qualification confidence** threshold between *0* and *99*\.

   1. Choose **Trigger a human review for all form keys identified by Amazon Textract with confidence scores in a specific range\.**

   1. For **identification confidence**, choose any number between 0 and 90\. 

   1. For **qualification confidence**, choose any number between 0 and 90\.

   This triggers a human review if Amazon Textract returns a confidence score that is less than 99 for *Mail Address* and its value, or if it returns a confidence score less than 90 for any key\-value pair detected in the document\. 

1. Under **Worker task template creation**, select **Create from a default template**\.

1. For **Template name**, enter a descriptive name\.\.

1. For **Task description**, add something similar to the following:

   **Read the instructions and review the document\.**

1. For **Workers** choose **Private**\.

1. From the menu choose the private team that you created\.

1. Select **Create**\.

After your human review workflow is created, it appears in the table on the **Human review workflows** page\. When the **Status** is **Active**, copy and save the workflow ARN\.