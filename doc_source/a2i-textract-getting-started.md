# Get Started Using Amazon A2I<a name="a2i-textract-getting-started"></a>

The following steps help you integrate Amazon A2I into an Amazon Textract single\-page document analysis task\. You do the following:

1. Create a human review workflow using the Amazon A2I console \(recommended for new users\) or Amazon A2I API\.

1. To analyze a form and include human review when necessary, use the `AnalyzeDocument` operation and specify the Amazon Resource Name \(ARN\) of the human review workflow\. The response tells you if human review is required\. 

1. Monitor your human loop using the Amazon A2I console and API\. 

1. Review the results of human review in an Amazon S3 bucket where the results are sent\.

To set up an SageMaker notebook instance and use an example notebook see [https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-textract-task-type.html#a2i-task-types-textract-notebook-demo](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-textract-task-type.html#a2i-task-types-textract-notebook-demo) in the *Amazon SageMaker Developer Guide*

**Note**  
This section explains how to create a human review workflow for the Amazon A2I, Amazon Textract task type\. To further customize Amazon A2I and Amazon Textract integration, you can use the Amazon A2I custom task type\. With this option, you provide a custom worker task template and specify the conditions under which a document is sent for human review directly in your application\. For information, see [Use Amazon Augmented AI with Custom Task Types](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-task-types-custom.html) in the *Amazon SageMaker Developer Guide*\.

**Topics**
+ [Create a Human Review Workflow](a2i-textract-create-flow-definition.md)
+ [Analyze the Document](a2i-textract-run-analyze-document.md)
+ [Monitor Human Loop](a2i-textract-monitor-human-loop.md)
+ [View Output Data and Worker Metrics](a2i-textract-view-output-data.md)