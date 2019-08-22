# Best Practices for Amazon Textract<a name="textract-best-practices"></a>

Amazon Textract uses machine learning to read documents as a person would\. It extracts text, tables, and forms from documents\. Use the following best practices to get the best results from your documents\.

## Provide an Optimal Input Document<a name="optimal-document"></a>
+ Ensure that your document text is in a language that Amazon Textract supports\. Currently, Amazon Textract only supports English\.
+ Provide a high quality image, ideally at least 150 DPI\.
+ If your document is already in one of the file formats that Amazon Textract supports \(PDF, JPEG, and PNG\), don't convert or downsample the document before uploading it to Amazon Textract\.

Amazon Textract table extraction works best under the following conditions\.
+ The tables in your document are visually separated from surrounding elements on the page\. For example, the table isn't overlaid onto an image or complex pattern\.
+ The text within the table is upright\. For example, the text isn't rotated relative to other text on the page\.

You might see inconsistent results with the following conditions\. We recommend using [text detection](how-it-works-detecting.md) as a workaround\.
+ Merged table cells that span multiple columns\.
+ Tables with cells, rows, or columns that are different from other parts of the same table\.

## Use Confidence Scores<a name="confidence-score"></a>

You should take into account the confidence scores returned by Amazon Textract API operations and the sensitivity of their use case\. A confidence score is a number between 0 and 100 that indicates the probability that a given prediction is correct\. It enables you to make informed decisions on how you want to use the results\. 

You should enforce a minimum confidence score threshold in applications that are sensitive to detection errors \(false positives\)\. The application should discard results below that threshold or apply a higher level of human scrutiny\. The optimal threshold depends on the application\. For archival purposes it might be as low as 50%\. Business processes involving financial decisions might require thresholds of 90% or higher\.

## Consider Using Human Review<a name="review"></a>

Also consider incorporating human review into your workflows\. This is especially important for sensitive applications such as business processes that involve financial decisions\.