# Step 2: Set Up the AWS CLI and AWS SDKs<a name="setup-awscli-sdk"></a>

The following steps show you how to install the AWS Command Line Interface \(AWS CLI\) and AWS SDKs that the examples in this documentation use\. 

There are a number of different ways to authenticate AWS SDK calls\. The examples in this guide assume that you're using a default credentials profile for calling AWS CLI commands and AWS SDK API operations\. Your default credentials will work across services, so if you have already configured your credentials you don't need to do so again\. However, if you would like to create another set of credentials for this service, you can create a name profile\. For more information about creating profiles, [see Named Profiles\.](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)

For a list of available AWS Regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

**To set up the AWS CLI and the AWS SDKs**

1. Download and install the AWS CLI and the AWS SDKs that you want to use\. This guide provides examples for the AWS CLI, Java, and Python\. For information about other AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/)\.
   + [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html)
   + [AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/)
   + [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

1. Create an access key for the user that you created in [Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane, choose **Users**\.

   1. Choose the name of the user that you created in [Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Choose the **Security credentials** tab\.

   1. Choose **Create access key**\. Then choose **Download \.csv file** to save the access key ID and secret access key to a CSV file on your computer\. Store the file in a secure location\. You will not have access to the secret access key again after this dialog box closes\. After you've downloaded the CSV file, choose **Close**\. 

1. Set credentials in the AWS credentials profile file on your local system, located at: 
   + `~/.aws/credentials` on Linux, macOS, or Unix\. 
   + `C:\Users\USERNAME\.aws\credentials` on Windows\.

   The `.aws` folder does not exist prior to your first initial configuration of your AWS instance\. The first time you configure your credentials with the CLI, this folder will be created\. For more information regarding AWS credentials, see [Configuration and Credential File Settings\.](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

   This file should contain lines in the following format:

   ```
   [default]
   aws_access_key_id = your_access_key_id
   aws_secret_access_key = your_secret_access_key
   ```

   Substitute your access key ID and secret access key for *your\_access\_key\_id* and *your\_secret\_access\_key*\.

1. Set the default AWS Region in the AWS `config` file on your local system, located at:
   + `~/.aws/config` on Linux, macOS, or Unix\.
   + `C:\Users\USERNAME\.aws\config` on Windows\.

   The `.aws` folder does not exist prior to your first initial configuration of your AWS instance\. The first time you configure your credentials with the CLI, this folder will be created\. For more information regarding AWS credentials, see [Configuration and Credential File Settings\.](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

   This file should contain the following lines: 

   ```
   [default]
   region = your_aws_region
   ```

   Substitute the AWS Region you want \(for example, "us\-west\-2"\) for *your\_aws\_region*\. 
**Note**  
If you don't choose a Region, then us\-east\-1 is used by default\. 

## Next Step<a name="setting-up-next-step-3"></a>

[Step 3: Get Started Using the AWS CLI and AWS SDK API](get-started-exercise.md)