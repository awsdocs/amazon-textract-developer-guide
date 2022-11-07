# Extracting and Sending Text to AWS Comprehend for Analysis<a name="textract-to-comprehend"></a>

Amazon Textract lets you include document text detection and analysis in your applications\. With Amazon Textract you can extract text from a variety of different document types using both synchronous and asynchronous document processing\. The extracted text can then be saved to a file or database, or sent to another AWS service for further processing\. 

In this tutorial you carry out a common end\-to\-end workflow\. This workflow involves:
+ Processing numerous input documents with Amazon Textract
+ Providing the extracted text to Amazon Comprehend for analysis
+ Saving both the analyzed text and the analysis data to an Amazon Simple Storage Service \(S3\) bucket

You use the [AWS SDK for Python](https://aws.amazon.com/sdk-for-python/) for this tutorial\. You can also see the AWS Documentation SDK examples [GitHub repo ](https://github.com/awsdocs/aws-doc-sdk-examples)for more Python tutorials\. 

## Prerequisites<a name="tutorial-prerequisites"></a>

Before you begin this tutorial, you’ll need to install Python and complete the steps required to [set up the Python AWS SDK](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html)\. Beyond this, ensure that you have:
+ [Created an AWS account and an IAM role](https://docs.aws.amazon.com/rekognition/latest/dg/setting-up.html)
+ [Properly configured your AWS access credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
+ [Created an Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)
+ [ Configured Amazon Textract for Asynchronous processing](https://docs.aws.amazon.com/en_us/textract/latest/dg/api-async-roles.html), copying down the Amazon Resource Number \(ARN\) of the IAM role you configured for use with Amazon Textract
+ [Granted your IAM role access to Amazon Comprehend](https://docs.aws.amazon.com/comprehend/latest/dg/access-control-managing-permissions.html) 
+ Selected a few documents for the purposes of text extraction/analysis and uploaded the document to Amazon S3\. Ensure that the files you select for analysis are of the formats supported by Amazon Textract\.

## Starting Asynchronous Document Text Detection<a name="tutorial-step-1"></a>

You can extract the text from your documents and then analyze the extracted text with a service like Amazon Comprehend\. Textract supports the extraction of text from multipage documents through asynchronous operations, which are for processing large, multipage documents\. Processing a PDF file asynchronously allows your application to complete other tasks while it waits for the process to complete\. This section will demonstrate how to import your documents from an Amazon S3 bucket and provide them to Textract’s asynchronous text detection operation\. 

This tutorial assumes that you will be using Amazon S3 to store the files you want to extract text from\. You’ll start by creating a class and functions that detect the text in your input documents\. Your application will need to connect to the Textract client, as well as the Amazon SQS and Amazon SNS clients for the purposes of monitoring the completion status of the asynchronous job\.

1. Start by writing the code to create an Amazon SNS topic and Amazon SQS queue\.

   The following code sample creates a `DocumentProcessor` class that connects to the three required services and then creates both an Amazon SQS queue and Amazon SNS topic\. The Amazon SNS topic is used to provide information about the job completion status to an Amazon SQS queue, which will be polled to obtain the completion status of a job\. There are also methods to delete the Amazon SQS queue and Amazon SNS topic once the job has been completed and the resources are no longer needed\.

   ```
   import boto3
   import json
   import sys
   import time
   
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
   
           # Instantiates necessary AWS clients
           self.textract = boto3.client('textract', region_name=self.region_name)
           self.sqs = boto3.client('sqs', region_name=self.region_name)
           self.sns = boto3.client('sns', region_name=self.region_name)
   
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
   ```

1. Write the code to call the `StartDocumentTextDetection` operation and get the results of the operation\.

   The `DocumentProcessor` class will also need methods to: 
   + Call the `StartDocumentTextDetection` operation
   + Poll an Amazon SQS for the job completion status
   + Retrieve the results of the job once it is done processing

   The following code creates the `ProcessDocument` and `GetResults` methods that call `StartDocumentTextDetection` and gets the extracted text, respectively\.

   ```
       def ProcessDocument(self):
       
               # Checks if job found
               jobFound = False
       
               # Starts the text detection operation on the documents in the provided bucket
               # Sends status to supplied SNS topic arn
               response = self.textract.start_document_text_detection(
                       DocumentLocation={'S3Object': {'Bucket': self.bucket, 'Name': self.document}},
                       NotificationChannel={'RoleArn': self.roleArn, 'SNSTopicArn': self.snsTopicArn})
               print('Processing type: Detection')
       
               print('Start Job Id: ' + response['JobId'])
               dotLine = 0
               while jobFound == False:
                   sqsResponse = self.sqs.receive_message(QueueUrl=self.sqsQueueUrl, MessageAttributeNames=['ALL'],
                                                       MaxNumberOfMessages=10)
       
                   # Waits until messages are found in the SQS queue
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
       
                       # Checks for a completed job that matches the jobID in the response from
                       # StartDocumentTextDetection
                       for message in sqsResponse['Messages']:
                           notification = json.loads(message['Body'])
                           textMessage = json.loads(notification['Message'])
                           if str(textMessage['JobId']) == response['JobId']:
                               print('Matching Job Found:' + textMessage['JobId'])
                               jobFound = True
                               text_data = self.GetResults(textMessage['JobId'])
                               self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                                       ReceiptHandle=message['ReceiptHandle'])
                               return text_data
                           else:
                               print("Job didn't match:" +
                                   str(textMessage['JobId']) + ' : ' + str(response['JobId']))
                           # Delete the unknown message. Consider sending to dead letter queue
                           self.sqs.delete_message(QueueUrl=self.sqsQueueUrl,
                                                   ReceiptHandle=message['ReceiptHandle'])
       
               print('Done!')
           
       # gets the results of the completed text detection job
       # checks for pagination tokens to determine if there are multiple pages in the input doc
       def GetResults(self, jobId):
           maxResults = 1000
           paginationToken = None
           finished = False
   
           while finished == False:
               response = None
               if paginationToken == None:
                   response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults)
               else:
                   response = self.textract.get_document_text_detection(JobId=jobId,
                                                                            MaxResults=maxResults,
                                                                            NextToken=paginationToken)
   
               blocks = response['Blocks']
   
               # List to hold detected text
               detected_text = []
   
               # Display block information and add detected text to list
               for block in blocks:
                   if 'Text' in block and block['BlockType'] == "LINE":
                       detected_text.append(block['Text'])
   
               # If response contains a next token, update pagination token
               if 'NextToken' in response:
                   paginationToken = response['NextToken']
               else:
                   finished = True
   
               return detected_text
   ```

1. Save the above code in a file called `detectFileAsync.py`\.

   You use this file in the next section to handle the detection of text in your input documents\.

## Processing Your Documents and Sending the Text to Comprehend<a name="tutorial-step-2"></a>

Your application will use the class you created in the preceding section to:
+ read documents from your Amazon S3 bucket
+ extract the text in those documents
+ send the text to Amazon Comprehend for analysis

You start by creating some functions that utilize Amazon Comprehend to analyze the text detected in your input documents\. A common type of text analysis is sentiment analysis, which aims to capture the affect of a statement \(whether it is positive, negative, or neutral\)\. You can also carry out entity detection and key phrase detection on the data\.

The code below takes in the detected text and invokes the `BatchDetectSentiment` operation from Amazon Comprehend in order to carry out sentiment analysis\.

1. Write the code to carry out sentiment analysis on your detected text\.

   ```
   from detectFileAsync import DocumentProcessor
   import boto3
   import pandas as pd
   
   # Detect sentiment
   def sentiment_analysis(detected_text, lang):
   
       comprehend = boto3.client("comprehend")
   
       detect_sent_response = comprehend.batch_detect_sentiment(
               TextList=detected_text, LanguageCode=lang)
   
       # Lists to hold sentiment labels and sentiment scores
       sentiments = []
       pos_score = []
       neg_score = []
       neutral_score = []
       mixed_score = []
   
       # for all results add the Sentiment label and sentiment scores to lists
       for res in detect_sent_response['ResultList']:
           sentiments.append(res['Sentiment'])
           print(res['SentimentScore'])
           print(type(res['SentimentScore']))
           for key, val in res['SentimentScore'].items():
               if key == "Positive":
                   pos_score.append(val)
               if key == "Negative":
                   neg_score.append(val)
               if key == "Neutral":
                   neutral_score.append(val)
               if key == "Mixed":
                   mixed_score.append(val)
   
       return sentiments, pos_score, neg_score, neutral_score, mixed_score
   ```

   You may also want to perform other analysis operations, such as entity detection or key phrase detection, on your detected text\. You can write the functions to carry out these analysis operations on your text, just like you did for the preceding sentiment analysis operation\.

1. Write the code to carry out entity detection on your detected text\.

   ```
   # detect entities
   def entity_detection(detected_text, lang):
   
       comprehend = boto3.client("comprehend")
   
       # convert and handle string here
       # do string handling
       detect_ent_response = comprehend.batch_detect_entities(
           TextList=detected_text, LanguageCode=lang)
   
       # To fold detected entities and entity types
       ents = []
       types = []
   
       # Get detected entities and types from the response returned by Comprehend
       for i in detect_ent_response['ResultList']:
           if len(i['Entities']) == 0:
               ents.append("N/A")
               types.append("N/A")
           else:
               sentence_ents = []
               sentence_types = []
               for entities in i['Entities']:
                   sentence_ents.append(entities['Text'])
                   sentence_types.append(entities['Type'])
               ents.append(sentence_ents)
               types.append(sentence_types)
   
       return ents, types
   ```

1.  Write the code to carry out key phrase detection on your detected text\.

   ```
   # Detect key phrases
   def key_phrases_detection(detected_text, lang):
   
       comprehend = boto3.client("comprehend")
   
       key_phrases = []
       detect_phrases_response = comprehend.batch_detect_key_phrases(
           TextList=detected_text, LanguageCode=lang)
       for i in detect_phrases_response['ResultList']:
           if len(i['KeyPhrases']) == 0:
               key_phrases.append("N/A")
           else:
               phrases = []
               for phrase in i['KeyPhrases']:
                   phrases.append(phrase['Text'])
               key_phrases.append(phrases)
   
       return key_phrases
   ```

   You need to create a function that invokes all of the code you’ve created so far\. The function will use the `DocumentProcessor` class you created in your `DetectAnalyzeFileAsync.py` file, and then save the detected text to a variable for input into the three functions utilizing Amazon Comprehend that you previously wrote\. The function will also need to construct a Pandas dataframe, into which the detected text and analysis data will be inserted\. Finally, the Pandas dataframe will be saved as a CSV file\.

1. Write the code to process your input documents with Textract and pass the detected text to Comprehend\.

   ```
   def process_document(roleArn, bucket, document, region_name):
   
       # Create analyzer class from DocumentProcessor, create a topic and queue, use Textract to get text,
       # then delete topica and queue
       analyzer = DocumentProcessor(roleArn, bucket, document, region_name)
       analyzer.CreateTopicandQueue()
       extracted_text = analyzer.ProcessDocument()
       analyzer.DeleteTopicandQueue()
   
       # detect dominant language
       comprehend = boto3.client("comprehend")
       response = comprehend.detect_dominant_language(Text=str(extracted_text[:10]))
       print(response)
       print(type(response))
       lang = ""
       for i in response['Languages']:
           lang = i['LanguageCode']
       print(lang)
   
       # or you can enter language code below
       # lang = "en"
   
       print("Lines in detected text:" + str(len(extracted_text)))
       sliced_list = []
       start = 0
       end = 24
       while end < len(extracted_text):
           sliced_list.append(extracted_text[start:end])
           start += 25
           end += 25
       print(sliced_list)
   
       # Create lists to hold analytics data, these will be turned into columns
       all_sents = []
       all_scores = []
       all_ents = []
       all_types = []
       all_key_phrases = []
       all_pos_ratings = []
       all_neg_ratings = []
       all_neutral_ratings = []
       all_mixed_ratings = []
   
       # For every slice, get sentiment analysis, entity detection and key phrases, append results to lists
       for slice in sliced_list:
           slice_labels, pos_ratings, neg_ratings, neutral_ratings, mixed_ratings = sentiment_analysis(slice, lang)
           all_sents.append(slice_labels)
           all_pos_ratings.append(pos_ratings)
           all_neg_ratings.append(neg_ratings)
           all_neutral_ratings.append(neutral_ratings)
           all_mixed_ratings.append(mixed_ratings)
           slice_ents, slice_types = entity_detection(slice, lang)
           all_ents.append(slice_ents)
           all_types.append(slice_types)
           key_phrases = key_phrases_detection(slice, lang)
           all_key_phrases.append(key_phrases)
   
       # List comprehension to flatten multiple lists into a single list
       extracted_text = [line for sublist in sliced_list for line in sublist]
       all_sents = [sent for sublist in all_sents for sent in sublist]
       all_scores = [score for sublist in all_scores for score in sublist]
       all_ents = [ents for sublist in all_ents for ents in sublist]
       all_types = [types for sublist in all_types for types in sublist]
       all_key_phrases = [kp for sublist in all_key_phrases for kp in sublist]
       all_mixed_ratings = [kp for sublist in all_mixed_ratings for kp in sublist]
       all_pos_ratings = [kp for sublist in all_pos_ratings for kp in sublist]
       all_neg_ratings = [kp for sublist in all_neg_ratings for kp in sublist]
       all_neutral_ratings = [kp for sublist in all_neutral_ratings for kp in sublist]
   
       print(len(extracted_text))
       print(len(all_sents))
       print(len(all_ents))
       print(len(all_types))
       print(len(all_key_phrases))
   
       print("List of Recognized Entities:")
   
       # Create dataframe and save as CSV
       df = pd.DataFrame({'Sentences':extracted_text, 'Sentiment':all_sents, 'SentPosScore':all_pos_ratings,
                          'SentNegScore':all_neg_ratings, 'SentNeutralScore':all_neutral_ratings, 'SentMixedRatings':all_mixed_ratings,
                          'Entities':all_ents, 'EntityTypes':all_types,'KeyPhrases:':all_key_phrases})
       analysis_results = str(document.replace(".","_") + "_" + "analysis" + ".csv")
       df.to_csv(analysis_results, index=False)
   
       print(df)
       print("Data written to file!")
   
       return extracted_text, analysis_results
   ```

1. Write the code to process your documents and upload the resulting data to S3\. In the code sample below, replace the value of `roleArn` with the ARN of the role you configured for use with Amazon Textract\. Replace the value of `region_name` with the region your account is operating in\. Finally, replace the value `bucket_name` with the name of the S3 bucket containing your documents\.

   ```
   def main():
   
       # Initialize S3 client and set RoleArn, region name, and bucket name
       s3 = boto3.client("s3")
       roleArn = ''
       region_name = ''
       bucket_name = ''
   
       # initialize global corpus
       full_corpus = []
   
       # to hold all docs in bucket
       docs_list = []
   
       # loop through docs in bucket, get names of all docs
       s3_resource = boto3.resource("s3")
       bucket = s3_resource.Bucket(bucket_name)
       for bucket_object in bucket.objects.all():
           docs_list.append(bucket_object.key)
       print(docs_list)
   
       # For all the docs in the bucket, invoke document processing function,
       # add detected text to corpus of all text in batch docs,
       # and save CSV of comprehend analysis data and textract detected to S3
       for i in docs_list:
           detected_text, analysis_results = process_document(roleArn, bucket_name, i, region_name)
           full_corpus.append(detected_text)
           print("Uploading file: {}".format(str(analysis_results)))
           name_of_file = str(analysis_results)
           s3.upload_file(name_of_file, bucket_name, name_of_file)
   
       # print the global corpus
       print(full_corpus)
   
   if __name__ == "__main__":
       main()
   ```

1. Put the preceding code in the section into a Python file and run it\. 

You have successfully extracted text using Amazon Textract, sent the text to Amazon Comprehend for analysis, and then saved the results in a Amazon S3 bucket\.
