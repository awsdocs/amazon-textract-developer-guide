# Prediction<a name="API_Prediction"></a>

Contains information regarding predicted values returned by Amazon Textract operations, including the predicted value and the confidence in the predicted value\.

## Contents<a name="API_Prediction_Contents"></a>

 ** Confidence **   <a name="Textract-Type-Prediction-Confidence"></a>
Amazon Textract's confidence in its predicted value\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** Value **   <a name="Textract-Type-Prediction-Value"></a>
The predicted value of a detected object\.  
Type: String  
Pattern: `.*\S.*`   
Required: No

## See Also<a name="API_Prediction_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Prediction) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Prediction) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Prediction) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Prediction) 