# Exporting Tables into a CSV File<a name="examples-export-table-csv"></a>

This Python example shows how to export tables into a comma\-separated values \(CSV\) file\. Table information is returned as [Block](API_Block.md) objects from a call to [AnalyzeDocument](API_AnalyzeDocument.md)\. For more information, see [Tables](how-it-works-tables.md)\. The `Block` objects are stored in a map structure that's used to export the table data into a CSV file\. 

The functions that are specific to Amazon Textract are: 
+ `get_table_csv_results` – Calls [AnalyzeDocument](API_AnalyzeDocument.md), and builds a map of tables that are detected in the document\. Creates a CSV representation of all detected tables\.
+ `generate_table_csv` – Generates the CSV file for an individual table\.
+ `get_rows_columns_map` – Gets the rows and columns from the map\.
+ `get_text` – Gets the text from a cell\.

**Note**  
You can download the source code from [textract\_python\_table\_parser\.py](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/python/example_code/textract/textract_python_table_parser.py)\.

**To export tables into a CSV file**

1. Configure your environment\. For more information, see [Prerequisites](examples-blocks.md#examples-prerequisites)\.

1. Save the following example code to a file named *textract\_python\_table\_parser\.py*\.

   ```
   import webbrowser, os
   import json
   import boto3
   import io
   from io import BytesIO
   import sys
   from pprint import pprint
   
   
   def get_rows_columns_map(table_result, blocks_map):
       rows = {}
       for relationship in table_result['Relationships']:
           if relationship['Type'] == 'CHILD':
               for child_id in relationship['Ids']:
                   cell = blocks_map[child_id]
                   if cell['BlockType'] == 'CELL':
                       row_index = cell['RowIndex']
                       col_index = cell['ColumnIndex']
                       if row_index not in rows:
                           # create new row
                           rows[row_index] = {}
                           
                       # get the text value
                       rows[row_index][col_index] = get_text(cell, blocks_map)
       return rows
   
   
   def get_text(result, blocks_map):
       text = ''
       if 'Relationships' in result:
           for relationship in result['Relationships']:
               if relationship['Type'] == 'CHILD':
                   for child_id in relationship['Ids']:
                       word = blocks_map[child_id]
                       if word['BlockType'] == 'WORD':
                           text += word['Text'] + ' '
                       if word['BlockType'] == 'SELECTION_ELEMENT':
                           if word['SelectionStatus'] =='SELECTED':
                               text +=  'X '    
       return text
   
   
   def get_table_csv_results(file_name):
   
       with open(file_name, 'rb') as file:
           img_test = file.read()
           bytes_test = bytearray(img_test)
           print('Image loaded', file_name)
   
       # process using image bytes
       # get the results
       client = boto3.client('textract')
   
       response = client.analyze_document(Document={'Bytes': bytes_test}, FeatureTypes=['TABLES'])
   
       # Get the text blocks
       blocks=response['Blocks']
       pprint(blocks)
   
       blocks_map = {}
       table_blocks = []
       for block in blocks:
           blocks_map[block['Id']] = block
           if block['BlockType'] == "TABLE":
               table_blocks.append(block)
   
       if len(table_blocks) <= 0:
           return "<b> NO Table FOUND </b>"
   
       csv = ''
       for index, table in enumerate(table_blocks):
           csv += generate_table_csv(table, blocks_map, index +1)
           csv += '\n\n'
   
       return csv
   
   def generate_table_csv(table_result, blocks_map, table_index):
       rows = get_rows_columns_map(table_result, blocks_map)
   
       table_id = 'Table_' + str(table_index)
       
       # get cells.
       csv = 'Table: {0}\n\n'.format(table_id)
   
       for row_index, cols in rows.items():
           
           for col_index, text in cols.items():
               csv += '{}'.format(text) + ","
           csv += '\n'
           
       csv += '\n\n\n'
       return csv
   
   def main(file_name):
       table_csv = get_table_csv_results(file_name)
   
       output_file = 'output.csv'
   
       # replace content
       with open(output_file, "wt") as fout:
           fout.write(table_csv)
   
       # show the results
       print('CSV OUTPUT FILE: ', output_file)
   
   
   if __name__ == "__main__":
       file_name = sys.argv[1]
       main(file_name)
   ```

1. At the command prompt, enter the following command\. Replace `file` with the document image file that you want to analyze\.

   ```
   python textract_python_table_parser.py file
   ```

When you run the example, the CSV output is saved to a file named `output.csv`\.