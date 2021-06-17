# BoundingBox<a name="API_BoundingBox"></a>

The bounding box around the detected page, text, key\-value pair, table, table cell, or selection element on a document page\. The `left` \(x\-coordinate\) and `top` \(y\-coordinate\) are coordinates that represent the top and left sides of the bounding box\. Note that the upper\-left corner of the image is the origin \(0,0\)\. 

The `top` and `left` values returned are ratios of the overall document page size\. For example, if the input image is 700 x 200 pixels, and the top\-left coordinate of the bounding box is 350 x 50 pixels, the API returns a `left` value of 0\.5 \(350/700\) and a `top` value of 0\.25 \(50/200\)\.

The `width` and `height` values represent the dimensions of the bounding box as a ratio of the overall document page dimension\. For example, if the document page size is 700 x 200 pixels, and the bounding box width is 70 pixels, the width returned is 0\.1\. 

## Contents<a name="API_BoundingBox_Contents"></a>

 **Height**   <a name="Textract-Type-BoundingBox-Height"></a>
The height of the bounding box as a ratio of the overall document page height\.  
Type: Float  
Required: No

 **Left**   <a name="Textract-Type-BoundingBox-Left"></a>
The left coordinate of the bounding box as a ratio of overall document page width\.  
Type: Float  
Required: No

 **Top**   <a name="Textract-Type-BoundingBox-Top"></a>
The top coordinate of the bounding box as a ratio of overall document page height\.  
Type: Float  
Required: No

 **Width**   <a name="Textract-Type-BoundingBox-Width"></a>
The width of the bounding box as a ratio of the overall document page width\.  
Type: Float  
Required: No

## See Also<a name="API_BoundingBox_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/BoundingBox) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/BoundingBox) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/BoundingBox) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/BoundingBox) 