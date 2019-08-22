# Creating an AWS Lambda Function<a name="lambda"></a>

You can call Amazon Textract API operations from within an AWS Lambda function\. The following instructions show how to create a Lambda function in Python that calls [DetectDocumentText](API_DetectDocumentText.md)\. It returns a list of [Block](API_Block.md) objects\. To run this example, you need an Amazon S3 bucket that contains a document in PNG or JPEG format\.

For an example that uses Lambda functions to process documents at a large scale, see [Large scale document processing with Amazon Textract](https://github.com/aws-samples/amazon-textract-serverless-large-scale-document-processing)\.<a name="create-deployment-package"></a>

**Step 1: Create an AWS Lambda deployment package**

1. Open a command window\.

1. Enter the following commands to create a deployment package with the most recent version of the AWS SDK\.

   ```
   pip install boto3 --target python/.
   zip boto3-layer.zip -r python/
   ```<a name="create-function"></a>

**Step 2: Create a AWS Lambda function \(console\)**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Select **Create function**\. For more information, see [Create a Lambda Function with the Console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.

1. Choose the following options\.
   + Choose **Author from scratch** 
   + Enter a value for **Function name**\.
   + For **Runtime** choose **Python 3\.7** or **Python 3\.6**\.
   + For **Choose or create an execution role**, choose **Create a new role with basic Lambda permissions**\. 

1. Choose **Create function** to create the AWS Lambda function\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. From the navigation pane choose **Roles**\.

1. From the resources list, choose the IAM role that AWS Lambda created for you\. The role name is prepended with the name of your Lambda function\.

1. In the **Permissions** tab, choose **Attach policies**\.

1. Add the *AmazonTextractFullAccess* and *AmazonS3ReadOnlyAccess* Policies\.

1. Select **Attach Policy**\.

**Step 3: Create and add a layer \(console\)**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the navigation pane, choose **Layers**\. 

1. Choose **Create layer**\.

1. Enter values for **Name** and **Description**\.

1. For **Code entry type**, choose **Upload \.zip file** and select **Upload**\.

1. In the dialog box, select the zip file \(boto3\-layer\.zip\) that you created in [Step 1: Create an AWS Lambda deployment package](#create-deployment-package)\.

1. For compatible runtimes, choose the runtime that you chose in [Step 2: Create a AWS Lambda function \(console\)](#create-function)\.

1. Choose **Create** to create the layer\.

1. Choose the navigation pane menu icon\.

1. In the navigation pane, choose **Functions**\.

1. In the resources list, select the function you created in [Step 2: Create a AWS Lambda function \(console\)](#create-function)\. 

1. In the **Designer** section of the **Configuration** tab, choose **Layers** \(under your Lambda function name\)\. 

1. In the **Layers** section, choose **Add a layer**\.

1. Choose **Select from list of runtime compatible layers**\.

1. In **Compatible layers**, select the **Name** and **Version** of the layer name and version you created in step 3\.

1. Choose **Add**\.

**Step 4: Add python code \(console\)**

1. In **Designer** choose your function name\.

1. In the function code editor, add the the following to the file **lambda\_function\.py**\. Change the values of `bucket` and `document` to your bucket and document\.

   ```
   import json
   import boto3
   
   def lambda_handler(event, context):
   
       bucket="bucket"
       document="document"
       client = boto3.client('textract')
   
   
   
       #process using S3 object
       response = client.detect_document_text(
           Document={'S3Object': {'Bucket': bucket, 'Name': document}})
   
       #Get the text blocks
       blocks=response['Blocks']
       
       return {
           'statusCode': 200,
           'body': json.dumps(blocks)
       }
   ```

1. Choose **Save** to save your Lambda function\.

**Step 5: Test your Lambda function \(console\)**

1. Select **Test**\.

1. Enter a value for **Event name**\.

1. Choose **Create**\.

1. Choose **Test**\. The lambda function is invoked\. The output is displayed in the **Execution results** pane of the code editor\. The output is a list of [Block](API_Block.md) objects\.

If the AWS Lambda function returns a timeout error, a call to an Amazon Textract API operation might be the cause\. For information about extending the timout period for an AWS Lambda function, see [AWS Lambda Function Configuration](https://docs.aws.amazon.com/lambda/latest/dg/resource-model.html)\.

For information about invoking a Lambda function from your code, see [Invoking AWS Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/invoking-lambda-functions.html)\. 