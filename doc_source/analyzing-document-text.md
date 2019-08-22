# Analyzing Document Text with Amazon Textract<a name="analyzing-document-text"></a>

To analyze text in a document, you use the [AnalyzeDocument](API_AnalyzeDocument.md) operation, and pass a document file as input\. `AnalyzeDocument` returns a JSON structure that contains the analyzed text\. For more information, see [Analyzing Text](how-it-works-analyzing.md)\. 

You can provide an input document as an image byte array \(base64\-encoded image bytes\), or as an Amazon S3 object\. In this procedure, you upload an image file to your S3 bucket and specify the file name\. 

**To analyze text in a document \(API\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonTextractFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 1: Set Up an AWS Account and Create an IAM User](setting-up.md#setting-up-iam)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\.

1. Upload an image that contains a document to your S3 bucket\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Use the following examples to call the `AnalyzeDocument` operation\.

------
#### [ Java ]

   The following example code displays the document and boxes around detected items\. 

   In the function `main`, replace the values of `bucket` and `document` with the names of the Amazon S3 bucket and document image that you used in step 2\. 

   ```
   //Loads document from S3 bucket. Displays the document and polygon around detected lines of text.
   package com.amazonaws.samples;
   
   import java.awt.*;
   import java.awt.image.BufferedImage;
   import java.util.List;
   import javax.imageio.ImageIO;
   import javax.swing.*;
   import com.amazonaws.services.s3.AmazonS3;
   import com.amazonaws.services.s3.AmazonS3ClientBuilder;
   import com.amazonaws.services.s3.model.S3ObjectInputStream;
   import com.amazonaws.services.textract.AmazonTextract;
   import com.amazonaws.services.textract.AmazonTextractClientBuilder;
   import com.amazonaws.services.textract.model.AnalyzeDocumentRequest;
   import com.amazonaws.services.textract.model.AnalyzeDocumentResult;
   import com.amazonaws.services.textract.model.Block;
   import com.amazonaws.services.textract.model.BoundingBox;
   import com.amazonaws.services.textract.model.Document;
   import com.amazonaws.services.textract.model.S3Object;
   import com.amazonaws.services.textract.model.Point;
   import com.amazonaws.services.textract.model.Relationship;
   import com.amazonaws.client.builder.AwsClientBuilder.EndpointConfiguration;
   
   public class AnalyzeDocument extends JPanel {
   
       private static final long serialVersionUID = 1L;
   
       BufferedImage image;
   
       AnalyzeDocumentResult result;
   
       public AnalyzeDocument(AnalyzeDocumentResult documentResult, BufferedImage bufImage) throws Exception {
           super();
   
           result = documentResult; // Results of text detection.
           image = bufImage; // The image containing the document.
   
       }
   
       // Draws the image and text bounding box.
       public void paintComponent(Graphics g) {
   
           int height = image.getHeight(this);
           int width = image.getWidth(this);
   
           Graphics2D g2d = (Graphics2D) g; // Create a Java2D version of g.
   
           // Draw the image.
           g2d.drawImage(image, 0, 0, image.getWidth(this), image.getHeight(this), this);
   
           // Iterate through blocks and display bounding boxes around everything.
   
           List<Block> blocks = result.getBlocks();
           for (Block block : blocks) {
               DisplayBlockInfo(block);
               switch(block.getBlockType()) {
               
               case "KEY_VALUE_SET":
                   if (block.getEntityTypes().contains("KEY")){
                       ShowBoundingBox(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(255,0,0));
                   }
                   else {  //VALUE
                       ShowBoundingBox(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(0,255,0));
                   }
                   break;
               case "TABLE":
                   ShowBoundingBox(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(0,0,255));
                   break;
               case "CELL":
                   ShowBoundingBox(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(255,255,0));
                   break;
               case "SELECTION_ELEMENT":
                   if (block.getSelectionStatus().equals("SELECTED"))
                       ShowSelectedElement(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(0,0,255));
                   break;    
               default:
                   //PAGE, LINE & WORD
                   //ShowBoundingBox(height, width, block.getGeometry().getBoundingBox(), g2d, new Color(200,200,0));
               }
           }
   
            // uncomment to show polygon around all blocks
            //ShowPolygon(height,width,block.getGeometry().getPolygon(),g2d);
         
         
       }
   
       // Show bounding box at supplied location.
       private void ShowBoundingBox(int imageHeight, int imageWidth, BoundingBox box, Graphics2D g2d, Color color) {
   
           float left = imageWidth * box.getLeft();
           float top = imageHeight * box.getTop();
   
           // Display bounding box.
           g2d.setColor(color);
           g2d.drawRect(Math.round(left), Math.round(top),
                   Math.round(imageWidth * box.getWidth()), Math.round(imageHeight * box.getHeight()));
   
       }
       private void ShowSelectedElement(int imageHeight, int imageWidth, BoundingBox box, Graphics2D g2d, Color color) {
   
           float left = imageWidth * box.getLeft();
           float top = imageHeight * box.getTop();
   
           // Display bounding box.
           g2d.setColor(color);
           g2d.fillRect(Math.round(left), Math.round(top),
                   Math.round(imageWidth * box.getWidth()), Math.round(imageHeight * box.getHeight()));
   
       }
   
       // Shows polygon at supplied location
       private void ShowPolygon(int imageHeight, int imageWidth, List<Point> points, Graphics2D g2d) {
   
           g2d.setColor(new Color(0, 0, 0));
           Polygon polygon = new Polygon();
   
           // Construct polygon and display
           for (Point point : points) {
               polygon.addPoint((Math.round(point.getX() * imageWidth)),
                       Math.round(point.getY() * imageHeight));
           }
           g2d.drawPolygon(polygon);
       }
       //Displays information from a block returned by text detection and text analysis
       private void DisplayBlockInfo(Block block) {
           System.out.println("Block Id : " + block.getId());
           if (block.getText()!=null)
               System.out.println("    Detected text: " + block.getText());
           System.out.println("    Type: " + block.getBlockType());
           
           if (block.getBlockType().equals("PAGE") !=true) {
               System.out.println("    Confidence: " + block.getConfidence().toString());
           }
           if(block.getBlockType().equals("CELL"))
           {
               System.out.println("    Cell information:");
               System.out.println("        Column: " + block.getColumnIndex());
               System.out.println("        Row: " + block.getRowIndex());
               System.out.println("        Column span: " + block.getColumnSpan());
               System.out.println("        Row span: " + block.getRowSpan());
   
           }
           
           System.out.println("    Relationships");
           List<Relationship> relationships=block.getRelationships();
           if(relationships!=null) {
               for (Relationship relationship : relationships) {
                   System.out.println("        Type: " + relationship.getType());
                   System.out.println("        IDs: " + relationship.getIds().toString());
               }
           } else {
               System.out.println("        No related Blocks");
           }
   
           System.out.println("    Geometry");
           System.out.println("        Bounding Box: " + block.getGeometry().getBoundingBox().toString());
           System.out.println("        Polygon: " + block.getGeometry().getPolygon().toString());
           
           List<String> entityTypes = block.getEntityTypes();
           
           System.out.println("    Entity Types");
           if(entityTypes!=null) {
               for (String entityType : entityTypes) {
                   System.out.println("        Entity Type: " + entityType);
               }
           } else {
               System.out.println("        No entity type");
           }
           
           if(block.getBlockType().equals("SELECTION_ELEMENT")) {
               System.out.print("    Selection element detected: ");
               if (block.getSelectionStatus().equals("SELECTED")){
                   System.out.println("Selected");
               }else {
                   System.out.println(" Not selected");
               }
           }
          
           if(block.getPage()!=null)
               System.out.println("    Page: " + block.getPage());            
           System.out.println();
       }
   
       public static void main(String arg[]) throws Exception {
   
           // The S3 bucket and document
           String document = "";
           String bucket = "";
   
           AmazonS3 s3client = AmazonS3ClientBuilder.standard()
                   .withEndpointConfiguration( 
                           new EndpointConfiguration("https://s3.amazonaws.com","us-east-1"))
                   .build();
           
                  
           // Get the document from S3
           com.amazonaws.services.s3.model.S3Object s3object = s3client.getObject(bucket, document);
           S3ObjectInputStream inputStream = s3object.getObjectContent();
           BufferedImage image = ImageIO.read(inputStream);
   
           // Call AnalyzeDocument 
           EndpointConfiguration endpoint = new EndpointConfiguration(
                   "https://textract.us-east-1.amazonaws.com", "us-east-1");
           AmazonTextract client = AmazonTextractClientBuilder.standard()
                   .withEndpointConfiguration(endpoint).build();
   
                   
           AnalyzeDocumentRequest request = new AnalyzeDocumentRequest()
                   .withFeatureTypes("TABLES","FORMS")
                    .withDocument(new Document().
                           withS3Object(new S3Object().withName(document).withBucket(bucket)));
   
   
           AnalyzeDocumentResult result = client.analyzeDocument(request);
   
           // Create frame and panel.
           JFrame frame = new JFrame("RotateImage");
           frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           AnalyzeDocument panel = new AnalyzeDocument(result, image);
           panel.setPreferredSize(new Dimension(image.getWidth(), image.getHeight()));
           frame.setContentPane(panel);
           frame.pack();
           frame.setVisible(true); 
   
       }
   }
   ```

------
#### [ AWS CLI ]

   This AWS CLI command displays the JSON output for the `detect-document-text` CLI operation\. 

   Replace the values of `Bucket` and `Name` with the names of the Amazon S3 bucket and document that you used in step 2\. 

   ```
   aws textract analyze-document \
    --document '{"S3Object":{"Bucket":"bucket","Name":"document"}}' \
    --feature-types '["TABLES","FORMS"]'
   ```

------
#### [ Python ]

   The following example code displays the document and boxes around detected items\. 

   In the function `main`, replace the values of `bucket` and `document` with the names of the Amazon S3 bucket and document that you used in step 2\. 

   ```
   #Analyzes text in a document stored in an S3 bucket. Display polygon box around text and angled text 
   import boto3
   import io
   from io import BytesIO
   import sys
   
   import math
   from PIL import Image, ImageDraw, ImageFont
   
   def ShowBoundingBox(draw,box,width,height,boxColor):
                
       left = width * box['Left']
       top = height * box['Top'] 
       draw.rectangle([left,top, left + (width * box['Width']), top +(height * box['Height'])],outline=boxColor)   
   
   def ShowSelectedElement(draw,box,width,height,boxColor):
                
       left = width * box['Left']
       top = height * box['Top'] 
       draw.rectangle([left,top, left + (width * box['Width']), top +(height * box['Height'])],fill=boxColor)  
   
   # Displays information about a block returned by text detection and text analysis
   def DisplayBlockInformation(block):
       print('Id: {}'.format(block['Id']))
       if 'Text' in block:
           print('    Detected: ' + block['Text'])
       print('    Type: ' + block['BlockType'])
      
       if 'Confidence' in block:
           print('    Confidence: ' + "{:.2f}".format(block['Confidence']) + "%")
   
       if block['BlockType'] == 'CELL':
           print("    Cell information")
           print("        Column:" + str(block['ColumnIndex']))
           print("        Row:" + str(block['RowIndex']))
           print("        Column Span:" + str(block['ColumnSpan']))
           print("        RowSpan:" + str(block['ColumnSpan']))    
       
       if 'Relationships' in block:
           print('    Relationships: {}'.format(block['Relationships']))
       print('    Geometry: ')
       print('        Bounding Box: {}'.format(block['Geometry']['BoundingBox']))
       print('        Polygon: {}'.format(block['Geometry']['Polygon']))
       
       if block['BlockType'] == "KEY_VALUE_SET":
           print ('    Entity Type: ' + block['EntityTypes'][0])
       
       if block['BlockType'] == 'SELECTION_ELEMENT':
           print('    Selection element detected: ', end='')
   
           if block['SelectionStatus'] =='SELECTED':
               print('Selected')
           else:
               print('Not selected')    
       
       if 'Page' in block:
           print('Page: ' + block['Page'])
       print()
   
   def process_text_analysis(bucket, document):
   
       #Get the document from S3
       s3_connection = boto3.resource('s3')
                             
       s3_object = s3_connection.Object(bucket,document)
       s3_response = s3_object.get()
   
       stream = io.BytesIO(s3_response['Body'].read())
       image=Image.open(stream)
   
       # Analyze the document
       client = boto3.client('textract')
       
       image_binary = stream.getvalue()
       response = client.analyze_document(Document={'Bytes': image_binary},
           FeatureTypes=["TABLES", "FORMS"])
     
   
       # Alternatively, process using S3 object
       #response = client.analyze_document(
       #    Document={'S3Object': {'Bucket': bucket, 'Name': document}},
       #    FeatureTypes=["TABLES", "FORMS"])
   
       
       #Get the text blocks
       blocks=response['Blocks']
       width, height =image.size  
       draw = ImageDraw.Draw(image)  
       print ('Detected Document Text')
      
       # Create image showing bounding box/polygon the detected lines/text
       for block in blocks:
   
           DisplayBlockInformation(block)
                
           draw=ImageDraw.Draw(image)
           if block['BlockType'] == "KEY_VALUE_SET":
               if block['EntityTypes'][0] == "KEY":
                   ShowBoundingBox(draw, block['Geometry']['BoundingBox'],width,height,'red')
               else:
                   ShowBoundingBox(draw, block['Geometry']['BoundingBox'],width,height,'green')  
               
           if block['BlockType'] == 'TABLE':
               ShowBoundingBox(draw, block['Geometry']['BoundingBox'],width,height, 'blue')
   
           if block['BlockType'] == 'CELL':
               ShowBoundingBox(draw, block['Geometry']['BoundingBox'],width,height, 'yellow')
           if block['BlockType'] == 'SELECTION_ELEMENT':
               if block['SelectionStatus'] =='SELECTED':
                   ShowSelectedElement(draw, block['Geometry']['BoundingBox'],width,height, 'blue')    
      
               #uncomment to draw polygon for all Blocks
               #points=[]
               #for polygon in block['Geometry']['Polygon']:
               #    points.append((width * polygon['X'], height * polygon['Y']))
               #draw.polygon((points), outline='blue')
               
       # Display the image
       image.show()
       return len(blocks)
   
   
   def main():
   
       bucket = ''
       document = ''
       block_count=process_text_analysis(bucket,document)
       print("Blocks detected: " + str(block_count))
       
   if __name__ == "__main__":
       main()
   ```

------

1. Run the example\. The Python and Java examples display the document image with the following colored bounding boxes:
   + Red –KEY Block objects 
   + Green – VALUE Block objects
   + Blue – TABLE Block objects
   + Yellow – CELL Block objects

   Selection elements that are selected are filled with blue\. 

   The AWS CLI example displays only the JSON output for the `AnalyzeDocument` operation\.