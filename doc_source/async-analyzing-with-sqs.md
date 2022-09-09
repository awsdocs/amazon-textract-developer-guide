# Detecting or Analyzing Text in a Multipage Document<a name="async-analyzing-with-sqs"></a>

This procedure shows you how to detect or analyze text in a multipage document by using Amazon Textract detection operations, a document stored in an Amazon S3 bucket, an Amazon SNS topic, and an Amazon SQS queue\. Multipage document processing is an asynchronous operation\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\.

You can choose the type of processing that you want the code to do: text detection, text analysis, or expense analysis\. 

The processing results are returned in an array of [Block](API_Block.md) objects, which differ depending on the type of processing you use\.

 To detect text in or analyze multipage documents, you do the following:

1. Create the Amazon SNS topic and the Amazon SQS queue\.

1. Subscribe the queue the topic\.

1. Give the topic permission to send messages to the queue\.

1. Start processing the document\. Use the appropriate operation for your chosen type of analysis:
   + [StartDocumentTextDetection](API_StartDocumentTextDetection.md) for text detection tasks\.
   + [StartDocumentAnalysis](API_StartDocumentAnalysis.md) for text analysis tasks\.
   + [StartExpenseAnalysis](API_StartExpenseAnalysis.md) for expense analysis tasks\.

1. Get the completion status from the Amazon SQS queue\. The example code tracks the job identifier \(`JobId`\) that's returned by the `Start` operation\. It only gets the results for matching job identifiers that are read from the completion status\. This is important if other applications are using the same queue and topic\. For simplicity, the example deletes jobs that don't match\. Consider adding the deleted jobs to an Amazon SQS dead\-letter queue for further investigation\.

1. Get and display the processing results by calling the appropriate operation for your chosen type of analysis:
   + [GetDocumentTextDetection](API_GetDocumentTextDetection.md) for text detection tasks\.
   + [GetDocumentAnalysis](API_GetDocumentAnalysis.md) for text analysis tasks\.
   + [GetExpenseAnalysis](API_GetExpenseAnalysis.md) for expense analysis tasks\.

1. Delete the Amazon SNS topic and the Amazon SQS queue\.

## Performing Asynchronous Operations<a name="async-prerequisites"></a>

The example code for this procedure is provided in Java, Python, and the AWS CLI\. Before you begin, install the appropriate AWS SDK\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\. 

**To detect or analyze text in a multipage document**

1. Configure user access to Amazon Textract, and configure Amazon Textract access to Amazon SNS\. For more information, see [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)\. To complete this procedure, you need a multipage document ﬁle in PDF format\. Skip steps 3 – 6 because the example code creates and configures the Amazon SNS topic and Amazon SQS queue\. If completing the CLI example, you don't need to set up an SQS queue\. 

1. Upload a multipage document file in PDF or TIFF format to your Amazon S3 bucket\. \(Single\-page documents in JPEG, PNG, TIFF, or PDF format can also be processed\)\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service User Guide*\.

1. Use the following AWS SDK for Java, SDK for Python \(Boto3\), or AWS CLI code to either detect text or analyze text in a multipage document\. In the `main` function:
   + Replace the value of `roleArn` with the IAM role ARN that you saved in [Giving Amazon Textract Access to Your Amazon SNS Topic](api-async-roles.md#api-async-roles-all-topics)\. 
   + Replace the values of `bucket` and `document` with the bucket and document file name that you specified in step 2\. 
   + Replace the value of the `type` input parameter of the `ProcessDocument` function with the type of processing that you want to do\. Use `ProcessType.DETECTION` to detect text\. Use `ProcessType.ANALYSIS` to analyze text\. 
   + For the Python example, replace the value of `region_name` with the region your client is operating in\.

   For the AWS CLI example, do the following:
   + When calling [StartDocumentTextDetection](API_StartDocumentTextDetection.md), replace the value of `bucket-name` with the name of your S3 bucket, and replace `file-name` with the name of the file you specified in step 2\. Specify the region of your bucket by replacing `region-name` with the name of your region\. Take note that the CLI example does not make use of SQS\. 
   + When calling [GetDocumentTextDetection](API_GetDocumentTextDetection.md) replace `job-id-number` with the `job-id` returned by [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\. Specify the region of your bucket by replacing `region-name` with the name of your region\.

------
#### [ Java ]

   ```
   package com.amazonaws.samples;
   
   import java.util.Arrays;
   import java.util.HashMap;
   import java.util.List;
   import java.util.Map;
   
   import com.amazonaws.auth.policy.Condition;
   import com.amazonaws.auth.policy.Policy;
   import com.amazonaws.auth.policy.Principal;
   import com.amazonaws.auth.policy.Resource;
   import com.amazonaws.auth.policy.Statement;
   import com.amazonaws.auth.policy.Statement.Effect;
   import com.amazonaws.auth.policy.actions.SQSActions;
   import com.amazonaws.services.sns.AmazonSNS;
   import com.amazonaws.services.sns.AmazonSNSClientBuilder;
   import com.amazonaws.services.sns.model.CreateTopicRequest;
   import com.amazonaws.services.sns.model.CreateTopicResult;
   import com.amazonaws.services.sqs.AmazonSQS;
   import com.amazonaws.services.sqs.AmazonSQSClientBuilder;
   import com.amazonaws.services.sqs.model.CreateQueueRequest;
   import com.amazonaws.services.sqs.model.Message;
   import com.amazonaws.services.sqs.model.QueueAttributeName;
   import com.amazonaws.services.sqs.model.SetQueueAttributesRequest;
   import com.amazonaws.services.textract.AmazonTextract;
   import com.amazonaws.services.textract.AmazonTextractClientBuilder;
   import com.amazonaws.services.textract.model.Block;
   import com.amazonaws.services.textract.model.DocumentLocation;
   import com.amazonaws.services.textract.model.DocumentMetadata;
   import com.amazonaws.services.textract.model.GetDocumentAnalysisRequest;
   import com.amazonaws.services.textract.model.GetDocumentAnalysisResult;
   import com.amazonaws.services.textract.model.GetDocumentTextDetectionRequest;
   import com.amazonaws.services.textract.model.GetDocumentTextDetectionResult;
   import com.amazonaws.services.textract.model.NotificationChannel;
   import com.amazonaws.services.textract.model.Relationship;
   import com.amazonaws.services.textract.model.S3Object;
   import com.amazonaws.services.textract.model.StartDocumentAnalysisRequest;
   import com.amazonaws.services.textract.model.StartDocumentAnalysisResult;
   import com.amazonaws.services.textract.model.StartDocumentTextDetectionRequest;
   import com.amazonaws.services.textract.model.StartDocumentTextDetectionResult;
   import com.fasterxml.jackson.databind.JsonNode;
   import com.fasterxml.jackson.databind.ObjectMapper;;
   public class DocumentProcessor {
   
       private static String sqsQueueName=null;
       private static String snsTopicName=null;
       private static String snsTopicArn = null;
       private static String roleArn= null;
       private static String sqsQueueUrl = null;
       private static String sqsQueueArn = null;
       private static String startJobId = null;
       private static String bucket = null;
       private static String document = null; 
       private static AmazonSQS sqs=null;
       private static AmazonSNS sns=null;
       private static AmazonTextract textract = null;
   
       public enum ProcessType {
           DETECTION,ANALYSIS
       }
   
       public static void main(String[] args) throws Exception {
           
           String document = "document";
           String bucket = "bucket";
           String roleArn="role";
   
           sns = AmazonSNSClientBuilder.defaultClient();
           sqs= AmazonSQSClientBuilder.defaultClient();
           textract=AmazonTextractClientBuilder.defaultClient();
           
           CreateTopicandQueue();
           ProcessDocument(bucket,document,roleArn,ProcessType.DETECTION);
           DeleteTopicandQueue();
           System.out.println("Done!");
           
           
       }
       // Creates an SNS topic and SQS queue. The queue is subscribed to the topic. 
       static void CreateTopicandQueue()
       {
           //create a new SNS topic
           snsTopicName="AmazonTextractTopic" + Long.toString(System.currentTimeMillis());
           CreateTopicRequest createTopicRequest = new CreateTopicRequest(snsTopicName);
           CreateTopicResult createTopicResult = sns.createTopic(createTopicRequest);
           snsTopicArn=createTopicResult.getTopicArn();
           
           //Create a new SQS Queue
           sqsQueueName="AmazonTextractQueue" + Long.toString(System.currentTimeMillis());
           final CreateQueueRequest createQueueRequest = new CreateQueueRequest(sqsQueueName);
           sqsQueueUrl = sqs.createQueue(createQueueRequest).getQueueUrl();
           sqsQueueArn = sqs.getQueueAttributes(sqsQueueUrl, Arrays.asList("QueueArn")).getAttributes().get("QueueArn");
           
           //Subscribe SQS queue to SNS topic
           String sqsSubscriptionArn = sns.subscribe(snsTopicArn, "sqs", sqsQueueArn).getSubscriptionArn();
           
           // Authorize queue
             Policy policy = new Policy().withStatements(
                     new Statement(Effect.Allow)
                     .withPrincipals(Principal.AllUsers)
                     .withActions(SQSActions.SendMessage)
                     .withResources(new Resource(sqsQueueArn))
                     .withConditions(new Condition().withType("ArnEquals").withConditionKey("aws:SourceArn").withValues(snsTopicArn))
                     );
                     
   
             Map queueAttributes = new HashMap();
             queueAttributes.put(QueueAttributeName.Policy.toString(), policy.toJson());
             sqs.setQueueAttributes(new SetQueueAttributesRequest(sqsQueueUrl, queueAttributes)); 
             
   
            System.out.println("Topic arn: " + snsTopicArn);
            System.out.println("Queue arn: " + sqsQueueArn);
            System.out.println("Queue url: " + sqsQueueUrl);
            System.out.println("Queue sub arn: " + sqsSubscriptionArn );
        }
       static void DeleteTopicandQueue()
       {
           if (sqs !=null) {
               sqs.deleteQueue(sqsQueueUrl);
               System.out.println("SQS queue deleted");
           }
           
           if (sns!=null) {
               sns.deleteTopic(snsTopicArn);
               System.out.println("SNS topic deleted");
           }
       }
       
       //Starts the processing of the input document.
       static void ProcessDocument(String inBucket, String inDocument, String inRoleArn, ProcessType type) throws Exception
       {
           bucket=inBucket;
           document=inDocument;
           roleArn=inRoleArn;
   
           switch(type)
           {
               case DETECTION:
                   StartDocumentTextDetection(bucket, document);
                   System.out.println("Processing type: Detection");
                   break;
               case ANALYSIS:
                   StartDocumentAnalysis(bucket,document);
                   System.out.println("Processing type: Analysis");
                   break;
               default:
                   System.out.println("Invalid processing type. Choose Detection or Analysis");
                   throw new Exception("Invalid processing type");
              
           }
   
           System.out.println("Waiting for job: " + startJobId);
           //Poll queue for messages
           List<Message> messages=null;
           int dotLine=0;
           boolean jobFound=false;
   
           //loop until the job status is published. Ignore other messages in queue.
           do{
               messages = sqs.receiveMessage(sqsQueueUrl).getMessages();
               if (dotLine++<40){
                   System.out.print(".");
               }else{
                   System.out.println();
                   dotLine=0;
               }
   
               if (!messages.isEmpty()) {
                   //Loop through messages received.
                   for (Message message: messages) {
                       String notification = message.getBody();
   
                       // Get status and job id from notification.
                       ObjectMapper mapper = new ObjectMapper();
                       JsonNode jsonMessageTree = mapper.readTree(notification);
                       JsonNode messageBodyText = jsonMessageTree.get("Message");
                       ObjectMapper operationResultMapper = new ObjectMapper();
                       JsonNode jsonResultTree = operationResultMapper.readTree(messageBodyText.textValue());
                       JsonNode operationJobId = jsonResultTree.get("JobId");
                       JsonNode operationStatus = jsonResultTree.get("Status");
                       System.out.println("Job found was " + operationJobId);
                       // Found job. Get the results and display.
                       if(operationJobId.asText().equals(startJobId)){
                           jobFound=true;
                           System.out.println("Job id: " + operationJobId );
                           System.out.println("Status : " + operationStatus.toString());
                           if (operationStatus.asText().equals("SUCCEEDED")){
                               switch(type)
                               {
                                   case DETECTION:
                                       GetDocumentTextDetectionResults();
                                       break;
                                   case ANALYSIS:
                                       GetDocumentAnalysisResults();
                                       break;
                                   default:
                                       System.out.println("Invalid processing type. Choose Detection or Analysis");
                                       throw new Exception("Invalid processing type");
                                  
                               }
                           }
                           else{
                               System.out.println("Document analysis failed");
                           }
   
                           sqs.deleteMessage(sqsQueueUrl,message.getReceiptHandle());
                       }
   
                       else{
                           System.out.println("Job received was not job " +  startJobId);
                           //Delete unknown message. Consider moving message to dead letter queue
                           sqs.deleteMessage(sqsQueueUrl,message.getReceiptHandle());
                       }
                   }
               }
               else {
                   Thread.sleep(5000);
               }
           } while (!jobFound);
   
           System.out.println("Finished processing document");
       }
       
       private static void StartDocumentTextDetection(String bucket, String document) throws Exception{
   
           //Create notification channel 
           NotificationChannel channel= new NotificationChannel()
                   .withSNSTopicArn(snsTopicArn)
                   .withRoleArn(roleArn);
   
           StartDocumentTextDetectionRequest req = new StartDocumentTextDetectionRequest()
                   .withDocumentLocation(new DocumentLocation()
                       .withS3Object(new S3Object()
                           .withBucket(bucket)
                           .withName(document)))
                   .withJobTag("DetectingText")
                   .withNotificationChannel(channel);
   
           StartDocumentTextDetectionResult startDocumentTextDetectionResult = textract.startDocumentTextDetection(req);
           startJobId=startDocumentTextDetectionResult.getJobId();
       }
       
     //Gets the results of processing started by StartDocumentTextDetection
       private static void GetDocumentTextDetectionResults() throws Exception{
           int maxResults=1000;
           String paginationToken=null;
           GetDocumentTextDetectionResult response=null;
           Boolean finished=false;
           
           while (finished==false)
           {
               GetDocumentTextDetectionRequest documentTextDetectionRequest= new GetDocumentTextDetectionRequest()
                       .withJobId(startJobId)
                       .withMaxResults(maxResults)
                       .withNextToken(paginationToken);
               response = textract.getDocumentTextDetection(documentTextDetectionRequest);
               DocumentMetadata documentMetaData=response.getDocumentMetadata();
   
               System.out.println("Pages: " + documentMetaData.getPages().toString());
               
               //Show blocks information
               List<Block> blocks= response.getBlocks();
               for (Block block : blocks) {
                   DisplayBlockInfo(block);
               }
               paginationToken=response.getNextToken();
               if (paginationToken==null)
                   finished=true;
               
           }
           
       }
   
       private static void StartDocumentAnalysis(String bucket, String document) throws Exception{
           //Create notification channel 
           NotificationChannel channel= new NotificationChannel()
                   .withSNSTopicArn(snsTopicArn)
                   .withRoleArn(roleArn);
           
           StartDocumentAnalysisRequest req = new StartDocumentAnalysisRequest()
                   .withFeatureTypes("TABLES","FORMS")
                   .withDocumentLocation(new DocumentLocation()
                       .withS3Object(new S3Object()
                           .withBucket(bucket)
                           .withName(document)))
                   .withJobTag("AnalyzingText")
                   .withNotificationChannel(channel);
   
           StartDocumentAnalysisResult startDocumentAnalysisResult = textract.startDocumentAnalysis(req);
           startJobId=startDocumentAnalysisResult.getJobId();
       }
       //Gets the results of processing started by StartDocumentAnalysis
       private static void GetDocumentAnalysisResults() throws Exception{
   
           int maxResults=1000;
           String paginationToken=null;
           GetDocumentAnalysisResult response=null;
           Boolean finished=false;
           
           //loops until pagination token is null
           while (finished==false)
           {
               GetDocumentAnalysisRequest documentAnalysisRequest= new GetDocumentAnalysisRequest()
                       .withJobId(startJobId)
                       .withMaxResults(maxResults)
                       .withNextToken(paginationToken);
               
               response = textract.getDocumentAnalysis(documentAnalysisRequest);
   
               DocumentMetadata documentMetaData=response.getDocumentMetadata();
   
               System.out.println("Pages: " + documentMetaData.getPages().toString());
   
               //Show blocks, confidence and detection times
               List<Block> blocks= response.getBlocks();
   
               for (Block block : blocks) {
                   DisplayBlockInfo(block);
               }
               paginationToken=response.getNextToken();
               if (paginationToken==null)
                   finished=true;
           }
   
       }
       //Displays Block information for text detection and text analysis
       private static void DisplayBlockInfo(Block block) {
           System.out.println("Block Id : " + block.getId());
           if (block.getText()!=null)
               System.out.println("\tDetected text: " + block.getText());
           System.out.println("\tType: " + block.getBlockType());
           
           if (block.getBlockType().equals("PAGE") !=true) {
               System.out.println("\tConfidence: " + block.getConfidence().toString());
           }
           if(block.getBlockType().equals("CELL"))
           {
               System.out.println("\tCell information:");
               System.out.println("\t\tColumn: " + block.getColumnIndex());
               System.out.println("\t\tRow: " + block.getRowIndex());
               System.out.println("\t\tColumn span: " + block.getColumnSpan());
               System.out.println("\t\tRow span: " + block.getRowSpan());
   
           }
           
           System.out.println("\tRelationships");
           List<Relationship> relationships=block.getRelationships();
           if(relationships!=null) {
               for (Relationship relationship : relationships) {
                   System.out.println("\t\tType: " + relationship.getType());
                   System.out.println("\t\tIDs: " + relationship.getIds().toString());
               }
           } else {
               System.out.println("\t\tNo related Blocks");
           }
   
           System.out.println("\tGeometry");
           System.out.println("\t\tBounding Box: " + block.getGeometry().getBoundingBox().toString());
           System.out.println("\t\tPolygon: " + block.getGeometry().getPolygon().toString());
           
           List<String> entityTypes = block.getEntityTypes();
           
           System.out.println("\tEntity Types");
           if(entityTypes!=null) {
               for (String entityType : entityTypes) {
                   System.out.println("\t\tEntity Type: " + entityType);
               }
           } else {
               System.out.println("\t\tNo entity type");
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
               System.out.println("\tPage: " + block.getPage());            
           System.out.println();
       }
   }
   ```

------
#### [ AWS CLI ]

   This AWS CLI command starts the asynchronous detection of text in a specified document\. It returns a `job-id` that can be used to retreive the results of the detection\. 

   ```
   aws textract start-document-text-detection --document-location 
   "{\"S3Object\":{\"Bucket\":\"bucket-name\",\"Name\":\"file-name\"}}" --region region-name
   ```

   This AWS CLI command returns the results for an Amazon Textract asynchronous operation when provided with a `job-id`\. 

   ```
   aws textract get-document-text-detection --region region-name --job-id job-id-number
   ```

   If you are accessing the CLI on a Windows device, use double quotes instead of single quotes and escape the inner double quotes by backslash \(i\.e\. \\\) to address any parser errors you may encounter\. For an example, see below

   ```
   aws textract start-document-text-detection --document-location "{\"S3Object\":{\"Bucket\":\"bucket\",\"Name\":\"document\"}}" --region region-name
   ```

    If you are analyzing a document with the StartDocumentAnalysis operation, you can provide values to the `feature-type` parameter\. The following example demonstrates how to include the `QUERIES` value in the `feature-types` parameter and then provide a `Queries` object to the `queries-config` parameter\. 

   ```
   aws textract start-document-analysis \ 
   --document '{"S3Object":{"Bucket":"bucket","Name":"document"}}'\
    --feature-types '["QUERIES"]' \
   --queries-config '{"Queries":[{"Text":"Question"}]}'
   ```

------
#### [ Python ]

   ```
   import boto3
   import json
   import sys
   import time
   
   
   class ProcessType:
       DETECTION = 1
       ANALYSIS = 2
   
   
   class DocumentProcessor:
       jobId = ''
       region_name = ''
   
       roleArn = ''
       bucket = ''
       document = ''
   
       sqsQueueUrl = ''
       snsTopicArn = ''
       processType = ''
   
       def __init__(self, role, bucket, document, region):
           self.roleArn = role
           self.bucket = bucket
           self.document = document
           self.region_name = region
   
           self.textract = boto3.client('textract', region_name=self.region_name)
           self.sqs = boto3.client('sqs', region_name=self.region_name)
           self.sns = boto3.client('sns', region_name=self.region_name)
   
       def ProcessDocument(self, type):
           jobFound = False
   
           self.processType = type
           validType = False
   
           # Determine which type of processing to perform
           if self.processType == ProcessType.DETECTION:
               response = self.textract.start_document_text_detection(
                   DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                   NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
               print('Processing type: Detection')
               validType = True
   
           # For document analysis, select which features you want to obtain with the FeatureTypes argument
           if self.processType == ProcessType.ANALYSIS:
               response = self.textract.start_document_analysis(
                   DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                   FeatureTypes=["TABLES", "FORMS"],
                   NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
               print('Processing type: Analysis')
               validType = True
   
           if validType == False:
               print("Invalid processing type. Choose Detection or Analysis.")
               return
   
           print('Start Job Id: ' + response['JobId'])
           dotLine = 0
           while jobFound == False:
               sqsResponse = self.sqs.receive_message(QueueUrl=self.sqsQueueUrl, MessageAttributeNames=['ALL'],
                                                      MaxNumberOfMessages=10)
   
               if sqsResponse:
   
                   if 'Messages' not in sqsResponse:
                       if dotLine < 40:
                           print('.', end='')
                           dotLine = dotLine + 1
                       else:
                           print()
                           dotLine = 0
                       sys.stdout.flush()
                       time.sleep(5)
                       continue
   
                   for message in sqsResponse['Messages']:
                       notification = json.loads(message['Body'])
                       textMessage = json.loads(notification['Message'])
                       print(textMessage['JobId'])
                       print(textMessage['Status'])
                       if str(textMessage['JobId']) == response['JobId']:
                           print('Matching Job Found:' + textMessage['JobId'])
                           jobFound = True
                           self.GetResults(textMessage['JobId'])
                           self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                                   ReceiptHandle=message['ReceiptHandle'])
                       else:
                           print("Job didn't match:" +
                                 str(textMessage['JobId']) + ' : ' + str(response['JobId']))
                       # Delete the unknown message. Consider sending to dead letter queue
                       self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                               ReceiptHandle=message['ReceiptHandle'])
   
           print('Done!')
   
       def CreateTopicandQueue(self):
   
           millis = str(int(round(time.time() * 1000)))
   
           # Create SNS topic
           snsTopicName = "AmazonTextractTopic" + millis
   
           topicResponse = self.sns.create_topic(Name=snsTopicName)
           self.snsTopicArn = topicResponse['TopicArn']
   
           # create SQS queue
           sqsQueueName = "AmazonTextractQueue" + millis
           self.sqs.create_queue(QueueName=sqsQueueName)
           self.sqsQueueUrl = self.sqs.get_queue_url(QueueName=sqsQueueName)['QueueUrl']
   
           attribs = self.sqs.get_queue_attributes(QueueUrl=self.sqsQueueUrl,
                                                   AttributeNames=['QueueArn'])['Attributes']
   
           sqsQueueArn = attribs['QueueArn']
   
           # Subscribe SQS queue to SNS topic
           self.sns.subscribe(
               TopicArn=self.snsTopicArn,
               Protocol='sqs',
               Endpoint=sqsQueueArn)
   
           # Authorize SNS to write SQS queue
           policy = """{{
     "Version":"2012-10-17",
     "Statement":[
       {{
         "Sid":"MyPolicy",
         "Effect":"Allow",
         "Principal" : {{"AWS" : "*"}},
         "Action":"SQS:SendMessage",
         "Resource": "{}",
         "Condition":{{
           "ArnEquals":{{
             "aws:SourceArn": "{}"
           }}
         }}
       }}
     ]
   }}""".format(sqsQueueArn, self.snsTopicArn)
   
           response = self.sqs.set_queue_attributes(
               QueueUrl=self.sqsQueueUrl,
               Attributes={
                   'Policy': policy
               })
   
       def DeleteTopicandQueue(self):
           self.sqs.delete_queue(QueueUrl=self.sqsQueueUrl)
           self.sns.delete_topic(TopicArn=self.snsTopicArn)
   
       # Display information about a block
       def DisplayBlockInfo(self, block):
   
           print("Block Id: " + block['Id'])
           print("Type: " + block['BlockType'])
           if 'EntityTypes' in block:
               print('EntityTypes: {}'.format(block['EntityTypes']))
   
           if 'Text' in block:
               print("Text: " + block['Text'])
   
           if block['BlockType'] != 'PAGE' and "Confidence" in str(block['BlockType']):
               print("Confidence: " + "{:.2f}".format(block['Confidence']) + "%")
   
           print('Page: {}'.format(block['Page']))
   
           if block['BlockType'] == 'CELL':
               print('Cell Information')
               print('\tColumn: {} '.format(block['ColumnIndex']))
               print('\tRow: {}'.format(block['RowIndex']))
               print('\tColumn span: {} '.format(block['ColumnSpan']))
               print('\tRow span: {}'.format(block['RowSpan']))
   
               if 'Relationships' in block:
                   print('\tRelationships: {}'.format(block['Relationships']))
   
           if ("Geometry") in str(block):
               print('Geometry')
               print('\tBounding Box: {}'.format(block['Geometry']['BoundingBox']))
               print('\tPolygon: {}'.format(block['Geometry']['Polygon']))
   
           if block['BlockType'] == 'SELECTION_ELEMENT':
               print('    Selection element detected: ', end='')
               if block['SelectionStatus'] == 'SELECTED':
                   print('Selected')
               else:
                   print('Not selected')
   
           if block["BlockType"] == "QUERY":
               print("Query info:")
               print(block["Query"])
           
           if block["BlockType"] == "QUERY_RESULT":
               print("Query answer:")
               print(block["Text"])        
                   
       def GetResults(self, jobId):
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
   
               response = None
   
               if self.processType == ProcessType.ANALYSIS:
                   if paginationToken == None:
                       response = self.textract.get_document_analysis(JobId=jobId,
                                                                      MaxResults=maxResults)
                   else:
                       response = self.textract.get_document_analysis(JobId=jobId,
                                                                      MaxResults=maxResults,
                                                                      NextToken=paginationToken)
   
               if self.processType == ProcessType.DETECTION:
                   if paginationToken == None:
                       response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults)
                   else:
                       response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults,
                                                                            NextToken=paginationToken)
   
               blocks = response['Blocks']
               print('Detected Document Text')
               print('Pages: {}'.format(response['DocumentMetadata']['Pages']))
   
               # Display block information
               for block in blocks:
                   self.DisplayBlockInfo(block)
                   print()
                   print()
   
               if 'NextToken' in response:
                   paginationToken = response['NextToken']
               else:
                   finished = True
   
       def GetResultsDocumentAnalysis(self, jobId):
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
   
               response = None
               if paginationToken == None:
                   response = self.textract.get_document_analysis(JobId=jobId,
                                                                  MaxResults=maxResults)
               else:
                   response = self.textract.get_document_analysis(JobId=jobId,
                                                                  MaxResults=maxResults,
                                                                  NextToken=paginationToken)
   
                   # Get the text blocks
               blocks = response['Blocks']
               print('Analyzed Document Text')
               print('Pages: {}'.format(response['DocumentMetadata']['Pages']))
               # Display block information
               for block in blocks:
                   self.DisplayBlockInfo(block)
                   print()
                   print()
   
                   if 'NextToken' in response:
                       paginationToken = response['NextToken']
                   else:
                       finished = True
   
   
   def main():
       roleArn = ''
       bucket = ''
       document = ''
       region_name = ''
   
       analyzer = DocumentProcessor(roleArn, bucket, document, region_name)
       analyzer.CreateTopicandQueue()
       analyzer.ProcessDocument(ProcessType.ANALYSIS)
       analyzer.DeleteTopicandQueue()
   
   
   if __name__ == "__main__":
       main()
   ```

   In order to use different features of the `AnalyzeDocument` operation, you provide the proper feature type to the `features-type` parameter\. For example, to use the Queries feature, include the `QUERIES` value in the `feature-types` parameter and then provide a `Queries` object to the `queries-config` parameter\. To query your document, replace the code block that makes a request to the `StartDocumentAnalysis` operation with the code block below, and enter your query\.

   ```
   if self.processType == ProcessType.ANALYSIS:
               response = self.textract.start_document_analysis(
                   DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                   FeatureTypes=["TABLES", "FORMS", "QUERIES"],
                                          QueriesConfig={'Queries':[
                                              {'Text':'{}'.format("Enter query here")}
                                          ]},
                   NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
   ```

------
#### [ Node\.JS ]

   In this example, replace the value of `roleArn` with the IAM role ARN that you saved in [Giving Amazon Textract Access to Your Amazon SNS Topic](api-async-roles.md#api-async-roles-all-topics)\. Replace the values of `bucket` and `document` with the bucket and document file name you specified in step 2 above\. Replace the value of `processType` with the type of processing you'd like to use on the input document\. Finally, replace the value of `REGION` with the region your client is operating in\.

   ```
    // snippet-start:[sqs.JavaScript.queues.createQueueV3]
   // Import required AWS SDK clients and commands for Node.js
   import { CreateQueueCommand, GetQueueAttributesCommand, GetQueueUrlCommand, 
       SetQueueAttributesCommand, DeleteQueueCommand, ReceiveMessageCommand, DeleteMessageCommand } from  "@aws-sdk/client-sqs";
     import {CreateTopicCommand, SubscribeCommand, DeleteTopicCommand } from "@aws-sdk/client-sns";
     import  { SQSClient } from "@aws-sdk/client-sqs";
     import  { SNSClient } from "@aws-sdk/client-sns";
     import  { TextractClient, StartDocumentTextDetectionCommand, StartDocumentAnalysisCommand, GetDocumentAnalysisCommand, GetDocumentTextDetectionCommand, DocumentMetadata } from "@aws-sdk/client-textract";
     import { stdout } from "process";
     
     // Set the AWS Region.
     const REGION = "us-east-1"; //e.g. "us-east-1"
     // Create SNS service object.
     const sqsClient = new SQSClient({ region: REGION });
     const snsClient = new SNSClient({ region: REGION });
     const textractClient = new TextractClient({ region: REGION });
     
     // Set bucket and video variables
     const bucket = "bucket-name";                                                                                                                  
     const documentName = "document-name";
     const roleArn = "role-arn"
     const processType = "DETECTION"
     var startJobId = ""
     
     var ts = Date.now();
     const snsTopicName = "AmazonTextractExample" + ts;
     const snsTopicParams = {Name: snsTopicName}
     const sqsQueueName = "AmazonTextractQueue-" + ts;
   
     // Set the parameters
     const sqsParams = {
       QueueName: sqsQueueName, //SQS_QUEUE_URL
       Attributes: {
         DelaySeconds: "60", // Number of seconds delay.
         MessageRetentionPeriod: "86400", // Number of seconds delay.
       },
     };
     
     // Process a document based on operation type
     const processDocumment = async (type, bucket, videoName, roleArn, sqsQueueUrl, snsTopicArn) =>
       {
       try
       {
           // Set job found and success status to false initially
         var jobFound = false
         var succeeded = false
         var dotLine = 0
         var processType = type
         var validType = false
   
         if (processType == "DETECTION"){
           var response = await textractClient.send(new StartDocumentTextDetectionCommand({DocumentLocation:{S3Object:{Bucket:bucket, Name:videoName}}, 
             NotificationChannel:{RoleArn: roleArn, SNSTopicArn: snsTopicArn}}))
           console.log("Processing type: Detection")
           validType = true
         }
   
         if (processType == "ANALYSIS"){
           var response = await textractClient.send(new StartDocumentAnalysisCommand({DocumentLocation:{S3Object:{Bucket:bucket, Name:videoName}}, 
             NotificationChannel:{RoleArn: roleArn, SNSTopicArn: snsTopicArn}}))
           console.log("Processing type: Analysis")
           validType = true
         }
   
         if (validType == false){
             console.log("Invalid processing type. Choose Detection or Analysis.")
             return
         }
       // while not found, continue to poll for response
       console.log(`Start Job ID: ${response.JobId}`)
       while (jobFound == false){
         var sqsReceivedResponse = await sqsClient.send(new ReceiveMessageCommand({QueueUrl:sqsQueueUrl, 
           MaxNumberOfMessages:'ALL', MaxNumberOfMessages:10}));
         if (sqsReceivedResponse){
           var responseString = JSON.stringify(sqsReceivedResponse)
           if (!responseString.includes('Body')){
             if (dotLine < 40) {
               console.log('.')
               dotLine = dotLine + 1
             }else {
               console.log('')
               dotLine = 0 
             };
             stdout.write('', () => {
               console.log('');
             });
             await new Promise(resolve => setTimeout(resolve, 5000));
             continue
           }
         }
   
           // Once job found, log Job ID and return true if status is succeeded
           for (var message of sqsReceivedResponse.Messages){
               console.log("Retrieved messages:")
               var notification = JSON.parse(message.Body)
               var rekMessage = JSON.parse(notification.Message)
               var messageJobId = rekMessage.JobId
               if (String(rekMessage.JobId).includes(String(startJobId))){
                   console.log('Matching job found:')
                   console.log(rekMessage.JobId)
                   jobFound = true
                   // GET RESUlTS FUNCTION HERE
                   var operationResults = await GetResults(processType, rekMessage.JobId)
                   //GET RESULTS FUMCTION HERE
                   console.log(rekMessage.Status)
               if (String(rekMessage.Status).includes(String("SUCCEEDED"))){
                   succeeded = true
                   console.log("Job processing succeeded.")
                   var sqsDeleteMessage = await sqsClient.send(new DeleteMessageCommand({QueueUrl:sqsQueueUrl, ReceiptHandle:message.ReceiptHandle}));
               }
               }else{
               console.log("Provided Job ID did not match returned ID.")
               var sqsDeleteMessage = await sqsClient.send(new DeleteMessageCommand({QueueUrl:sqsQueueUrl, ReceiptHandle:message.ReceiptHandle}));
               }
           }
   
       console.log("Done!")
       }
       }catch (err) {
           console.log("Error", err);
         }
     }
   
     // Create the SNS topic and SQS Queue
     const createTopicandQueue = async () => {
       try {
         // Create SNS topic
         const topicResponse = await snsClient.send(new CreateTopicCommand(snsTopicParams));
         const topicArn = topicResponse.TopicArn
         console.log("Success", topicResponse);
         // Create SQS Queue
         const sqsResponse = await sqsClient.send(new CreateQueueCommand(sqsParams));
         console.log("Success", sqsResponse);
         const sqsQueueCommand = await sqsClient.send(new GetQueueUrlCommand({QueueName: sqsQueueName}))
         const sqsQueueUrl = sqsQueueCommand.QueueUrl
         const attribsResponse = await sqsClient.send(new GetQueueAttributesCommand({QueueUrl: sqsQueueUrl, AttributeNames: ['QueueArn']}))
         const attribs = attribsResponse.Attributes
         console.log(attribs)
         const queueArn = attribs.QueueArn
         // subscribe SQS queue to SNS topic
         const subscribed = await snsClient.send(new SubscribeCommand({TopicArn: topicArn, Protocol:'sqs', Endpoint: queueArn}))
         const policy = {
           Version: "2012-10-17",
           Statement: [
             {
               Sid: "MyPolicy",
               Effect: "Allow",
               Principal: {AWS: "*"},
               Action: "SQS:SendMessage",
               Resource: queueArn,
               Condition: {
                 ArnEquals: {
                   'aws:SourceArn': topicArn
                 }
               }
             }
           ]
         };
     
         const response = sqsClient.send(new SetQueueAttributesCommand({QueueUrl: sqsQueueUrl, Attributes: {Policy: JSON.stringify(policy)}}))
         console.log(response)
         console.log(sqsQueueUrl, topicArn)
         return [sqsQueueUrl, topicArn]
     
       } catch (err) {
         console.log("Error", err);
   
       }
     }
   
     const deleteTopicAndQueue = async (sqsQueueUrlArg, snsTopicArnArg) => {
       const deleteQueue = await sqsClient.send(new DeleteQueueCommand({QueueUrl: sqsQueueUrlArg}));
       const deleteTopic = await snsClient.send(new DeleteTopicCommand({TopicArn: snsTopicArnArg}));
       console.log("Successfully deleted.")
     }
   
     const displayBlockInfo = async (block) => {
       console.log(`Block ID: ${block.Id}`)
       console.log(`Block Type: ${block.BlockType}`)
       if (String(block).includes(String("EntityTypes"))){
           console.log(`EntityTypes: ${block.EntityTypes}`)
       }
       if (String(block).includes(String("Text"))){
           console.log(`EntityTypes: ${block.Text}`)
       }
       if (!String(block.BlockType).includes('PAGE')){
           console.log(`Confidence: ${block.Confidence}`)
       }
       console.log(`Page: ${block.Page}`)
       if (String(block.BlockType).includes("CELL")){
           console.log("Cell Information")
           console.log(`Column: ${block.ColumnIndex}`)
           console.log(`Row: ${block.RowIndex}`)
           console.log(`Column Span: ${block.ColumnSpan}`)
           console.log(`Row Span: ${block.RowSpan}`)
           if (String(block).includes("Relationships")){
               console.log(`Relationships: ${block.Relationships}`)
           }
       }
   
       console.log("Geometry")
       console.log(`Bounding Box: ${JSON.stringify(block.Geometry.BoundingBox)}`)
       console.log(`Polygon: ${JSON.stringify(block.Geometry.Polygon)}`)
   
       if (String(block.BlockType).includes('SELECTION_ELEMENT')){
         console.log('Selection Element detected:')
         if (String(block.SelectionStatus).includes('SELECTED')){
           console.log('Selected')
         } else {
           console.log('Not Selected')
         }
   
       }
     }
   
     const GetResults = async (processType, JobID) => {
   
       var maxResults = 1000
       var paginationToken = null
       var finished = false
   
       while (finished == false){
         var response = null
         if (processType == 'ANALYSIS'){
           if (paginationToken == null){
             response = textractClient.send(new GetDocumentAnalysisCommand({JobId:JobID, MaxResults:maxResults}))
         
           }else{
             response = textractClient.send(new GetDocumentAnalysisCommand({JobId:JobID, MaxResults:maxResults, NextToken:paginationToken}))
           }
         }
           
         if(processType == 'DETECTION'){
           if (paginationToken == null){
             response = textractClient.send(new GetDocumentTextDetectionCommand({JobId:JobID, MaxResults:maxResults}))
         
           }else{
             response = textractClient.send(new GetDocumentTextDetectionCommand({JobId:JobID, MaxResults:maxResults, NextToken:paginationToken}))
           }
         }
   
         await new Promise(resolve => setTimeout(resolve, 5000));
         console.log("Detected Documented Text")
         console.log(response)
         //console.log(Object.keys(response))
         console.log(typeof(response))
         var blocks = (await response).Blocks
         console.log(blocks)
         console.log(typeof(blocks))
         var docMetadata = (await response).DocumentMetadata
         var blockString = JSON.stringify(blocks)
         var parsed = JSON.parse(JSON.stringify(blocks))
         console.log(Object.keys(blocks))
         console.log(`Pages: ${docMetadata.Pages}`)
         blocks.forEach((block)=> {
           displayBlockInfo(block)
           console.log()
           console.log()
         })
   
         //console.log(blocks[0].BlockType)
         //console.log(blocks[1].BlockType)
   
   
         if(String(response).includes("NextToken")){
           paginationToken = response.NextToken
         }else{
           finished = true
         }
       }
   
     }
   
   
     // DELETE TOPIC AND QUEUE
     const main = async () => {
       var sqsAndTopic = await createTopicandQueue();
       var process = await processDocumment(processType, bucket, documentName, roleArn, sqsAndTopic[0], sqsAndTopic[1])
       var deleteResults = await deleteTopicAndQueue(sqsAndTopic[0], sqsAndTopic[1])
     }
   
   main()
   ```

------

1. Run the code\. The operation might take a while to finish\. After it's finished, a list of blocks for detected or analyzed text is displayed\.