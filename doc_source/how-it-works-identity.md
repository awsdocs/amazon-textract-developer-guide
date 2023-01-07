# Analyzing Identity Documents<a name="how-it-works-identity"></a>

 Amazon Textract can extract relevant information from passports, driver licenses, and other identity documentation issued by the US Government using the AnalyzeID API\. With Analyze ID, businesses can quickly, and accurately extract information from IDs such as U\.S\. driver licenses, and passports that have different template or format\. AnalyzeID API returns three categories of data types: 
+  Key\-value pairs available on ID such as Date of Birth, Date of Issue, ID \#, Class, and Restrictions\. 
+  Implied fields on the document that may not have explicit keys associated with them such as Name, Address, and Issued By\. 
+  The text of the document, the same as would be returned by document text detection\.

Key names are standardized within the response\. For example, if your driver license says LIC\# \(license number\) and passport says Passport No, Analyze ID response will return the standardized key as “Document ID” along with the raw key \(e\.g\. LIC\#\)\. This standardization lets customers easily combine information across many IDs that use different terms for the same concept\.

![\[Descriptive text here: A mock driver's license from the state of Massachusetts. The name of the individual who owns the license is Maria Garcia. The ISS field has a value of 03/18/2018. The Number field has a value of 736HDV7874JSB. The EXP field has a value of 01/20/2028. The DOB field has a value of 03/18/2001. The CLASS field has a value of D. The REST field is NONE. The END field is NONE. The address on the ID is 100 Market Street, Bigtown, MA, 02801. The EYES field is BLK, the SEX field is F, the HGT field is 4-6'', the DD field is 03/12/2019, and the REV field is 03/12/2017.\]](http://docs.aws.amazon.com/textract/latest/dg/images/passport2.png)

 Analyze ID returns information in the structures called `IdentityDocumentFields`\. These are `JSON` structures containing two pieces of information: the normalized Type and the Value associated with the Type\. These both also have a confidence score\. For more information, see [Identity Documentation Response Objects](identitydocumentfields.md)\. For more information regarding the text detection returned by Analyze ID, see [Text Detection and Document Analysis Response Objects](how-it-works-document-layout.md)

 You can use synchronous operations to analyze a driver's license or passport\. To analyze these documents, you use the AnalyzeID operation and pass an identity document to it\. `AnalyzeID` returns the entire set of results\. For more information, see [Analyzing Identity Documentation with Amazon Textract](analyzing-document-identity.md)\. 

**Note**  
 Some identity documents, such as driver's licenses, have two sides\. You can pass the front and back images of driver licenses as separate images within the same Analyze ID API request\. 