# Using Amazon Augmented AI to Add Human Review to Amazon Textract Output<a name="a2i-textract"></a>

Amazon Augmented AI \(Amazon A2I\) is a machine learning \(ML\) service that makes it easy to build workflows for human review of ML analyses\. 

Amazon Textract is integrated with Amazon A2I\. You can use it to route document analysis results that have a low confidence score to human reviewers\. 

You can use Amazon Textract `AnalyzeDocument` API to extract data from forms and the Amazon A2I console\. You can specify the conditions under which Amazon A2I routes predictions to reviewers\. You set conditions based on the confidence threshold of important form keys\. For example, you can send a document to a human reviewer if the key *Name* or its associated value *Jane Doe* was detected with low confidence\.

**Topics**
+ [Core Concepts of Amazon A2I](a2i-textract-core-components.md)
+ [Get Started Using Amazon A2I](a2i-textract-getting-started.md)