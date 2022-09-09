# View Output Data and Worker Metrics<a name="a2i-textract-view-output-data"></a>

When a human review task is completed by a worker, Amazon A2I stores your output data in the Amazon S3 bucket that you specified in your human review workflow\.

If you use a private workforce, your output data contains worker metadata that you can use to track individual worker activity\. 

## Find Output Data in Amazon S3<a name="a2i-textract-output-data"></a>

Amazon A2I uses your human review workflow name as prefix to the name of the file that stores the output data of human loops created using that human review workﬂow\.

The path to a human loop output uses the following pattern in which `YYYY/MM/DD/hh/mm/ss` represents the human loop creation date with year \(`YYYY`\), month \(`MM`\), and day \(`DD`\) and the creation time with hour \(`hh`\), minute \(`mm`\), and second \(`ss`\)\. 

```
s3://output-bucket-specified-in-flow-definition/flow-definition-name/YYYY/MM/DD/hh/mm/ss/human-loop-name/output.json
```

To see the output for a human loop, use the Amazon A2I console\.

**To see human loop output**

1. Open the Amazon A2I console at [https://console\.aws\.amazon\.com/a2i](https://console.aws.amazon.com/a2i/) to access the **Human review workflows** page\.

1. Choose the human review workflow that you use to configure `HumanLoopConfig` in `AnalyzeDocument`\.

1. In the **Human loops** section, choose the human loop whose output you want to review\. 

1. Under **Output location**, choose the link to the output data\.

## Track Private Worker Activity<a name="a2i-textrct-worker-id-private"></a>

When you use a private workforce for human review tasks, the output data includes following information about the worker who completed the review:
+ The `workerId`\. 
+ In `workerMetadata`:
  + `identityProviderType` – The service used to manage the private workforce\. 
  + `issuer` – The Amazon Cognito user pool or OIDC Identity Provider \(IdP\) issuer associated with the work team assigned to this human review task\.
  + `sub` – A unique identifier that refers to the worker\. If you created a workforce using Amazon Cognito, you can retrieve details about this worker \(such as the name or username\) using this ID using Amazon Cognito\. To learn how, see [Managing and Searching for User Accounts](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-manage-user-accounts.html#manage-user-accounts-searching-user-attributes) in [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/)\.

The following is an example of the output you might see if you used Amazon Cognito to create a private workforce\.

```
"workerId": "a12b3cdefg4h5i67",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Cognito",
                    "issuer": "https://cognito-idp.aws-region.amazonaws.com/aws-region_123456789",
                    "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
```

 The following is an example of the output you might see if you used your own OIDC IdP to create a private workforce:

```
"workerId": "a12b3cdefg4h5i67",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Oidc",
                    "issuer": "https://example-oidc-ipd.com/adfs",
                    "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
```

To learn more about using private workforces, see [ Use a Private Workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private.html) in the *Amazon SageMaker Developer Guide*\.