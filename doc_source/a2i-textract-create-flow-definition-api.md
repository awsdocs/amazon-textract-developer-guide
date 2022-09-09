# Create a Human Review Workflow \(API\)<a name="a2i-textract-create-flow-definition-api"></a>

You can create a human review workflow, or a *flow definition*, using the Amazon A2I, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html) operation\. 

For this example, you can either use your own document in Amazon S3, or you can download [this sample document](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2020/04/17/sample-document-final.png) and store it in your S3 bucket\.

Make sure your Amazon S3 bucket is in the same AWS Region that you plan to use to call `AnalyzeDocument`\. To create a bucket, follow the instructions in [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

**Prerequisites**

To use the Amazon A2I API to create a human review workflow, you must complete the following prerequisites:
+ Configure an IAM role with permission to call both Amazon A2I and Amazon Textract API operations\. To get started, you can attach the AWS policies, AmazonAugmentedAIFullAccess, and AmazonTextractFullAccess to an IAM role\. Record the IAM role Amazon Resources Name \(ARN\) because you will need it later\.

  For more granular permissions when using Amazon Textract, see [Amazon Textract Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\. For Amazon A2I, see [Permissions and Security in Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-permissions-security.html) in the *Amazon SageMaker Developer Guide*\.
+ Create a private work team and record the workteam ARN\. If you are a new user of Amazon A2I, follow the instructions in [Step 1: Create a Work Team \(Console\)](a2i-textract-create-flow-definition-console.md#a2i-textract-create-flow-definition-workteam)\.
+ Create a worker task template\. Follow the instructions in [Create a Worker Task Template](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-worker-template-console.html#a2i-create-worker-template-console) to create a template using the Amazon A2I console\. When you are creating the template, choose **Textract\-Form Extraction** for **Template type**\. In the template, replace `s3_arn` with the Amazon S3 ARN of your document\. Add additional worker instructions in `<full-instructions header="Instructions"></full-instructions>`\. 

  If you want to preview your template, make sure your IAM role has the permissions described in [Enable Worker Task Template Previews](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-permissions-security.html#permissions-for-worker-task-templates-augmented-ai)\.

  After you've created your template, record the worker task template ARN\.

You use the resources you created in **Prerequisites** to configure your `CreateFlowDefinition` request\. In this request, you also specify activation conditions in JSON format\. To learn how to configure your activation conditions, see [Use Human Loop Activation Conditions JSON Schema with Amazon Textract](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-json-humantaskactivationconditions-textract-example.html)\.

## Creating a Human Review Workflow \(AWS SDK for Python \(Boto3\)\)<a name="a2i-textract-create-flow-definition-api-example"></a>

To use this example, replace the *red* text with your specifications and resources\.

First, encode your activation conditions into a JSON object using the following code\. This triggers a human review if Amazon Textract returns a confidence score that is less than 99 for *Mail Address* and its value, or if it returns a confidence score less than 90 for any key\-value pair detected in the document\. If you are using the sample document provided in this example, these activation conditions create a human review task\.

```
import json

humanLoopActivationConditions = json.dumps("{
                "Conditions": [
                    {
                        "ConditionType": "ImportantFormKeyConfidenceCheck",
                        "ConditionParameters": {
                            "ImportantFormKey": "Mail Address",
                            "KeyValueBlockConfidenceLessThan": 99,
                            "WordBlockConfidenceLessThan": 99
                        }
                    },
                    {
                        "ConditionType": "ImportantFormKeyConfidenceCheck",
                        "ConditionParameters": {
                            "ImportantFormKey": "*",
                            "KeyValueBlockConfidenceLessThan": 90,
                            "WordBlockConfidenceLessThan": 90
                        }
                    }
                ]
            }"
)
```

Use `humanLoopActivationConditions` to configure the `create_flow_definition` request\. The following example uses the SDK for Python \(Boto3\) to call [https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateFlowDefinition](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateFlowDefinition) in us\-west\-2 AWS Region\. It specifies using a private work team\. 

```
response = client.create_flow_definition(
    FlowDefinitionName='string',
    HumanLoopRequestSource={
         'AwsManagedHumanLoopRequestSource': "AWS/Textract/AnalyzeDocument/Forms/V1"
    }, 
    HumanLoopActivationConfig={
        'HumanLoopActivationConditionsConfig': {
            'HumanLoopActivationConditions': humanLoopActivationConditions
        }
    },
    HumanLoopConfig={
        'WorkteamArn': "arn:aws:sagemaker:us-west-2:111122223333:workteam/private-crowd/work-team-name",
        'HumanTaskUiArn': "arn:aws:sagemaker:us-west-2:111122223333:human-task-ui/worker-task-template-name",
        'TaskTitle': "Add a task title",
        'TaskDescription': "Describe your task",
        'TaskCount': 1,
        'TaskAvailabilityLifetimeInSeconds': 3600,
        'TaskTimeLimitInSeconds': 86400,
        'TaskKeywords': ["Document Review", "Content Review"]
        }
    },
    OutputConfig={
        'S3OutputPath': "s3://DOC-EXAMPLE-BUCKET/prefix/",
    },
    RoleArn="arn:aws:iam::111122223333:role/role-name"
)
```