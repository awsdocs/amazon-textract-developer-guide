# Tables<a name="how-it-works-tables"></a>

Amazon Textract can extract tables in a document, and extract cells, merged cells, and column headers within a table\. For example, when the following table is detected in a document, Amazon Textract detects a table with thirty cells, 3 merged cells, and 5 cells that are column headers\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/tableexample.png)

Detected tables are returned as [Block](API_Block.md) objects in the responses from [AnalyzeDocument](API_AnalyzeDocument.md) and [GetDocumentAnalysis](API_GetDocumentAnalysis.md)\. You can use the `FeatureTypes` input parameter to retrieve information about key\-value pairs, tables, or both\. For tables only, use the value `TABLES`\. For an example, see [Exporting Tables into a CSV File](examples-export-table-csv.md)\. For general information about how a document is represented by `Block` objects, see [Text Detection and Document Analysis Response Objects](how-it-works-document-layout.md)\.

The following diagram shows how a single cell in a table is represented by `Block` objects\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/textract/latest/dg/images/tableexampleimage.png)

A cell contains `WORD` blocks for detected words, and `SELECTION_ELEMENT` blocks for selection elements such as check boxes\. 

The following is partial JSON for the preceding table, which has 23 cells \(counting each merged cell as one cell\)\. The PAGE Block object has a list of CHILD Block IDs for the TABLE block and each LINE of text that's detected\.

The PAGE Block object has a list of CHILD Block IDs for the TABLE block and each LINE of text that's detected\. 

```
{
     {
            "BlockType": "PAGE",
            "Geometry": {
                "BoundingBox": {
                    "Width": 1.0,
                    "Height": 1.0,
                    "Left": 0.0,
                    "Top": 0.0 
                    },
            },
            "Id": "ddfcf314-11bb-4088-ae57-542843c4f7b1",
            "Relationships": [
                {
                    "Type": "CHILD",
                    "Ids": [
                        "7cf63193-d316-4cf5-8fa3-0210a19a8ef0",
                        "06d7f631-2ce0-4f8e-8a02-b05a54b09900",
                        "05830c30-a8b7-43f4-9744-d0a85db92ca7",
                        "c9ca14ac-f8f1-4d90-8ef9-3a85493d348e",
                        "da6052ad-b763-428b-8a5f-88818a34ed33",
                        "c98ffa0d-19f3-44f9-a2a1-40596fe8a990",
                        "af191f61-793d-47df-aa99-043c2d460873",
                        "394a838c-82eb-4ca4-b1fb-d7c596051d51",
                        "d575c7f2-580e-4e39-8012-09d406a52171",
                        "cccb2ecf-618e-4928-ae7c-668f6508f26c",
                        "566ee5ac-408f-4a1b-aa4d-c4f0e90b9ecc",
                        "6290f376-3431-4c74-8547-edc740bae6e2",
                        "6ece89b1-dcd1-4f0f-91be-523510f19ed1",
                        "ea9677a3-6745-4013-ae85-e6f6d4fce468",
                        "42ebe479-081f-4bdc-ad0d-747e15f6e838",
                        "acfc52ea-46d1-4203-8a6d-c30a00ca9d07",
                        "9c3953ce-1aa1-431a-b663-eb6d32e18ef4",
                        "49160b50-c75a-46b0-a2b9-340e6edd8b40",
                        "c15ade38-231c-422f-8b2a-122937d01277",
                        "63e465d6-c927-4367-895d-4403ba666b2f",
                        "82dd37f1-f17b-4fa1-864a-c2f2daf3bfb5"                    ]
                }
            ]
        },
},
```

To learn more about the `TABLE`, access the `TABLE` Block object\. The `TABLE` block includes two types of relationships: “Child” and “Merged Cells”\. For relationship type “CHILD”, each child ID represents a single cell within the table\. A merged cell will be broken down all the individual cells that are combined to make one merged cell\. The following JSON shows that the preceding table has 30 cells for 6 rows and 5 columns, which are listed in the `Ids` array\. For relationship type “MERGED\_CELL”, each merged cell ID represents a single merged cell within the table\. The following JSON shows that the table has 3 merged cells, which are listed in the `Ids` array\.

```
{
    "BlockType": "TABLE",
            "Confidence": 99.79180145263672,
            "Geometry": {},
            "Id": "82dd37f1-f17b-4fa1-864a-c2f2daf3bfb5",
            "Relationships": [
                {
                    "Type": "CHILD",
                    "Ids": [
                        "cda64a58-28d2-47d9-857e-bd7fd9f99d57",
                        "3bc1c578-b733-4e69-af6b-a7ae44b4be5d",
                        "c31985ad-959a-42b7-b150-91d1b446261a",
                        "ec1de7f2-f402-48a2-a787-f92e1559dd3a",
                        "28680139-13be-4a36-bb5d-a47d01d5f8bd",
                        "e106eaff-8269-43c7-9217-22fa9af654d1",
                        "12e26ecd-a58a-408b-a2a3-276752bb0fe4",
                        "f5174a5d-6c69-4626-b96b-80bbfa7190c9",
                        "59949cfc-a2bf-4106-9b04-ace67a95ed5f",
                        "09ee8754-be3c-4c6e-bc03-afd54eb376de",
                        "26e02458-559a-4baf-91f8-4c4cd23e19a6",
                        "fb50ddd5-242e-4468-80d3-3802ed61279a",
                        "44546a71-1f3e-4b96-8811-fd049f254bea",
                        "d991cbe5-3f5c-4c97-887c-ae6c907bb33d",
                        "4952bd89-096f-42f0-afe7-eb35a0378089",
                        "00d2969f-79c3-40c3-b37d-bfd49a36c599",
                        "4df5c57d-5677-4fa9-af7d-ca3ae3aa75aa",
                        "4f502514-c231-4722-a3a2-ba10d3c681b4",
                        "f8ea9a9e-afd3-472c-81d3-112b5dbdfa8c",
                        "13d6cebf-e8ff-4195-b47e-28aabde23e46",
                        "da50d1c0-6c3b-432b-a376-b0a55e2ea9d7",
                        "3dbc11a3-c1ba-4b91-ae50-eb372ac60d2c",
                        "35af88fc-fce7-4841-8794-a83aaa7bebde",
                        "a95c0b14-2043-444c-9c02-a1b664735d90",
                        "cf9e1225-31b3-4ddd-8150-63f88ad11d1f",
                        "d0fa361f-b30c-4b75-a616-6971b3695dc0",
                        "bf7eed90-df5e-4c6a-acb9-23c03b941e0e",
                        "b711c551-432b-4e32-bf95-a5e7d2e28fec",
                        "a69add2b-3afe-4dd0-88e8-f4c61bcf4486",
                        "fa3ef61d-0408-4b89-9732-cff3872d004e"                    ]
                },
}, 
                 {
                    "Type": "MERGED_CELL",
                    "Ids": [
                        "7063a4ff-4e60-4d8a-ba44-b79e5c0e22f4",
                        "21f2ef7d-2fda-4d72-b771-4ce67cd505ec",
                        "6b46b39c-9bbc-47f6-af09-3e8c71697dd8"                    ]
                }
```

The Block type for each table cell is `CELL`\. The CELL Block type will always have row span of 1 and column span of 1\. The Block object for each cell includes information about the cell location compared to other cells in the table\. It also includes geometry information for the location of the cell on the document\. If addition, an `ENTITY TYPE` of `COLUMN HEADER` identifies `CELLs` that are column headers in the table\. In the preceding example, `dacc81f7-e304-43e2-b5ab-9fc45ec24d95` is the child ID for the cell that contains the word ‘Date’ and this cell is a column header, see below\.

```
 
{
            "BlockType": "CELL",
            "Confidence": 93.32925415039062,
            "RowIndex": 1,
            "ColumnIndex": 1,
            "RowSpan": 1,
            "ColumnSpan": 1,
            "Geometry": {...},
            "Id": "cda64a58-28d2-47d9-857e-bd7fd9f99d57",
            "Relationships": [
                {
                    "Type": "CHILD",
                    "Ids": [
                        "b49e883c-bd8b-43e2-aed6-0aa93a52b2b1"                    ]
                }
            ],
            "EntityTypes": [
                "COLUMN_HEADER"            ]
        },
```

For the cell that contains word ‘Deposit’, the cell is not a column header as shown by the lack of field `"EntityTypes": "COLUMN_HEADER"`\.

```
 {
            "BlockType": "CELL",
            "Confidence": 92.3740005493164,
            "RowIndex": 1,
            "ColumnIndex": 4,
            "RowSpan": 1,
            "ColumnSpan": 1,
            "Geometry": {...},
            "Id": "ec1de7f2-f402-48a2-a787-f92e1559dd3a",
            "Relationships": [
```

All the merged cells are listed under `"Type": "MERGED_CELL"`\. In the example table, we have 3 merged cells\. 

```
                 "Type": "MERGED_CELL",
                 "Ids": [
                        "7d0ed24e-ec5e-47ef-998f-8aed26218d76",
                        "27ccf2ef-ff8f-45a1-ad1d-45d9c06697fc",
                        "0a0d399a-9ae3-4f92-b9eb-c5c9038a585c"
                        ]
```

To find specific details associated with each merged cell, go to `"BlockType": "MERGED_CELL"`\. For the merged cell “Previous Balance”, the ID associated with it is "7d0ed24e\-ec5e\-47ef\-998f\-8aed26218d76"\.

There are 4 cells that constitute this merged cell as seen by the "ColumnSpan" of 4\. To find the text within the merged cell, proceed further down to the Ids array to find details on `"BlockType": "CELL"` followed by `"BlockType": "WORD"`\.

```
 "BlockType": "MERGED_CELL",
            "Confidence": 92.46913146972656,
            "RowIndex": 2,
            "ColumnIndex": 1,
            "RowSpan": 1,
            "ColumnSpan": 4,
            "Geometry": {...},
            "Id": "7063a4ff-4e60-4d8a-ba44-b79e5c0e22f4",
            "Relationships": [
                {
                    "Type": "CHILD",
                    "Ids": [
                        "e106eaff-8269-43c7-9217-22fa9af654d1",
                        "12e26ecd-a58a-408b-a2a3-276752bb0fe4",
                        "f5174a5d-6c69-4626-b96b-80bbfa7190c9",
                        "59949cfc-a2bf-4106-9b04-ace67a95ed5f"                    ]
                }
            ]
        },
```

On the cell level, we have 4 cells for this merged cell “Previous Balance”\.

```
{
                "BlockType": "CELL",
                "Confidence": 92.35404205322266,
                "RowIndex": 2,
                "ColumnIndex": 1,
                "RowSpan": 1,
                "ColumnSpan": 1,
                "Geometry": {…},
                "Id": "2a5b5530-afc9-49ac-8c18-208c6f28ac51",
                "Relationships": [
                 {
                    "Type": "CHILD",
                    "Ids": [
                        "c398d626-eff0-4cdd-af52-4f4961585778"]}]},
                 {
                "BlockType": "CELL",
                "Confidence": 92.35404205322266,
                "RowIndex": 2,
                "ColumnIndex": 2,
                "RowSpan": 1,
                "ColumnSpan": 1,
                "Geometry": {…},
                "Id": "6b70be2d-403b-4fd3-b409-a6dcbad0d5ff",
                "Relationships": [
                {
                    "Type": "CHILD",
                    "Ids": [
                        "22c9cb13-2016-45b4-a4ce-2a10dd4b3cc4"]}]},
            {
                "BlockType": "CELL",
                "Confidence": 92.35404205322266,
                "RowIndex": 2,
                "ColumnIndex": 3,
                "RowSpan": 1,
                "ColumnSpan": 1,
                "Geometry": {…},
                "Id": "249baa50-17a6-45e2-abf5-c1546dc84798"},
            {
                "BlockType": "CELL",
                "Confidence": 92.35404205322266,
                "RowIndex": 2,
                "ColumnIndex": 4,
                "RowSpan": 1,
                "ColumnSpan": 1,
                "Geometry": {…},
                "Id": "947d71fb-b8af-47be-b1ab-695690a71552"
            }
```

On word level, there are two words, “Previous” and “Balance”\. Since the last two cells on column 3 and 4 are blank, there are no words associated with them\.

```
{
    "BlockType": "WORD",
    "Confidence": 99.517578125,
    "Text": "Previous",
    "TextType": "PRINTED",
    "Geometry": {…},
    "Id": "c398d626-eff0-4cdd-af52-4f4961585778"
}
{
    "BlockType": "WORD",
    "Confidence": 99.52200317382812,
    "Text": "Balance",
    "TextType": "PRINTED",
    "Geometry": {…},
    "Id": "22c9cb13-2016-45b4-a4ce-2a10dd4b3cc4"
}
```