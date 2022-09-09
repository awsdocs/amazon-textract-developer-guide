# Query<a name="API_Query"></a>

Each query contains the question you want to ask in the Text and the alias you want to associate\.

## Contents<a name="API_Query_Contents"></a>

 ** Alias **   <a name="Textract-Type-Query-Alias"></a>
Alias attached to the query, for ease of location\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Pattern: `^[a-zA-Z0-9\s!"\#\$%'&\(\)\*\+\,\-\./:;=\?@\[\\\]\^_`\{\|\}~><]+$`   
Required: No

 ** Pages **   <a name="Textract-Type-Query-Pages"></a>
Pages is a parameter that the user inputs to specify which pages to apply a query to\. The following is a list of rules for using this parameter\.  
+ If a page is not specified, it is set to `["1"]` by default\.
+ The following characters are allowed in the parameter's string: `0 1 2 3 4 5 6 7 8 9 - *`\. No whitespace is allowed\.
+ When using \* to indicate all pages, it must be the only element in the list\.
+ You can use page intervals, such as `[“1-3”, “1-1”, “4-*”]`\. Where `*` indicates last page of document\.
+ Specified pages must be greater than 0 and less than or equal to the number of pages in the document\.
Type: Array of strings  
Array Members: Minimum number of 1 item\.  
Length Constraints: Minimum length of 1\. Maximum length of 9\.  
Pattern: `^[0-9\*\-]+$`   
Required: No

 ** Text **   <a name="Textract-Type-Query-Text"></a>
Question that Amazon Textract will apply to the document\. An example would be "What is the customer's SSN?"  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Pattern: `^[a-zA-Z0-9\s!"\#\$%'&\(\)\*\+\,\-\./:;=\?@\[\\\]\^_`\{\|\}~><]+$`   
Required: Yes

## See Also<a name="API_Query_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Query) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Query) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/textract-2018-06-27/Query) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Query) 