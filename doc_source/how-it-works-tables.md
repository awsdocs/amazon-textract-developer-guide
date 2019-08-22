# Tables<a name="how-it-works-tables"></a>

Amazon Textract can extract tables and the cells in a table\. For example, when the following table is detected on a form, Amazon Textract detects a table with four cells\. 


| Name | Address | 
| --- | --- | 
|  Ana Carolina  |  123 Any Town  | 

Detected tables are returned as [Block](API_Block.md) objects in the responses from [AnalyzeDocument](API_AnalyzeDocument.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. You can use the `FeatureTypes` input parameter to retrieve information about key\-value pairs, tables, or both\. For tables only, use the value `TABLES`\. For an example, see [Exporting Tables into a CSV File](examples-export-table-csv.md)\. For general information about how a document is represented by `Block` objects, see [Documents and Block Objects](how-it-works-document-layout.md)\.

The following diagram shows how a single cell in a table is represented by `Block` objects\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/hieroglyph-table-cell.png)

A cell contains `WORD` blocks for detected words, and `SELECTION_ELEMENT` blocks for selection elements such as check boxes\. 

The following is partial JSON for the preceding table, which has four cells\.

The PAGE Block object has a list of CHILD Block IDs for the TABLE block and each LINE of text that's detected\. 

```
{
    "Geometry": {...}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "f2a4ad7b-f21d-4966-b548-c859b84f66a4",   // Line - Name
                "4dce3516-ffeb-45e0-92a2-60770e9cb744",   // Line  - Address 
                "ee506578-768f-4696-8f4b-e4917e429f50",   // Line - Ana Carolina
                "33fc7223-411b-4399-8a90-ccd3c5a2c196",   // Line  - 123 Any Town
                "3f9665be-379d-4ae7-be44-d02f32b049c2"    // Table
            ]
        }
    ], 
    "BlockType": "PAGE", 
    "Id": "78c3ce84-ae70-418e-add7-27058418adf6"
},
```

The TABLE block includes a list of child IDs for the cells within the table\. A TABLE block also includes geometry information for the table location in the document\. The following JSON shows that the table has four cells, which are listed in the `Ids` array\.

```
{
    "Geometry": {...}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "505e9581-0d1c-42fb-a214-6ff736822e8c", 
                "6fca44d4-d3d3-46ab-b22f-7fca1fbaaf02", 
                "9778bd78-f3fe-4ae1-9b78-e6d29b89e5e9", 
                "55404b05-ae12-4159-9003-92b7c129532e"
            ]
        }
    ], 
    "BlockType": "TABLE", 
    "Confidence": 92.5705337524414, 
    "Id": "3f9665be-379d-4ae7-be44-d02f32b049c2"
},
```

The Block type for the table cells is CELL\. The `Block` object for each cell includes information about the cell location compared to other cells in the table\. It also includes geometry information for the location of the cell on the document\. In the preceding example, `505e9581-0d1c-42fb-a214-6ff736822e8c` is the child ID for the cell that contains the word *Name*\. The following example is the information for the cell\. 

```
{
    "Geometry": {...}, 
    "Relationships": [
        {
            "Type": "CHILD", 
            "Ids": [
                "e9108c8e-0167-4482-989e-8b6cd3c3653e"
            ]
        }
    ], 
    "Confidence": 100.0, 
    "RowSpan": 1, 
    "RowIndex": 1, 
    "ColumnIndex": 1, 
    "ColumnSpan": 1, 
    "BlockType": "CELL", 
    "Id": "505e9581-0d1c-42fb-a214-6ff736822e8c"
},
```

Each cell has a location in a table\. In the preceding example, the cell with the value *Name* is at row 1, column 1\. The cell with the value *123 Any Town* is at row 2, column 2\. A cell block object contains this information in the `RowIndex` and `ColumnIndex` fields\. The child list contains the IDs for the WORD Block objects that contain the text that's within the cell\. The words in the list are in the order in which they're detected, from the top left of the cell to the bottom right of the cell\. In the preceding example, the cell has a child ID with the value e9108c8e\-0167\-4482\-989e\-8b6cd3c3653e\. The following output is for the WORD Block with the ID value of e9108c8e\-0167\-4482\-989e\-8b6cd3c3653e: 

```
"Geometry": {...}, 
"Text": "Name", 
"BlockType": "WORD", 
"Confidence": 99.81139373779297, 
"Id": "e9108c8e-0167-4482-989e-8b6cd3c3653e"
},
```