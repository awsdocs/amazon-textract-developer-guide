# Amazon Textract and interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-interface-endpoints"></a>

You can establish a private connection between your VPC and Amazon Textract by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), a technology that enables you to privately access Amazon Textract APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with Amazon Textract APIs\. Traffic between your VPC and Amazon Textract does not leave the AWS network\. 

Each interface endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in your subnets\. 

For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\. 

## Considerations for Amazon Textract VPC endpoints<a name="vpc-endpoint-considerations"></a>

Before you set up an interface VPC endpoint for Amazon Textract, ensure that you review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\. 

Amazon Textract supports making calls to all of its API actions from your VPC\. 

## Creating an interface VPC endpoint for Amazon Textract<a name="vpc-endpoint-create"></a>

You can create a VPC endpoint for the Amazon Textract service using either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

Create a VPC endpoint for Amazon Textract using the following service name: 
+ com\.amazonaws\.*region*\.textract \- For creating an endpoint for most Amazon Textract operations\.
+ com\.amazonaws\.*region*\.textract\-fips \- For creating an endpoint for Amazon Textract that complies with the Federal Information Processing Standard \(FIPS\) Publication 140\-2 US government standard\. 

If you enable private DNS for the endpoint, you can make API requests to Amazon Textract using its default DNS name for the Region, for example, `textract.us-east-1.amazonaws.com`\.

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

## Creating a VPC endpoint policy for Amazon Textract<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to Amazon Textract\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

**Example: VPC endpoint policy for Amazon Textract actions**  
The following is an example of an endpoint policy for Amazon Textract\. When attached to an endpoint, this policy grants access to the listed Amazon Textract actions for all principals on all resources\.

 This example policy allows access to only the operations `DetectDocumentText` and `AnalyzeDocument`\. Users can still call Amazon Textract operations from outside the VPC Endpoint\. 

```
   "Statement":[
      {
         "Principal":"*",
         "Effect":"Allow",
         "Action":[
            "textract:DetectDocumentText",
            "textract:AnalyzeDocument",
         ],
         "Resource":"*"
      }
   ]
}
```