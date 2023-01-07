# PageClassification<a name="API_PageClassification"></a>

The class assigned to a Page object detected in an input document\. Contains information regarding the predicted type/class of a document's page and the page number that the Page object was detected on\.

## Contents<a name="API_PageClassification_Contents"></a>

 ** PageNumber **   <a name="Textract-Type-PageClassification-PageNumber"></a>
 The page number the value was detected on, relative to Amazon Textract's starting position\.  
Type: Array of [Prediction](API_Prediction.md) objects  
Required: Yes

 ** PageType **   <a name="Textract-Type-PageClassification-PageType"></a>
The class, or document type, assigned to a detected Page object\. The class, or document type, assigned to a detected Page object\.  
Type: Array of [Prediction](API_Prediction.md) objects  
Required: Yes

## See Also<a name="API_PageClassification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/PageClassification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/PageClassification) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/PageClassification) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/PageClassification) 