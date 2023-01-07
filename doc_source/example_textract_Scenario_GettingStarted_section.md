# Get started with Amazon Textract document analysis using an AWS SDK<a name="example_textract_Scenario_GettingStarted_section"></a>

The following code example shows how to:
+ Start asynchronous analysis\.
+ Get document analysis\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ SAP ABAP ]

**SDK for SAP ABAP**  
This documentation is for an SDK in developer preview release\. The SDK is subject to change and is not recommended for use in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/sap-abap/services/textract#code-examples)\. 
  

```
    DATA lo_documentlocation TYPE REF TO /aws1/cl_texdocumentlocation.
    DATA lo_s3object TYPE REF TO /aws1/cl_texs3object.
    DATA lo_featuretypes TYPE REF TO /aws1/cl_texfeaturetypes_w.
    DATA lt_featuretypes TYPE /aws1/cl_texfeaturetypes_w=>tt_featuretypes.

    "Create ABAP objects for feature type."
    "Add TABLES to return information about the tables."
    "Add FORMS to return detected form data."
    "To perform both types of analysis, add TABLES and FORMS to FeatureTypes."

    CREATE OBJECT lo_featuretypes EXPORTING iv_value = 'FORMS'.
    INSERT lo_featuretypes INTO TABLE lt_featuretypes.

    CREATE OBJECT lo_featuretypes EXPORTING iv_value = 'TABLES'.
    INSERT lo_featuretypes INTO TABLE lt_featuretypes.

    "Create an ABAP object for the Amazon Simple Storage Service (Amazon S3) object."
    CREATE OBJECT lo_s3object
      EXPORTING
        iv_bucket = iv_s3bucket
        iv_name   = iv_s3object.

    "Create an ABAP object for the document."
    CREATE OBJECT lo_documentlocation EXPORTING io_s3object = lo_s3object.

    "Start document analysis."
    TRY.
        DATA(lo_start_result) = lo_tex->startdocumentanalysis(
      EXPORTING
        io_documentlocation     = lo_documentlocation
        it_featuretypes         = lt_featuretypes
      ).
        MESSAGE 'Document analysis started.' TYPE 'I'.
      CATCH /aws1/cx_texaccessdeniedex .
        MESSAGE 'You do not have permission to perform this action.' TYPE 'E'.
      CATCH /aws1/cx_texbaddocumentex .
        MESSAGE 'Amazon Textract is not able to read the document.' TYPE 'E'.
      CATCH /aws1/cx_texdocumenttoolargeex .
        MESSAGE 'The document is too large.' TYPE 'E'.
      CATCH /aws1/cx_texidempotentprmmis00 .
        MESSAGE 'Idempotent parameter mismatch exception.' TYPE 'E'.
      CATCH /aws1/cx_texinternalservererr .
        MESSAGE 'Internal server error.' TYPE 'E'.
      CATCH /aws1/cx_texinvalidkmskeyex .
        MESSAGE 'AWS KMS key isn't valid.' TYPE 'E'.
      CATCH /aws1/cx_texinvalidparameterex .
        MESSAGE 'Request has non-valid parameters.' TYPE 'E'.
      CATCH /aws1/cx_texinvalids3objectex .
        MESSAGE 'Amazon S3 object isn't valid.' TYPE 'E'.
      CATCH /aws1/cx_texlimitexceededex .
        MESSAGE 'An Amazon Textract service limit was exceeded.' TYPE 'E'.
      CATCH /aws1/cx_texprovthruputexcdex .
        MESSAGE 'Provisioned throughput exceeded limit.' TYPE 'E'.
      CATCH /aws1/cx_texthrottlingex .
        MESSAGE 'The request processing exceeded the limit.' TYPE 'E'.
      CATCH /aws1/cx_texunsupporteddocex .
        MESSAGE 'The document is not supported.' TYPE 'E'.
    ENDTRY.

    "Get job ID from the output."
    DATA(lv_jobid) = lo_start_result->get_jobid( ).

    "Wait for job to complete."
    oo_result = lo_tex->getdocumentanalysis( EXPORTING iv_jobid = lv_jobid ).     " oo_result is returned for testing purposes. "
    WHILE oo_result->get_jobstatus( ) <> 'SUCCEEDED'.
      IF sy-index = 10.
        EXIT.               "Maximum 300 seconds."
      ENDIF.
      WAIT UP TO 30 SECONDS.
      oo_result = lo_tex->getdocumentanalysis( EXPORTING iv_jobid = lv_jobid ).
    ENDWHILE.
```
+ For API details, see the following topics in *AWS SDK for SAP ABAP API reference*\.
  + [GetDocumentAnalysis](https://docs.aws.amazon.com/sdk-for-sap-abap/v1/api/latest/index.html)
  + [StartDocumentAnalysis](https://docs.aws.amazon.com/sdk-for-sap-abap/v1/api/latest/index.html)

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon Textract with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.