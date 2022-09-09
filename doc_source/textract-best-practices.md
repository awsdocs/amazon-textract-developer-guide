# Best Practices for Amazon Textract<a name="textract-best-practices"></a>

Amazon Textract uses machine learning to read documents as a person would\. It extracts text, tables, and forms from documents\. Use the following best practices to get the best results from your documents\.

## Provide an Optimal Input Document<a name="optimal-document"></a>

The following is a list of a few ways that you can optimize your input documents for better results\.
+ Ensure that your document text is in a language that Amazon Textract supports\. Currently, Amazon Textract supports English, Spanish, German, Italian, French, and Portuguese\.
+ Provide a high quality image, ideally at least 150 DPI\.
+ If your document is already in one of the file formats that Amazon Textract supports \(PDF, TIFF, JPEG, and PNG\), don't convert or downsample the document before uploading it to Amazon Textract\.

For the best results when extracting text from tables in documents, ensure that:
+ Tables in your document are visually separated from surrounding elements on the page\. For example, the table isn't overlaid onto an image or complex pattern\.
+ Text within the table is upright\. For example, the text isn't rotated relative to other text on the page\.

When extracting text from tables, you might see inconsistent results when: 
+ Merged table cells that span multiple columns\.
+ Tables with cells, rows, or columns that are different from other parts of the same table\.

We recommend using [text detection](how-it-works-detecting.md) as a workaround\.

## Use Confidence Scores<a name="confidence-score"></a>

You should take into account the confidence scores returned by Amazon Textract API operations and the sensitivity of their use case\. A confidence score is a number between 0 and 100 that indicates the probability that a given prediction is correct\. It helps you make informed decisions about how you use the results\. 

In applications that are sensitive to detection errors \(false positives\), enforce a minimum confidence score threshold\. The application should discard results below that threshold or flag situations as requiring a higher level of human scrutiny\.

The optimal threshold depends on the application\. For archival purposes, such as documenting handwritten notes, it might be as low as 50%\. Business processes involving financial decisions might require thresholds of 90% or higher\.

## Consider Using Human Review<a name="review"></a>

Also consider incorporating human review into your workflows\. This is especially important for sensitive applications, such as business processes that involve financial decisions\.