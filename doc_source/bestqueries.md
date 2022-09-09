# Best Practices for Queries<a name="bestqueries"></a>

## General Best Practices for Queries<a name="generalbestqueries"></a>

## Extracting Cells from Tables<a name="w111aac31c11b5"></a>

Construct a query that contains words from both row header and column header\.

Examples, for the image below

Query 1: What date was the 2nd dose administered?

Answer 1: 2/8/2021

Query 2: Who is the manufacturer of the 1st dose?

Answer 2: Pfizer

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/best-practice-cells.png)

## Extracting Tables using Queries<a name="w111aac31c11b7"></a>

Extraction of entire tables or whole rows or columns of information using queries is not supported\.

## Long Answers<a name="w111aac31c11b9"></a>

Long answers increase response latency and can lead to timeouts\. Try to ask questions that respond with answers to less than 100 words\.

## Passing Only Hints<a name="w111aac31c11c11"></a>

Passing only the the key name as the question will work when trying to extract standard key\-value pairs from a form\. We recommend framing full questions for all other extraction use cases\.

Examples, for the image below

Query 1: Borrower's Name\.

Answer 1: Carlos Salazar

Query 2: Social Security Number\.

Answer 2: 999\-99\-9999

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/best-practice-hints.png)

## Genral Phrasing of Questions<a name="w111aac31c11c13"></a>

Where possible, use words from the document to construct the query\.
+ While Queries tries to do acronym / synonym matching for some common industry terms \(SSN vs Tax ID vs Social Security Number\), using language directly from the document will in general improve results\.
+ Example: If the document says “job progress”, try to avoid calling it using other variations like “ project progress” or “program progress” or “job status”

In general, ask a natural language question that starts with "What is / Where is / Who is\.\."\. The exception to this rule is when trying to extract standard key\-value pairs in which case you can pass the key name as a a query\.

Avoid ill\-formed or grammatically incorrect questions since these could result in unexpected answers\. 
+ Example of ill\-formed Query: When?
+ Example of well formed Query: When was the first vaccine dose administered?

Be as specific as possible\. Some examples follow\.
+ When the document contains multiple sections \(e\. g\. “Borrower” and “Co\-Borrower”\) and both sections have a field called SSN, ask: “What is the SSN for Borrower?” and “What is the SSN for Co\-Borrower?”
+ When the document has multiple date related fields, be specific in the query language and ask “what is the date the document was signed on? or ”what is the the date of birth of the application“\. Avoid asking ambiguous questions like ”What is the date?“

If you know the layout of the document beforehand, giving location hints improve accuracy of results\. For example “What is the date at the top?” or ask “What is the date on the left?”, “What is the date at the bottom?

## Example Queries<a name="example-queries"></a>

If you are having trouble with forming questions, this section has examples for different common document types\.

## Paystubs<a name="w111aac31c11c19b1"></a>
+ What is the Pay Period Start Date?
+ What is the Pay Period End Date?
+ What is the Pay Date?
+ What is the Employee Name?
+ What is the company Name?
+ What is the Current Gross Pay?
+ What is the YTD Gross Pay?
+ What is the regular hourly rate?
+ What is the holiday rate?

## Bank Statements<a name="w111aac31c11c19b3"></a>
+ What is the Customer/Account Name?
+ What is the Bank Name?
+ What is the Account Number?
+ What is the Account Type?
+ What is the Beginning Balance?
+ What is the Total Deposits?
+ What is the Total Withdrawal?
+ What is the Ending balance?
+ What is the Average balance?

## Checks<a name="w111aac31c11c19b5"></a>
+ What is the check number?
+ What is the date?
+ What is the check amount?
+ What is the dollar amount?
+ Who is the payee of this check?
+ What is the bank routing number?

## Credit Card Statements<a name="w111aac31c11c19b7"></a>
+ What is the customer name?
+ What is the credit card company name?
+ What is the account number?
+ What is the payment due date?
+ What is the new balance?
+ What is the minimum payment due?
+ What is the past due amount?
+ What is the previous balance?

## W2 Form<a name="w111aac31c11c19b9"></a>
+ What is the form year?
+ What is the form type?
+ What is the Employee SSN?
+ What is the Employer Name?
+ What is wages, tips, other compensation amount?
+ What is the Federal Income Tax withheld amount?
+ What is the social security wages amount?
+ What is the social security Taxes withheld amount?
+ What is the value type in Box 12a?
+ What is the value amount in Box 12a?
+ What is the value type in Box 12b?
+ What is the value amount in Box 12b?
+ What is the value type in Box 12c?
+ Is Box 13 Statutory employee selected?
+ s Box 13 Retirement plan selected?
+ Is Box 13 Third\-party sick pay selected?

## 1099DIV Form<a name="w111aac31c11c19c11"></a>
+ What is the form year?
+ What is the form type?
+ Is the document corrected?
+ Is the document void?
+ What is payer name?
+ What is payer address?
+ What is the payer TIN?
+ What is the account number?
+ What is the amount in box 1a?
+ How much is the total ordinary divdends?
+ What is the amount in box 1b?
+ How much is state tax withheld?

## 1099INT Form<a name="w111aac31c11c19c13"></a>
+ What is the form year?
+ What is the form type?
+ Is the document corrected?
+ Is Payer's RTN?
+ What is the amount in box 1?
+ How much is interest income?
+ What is the amount in box 2?
+ How much is early withdrawal penalty?
+ What is the amount in box 17?
+ How much is state tax withheld?

## 1099MISC Form<a name="w111aac31c11c19c15"></a>
+ What is the Form year?
+ What is the Form type?
+ Is the document corrected?
+ Is Payer's TIN?
+ What is the account number?
+ What is the amount in box 1?
+ How much is rents?
+ What is the amount in box 2?
+ How much is royalties??
+ What is the amount in box 17?
+ How much is state income?

## 1099R Form<a name="w111aac31c11c19c17"></a>
+ What is the form year?
+ What is the form type?
+ Is the document corrected?
+ Is the FATCA filling requirement checked?
+ Is box 12 checked?
+ What is the account number?
+ What is the amount in box 1?
+ How much is the gross distribution?
+ What is the amount in 2a?
+ How much is the taxable amount
+ What is the amount in box 19?
+ How much is local distribution

## Vaccination Cards<a name="w111aac31c11c19c19"></a>
+ What is the date for the 1st dose?
+ Which clinic site or health care professional was the 2nd dose administrated?
+ What is the lot number for 1st dose ?
+ Who is the manufacturer for the 2nd dose?
+ What is the first name?
+ What is the lot number for the 3rd dose?

## Fannie Mae 1003 Form<a name="w111aac31c11c19c21"></a>
+ Borrower's name
+ Co\-borrower's name
+ What is the Co\-Borrower DOB?
+ What is the Borrower's DOB?
+ What is borrower present address?
+ What is co\-borrower present address?
+ What is borrower mailing address?
+ What is co\-borrower mailing address?

## Fannie Mae 1005 Form<a name="w111aac31c11c19c23"></a>
+ What is the form type?
+ What is the name of the current employer?
+ What is the address of the current employer?
+ What is the name of the lender?
+ What is the YTD Base Pay amount?
+ What is the YTD Overtime amount?
+ What is the YTD Commissions amount?
+ What is the YTD Bonus amount?
+ What is authorized signature date?

## Health Insurance Cards<a name="w111aac31c11c19c25"></a>
+ What is medical insurance provider
+ What is the plan type?
+ What is the member/subscriber ID?
+ What is the group number?
+ What is the memeber name?
+ What is the plan number
+ What is the RxBIN?
+ What is the RxPCN?
+ What is the RxGRP?
+ What is the coninsurance amount?
+ What is the out of pocket maximum?

## Perscription Labels<a name="w111aac31c11c19c27"></a>
+ What is the pharmacy name?
+ What is the pharmacy address?
+ What is the doctor/physician name?
+ What is the prescription number?
+ What is the patient name?
+ What is the patient address?
+ What is the number of refills allowed?
+ What is the drug expiration date?

## College IDs<a name="w111aac31c11c19c29"></a>
+ What is the institution name?
+ What is the student/employee name?
+ What is the student/employee ID?
+ What is the expiry date?
+ What is the issue date?

## Setting up Pages for Queries<a name="querypages"></a>

When working with queries for multipage documents, you can use the `Page` parameter to specify which pages to look for the query answer on\. What follows is a list of best practices for setting up Pages
+ If a page is not specified, it is set to `["1"]` by default\.
+ The following characters are allowed in the parameter's string: `0 1 2 3 4 5 6 7 8 9 - *`\. No whitespace is allowed\.
+ When using \* to indicate all pages, it must be the only element in the list\.
+ You can use page intervals, such as `[“1-3”, “1-1”, “4-*”]`\. Where `*` indicates last page of document\.
+ Specified pages must be greater than 0 and less than or equal to the number of pages in the document\.