# Analyzing Invoices and Receipts with Amazon Textract<a name="analyzing-document-expense"></a>

To analyze invoice and receipt documents, you use the AnalyzeExpense API, and pass a document file as input\. `AnalyzeExpense` is a synchronous operation that returns a JSON structure that contains the analyzed text\. For more information, see [Analyzing Invoices and Receipts](invoices-receipts.md)\.

To analyze invoice and receipts asynchronously, use `StartExpenseAnalysis` to start processing an input document file and use `GetExpenseAnalysis` to get the results\. 

You can provide an input document as an image byte array \(base64\-encoded image bytes\), or as an Amazon S3 object\. In this procedure, you upload an image file to your S3 bucket and specify the file name\.

**To analyze an invoice or receipt \(API\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonTextractFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

1. Upload an image that contains a document to your S3 bucket\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service User Guide*\.

1. Use the following examples to call the `AnalyzeExpense` operation\.

------
#### [ CLI ]

   ```
     aws textract analyze-expense --document '{"S3Object": {"Bucket": "bucket name","Name":
   "object name"}}'
   ```

------
#### [ Python ]

   ```
                       import boto3
   import io
   from PIL import Image, ImageDraw
   
   def draw_bounding_box(key, val, width, height, draw):
       # If a key is Geometry, draw the bounding box info in it
       if "Geometry" in key:
           # Draw bounding box information
           box = val["BoundingBox"]
           left = width * box['Left']
           top = height * box['Top']
           draw.rectangle([left, top, left + (width * box['Width']), top + (height * box['Height'])],
                          outline='black')
   
   # Takes a field as an argument and prints out the detected labels and values
   def print_labels_and_values(field):
       # Only if labels are detected and returned
       if "LabelDetection" in field:
           print("Summary Label Detection - Confidence: {}".format(
               str(field.get("LabelDetection")["Confidence"])) + ", "
                 + "Summary Values: {}".format(str(field.get("LabelDetection")["Text"])))
           print(field.get("LabelDetection")["Geometry"])
       else:
           print("Label Detection - No labels returned.")
       if "ValueDetection" in field:
           print("Summary Value Detection - Confidence: {}".format(
               str(field.get("ValueDetection")["Confidence"])) + ", "
                 + "Summary Values: {}".format(str(field.get("ValueDetection")["Text"])))
           print(field.get("ValueDetection")["Geometry"])
       else:
           print("Value Detection - No values returned")
   
   def process_expense_analysis(bucket, document, region):
       # Get the document from S3
       s3_connection = boto3.resource('s3')
       s3_object = s3_connection.Object(bucket, document)
       s3_response = s3_object.get()
   
       # opening binary stream using an in-memory bytes buffer
       stream = io.BytesIO(s3_response['Body'].read())
   
       # loading stream into image
       image = Image.open(stream)
   
       # Detect text in the document
       client = boto3.client('textract', region_name=region)
   
       # process using S3 object
       response = client.analyze_expense(
           Document={'S3Object': {'Bucket': bucket, 'Name': document}})
   
       # Set width and height to display image and draw bounding boxes
       # Create drawing object
       width, height = image.size
       draw = ImageDraw.Draw(image)
   
       for expense_doc in response["ExpenseDocuments"]:
           for line_item_group in expense_doc["LineItemGroups"]:
               for line_items in line_item_group["LineItems"]:
                   for expense_fields in line_items["LineItemExpenseFields"]:
                       print_labels_and_values(expense_fields)
                       print()
   
           print("Summary:")
           for summary_field in expense_doc["SummaryFields"]:
               print_labels_and_values(summary_field)
               print()
   
           #For draw bounding boxes
           for line_item_group in expense_doc["LineItemGroups"]:
               for line_items in line_item_group["LineItems"]:
                   for expense_fields in line_items["LineItemExpenseFields"]:
                       for key, val in expense_fields["ValueDetection"].items():
                           if "Geometry" in key:
                               draw_bounding_box(key, val, width, height, draw)
   
           for label in expense_doc["SummaryFields"]:
               if "LabelDetection" in label:
                   for key, val in label["LabelDetection"].items():
                       draw_bounding_box(key, val, width, height, draw)
   
       # Display the image
       image.show()
   
   def main():
       bucket = 'Bucket-Name'
       document = 'Document-Name'
       region = 'region-name'
       process_expense_analysis(bucket, document, region)
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   ```
                       
   package com.amazonaws.samples;
   
   import java.awt.*;
   import java.awt.image.BufferedImage;
   import java.io.ByteArrayInputStream;
   import java.io.IOException;
   import java.util.List;
   import java.util.concurrent.CompletableFuture;
   import javax.imageio.ImageIO;
   import javax.swing.*;
   import software.amazon.awssdk.auth.credentials.AwsBasicCredentials;
   import software.amazon.awssdk.auth.credentials.StaticCredentialsProvider;
   import software.amazon.awssdk.core.ResponseBytes;
   import software.amazon.awssdk.core.async.AsyncResponseTransformer;
   import software.amazon.awssdk.regions.Region;
   import software.amazon.awssdk.services.s3.*;
   import software.amazon.awssdk.services.s3.model.GetObjectRequest;
   import software.amazon.awssdk.services.s3.model.GetObjectResponse;
   import software.amazon.awssdk.services.textract.TextractClient;
   import software.amazon.awssdk.services.textract.model.AnalyzeExpenseRequest;
   import software.amazon.awssdk.services.textract.model.AnalyzeExpenseResponse;
   import software.amazon.awssdk.services.textract.model.BoundingBox;
   import software.amazon.awssdk.services.textract.model.Document;
   import software.amazon.awssdk.services.textract.model.ExpenseDocument;
   import software.amazon.awssdk.services.textract.model.ExpenseField;
   import software.amazon.awssdk.services.textract.model.LineItemFields;
   import software.amazon.awssdk.services.textract.model.LineItemGroup;
   import software.amazon.awssdk.services.textract.model.S3Object;
   import software.amazon.awssdk.services.textract.model.Point;
   
   /**
    * 
    * Demo code to parse Textract AnalyzeExpense API
    *
    */
   public class TextractAnalyzeExpenseSample extends JPanel {
   
   	private static final long serialVersionUID = 1L;
   
   	BufferedImage image;
   	static AnalyzeExpenseResponse result;
   
   	public TextractAnalyzeExpenseSample(AnalyzeExpenseResponse documentResult, BufferedImage bufImage) throws Exception {
   		super();
   
   		result = documentResult; // Results of analyzeexpense summaryfields and lineitemgroups detection.
   		image = bufImage; // The image containing the document.
   
   	}
   
   	// Draws the image and text bounding box.
   	public void paintComponent(Graphics g) {
   
   		Graphics2D g2d = (Graphics2D) g; // Create a Java2D version of g.
   
   		// Draw the image.
   		g2d.drawImage(image, 0, 0, image.getWidth(this), image.getHeight(this), this);
   
   		// Iterate through summaryfields and lineitemgroups and display boundedboxes around lines of detected label and value.
   		List<ExpenseDocument> expenseDocuments = result.expenseDocuments();
   		for (ExpenseDocument expenseDocument : expenseDocuments) {
   
   			if (expenseDocument.hasSummaryFields()) {
   				DisplayAnalyzeExpenseSummaryInfo(expenseDocument);
   				List<ExpenseField> summaryfields = expenseDocument.summaryFields();
   				for (ExpenseField summaryfield : summaryfields) {
   
   					if (summaryfield.valueDetection() != null) {
   						ShowBoundingBox(image.getHeight(this), image.getWidth(this),
   								summaryfield.valueDetection().geometry().boundingBox(), g2d, new Color(0, 0, 0));
   					}
   
   					if (summaryfield.labelDetection() != null) {
   
   						ShowBoundingBox(image.getHeight(this), image.getWidth(this),
   								summaryfield.labelDetection().geometry().boundingBox(), g2d, new Color(0, 0, 0));
   
   					}
   
   				}
   
   			}
   
   			if (expenseDocument.hasLineItemGroups()) {
   				DisplayAnalyzeExpenseLineItemGroupsInfo(expenseDocument);
   
   				List<LineItemGroup> lineitemgroups = expenseDocument.lineItemGroups();
   
   				for (LineItemGroup lineitemgroup : lineitemgroups) {
   
   					if (lineitemgroup.hasLineItems()) {
   
   						List<LineItemFields> lineItems = lineitemgroup.lineItems();
   						for (LineItemFields lineitemfield : lineItems) {
   
   							if (lineitemfield.hasLineItemExpenseFields()) {
   
   								List<ExpenseField> expensefields = lineitemfield.lineItemExpenseFields();
   								for (ExpenseField expensefield : expensefields) {
   
   									if (expensefield.valueDetection() != null) {
   										ShowBoundingBox(image.getHeight(this), image.getWidth(this),
   												expensefield.valueDetection().geometry().boundingBox(), g2d,
   												new Color(0, 0, 0));
   									}
   
   									if (expensefield.labelDetection() != null) {
   										ShowBoundingBox(image.getHeight(this), image.getWidth(this),
   												expensefield.labelDetection().geometry().boundingBox(), g2d,
   												new Color(0, 0, 0));
   									}
   
   								}
   							}
   
   						}
   
   					}
   
   				}
   
   			}
   		}
   
   	}
   
   	// Show bounding box at supplied location.
   	private void ShowBoundingBox(float imageHeight, float imageWidth, BoundingBox box, Graphics2D g2d, Color color) {
   
   		float left = imageWidth * box.left();
   		float top = imageHeight * box.top();
   
   		// Display bounding box.
   		g2d.setColor(color);
   		g2d.drawRect(Math.round(left), Math.round(top), Math.round(imageWidth * box.width()),
   				Math.round(imageHeight * box.height()));
   
   	}
   
   	private void ShowSelectedElement(float imageHeight, float imageWidth, BoundingBox box, Graphics2D g2d,
   			Color color) {
   
   		float left = (float) imageWidth * (float) box.left();
   		float top = (float) imageHeight * (float) box.top();
   		System.out.println(left);
   		System.out.println(top);
   
   		// Display bounding box.
   		g2d.setColor(color);
   		g2d.fillRect(Math.round(left), Math.round(top), Math.round(imageWidth * box.width()),
   				Math.round(imageHeight * box.height()));
   
   	}
   
   	// Shows polygon at supplied location
   	private void ShowPolygon(int imageHeight, int imageWidth, List<Point> points, Graphics2D g2d) {
   
   		g2d.setColor(new Color(0, 0, 0));
   		Polygon polygon = new Polygon();
   
   		// Construct polygon and display
   		for (Point point : points) {
   			polygon.addPoint((Math.round(point.x() * imageWidth)), Math.round(point.y() * imageHeight));
   		}
   		g2d.drawPolygon(polygon);
   	}
   
   	private void DisplayAnalyzeExpenseSummaryInfo(ExpenseDocument expensedocument) {
   		System.out.println("	ExpenseId : " + expensedocument.expenseIndex());
   		System.out.println("    Expense Summary information:");
   		if (expensedocument.hasSummaryFields()) {
   
   			List<ExpenseField> summaryfields = expensedocument.summaryFields();
   
   			for (ExpenseField summaryfield : summaryfields) {
   
   				System.out.println("    Page: " + summaryfield.pageNumber());
   				if (summaryfield.type() != null) {
   
   					System.out.println("    Expense Summary Field Type:" + summaryfield.type().text());
   
   				}
   				if (summaryfield.labelDetection() != null) {
   
   					System.out.println("    Expense Summary Field Label:" + summaryfield.labelDetection().text());
   					System.out.println("    Geometry");
   					System.out.println("        Bounding Box: "
   							+ summaryfield.labelDetection().geometry().boundingBox().toString());
   					System.out.println(
   							"        Polygon: " + summaryfield.labelDetection().geometry().polygon().toString());
   
   				}
   				if (summaryfield.valueDetection() != null) {
   					System.out.println("    Expense Summary Field Value:" + summaryfield.valueDetection().text());
   					System.out.println("    Geometry");
   					System.out.println("        Bounding Box: "
   							+ summaryfield.valueDetection().geometry().boundingBox().toString());
   					System.out.println(
   							"        Polygon: " + summaryfield.valueDetection().geometry().polygon().toString());
   
   				}
   
   			}
   
   		}
   
   	}
   
   	private void DisplayAnalyzeExpenseLineItemGroupsInfo(ExpenseDocument expensedocument) {
   
   		System.out.println("	ExpenseId : " + expensedocument.expenseIndex());
   		System.out.println("    Expense LineItemGroups information:");
   
   		if (expensedocument.hasLineItemGroups()) {
   
   			List<LineItemGroup> lineitemgroups = expensedocument.lineItemGroups();
   
   			for (LineItemGroup lineitemgroup : lineitemgroups) {
   
   				System.out.println("    Expense LineItemGroupsIndexID :" + lineitemgroup.lineItemGroupIndex());
   
   				if (lineitemgroup.hasLineItems()) {
   
   					List<LineItemFields> lineItems = lineitemgroup.lineItems();
   
   					for (LineItemFields lineitemfield : lineItems) {
   
   						if (lineitemfield.hasLineItemExpenseFields()) {
   
   							List<ExpenseField> expensefields = lineitemfield.lineItemExpenseFields();
   							for (ExpenseField expensefield : expensefields) {
   
   								if (expensefield.type() != null) {
   									System.out.println("    Expense LineItem Field Type:" + expensefield.type().text());
   
   								}
   
   								if (expensefield.valueDetection() != null) {
   									System.out.println(
   											"    Expense Summary Field Value:" + expensefield.valueDetection().text());
   									System.out.println("    Geometry");
   									System.out.println("        Bounding Box: "
   											+ expensefield.valueDetection().geometry().boundingBox().toString());
   									System.out.println("        Polygon: "
   											+ expensefield.valueDetection().geometry().polygon().toString());
   
   								}
   
   								if (expensefield.labelDetection() != null) {
   									System.out.println(
   											"    Expense LineItem Field Label:" + expensefield.labelDetection().text());
   									System.out.println("    Geometry");
   									System.out.println("        Bounding Box: "
   											+ expensefield.labelDetection().geometry().boundingBox().toString());
   									System.out.println("        Polygon: "
   											+ expensefield.labelDetection().geometry().polygon().toString());
   								}
   
   							}
   						}
   
   					}
   
   				}
   			}
   		}
   
   	}
   
   	public static void main(String arg[]) throws Exception {
   
   		// Creates a default async client with credentials and AWS Region loaded from
   		// the
   		// environment
   
   		
   		S3AsyncClient client = S3AsyncClient.builder().region(Region.US_EAST_1).build();
   
   		System.out.println("Creating the S3 Client");
   
   		// Start the call to Amazon S3, not blocking to wait for the result
   		CompletableFuture<ResponseBytes<GetObjectResponse>> responseFuture = client.getObject(
   				GetObjectRequest.builder().bucket("textractanalyzeexpense").key("input/sample-receipt.jpg").build(),
   				AsyncResponseTransformer.toBytes());
   
   		System.out.println("Successfully read the object");
   
   		// When future is complete (either successfully or in error), handle the
   		// response
   		CompletableFuture<ResponseBytes<GetObjectResponse>> operationCompleteFuture = responseFuture
   				.whenComplete((getObjectResponse, exception) -> {
   					if (getObjectResponse != null) {
   						// At this point, the file my-file.out has been created with the data
   						// from S3; let's just print the object version
   						// Convert this into Async call and remove the below block from here and put it
   						// outside
   
   						
   						TextractClient textractclient = TextractClient.builder().region(Region.US_EAST_1).build();
   
   						AnalyzeExpenseRequest request = AnalyzeExpenseRequest.builder()
   								.document(
   										Document.builder().s3Object(S3Object.builder().name("YOURObjectName")
   												.bucket("YOURBucket").build()).build())
   								.build();
   
   						AnalyzeExpenseResponse result = textractclient.analyzeExpense(request);
   
   						System.out.print(result.toString());
   
   						ByteArrayInputStream bais = new ByteArrayInputStream(getObjectResponse.asByteArray());
   						try {
   							BufferedImage image = ImageIO.read(bais);
   							System.out.println("Successfully read the image");
   							JFrame frame = new JFrame("Expense Image");
   							frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
   							TextractAnalyzeExpense panel = new TextractAnalyzeExpense(result, image);
   							panel.setPreferredSize(new Dimension(image.getWidth(), image.getHeight()));
   							frame.setContentPane(panel);
   							frame.pack();
   							frame.setVisible(true);
   						} catch (IOException e) {
   							throw new RuntimeException(e);
   						} catch (Exception e) {
   							// TODO Auto-generated catch block
   							e.printStackTrace();
   						}
   					} else {
   						// Handle the error
   						exception.printStackTrace();
   					}
   				});
   
   		// We could do other work while waiting for the AWS call to complete in
   		// the background, but we'll just wait for "whenComplete" to finish instead
   		operationCompleteFuture.join();
   
   	}
   }
   ```

------
#### [ Node\.Js ]

   ```
                       // Import required AWS SDK clients and commands for Node.js
   import { AnalyzeExpenseCommand } from  "@aws-sdk/client-textract";
   import  { TextractClient } from "@aws-sdk/client-textract";
   
   // Set the AWS Region.
   const REGION = "region"; //e.g. "us-east-1"
   // Create SNS service object.
   const textractClient = new TextractClient({ region: REGION });
   
   const bucket = 'bucket'
   const photo = 'photo'
   
   // Set params
   const params = {
       Document: {
         S3Object: {
           Bucket: bucket,
           Name: photo
         },
       },
     }
     
   const process_text_detection = async () => {
       try {
           const aExpense = new AnalyzeExpenseCommand(params);
           const response = await textractClient.send(aExpense);
           //console.log(response)
           response.ExpenseDocuments.forEach(doc => {
               doc.LineItemGroups.forEach(items => {
                   items.LineItems.forEach(fields => {
                       fields.LineItemExpenseFields.forEach(expenseFields =>{
                           console.log(expenseFields)
                       })
                   }
                   )}
                   )      
               }
           )
           return response; // For unit tests.
         } catch (err) {
           console.log("Error", err);
         }
   }
   
   process_text_detection()
   ```

------

1. This will provide you with the JSON output for the `AnalyzeExpense` operation\.