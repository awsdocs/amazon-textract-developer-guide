# Analyze the Document<a name="a2i-textract-run-analyze-document"></a>

To incorporate Amazon A2I into an Amazon Textract document analysis workflow, you configure `HumanLoopConfig` in the [https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html](https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html) operation\.

In `HumanLoopConfig`, you specify your human review workflow \(flow definition\) ARN in `FlowDefinitionArn`, and give your human loop a name in `HumanLoopName`\.

------
#### [  Analyze the Document \(AWS SDK for Python \(Boto3\)\) ]

The following example uses the SDK for Python \(Boto3\) to call `analyze_document` in us\-west\-2\. Replace the *red, italicized* text with your resources\. For more information, see [analyze\_document](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/textract.html#Textract.Client.analyze_document) in the *AWS SDK for Python \(Boto\) API Reference*\.

```
client.analyze_document(Document={'S3Object': {"Bucket": "DOC-EXAMPLE-BUCKET", "Name": "document-name.png"}},
         HumanLoopConfig={"FlowDefinitionArn":"arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
                          "HumanLoopName":"human-loop-name",
                          "DataAttributes":{"ContentClassifiers":["FreeOfPersonallyIdentifiableInformation"|"FreeOfAdultContent",]}},
         FeatureTypes=["FORMS"])
```

------
#### [  Analyze the Document \(AWS CLI\) ]

The following example uses the AWS CLI to call `analyze_document`\. These examples are compatible with AWS CLI version 2\. The first is shorthand syntax, the second in JSON syntax\. For more information, see [analyze\-document](https://docs.aws.amazon.com/cli/latest/reference/textract/analyze-document.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

```
aws textract analyze-document \
     --document '{"S3Object":{"Bucket":"bucket_name","Name":"file_name"}}' \
     --human-loop-config HumanLoopName="test",FlowDefinitionArn="arn:aws:sagemaker:eu-west-1:xyz:flow-definition/hl_name",DataAttributes='{ContentClassifiers=["FreeOfPersonallyIdentifiableInformation","FreeOfAdultContent"]}'\
     --feature-types '["FORMS"]'
```

```
aws textract analyze-document \
     --document '{"S3Object":{"Bucket":"bucket_name","Name":"file_name"}}' \
     --human-loop-config \
          '{"HumanLoopName":"test","FlowDefinitionArn":"arn:aws:sagemaker:eu-west-1:xyz:flow-definition/hl_name","DataAttributes": {"ContentClassifiers":["FreeOfPersonallyIdentifiableInformation","FreeOfAdultContent"]}}' \
     --feature-types '["FORMS"]'
```

------

**Note**  
 Avoid whites spaces in your \-\-human\-loop\-config parameter, as this can cause processing issues for your code\. 

The response to this request contains [HumanLoopActivationOutput](https://docs.aws.amazon.com/textract/latest/dg/API_HumanLoopActivationOutput.html), which indicates whether a human loop was created, and if it was, why\. If a human loop was created, this object also contains the `HumanLoopArn`\.

For more information about and examples using the `AnalyzeDocument` operation, see [Analyzing Document Text with Amazon Textract](analyzing-document-text.md)\.