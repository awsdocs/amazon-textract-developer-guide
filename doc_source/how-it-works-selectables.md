# Selection Elements<a name="how-it-works-selectables"></a>

Amazon Textract can detect selection elements such as option buttons \(radio buttons\) and check boxes on a document page\. Selection elements can be detected in [form data](how-it-works-kvp.md) and in [tables](how-it-works-tables.md)\. For example, when the following table is detected on a form, Amazon Textract detects the check boxes in the table cells\.


|  |  |  |  | 
| --- |--- |--- |--- |
|  |  **Agree**  |  **Neutral**  |  **Disagree**  | 
|  **Good Service**  |  ☑  |  ☐  |  ☐  | 
|  **Easy to Use**  |  ☐  |  ☑  |  ☐  | 
|  **Fair Price**  |  ☑  |  ☐  |  ☐  | 

Detected selection elements are returned as [Block](API_Block.md) objects in the responses from [AnalyzeDocument](API_AnalyzeDocument.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\.

**Note**  
You can use the `FeatureTypes` input parameter to retrieve information about key\-value pairs, tables, or both\. For example, if you filter on tables, the response includes the selection elements that are detected in tables\. Selection elements that are detected in key\-value pairs aren't included in the response\.

Information about a selection element is contained in a `Block` object of type `SELECTION_ELEMENT`\. To determine the status of a selectable element, use the `SelectionStatus` field of the `SELECTION_ELEMENT` block\. The status can be either *SELECTED* or *NOT\_SELECTED*\. For example, the value of `SelectionStatus` for the previous image is *SELECTED*\.

A `SELECTION_ELEMENT` `Block` object is associated with either a key\-value pair or a table cell\. A `SELECTION_ELEMENT` `Block` object contains bounding box information for a selection element in the `Geometry` field\. A `SELECTION_ELEMENT` `Block` object isn't a child of a `PAGE` `Block` object\.

## Form data \(Key\-Value Pairs\)<a name="how-it-works-selectable-kvp"></a>

A key\-value pair is used to represent a selection element that's detected on a form\. The `KEY` block contains the text for the selection element\. The `VALUE` block contains the SELECTION\_ELEMENT block\. The following diagram shows how selection elements are represented by [Block](API_Block.md) objects\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-key-value-set-selectable.png)

For more information about key\-value pairs, see [Form Data \(Key\-Value Pairs\)](how-it-works-kvp.md)\.

The following JSON snippet shows the key for a key\-value pair that contains a selection element \(**male ☑**\)\. The child ID \(Id bd14cfd5\-9005\-498b\-a7f3\-45ceb171f0ff\) is the ID of the WORD block that contains the text for the selection element \(*male*\)\. The value ID \(Id 24aaac7f\-fcce\-49c7\-a4f0\-3688b05586d4\) is the ID of the `VALUE` block that contains the `SELECTION_ELEMENT` block object\.

```
{
    "Relationships": [
        {
            "Type": "VALUE", 
            "Ids": [
                "24aaac7f-fcce-49c7-a4f0-3688b05586d4"  Value containing Selection Element
            ]
        }, 
        {
            "Type": "CHILD", 
            "Ids": [
                "bd14cfd5-9005-498b-a7f3-45ceb171f0ff"  WORD - male
            ]
        }
    ], 
    "Confidence": 94.15619659423828, 
    "Geometry": {
        "BoundingBox": {
            "Width": 0.022914813831448555, 
            "Top": 0.08072036504745483, 
            "Left": 0.18966935575008392, 
            "Height": 0.014860388822853565
        }, 
        "Polygon": [
            {
                "Y": 0.08072036504745483, 
                "X": 0.18966935575008392
            }, 
            {
                "Y": 0.08072036504745483, 
                "X": 0.21258416771888733
            }, 
            {
                "Y": 0.09558075666427612, 
                "X": 0.21258416771888733
            }, 
            {
                "Y": 0.09558075666427612, 
                "X": 0.18966935575008392
            }
        ]
    }, 
    "BlockType": "KEY_VALUE_SET", 
    "EntityTypes": [
        "KEY"
    ], 
    "Id": "a118dc43-d5f7-49a2-a20a-5f876d9ffd79"
}
```

The following JSON snippet is the WORD block for the word *Male*\. The WORD block also has a parent LINE block\.

```
{
    "Geometry": {
        "BoundingBox": {
            "Width": 0.022464623674750328, 
            "Top": 0.07842985540628433, 
            "Left": 0.18863198161125183, 
            "Height": 0.01617223583161831
        }, 
        "Polygon": [
            {
                "Y": 0.07842985540628433, 
                "X": 0.18863198161125183
            }, 
            {
                "Y": 0.07842985540628433, 
                "X": 0.2110965996980667
            }, 
            {
                "Y": 0.09460209310054779, 
                "X": 0.2110965996980667
            }, 
            {
                "Y": 0.09460209310054779, 
                "X": 0.18863198161125183
            }
        ]
    }, 
    "Text": "Void", 
    "BlockType": "WORD", 
    "Confidence": 54.06439208984375, 
    "Id": "bd14cfd5-9005-498b-a7f3-45ceb171f0ff"
},
```

The VALUE block has a child \(Id f2f5e8cd\-e73a\-4e99\-a095\-053acd3b6bfb\) that's the SELECTION\_ELEMENT block\. 

```
{
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "f2f5e8cd-e73a-4e99-a095-053acd3b6bfb"  //Selection element
            ]
        }
    ], 
    "Confidence": 94.15619659423828, 
    "Geometry": {
        "BoundingBox": {
            "Width": 0.017281491309404373, 
            "Top": 0.07643391191959381, 
            "Left": 0.2271782010793686, 
            "Height": 0.026274094358086586
        }, 
        "Polygon": [
            {
                "Y": 0.07643391191959381, 
                "X": 0.2271782010793686
            }, 
            {
                "Y": 0.07643391191959381, 
                "X": 0.24445968866348267
            }, 
            {
                "Y": 0.10270800441503525, 
                "X": 0.24445968866348267
            }, 
            {
                "Y": 0.10270800441503525, 
                "X": 0.2271782010793686
            }
        ]
    }, 
    "BlockType": "KEY_VALUE_SET", 
    "EntityTypes": [
        "VALUE"
    ], 
    "Id": "24aaac7f-fcce-49c7-a4f0-3688b05586d4"
}, 
}
```

The following JSON is the SELECTION\_ELEMENT block\. The value of `SelectionStatus` indicates that the check box is selected\.

```
{
    "Geometry": {
        "BoundingBox": {
            "Width": 0.020316146314144135, 
            "Top": 0.07575977593660355, 
            "Left": 0.22590067982673645, 
            "Height": 0.027631107717752457
        }, 
        "Polygon": [
            {
                "Y": 0.07575977593660355, 
                "X": 0.22590067982673645
            }, 
            {
                "Y": 0.07575977593660355, 
                "X": 0.2462168186903
            }, 
            {
                "Y": 0.1033908873796463, 
                "X": 0.2462168186903
            }, 
            {
                "Y": 0.1033908873796463, 
                "X": 0.22590067982673645
            }
        ]
    }, 
    "BlockType": "SELECTION_ELEMENT", 
    "SelectionStatus": "NOT_SELECTED", 
    "Confidence": 74.14942932128906, 
    "Id": "f2f5e8cd-e73a-4e99-a095-053acd3b6bfb"
}
```

## Table Cells<a name="how-it-works-selectable-table"></a>

Amazon Textract can detect selection elements inside a table cell\. For example, the cells in the following table have check boxes\.


|  |  |  |  | 
| --- |--- |--- |--- |
|  |  **Agree**  |  **Neutral**  |  **Disagree**  | 
|  **Good Service**  |  ☑  |  ☐  |  ☐  | 
|  **Easy to Use**  |  ☐  |  ☑  |  ☐  | 
|  **Fair Price**  |  ☑  |  ☐  |  ☐  | 

A `CELL` block can contain child `SELECTION_ELEMENT` objects for selection elements, as well as child `WORD` blocks for detected text\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-table-cell-selectable.png)

For more information about tables, see [Tables](how-it-works-tables.md)\.

The TABLE `Block` object for the previous table looks similar to this\.

```
{
    "Geometry": {.....}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "652c09eb-8945-473d-b1be-fa03ac055928", 
                "37efc5cc-946d-42cd-aa04-e68e5ed4741d", 
                "4a44940a-435a-4c5c-8a6a-7fea341fa295", 
                "2de20014-9a3b-4e26-b453-0de755144b1a", 
                "8ed78aeb-5c9a-4980-b669-9e08b28671d2", 
                "1f8e1c68-2c97-47b2-847c-a19619c02ca9", 
                "9927e1d1-6018-4960-ac17-aadb0a94f4d9", 
                "68f0ed8b-a887-42a5-b618-f68b494a6034", 
                "fcba16e0-6bd7-4ea5-b86e-36e8330b68ea", 
                "2250357c-ae34-4ed9-86da-45dac5a5e903", 
                "c63ad40d-5a14-4646-a8df-2d4304213dbc",   // Cell
                "2b8417dc-e65f-4fcd-aa0f-61a23f1e8cb0", 
                "26c62932-72f0-4dc2-9893-1ae27829c060", 
                "27f291cc-abf4-4c23-aa24-676abe99cb1e", 
                "7e5ce028-1bcd-4d9f-ad42-15ac181c5b47", 
                "bf32e3d2-efa2-4fc1-b09b-ab9cc52ff734"
            ]
        }
    ], 
    "BlockType": "TABLE", 
    "Confidence": 99.99993896484375, 
    "Id": "f66eac36-2e74-406e-8032-14d1c14e0b86"
}
```

The CELL `BLOCK` object \(Id c63ad40d\-5a14\-4646\-a8df\-2d4304213dbc\) for the cell that contains the check box *Good Service* looks like the following\. It includes a child `Block` \(Id = 26d122fd\-c5f4\-4b53\-92c4\-0ae92730ee1e\) that's the `SELECTION_ELEMENT` `Block` object for the check box\.

```
{
    "Geometry": {.....}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "26d122fd-c5f4-4b53-92c4-0ae92730ee1e"  //Selection Element
            ]
        }
    ], 
    "Confidence": 79.741689682006836, 
    "RowSpan": 1, 
    "RowIndex": 3, 
    "ColumnIndex": 3, 
    "ColumnSpan": 1, 
    "BlockType": "CELL", 
    "Id": "c63ad40d-5a14-4646-a8df-2d4304213dbc"
}
```

The SELECTION\_ELEMENT `Block` object for the check box is as follows\. The value of `SelectionStatus` indicates that the check box is selected\.

```
{
    "Geometry": {.......}, 
    "BlockType": "SELECTION_ELEMENT", 
    "SelectionStatus": "SELECTED", 
    "Confidence": 88.79517364501953, 
    "Id": "26d122fd-c5f4-4b53-92c4-0ae92730ee1e"
}
```