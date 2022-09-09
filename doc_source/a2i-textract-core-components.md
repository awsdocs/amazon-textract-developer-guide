# Core Concepts of Amazon A2I<a name="a2i-textract-core-components"></a>

Review the following terms to familiarize yourself with the core concepts of Amazon A2I\. 

## Human Review Activation Conditions<a name="a2i-textract-activation-conditions"></a>

You can use Amazon A2I *activation conditions* to specify when a document is sent to humans for review and the form\-content that workers are asked to review\. 

For example, you can set an activation condition to have Amazon Textract route forms to Amazon A2I when an important key is detected with low confidence, like *Phone Number*\. In this example, human reviewers are asked to review the *Phone Number* field and value associated detected by Amazon Textract\.

You can use the following activation conditions to specify when forms gets sent to humans for review:
+ Trigger a human review for specific form keys based on the form key confidence score\. Human reviewers are asked to review these form keys and associated values\. 
+ Trigger a human review when specific form keys are missing\. Human reviewers are asked to identify these form keys and associated values\. 
+ Trigger human review for all form keys identified by Amazon Textract with confidence scores in a specified range\.
+ Randomly send a sample of forms to humans for review\. Human reviewers are asked to review all form keys and values detected by Amazon Textract\.

When your activation condition depends on form key confidence scores, you can use two types of prediction\-confidence thresholds to trigger human review:
+ **Identification confidence** – The confidence score for key\-value pairs detected within a form\.
+ **Qualification confidence** – The confidence score for text contained within a key\-value pair in a form\.

If you specify a confidence threshold, Amazon A2I routes only those predictions that fall within the threshold to human reviewers\. You can adjust these thresholds at any time to achieve the right balance between accuracy and cost\-effectiveness\. This can help you implement audits to regularly monitor prediction accuracy\. 

**Note**  
You can further customize the conditions under which documents are sent to humans for review using the Amazon A2I custom task type\. With this task type, you specify conditions for a human review directly in your application\. For information , see [Use Amazon Augmented AI with Custom Task Types](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-task-types-custom.html) in the Amazon SageMaker Developer Guide\.

## Human review workflow \(flow definition\)<a name="a2i-textract-core-components-human-review-workflow"></a>

You use a *human review workflow*, also referred to as a *flow definition*, to specify resources used to create your human review workflow, and to specify your activation conditions\. 

The resources that you specify are:
+ An IAM role with permission to call Amazon A2I API operations
+ An Amazon S3 bucket where you want to store the output of the human review
+ Your human *work team* 
+ A *worker task template* which includes instructions and examples to help workers complete review task

You also use the human review workflow to specify activation conditions\. For more information, see [Human Review Activation Conditions](#a2i-textract-activation-conditions)\.

You can use a single human review workflow to create multiple [Human loops](#a2i-textract-core-components-human-review-workflow-human-loop)\.

You can create a human review workflow in the SageMaker console or with the SageMaker API\. For information, see [Create a Human Review Workflow](a2i-textract-create-flow-definition.md)\.

**Worker task template**  
You use a *worker task template* to create a worker UI that is used for your human review tasks\. 

The worker UI displays your documents and worker instructions\. It also provides tools that workers use to complete your tasks\. 

You can use the SageMaker console to configure your worker task template when you create a human review workflow\. For information, see [Create a Human Review Workflow](a2i-textract-create-flow-definition.md)\.

**Work team**  
A *work team* is a group of human workers that you send your human review tasks to\.

When you create a human review workflow, you specify a single work team\. 

With Amazon A2I, you can use a pool of reviewers within your own organization\. You can also access the workforce comprised of over 500,000 independent contractors who are already performing machine learning tasks through Amazon Mechanical Turk\. Another option is to use workforce vendors that are prescreened by AWS for quality and adherence to security procedures\.

Amazon A2I also provides reviewers with a web interface that consists of all the instructions and tools that they need to complete their review tasks\.

For each type of workforce \(private, vendor, and Mechanical Turk\), you can create multiple work teams\. You can use each work team in multiple human review workflows\. To learn to create a workforce and work teams, see [Create and Manage Workforces](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management.html) in the Amazon SageMaker Developer Guide\.

**Important**  
Click [here](https://aws.amazon.com/compliance/services-in-scope/) to see the compliance programs that cover Amazon Augmented AI at this time\. If you use Amazon Augmented AI in conjunction with other AWS services \(such as Amazon Rekognition and Amazon Textract\), please note that Amazon Augmented AI may not be in scope for the same compliance programs as those other services\. You are responsible for how you use Amazon Augmented AI, including understanding how the service will process or store customer data, and any impact on the compliance of your data environment\. You should discuss your workload objectives and goals with your AWS account team; they can help you evaluate whether the service is a good fit for your proposed use case and architecture\.  
Currently, Amazon Augmented AI is PCI compliant except for public and vendor workforce cases\.  
For information regarding Amazon Augmented AI HIPAA compliance, click [here](https://aws.amazon.com/blogs/machine-learning/amazon-augmented-ai-is-now-a-hipaa-eligible-service/)\.

## Human loops<a name="a2i-textract-core-components-human-review-workflow-human-loop"></a>

You use a human loop to create a human review task\. 

You assign a human review task to a worker on your human worker team\. The worker is asked to review key\-value pairs detected by Amazon Textract in your input document that you specified in your activation conditions\.

For example, say a picture falls between fifty and sixty percent confidence that it contains an apple\. This falls within your confidence threshold for human review and is sent to a worker\. The worker checks the document for an apple, marking it as containing or not containing an apple\. Then, Amazon A2I sends the document back into the workflow\.

When you call Amazon Textract `AnalyzeDocument` and specify a human review workflow \(flow definition\) and human loop name, a human review task is created when the activation conditions specified in your human review workflow are met\. These tasks are created using the resources you specify in your human review workflow\. 