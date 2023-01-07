# CloudWatch Metrics for Amazon Textract<a name="cloudwatch-metricsdim"></a>

This section contains information about the Amazon CloudWatch metrics and the *Operation* dimension that are available for Amazon Textract\.

You can also see an aggregate view of Amazon Textract metrics from the Amazon Textract console\.

## CloudWatch Metrics for Amazon Textract<a name="cloudwatch-metrics"></a>

The following table summarizes the Amazon Textract metrics\.


| Metric | Description | 
| --- | --- | 
|  SuccessfulRequestCount  |  The number of successful requests\. The response code range for a successful request is 200 to 299\.  Unit: Count Valid statistics: `Sum,Average`  | 
|  ThrottledCount  |  The number of throttled requests\. Amazon Textract throttles a request when it receives more requests than the limit of transactions per second set for your account\. If the limit set for your account is frequently exceeded, you can request a limit increase\. To change a limit, select the Amazon Textract option in the Service Quotas console\. Unit: Count Valid statistics: `Sum,Average`  | 
|  ResponseTime  |  The time in milliseconds for Amazon Textract to compute the response\.  Units: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/textract/latest/dg/cloudwatch-metricsdim.html) Valid statistics: `Data Samples,Average` The `ResponseTime` metric isn't included in the Amazon Textract metric pane\.  | 
|  ServerErrorCount  |  The number of server errors\. The response code range for a server error is 500 to 599\. Unit: Count Valid statistics: `Sum,Average`  | 
|  UserErrorCount  |  The number of user errors \(invalid parameters, invalid image, no permission, and so on\)\. The response code range for a user error is 400 to 499\. Unit: Count Valid statistics: `Sum,Average`  | 

## CloudWatch Dimension for Amazon Textract<a name="cloudwatch-dimensions"></a>

To retrieve operation\-specific metrics, use the `AWS/Textract` namespace and provide an operation dimension\. For more information about dimensions, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*\. 