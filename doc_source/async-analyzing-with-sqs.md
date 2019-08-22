# Detecting or Analyzing Text in a Multipage Document<a name="async-analyzing-with-sqs"></a>

This procedure shows you how to detect or analyze text in a multipage document by using Amazon Textract detection operations, a document stored in an Amazon S3 bucket, an Amazon SNS topic, and an Amazon SQS queue\. Multipage document processing is an asynchronous operation\. For more information, see [Calling Amazon Textract Asynchronous Operations](api-async.md)\.

The procedure enables you to choose the type of processing that you want the code to do—text detection or text analysis\. If you choose text detection, [StartDocumentTextDetection](API_StartDocumentTextDetection.md) is called to start text detection\. The results are returned by calling [GetDocumentTextDetection](API_GetDocumentTextDetection.md)\. If you choose text analysis, [StartDocumentAnalysis](API_StartDocumentAnalysis.md) is called to start text analysis\. You get the results by calling [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. 

The processing results are returned in an array of [Block](API_Block.md) objects\. They are different depending on the type of processing\. For information about text detection blocks, see [Detecting Text](how-it-works-detecting.md)\. For text analysis blocks, see [Analyzing Text](how-it-works-analyzing.md)\.

The example code in the procedure shows you how to do the following steps: 

1. Create the Amazon SNS topic and the Amazon SQS queue\.

1. Subscribe the Amazon SQS queue to the Amazon SNS topic\.

1. Give permission to the Amazon SNS topic to send messages to the Amazon SQS queue\.

1. Start processing the document\. Start text detection by calling [StartDocumentTextDetection](API_StartDocumentTextDetection.md)\. Start text analysis by calling [StartDocumentAnalysis](API_StartDocumentAnalysis.md)\. 

1. Get the completion status from the Amazon SQS queue\. The example tracks the job identifier \(`JobId`\) that's returned by the `Start` operation\. It only gets the results for matching job identifiers that are read from the completion status\. This is an important consideration if other applications are using the same queue and topic\. For simplicity, the example deletes jobs that don't match\. Consider adding them to an Amazon SQS dead\-letter queue for further investigation\.

1. Get and display the processing results by calling [GetDocumentTextDetection](API_GetDocumentTextDetection.md) or [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\.

1. Delete the Amazon SNS topic and the Amazon SQS queue\.

## Detecting or Analyzing Text<a name="async-prerequisites"></a>

The example code for this procedure is provided in Java and Python\. You need to have the appropriate AWS SDK installed\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](setup-awscli-sdk.md)\. 

**To detect or analyze text in a multipage document**

1. Configure user access to Amazon Textract, and configure Amazon Textract access to Amazon SNS\. For more information, see [Configuring Amazon Textract for Asynchronous Operations](api-async-roles.md)\. You don't need to do steps 3, 4, 5, and 6 because the example code creates and configures the Amazon SNS topic and Amazon SQS queue\.

1. Upload a multipage document file in PDF format to your Amazon S3 bucket\. \(Single\-page documents in JPEG, PNG, or PDF format can also be processed\)\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. Use the following AWS SDK for Java or SDK for Python \(Boto 3\) code to either detect text or analyze text in a multipage document\. In the function `main`:
   + Replace the value of `roleArn` with the IAM role ARN that you saved in [Giving Amazon Textract Access to Your Amazon SNS Topic](api-async-roles.md#api-async-roles-all-topics)\. 
   + Replace the values of `bucket` and `document` with the bucket and document file name that you specified in step 2\. 
   + Replace the value of the `type` input parameter of the `ProcessDocument` function with the type of processing that you want to do\. Use `ProcessType.DETECTION` to detect text\. Use `ProcessType.ANALYSIS` to analyze text\. 

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
                               System.out.println("Video analysis failed");
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
       textract = boto3.client('textract')
       sqs = boto3.client('sqs')
       sns = boto3.client('sns')
   
       roleArn = ''   
       bucket = ''
       document = ''
       
       sqsQueueUrl = ''
       snsTopicArn = ''
       processType = ''
   
   
       def __init__(self, role, bucket, document):    
           self.roleArn = role
           self.bucket = bucket
           self.document = document    
   
    
       def ProcessDocument(self,type):
           jobFound = False
           
           self.processType=type
           validType=False
   
           #Determine which type of processing to perform
           if self.processType==ProcessType.DETECTION:
               response = self.textract.start_document_text_detection(DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                       NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
               print('Processing type: Detection')
               validType=True        
   
           
           if self.processType==ProcessType.ANALYSIS:
               response = self.textract.start_document_analysis(DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                   FeatureTypes=["TABLES", "FORMS"],
                   NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
               print('Processing type: Analysis')
               validType=True    
   
           if validType==False:
               print("Invalid processing type. Choose Detection or Analysis.")
               return
   
           print('Start Job Id: ' + response['JobId'])
           dotLine=0
           while jobFound == False:
               sqsResponse = self.sqs.receive_message(QueueUrl=self.sqsQueueUrl, MessageAttributeNames=['ALL'],
                                             MaxNumberOfMessages=10)
   
               if sqsResponse:
                   
                   if 'Messages' not in sqsResponse:
                       if dotLine<40:
                           print('.', end='')
                           dotLine=dotLine+1
                       else:
                           print()
                           dotLine=0    
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
   
           #Create SNS topic
           snsTopicName="AmazonTextractTopic" + millis
   
           topicResponse=self.sns.create_topic(Name=snsTopicName)
           self.snsTopicArn = topicResponse['TopicArn']
   
           #create SQS queue
           sqsQueueName="AmazonTextractQueue" + millis
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
   
           #Authorize SNS to write SQS queue 
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
               QueueUrl = self.sqsQueueUrl,
               Attributes = {
                   'Policy' : policy
               })
   
       def DeleteTopicandQueue(self):
           self.sqs.delete_queue(QueueUrl=self.sqsQueueUrl)
           self.sns.delete_topic(TopicArn=self.snsTopicArn)
   
       #Display information about a block
       def DisplayBlockInfo(self,block):
           
           print ("Block Id: " + block['Id'])
           print ("Type: " + block['BlockType'])
           if 'EntityTypes' in block:
               print('EntityTypes: {}'.format(block['EntityTypes']))
   
           if 'Text' in block:
               print("Text: " + block['Text'])
   
           if block['BlockType'] != 'PAGE':
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
       
           print('Geometry')
           print('\tBounding Box: {}'.format(block['Geometry']['BoundingBox']))
           print('\tPolygon: {}'.format(block['Geometry']['Polygon']))
           
           if block['BlockType'] == 'SELECTION_ELEMENT':
               print('    Selection element detected: ', end='')
               if block['SelectionStatus'] =='SELECTED':
                   print('Selected')
               else:
                   print('Not selected')  
   
   
       def GetResults(self, jobId):
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
   
               response=None
   
               if self.processType==ProcessType.ANALYSIS:
                   if paginationToken==None:
                       response = self.textract.get_document_analysis(JobId=jobId,
                           MaxResults=maxResults)
                   else: 
                       response = self.textract.get_document_analysis(JobId=jobId,
                           MaxResults=maxResults,
                           NextToken=paginationToken)                           
   
               if self.processType==ProcessType.DETECTION:
                   if paginationToken==None:
                       response = self.textract.get_document_text_detection(JobId=jobId,
                           MaxResults=maxResults)
                   else: 
                       response = self.textract.get_document_text_detection(JobId=jobId,
                           MaxResults=maxResults,
                           NextToken=paginationToken)   
   
               blocks=response['Blocks'] 
               print ('Detected Document Text')
               print ('Pages: {}'.format(response['DocumentMetadata']['Pages']))
           
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
   
               response=None
               if paginationToken==None:
                   response = self.textract.get_document_analysis(JobId=jobId,
                                               MaxResults=maxResults)
               else: 
                   response = self.textract.get_document_analysis(JobId=jobId,
                                               MaxResults=maxResults,
                                               NextToken=paginationToken)  
               
   
               #Get the text blocks
               blocks=response['Blocks']
               print ('Analyzed Document Text')
               print ('Pages: {}'.format(response['DocumentMetadata']['Pages']))
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
   
       analyzer=DocumentProcessor(roleArn, bucket,document)
       analyzer.CreateTopicandQueue()
       analyzer.ProcessDocument(ProcessType.DETECTION)
       analyzer.DeleteTopicandQueue()
   
   
   if __name__ == "__main__":
       main()
   ```

------

1. Build and run the code\. The operation might take a while to finish\. After it's finished, a list of blocks for detected or analyzed text is displayed\.