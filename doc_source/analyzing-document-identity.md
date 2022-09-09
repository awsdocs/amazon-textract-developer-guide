# Analyzing Identity Documentation with Amazon Textract<a name="analyzing-document-identity"></a>

To analyze identity documents, you use the AnalyzeID API, and pass a document file as input\. `AnalyzeID` returns a JSON structure that contains the analyzed text\. For more information, see [Analyzing Identity Documents](how-it-works-identity.md)\.

You can provide an input document as an image byte array \(base64\-encoded image bytes\), or as an Amazon S3 object\. In this procedure, you upload an image file to your S3 bucket and specify the file name\.

**To analyze an identity document \(API\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonTextractFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

1. Upload an image that contains a document to your S3 bucket\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service User Guide*\.

1. Use the following examples to call the `AnalyzeID` operation\.

------
#### [ CLI ]

   

   The following example takes in an input file from an S3 bucket and runs the `AnalyzeID` operation on it\. In the code below, replace the value of *bucket* with the name of your S3 bucket, the value of *file* with the name of the file in your bucket, and the value of *region* with the name of the `region` associated with your account\. 

   

   ```
   aws textract analyze-id --document-pages '[{"S3Object":{"Bucket":"bucket","Name":"name"}}]' --region region
   ```

   You can also call the API with the front and back of a driver's license by adding another S3 object to the input\.

   ```
   aws textract analyze-id --document-pages '[{"S3Object":{"Bucket":"bucket","Name":"name front"}}, {"S3Object":{"Bucket":"bucket","Name":"name back"}}]' --region us-east-1
   ```

   If you are accessing the CLI on a Windows device, use double quotes instead of single quotes and escape the inner double quotes by backslash \(i\.e\. \\\) to address any parser errors you may encounter\. For an example, see below:

   ```
   aws textract analyze-id --document-pages "[{\"S3Object\":{\"Bucket\":\"bucket\",\"Name\":\"name\"}}]" --region region
   ```

------
#### [ Python ]

   The following example takes in an input file from an S3 bucket and runs the `AnalyzeID` operation on it, returning the detected key\-value pairs\. In the code below, replace the value of *bucket\_name* with the name of your S3 bucket, the value of *file\_name* with the name of the file in your bucket, and the value of *region* with the name of the `region` associated with your account\. 

   ```
   import boto3
   
   def analyze_id(region, bucket_name, file_name):
   
       # Detect text in the document
       client = boto3.client('textract', region_name=region)
   
       # process using S3 object
       response = client.analyze_id(
           DocumentPages=[{'S3Object': {'Bucket': bucket_name, 'Name': file_name}}])
   
       for doc_fields in response['IdentityDocuments']:
           for id_field in doc_fields['IdentityDocumentFields']:
               for key, val in id_field.items():
                   if "Type" in str(key):
                       print("Type: " + str(val['Text']))
               for key, val in id_field.items():
                   if "ValueDetection" in str(key):
                       print("Value Detection: " + str(val['Text']))
               print()
   
   def main():
       bucket_name = "bucket-name"
       file_name = "file-name"
       region = "region-name"
   
       analyze_id(region, bucket_name, file_name)
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   The following example takes in an input file from an S3 bucket and runs the `AnalyzeID` operation on it, returning the detected data\. In the function main, replace the values of `s3bucket` and `sourceDoc` with the names of the Amazon S3 bucket and document image that you used in step 2\. 

   ```
   /*
      Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
      SPDX-License-Identifier: Apache-2.0
   */
   
   package com.amazonaws.samples;
   
   import com.amazonaws.regions.Regions;
   import com.amazonaws.services.textract.AmazonTextractClient;
   import com.amazonaws.services.textract.AmazonTextractClientBuilder;
   import com.amazonaws.services.textract.model.*;
   import java.util.ArrayList;
   import java.util.List;
   
   public class AnalyzeIdentityDocument {
   
       public static void main(String[] args) {
   
           final String USAGE = "\n" +
                   "Usage:\n" +
                   "    <s3bucket><sourceDoc> \n\n" +
                   "Where:\n" +
                   "    s3bucket - the Amazon S3 bucket where the document is located. \n" +
                   "    sourceDoc - the name of the document. \n";
   
           if (args.length != 1) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           String s3bucket = "bucket-name"; //args[0];
           String sourceDoc = "sourcedoc-name";  //args[1];
           AmazonTextractClient textractClient = (AmazonTextractClient) AmazonTextractClientBuilder.standard()
                   .withRegion(Regions.US_EAST_1)
                   .build();
   
           getDocDetails(textractClient, s3bucket, sourceDoc);
       }
   
       public static void getDocDetails(AmazonTextractClient textractClient, String s3bucket, String sourceDoc ) {
   
          try {
   
               S3Object s3 = new S3Object();
               s3.setBucket(s3bucket);
               s3.setName(sourceDoc);
   
               com.amazonaws.services.textract.model.Document myDoc = new com.amazonaws.services.textract.model.Document();
               myDoc.setS3Object(s3);
   
               List<Document> list1 = new ArrayList();
               list1.add(myDoc);
   
               AnalyzeIDRequest idRequest = new AnalyzeIDRequest();
               idRequest.setDocumentPages(list1);
   
               AnalyzeIDResult result = textractClient.analyzeID(idRequest);
               List<IdentityDocument> docs =  result.getIdentityDocuments();
               for (IdentityDocument doc: docs) {
   
                   List<IdentityDocumentField>idFields = doc.getIdentityDocumentFields();
                   for (IdentityDocumentField field: idFields) {
                       System.out.println("Field type is "+ field.getType().getText());
                       System.out.println("Field value is "+ field.getValueDetection().getText());
                   }
               }
   
          } catch (Exception e) {
               e.printStackTrace();
          }
       }
   }
   ```

------

1. This will provide you with the JSON output for the `AnalyzeID` operation\.