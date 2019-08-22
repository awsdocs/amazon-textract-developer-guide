# Calling Amazon Textract Synchronous Operations<a name="sync-calling"></a>

Amazon Textract operations process document images that are stored on a local file system, or document images stored in an Amazon S3 bucket\. You specify where the input document is located by using the [Document](API_Document.md) input parameter\. The document image can be in either PNG or JPEG format\.

For a complete example, see [Detecting Document Text with Amazon Textract](detecting-document-text.md)\.

## Request<a name="sync-request"></a>

### Documents Passed as Image Bytes<a name="sync-pass-image-bytes"></a>

You can pass a document image to an Amazon Textract operation by passing the image as a base64\-encoded byte array\. An example is a document image that's loaded from a local file system\. Your code might not need to encode document file bytes if you're using an AWS SDK to call Amazon Textract API operations\.

The image bytes are specified in the `Bytes` field of the `Document` input parameter\. The following example shows the input JSON for an Amazon Textract operation that passes the image bytes in the `Bytes` input parameter\.

```
{
    "Document": {
        "Bytes": "/9j/4AAQSk....."
    }
}
```

**Note**  
If you're using the AWS CLI, you can't pass image bytes to Amazon Textract operations\. Instead, you have to reference an image that's stored in an Amazon S3 bucket\.

The following code shows how to load an image from a local file system and call an Amazon Textract operation\. 

```
String document="input.png";

ByteBuffer imageBytes;
try (InputStream inputStream = new FileInputStream(new File(document))) {
    imageBytes = ByteBuffer.wrap(IOUtils.toByteArray(inputStream));
}
AmazonTextract client = AmazonTextractClientBuilder.defaultClient();

DetectDocumentTextRequest request = new DetectDocumentTextRequest()
        .withDocument(new Document()
                .withBytes(imageBytes));


DetectDocumentTextResult result = client.detectDocumentText(request);
```

### Documents Stored in an Amazon S3 Bucket<a name="sync-pass-s3"></a>

Amazon Textract can analyze document images that are stored in an Amazon S3 bucket\. You specify the bucket and file name by using the [S3Object](API_S3Object.md) field of the `Document` input parameter\. The following example shows the input JSON for an Amazon Textract operation that processes a document stored in an Amazon S3 bucket\. 

```
{
    "Document": {
        "S3Object": {
            "Bucket": "bucket",
            "Name": "input.png"
        }
    }
}
```

The following example shows how to call an Amazon Textract operation using an image stored in an Amazon S3 bucket\.

```
String document="input.png";
String bucket="bucket";

AmazonTextract client = AmazonTextractClientBuilder.defaultClient();

DetectDocumentTextRequest request = new DetectDocumentTextRequest()
        .withDocument(new Document()
                .withS3Object(new S3Object()
                        .withName(document)
                        .withBucket(bucket)));

DetectDocumentTextResult result = client.detectDocumentText(request);
```

## Response<a name="sync-response"></a>

The following sample is the JSON response from a call to `DetectDocumentText`\. For more information, see [Detecting Text](how-it-works-detecting.md)\.

```
{
    "Blocks": [
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 1.0, 
                    "Top": 0.0, 
                    "Left": 0.0, 
                    "Height": 1.0
                }, 
                "Polygon": [
                    {
                        "Y": 0.0, 
                        "X": 0.0
                    }, 
                    {
                        "Y": 0.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 0.0
                    }
                ]
            }, 
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "d7fbd604-d609-4d69-857d-247a3f591238", 
                        "b6c19a93-6493-4d8e-958f-853c8f7ca055"
                    ]
                }
            ], 
            "BlockType": "PAGE", 
            "Id": "56ec1d77-171f-4881-9852-2b5b7e761608"
        }, 
        {
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "7f97e2ca-063e-47a8-981c-8beee31afc01", 
                        "4b990aa0-af96-4369-b90f-dbe02538ed21"
                    ]
                }
            ], 
            "Confidence": 99.63229370117188, 
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.2522801458835602, 
                    "Top": 0.11217361688613892, 
                    "Left": 0.16102784872055054, 
                    "Height": 0.02862064726650715
                }, 
                "Polygon": [
                    {
                        "Y": 0.11217361688613892, 
                        "X": 0.16102784872055054
                    }, 
                    {
                        "Y": 0.11217361688613892, 
                        "X": 0.4133079946041107
                    }, 
                    {
                        "Y": 0.14079426229000092, 
                        "X": 0.4133079946041107
                    }, 
                    {
                        "Y": 0.14079426229000092, 
                        "X": 0.16102784872055054
                    }
                ]
            }, 
            "Text": "Hello, world.", 
            "BlockType": "LINE", 
            "Id": "d7fbd604-d609-4d69-857d-247a3f591238"
        }, 
        {
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "5a87995f-5ba0-4010-a2b1-ac1f100a6412", 
                        "f287dfbb-94fb-439c-9907-a431f9e2269e", 
                        "74cf9c3a-6aa6-427b-9e0c-f29741453e5f"
                    ]
                }
            ], 
            "Confidence": 99.64396667480469, 
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.26536890864372253, 
                    "Top": 0.1513642966747284, 
                    "Left": 0.1621238887310028, 
                    "Height": 0.02884846366941929
                }, 
                "Polygon": [
                    {
                        "Y": 0.1513642966747284, 
                        "X": 0.1621238887310028
                    }, 
                    {
                        "Y": 0.1513642966747284, 
                        "X": 0.42749279737472534
                    }, 
                    {
                        "Y": 0.18021276593208313, 
                        "X": 0.42749279737472534
                    }, 
                    {
                        "Y": 0.18021276593208313, 
                        "X": 0.1621238887310028
                    }
                ]
            }, 
            "Text": "How are you?", 
            "BlockType": "LINE", 
            "Id": "b6c19a93-6493-4d8e-958f-853c8f7ca055"
        }, 
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.11247961968183517, 
                    "Top": 0.11266519874334335, 
                    "Left": 0.16102784872055054, 
                    "Height": 0.02812906540930271
                }, 
                "Polygon": [
                    {
                        "Y": 0.11266519874334335, 
                        "X": 0.16102784872055054
                    }, 
                    {
                        "Y": 0.11266519874334335, 
                        "X": 0.2735074758529663
                    }, 
                    {
                        "Y": 0.14079426229000092, 
                        "X": 0.2735074758529663
                    }, 
                    {
                        "Y": 0.14079426229000092, 
                        "X": 0.16102784872055054
                    }
                ]
            }, 
            "Text": "Hello,", 
            "BlockType": "WORD", 
            "Confidence": 99.74746704101562, 
            "Id": "7f97e2ca-063e-47a8-981c-8beee31afc01"
        }, 
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.12675164639949799, 
                    "Top": 0.11217361688613892, 
                    "Left": 0.28655633330345154, 
                    "Height": 0.027762649580836296
                }, 
                "Polygon": [
                    {
                        "Y": 0.11217361688613892, 
                        "X": 0.28655633330345154
                    }, 
                    {
                        "Y": 0.11217361688613892, 
                        "X": 0.4133079946041107
                    }, 
                    {
                        "Y": 0.13993626832962036, 
                        "X": 0.4133079946041107
                    }, 
                    {
                        "Y": 0.13993626832962036, 
                        "X": 0.28655633330345154
                    }
                ]
            }, 
            "Text": "world.", 
            "BlockType": "WORD", 
            "Confidence": 99.5171127319336, 
            "Id": "4b990aa0-af96-4369-b90f-dbe02538ed21"
        }, 
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.09069254994392395, 
                    "Top": 0.15190091729164124, 
                    "Left": 0.1621238887310028, 
                    "Height": 0.02474799193441868
                }, 
                "Polygon": [
                    {
                        "Y": 0.15190091729164124, 
                        "X": 0.1621238887310028
                    }, 
                    {
                        "Y": 0.15190091729164124, 
                        "X": 0.25281643867492676
                    }, 
                    {
                        "Y": 0.17664891481399536, 
                        "X": 0.25281643867492676
                    }, 
                    {
                        "Y": 0.17664891481399536, 
                        "X": 0.1621238887310028
                    }
                ]
            }, 
            "Text": "How", 
            "BlockType": "WORD", 
            "Confidence": 99.76000213623047, 
            "Id": "f287dfbb-94fb-439c-9907-a431f9e2269e"
        }, 
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.06475766748189926, 
                    "Top": 0.15600404143333435, 
                    "Left": 0.2611062824726105, 
                    "Height": 0.020542677491903305
                }, 
                "Polygon": [
                    {
                        "Y": 0.15600404143333435, 
                        "X": 0.2611062824726105
                    }, 
                    {
                        "Y": 0.15600404143333435, 
                        "X": 0.32586392760276794
                    }, 
                    {
                        "Y": 0.17654670774936676, 
                        "X": 0.32586392760276794
                    }, 
                    {
                        "Y": 0.17654670774936676, 
                        "X": 0.2611062824726105
                    }
                ]
            }, 
            "Text": "are", 
            "BlockType": "WORD", 
            "Confidence": 99.70841217041016, 
            "Id": "5a87995f-5ba0-4010-a2b1-ac1f100a6412"
        }, 
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.092494897544384, 
                    "Top": 0.1513642966747284, 
                    "Left": 0.33499792218208313, 
                    "Height": 0.02884846366941929
                }, 
                "Polygon": [
                    {
                        "Y": 0.1513642966747284, 
                        "X": 0.33499792218208313
                    }, 
                    {
                        "Y": 0.1513642966747284, 
                        "X": 0.42749279737472534
                    }, 
                    {
                        "Y": 0.18021276593208313, 
                        "X": 0.42749279737472534
                    }, 
                    {
                        "Y": 0.18021276593208313, 
                        "X": 0.33499792218208313
                    }
                ]
            }, 
            "Text": "you?", 
            "BlockType": "WORD", 
            "Confidence": 99.46348571777344, 
            "Id": "74cf9c3a-6aa6-427b-9e0c-f29741453e5f"
        }
    ], 
    "DocumentMetadata": {
        "Pages": 1
    }
}
```