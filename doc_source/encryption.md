# Encryption in Amazon Textract<a name="encryption"></a>

Data encryption refers to protecting data while in transit and at rest\. You can protect your data by using Amazon S3\-Managed Keys or AWS KMS key at rest, alongside standard Transport Layer Security while in transit\.

## Encryption at Rest<a name="encryption-at-rest"></a>

The primary method of encrypting data in Amazon Textract is server\-side encryption\. Input documents passed from Amazon S3 buckets are encrypted by Amazon S3 and decrypted when you access them\. As long as you authenticate your request and you have access permissions, there is no difference in the way you access encrypted or unencrypted objects\. For example, if you share your objects using a presigned URL, that URL works the same way for both encrypted and unencrypted objects\. Additionally, when you list objects in your bucket, the `List` API returns a list of all objects, regardless of whether they are encrypted\. 

Amazon Textract uses two mutually exclusive methods of server\-side encryption\.

**Server\-Side Encryption with Amazon S3\-Managed Keys \(SSE\-S3\)**

When you use server\-side encryption with Amazon S3\-Managed Keys \(SSE\-S3\), each object is encrypted with a unique key\. As an additional safeguard, this method encrypts the key itself with a master key that it regularly rotates\. Amazon S3 server\-side encryption uses one of the strongest block ciphers available, 256\-bit Advanced Encryption Standard \(AES\-256\), to encrypt your data\. For more information, see Protecting Data Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)\. 

**Server\-Side Encryption with KMS keys Stored in AWS Key Management Service \(SSE\-KMS\)**

Server\-side encryption with KMS keys stored in AWS Key Management Service \(SSE\-KMS\) is similar to SSE\-S3, but with some additional benefits and charges for using this service\. There are separate permissions for the use of a KMS key that provides added protection against unauthorized access of your objects in Amazon S3\. SSE\-KMS also provides you with an audit trail that shows when your KMS key was used and by whom\. Additionally, you can create and manage KMS keys or use AWS managed keys that are unique to you, your service, and your Region\. For more information, see Protecting Data Using Server\-Side Encryption with KMS keys Stored in AWS Key Management Service \(SSE\-KMS\)\. 

## Encryption in Transit<a name="encryption-in-transit"></a>

For data in transit, Amazon Textract uses Transport Layer Security \(TLS\) to encrypt data sent between the service and the agent\. Additionally, Amazon Textract uses VPC endpoints to send data between the various microservices used when Amazon Textract processes a document\.