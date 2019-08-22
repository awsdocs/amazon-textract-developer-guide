# Item Location on a Document Page<a name="text-location"></a>

Amazon Textract operations return the location and geometry of items found on a document page\. [DetectDocumentText](API_DetectDocumentText.md) and [GetDocumentTextDetection](API_GetDocumentTextDetection.md) return the location and geometry for lines and words, while [AnalyzeDocument](API_AnalyzeDocument.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md) return the location and geometry of key\-value pairs, tables, cells, and selection elements\.

To determine where an item is on a document page, use the bounding box \([Geometry](API_Geometry.md)\) information that's returned by the Amazon Textract operation in a [Block](API_Block.md) object\. The `Geometry` object contains two types of location and geometric information for detected items:
+ An axis\-aligned [BoundingBox](API_BoundingBox.md) object that contains the top\-left coordinate and the width and height of the item\.
+ A polygon object that describes the outline of the item, specified as an array of [Point](API_Point.md) objects that contain `X` \(horizontal axis\) and `Y` \(vertical axis\) document page coordinates of each point\.

The JSON for a `Block` object looks similar to the following\. Note the `BoundingBox` and `Polygon` fields\.

```
{
    "Geometry": {
        "BoundingBox": {
            "Width": 0.053907789289951324, 
            "Top": 0.08913730084896088, 
            "Left": 0.11085548996925354, 
            "Height": 0.013171200640499592
        }, 
        "Polygon": [
            {
                "Y": 0.08985357731580734, 
                "X": 0.11085548996925354
            }, 
            {
                "Y": 0.08913730084896088, 
                "X": 0.16447919607162476
            }, 
            {
                "Y": 0.10159222036600113, 
                "X": 0.16476328670978546
            }, 
            {
                "Y": 0.10230850428342819, 
                "X": 0.11113958805799484
            }
        ]
    }, 
    "Text": "Name:", 
    "BlockType": "WORD", 
    "Confidence": 99.56285858154297, 
    "Id": "c734fca6-c4c4-415c-b6c1-30f7510b72ee"
},
```

You can use geometry information to draw bounding boxes around detected items\. For an example that uses `BoundingBox` and `Polygon` information to draw boxes around lines and vertical lines at the start and end of each word, see [Detecting Document Text with Amazon Textract](detecting-document-text.md)\. The example output is similar to the following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/janedoe.png)

## Bounding Box<a name="bounding-box"></a>

A bounding box \(`BoundingBox`\) has the following properties:
+ Height – The height of the bounding box as a ratio of the overall document page height\.
+ Left – The X coordinate of the top\-left point of the bounding box as a ratio of the overall document page width\.
+ Top – The Y coordinate of the top\-left point of the bounding box as a ratio of the overall document page height\.
+ Width – The width of the bounding box as a ratio of the overall document page width\.

Each BoundingBox property has a value between 0 and 1\. The value is a ratio of the overall image width \(applies to `Left` and `Width`\) or height \(applies to `Height` and `Top`\)\. For example, if the input image is 700 x 200 pixels, and the top\-left coordinate of the bounding box is \(350,50\) pixels, the API returns a `Left` value of 0\.5 \(350/700\) and a `Top` value of 0\.25 \(50/200\)\. 

The following diagram shows the range of a document page that each BoundingBox property covers\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/bounding-box.png)

To display the bounding box with the correct location and size, you have to multiply the BoundingBox values by the document page width or height \(depending on the value you want\) to get the pixel values\. You use the pixel values to display the bounding box\. An example is using a document page of 608 pixels width x 588 pixels height, and the following bounding box values for analyzed text: 

```
BoundingBox.Left: 0.3922065
Bounding.Top: 0.15567766
BoundingBox.Width: 0.284666
BoundingBox.Height: 0.2930403
```

The location of the text bounding box in pixels is calculated as follows: 

`Left coordinate = BoundingBox.Left (0.3922065) * document page width (608) = 238`

`Top coordinate = BoundingBox.Top (0.15567766) * document page height (588) = 91`

`Bounding box width = BoundingBox.Width (0.284666) * document page width (608) = 173`

`Bounding box height = BoundingBox.Height (0.2930403) * document page height (588) = 172`

You use these values to display a bounding box around the analyzed text\. The following example shows how to display a bounding box\.

```
    public void ShowBoundingBox(int imageHeight, int imageWidth, BoundingBox box, Graphics2D g2d) {

        float left = imageWidth * box.getLeft();
        float top = imageHeight * box.getTop();

        // Display bounding box.
        g2d.setColor(new Color(0, 212, 0));
        g2d.drawRect(Math.round(left / scale), Math.round(top / scale),
                Math.round((imageWidth * box.getWidth()) / scale), Math.round((imageHeight * box.getHeight())) / scale);

    }
```

## Polygon<a name="polygon"></a>

The polygon that's returned by `AnalyzeDocument` is an array of [Point](API_Point.md) objects\. Each `Point` has an X and Y coordinate for a specific location on the document page\. Like the BoundingBox coordinates, the polygon coordinates are normalized to the document width and height, and are between 0 and 1\. 

You can use points in the polygon array to display a finer\-grain bounding box around a `Block` object\. You calculate the position of each polygon point on the document page by using the same technique used for `BoundingBoxes`\. Multiply the X coordinate by the document page width, and multiply the Y coordinate by the document page height\.

The following example shows how to display the vertical lines of a polygon\.

```
    public void ShowPolygonVerticals(int imageHeight, int imageWidth, List <Point> points, Graphics2D g2d) {

        g2d.setColor(new Color(0, 212, 0));
        Object[] parry = points.toArray();
        g2d.setStroke(new BasicStroke(2));

        g2d.drawLine(Math.round(((Point) parry[0]).getX() * imageWidth),
                Math.round(((Point) parry[0]).getY() * imageHeight), Math.round(((Point) parry[3]).getX() * imageWidth),
                Math.round(((Point) parry[3]).getY() * imageHeight));

        g2d.setColor(new Color(255, 0, 0));
        g2d.drawLine(Math.round(((Point) parry[1]).getX() * imageWidth),
                Math.round(((Point) parry[1]).getY() * imageHeight), Math.round(((Point) parry[2]).getX() * imageWidth),
                Math.round(((Point) parry[2]).getY() * imageHeight));

    }
```