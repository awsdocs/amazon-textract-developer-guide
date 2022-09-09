# Monitor Human Loop<a name="a2i-textract-monitor-human-loop"></a>

You can view details about your human loop and stop an active human loop in case of an error using the Amazon A2I console and API\.

## View Human Loop Details<a name="a2i-textract-monitor-human-loop-view"></a>

You can view your human loop status in the Amazon A2I console and by using the [Amazon A2I Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. 

**To find details about your human loop \(console\)**

1. Open the Amazon A2I console at [https://console\.aws\.amazon\.com/a2i](https://console.aws.amazon.com/a2i/) to access the **Human review workflows** page\.

1. Choose the human review workflow that you used also to configure `HumanLoopConfig` in `AnalyzeDocument`\.

1. In the **Human loops** section, choose the human loop whose details you want to view\.

**To find details about your human loop \(API\):**  
Use the Amazon A2I [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html) operation\. Specify the human loop name you used to call `AnalyzeDocument`\. 

The followingSDK for Python \(Boto3\) example calls [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.describe_human_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.describe_human_loop)\.

```
response = client.describe_human_loop(HumanLoopName="human-loop-name")
```

## Stop a Human Loop<a name="a2i-textract-monitor-human-loop-stop"></a>

After a human loop has started, you can stop it using the Amazon A2I console and API\. 

**To stop your human loop \(console\)**

1. Open the Amazon A2I console at [https://console\.aws\.amazon\.com/a2i](https://console.aws.amazon.com/a2i/) to access the **Human review workflows** page\.

1. Choose the human review workflow that you used to configure `HumanLoopConfig` in the `AnalyzeDocument` operation\.

1. In the **Human loops** section, choose the human loop that you want to stop\. 

1. Choose **Stop**\.

**To stop your human loop \(API\)**  
Use the Amazon A2I [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StopHumanLoop.html) operation\. Specify the name of the human loop you used to call `AnalyzeDocument`\. 

The following SDK for Python \(Boto3\) example calls [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.stop_human_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.stop_human_loop)\.

```
response = client.stop_human_loop(HumanLoopName="human-loop-name")
```