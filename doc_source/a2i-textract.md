# Using Amazon Augmented AI \(Preview\) with Amazon Textract<a name="a2i-textract"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We recommend that you use this tool only with test datasets, and not in production environments\. | 

Amazon Augmented AI \(Amazon A2I\) enables you to build the workflows that are required for human review of machine learning predictions\. 

Amazon Textract is directly integrated with Amazon A2I so that you can easily get low\-confidence predictions from Amazon Textract reviewed by humans\. You can use Amazon Textract’s `AnalyzeDocument` API for form data extraction and the Amazon A2I console to specify the conditions under which Amazon A2I routes predictions to reviewers\. The conditions are set based on the confidence threshold of important key forms\. 

If you specify a confidence threshold, Amazon A2I routes only those predictions that fall below the threshold for human review\. You can adjust these thresholds at any time to achieve the right balance between accuracy and cost\-effectiveness\. This can help you implement audits to monitor the prediction accuracy regularly\. 

With Amazon A2I, you can use a pool of reviewers within your own organization\. You can also access the workforce of over 500,000 independent contractors who are already performing machine learning tasks through Amazon Mechanical Turk\. Another option is to use workforce vendors that are prescreened by AWS for quality and adherence to security procedures\. Amazon A2I also provides reviewers a web interface that consists of all the instructions and tools that they need to complete their review tasks\.

The following steps walk you through how to set up Amazon A2I with Amazon Textract's single\-page document analysis\. You create a flow definition with the Amazon A2I API that has the conditions that trigger human review\. Then, you pass the flow definition's Amazon Resource Name \(ARN\) to `AnalyzeDocument`\. In the `AnalyzeDocument` response, you can see if human review is required\. The results of human review are available in an Amazon S3 bucket that is set by the flow definition\.

**Running AnalyzeDocument with Amazon A2I \(Preview\)**

1. Complete the prerequisites that are listed in [Getting Started with Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-getting-started.html) in the *Amazon SageMaker Documentation*\.

   Additionally, remember to set up your IAM permissions as in the page [ Permissions and Security in Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-permissions-security.html) in the *Amazon SageMaker Documentation*\.

1. Follow the instructions for [Creating a Human Review Workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-getting-started.html) in the *Amazon SageMaker Documentation*\.

   A human review workflow manages the analysis of a document\. It holds the conditions that trigger a human review, the work team that the document is sent to, the UI template that the work team uses, and the Amazon S3 bucket that the work team's results are sent to\.

   Within your `CreateFlowDefinition` call, you need to set the `HumanLoopRequestSource` to "AWS/Textract/AnalyzeDocument/Forms/V1"\. Then you need to decide what conditions trigger your document for human review\.

   For Amazon Textract, the ConditionType used is ImportantFormKeyConfidenceCheck, which has the condition parameters ImportantFormKey and ImportantFormKeyAliases\. Additionally, the parameters KeyValueBlockConfidence and WordBlockConfidence can each be set to "less than", "equal to", or "greater than"\. 

   These parameters enable you to look for specific keys in a document\. Then, based on their confidence evaluation, you can decide whether to send the document to human review or not\. You can also use logical operators, such as "And", "Or", or "Not" to create complex conditions\. If you set no conditions, every document analyzed is sent to human review\. To send all documents that are analyzed to human review, you can set the ImportantFormKey to "\*" and the ConfidenceGreaterThan parameter to 0\.

   The following code is a partial call of `CreateFlowDefinition`\. This sends a document for human review if Amazon Textract is less than 90% confident on the `KeyValueBlockConfidence` for a document's mailing address or phone number form\. It also sends the document for review if it has less than a 100% `WordBlockConfidence`\.

   ```
       def create_flow_definition():
       '''
       Creates a Flow Definition resource
   
       Returns:
       struct: FlowDefinitionArn
       '''
       humanLoopActivationConditions = json.dumps(
           {
               "Conditions": [
                   {
                     "Or": [
                       {
                           "ConditionType": "ImportantFormKeyConfidenceCheck",
                           "ConditionParameters": {
                               "ImportantFormKey": "Mailing Address",
                               "ImportantFormKeyAliases": ["Mailing Address:"],
                               "KeyValueBlockConfidenceLessThan": 90,
                               "WordBlockConfidenceLessThan": 100
                           }
                       },
                       {
                           "ConditionType": "ImportantFormKeyConfidenceCheck",
                           "ConditionParameters": {
                               "ImportantFormKey": "Phone Number",
                               "ImportantFormKeyAliases": ["Phone Number:"],
                               "KeyValueBlockConfidenceLessThan": 90,
                               "WordBlockConfidenceLessThan": 100
                           }
                       }
                     ]
                  }
               ]
           }
       )
   ```

   `CreateFlowDefinition` returns a `FlowDefinitionArn`, which you use in the next step when you call `AnalyzeDocument`\.

   For more information see [CreateFlowDefinition](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateFlowDefinition.html) in the * Amazon SageMaker API Reference*\.

1. Set the `HumanLoopConfig` parameter when you call `AnalyzeDocument`, as in [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)\.

   1. Within the `HumanLoopConfig` parameter, set the `FlowDefinitionArn` to the ARN of the flow definition that you created in step 2\.

   1. Set your `HumanLoopName`\. This should be unique within a Region and must be lowercase\.

   1. \(Optional\) You can use `DataAttributes` to set whether the document that you passed to Amazon Textract is free of adult content or personally identifiable information\. You must set this parameter as free of personally identifiable information in order to send the document to Amazon Mechanical Turk\.

   The following is an example of what a call to `AnalyzeDocument` looks like with the `HumanLoopConfig` set in Python\.

   ```
            client.analyze_document(
            Document={'S3Object': {'Bucket': bucket, 'Name': document }},
             HumanLoopConfig={'FlowDefinitionArn':string,'HumanLoopName':string},
            FeatureTypes=["FORMS"])
   ```

1. Run `AnalyzeDocument`\.

   When you run `AnalyzeDocument` with `HumanLoopConfig` enabled, Amazon Textract calls the Amazon A2I Runtime API operation `StartHumanLoop`\. This operation takes the response from `AnalyzeDocument` and checks it against the flow definition's conditions\. 

   If it meets the conditions for review, it returns a `HumanLoopArn`\. This means that the members of the work team that you set in your flow definition now can review the document\. Calling the Amazon SageMaker API `DescribeHumanLoop` provides information about the outcome of the loop\. For more information see [ DescribeHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_DescribeHumanLoop.html) in the * Amazon SageMaker API Reference*\.

   After the document has been reviewed, you can see the results in the bucket that is specified in your flow definition's output path\. Amazon A2I will also notify you with Amazon CloudWatch Events when the review is complete\. To see what events to look for, see [CloudWatch Events](https://docs.aws.amazon.com/sagemaker/latest/dg/augmented-ai-cloudwatch-events.html) in the *Amazon SageMaker Documentation*\.

   For more information, see [Getting Started with Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-getting-started.html) in the *Amazon SageMaker Documentation*\.
