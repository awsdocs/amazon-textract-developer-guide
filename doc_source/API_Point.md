# Point<a name="API_Point"></a>

The X and Y coordinates of a point on a document page\. The X and Y values that are returned are ratios of the overall document page size\. For example, if the input document is 700 x 200 and the operation returns X=0\.5 and Y=0\.25, then the point is at the \(350,50\) pixel coordinate on the document page\.

An array of `Point` objects, `Polygon`, is returned as part of the [Geometry](API_Geometry.md) object that's returned in a [Block](API_Block.md) object\. A `Polygon` object represents a fine\-grained polygon around detected text, a key\-value pair, a table, a table cell, or a selection element\. 

## Contents<a name="API_Point_Contents"></a>

 **X**   <a name="Textract-Type-Point-X"></a>
The value of the X coordinate for a point on a `Polygon`\.  
Type: Float  
Required: No

 **Y**   <a name="Textract-Type-Point-Y"></a>
The value of the Y coordinate for a point on a `Polygon`\.  
Type: Float  
Required: No

## See Also<a name="API_Point_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/textract-2018-06-27/Point) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/textract-2018-06-27/Point) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/textract-2018-06-27/Point) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/textract-2018-06-27/Point) 