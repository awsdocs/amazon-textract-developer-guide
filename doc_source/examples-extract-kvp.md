# Extracting Key\-Value Pairs from a Form Document<a name="examples-extract-kvp"></a>

The following Python example shows how to extract key\-value pairs in form documents from [Block](API_Block.md) objects that are stored in a map\. Block objects are returned from a call to [AnalyzeDocument](API_AnalyzeDocument.md)\. For more information, see [Form Data \(Key\-Value Pairs\)](how-it-works-kvp.md)\.

The functions that are specific to Amazon Textract are: 
+ `get_kv_map` – Calls [AnalyzeDocument](API_AnalyzeDocument.md), and stores the KEY and VALUE BLOCK objects in a map\.
+ `get_kv_relationship` and `find_value_block` – Constructs the key\-value relationships from the map\.

**Note**  
You can download the source file from [textract\_python\_kv\_parser\.py](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/python/example_code/textract/textract_python_kv_parser.py)\.

**To extract key\-value pairs from a form document**

1. Configure your environment\. For more information, see [Prerequisites](examples-blocks.md#examples-prerequisites)\.

1. Save the following example code to a file named *textract\_python\_kv\_parser\.py*\.

   ```
   import boto3
   import sys
   import re
   import json
   
   
   def get_kv_map(file_name):
   
       with open(file_name, 'rb') as file:
           img_test = file.read()
           bytes_test = bytearray(img_test)
           print('Image loaded', file_name)
   
       # process using image bytes
       client = boto3.client('textract')
       response = client.analyze_document(Document={'Bytes': bytes_test}, FeatureTypes=['FORMS'])
   
       # Get the text blocks
       blocks=response['Blocks']
       
   
       # get key and value maps
       key_map = {}
       value_map = {}
       block_map = {}
       for block in blocks:
           block_id = block['Id']
           block_map[block_id] = block
           if block['BlockType'] == "KEY_VALUE_SET":
               if 'KEY' in block['EntityTypes']:
                   key_map[block_id] = block
               else:
                   value_map[block_id] = block
   
       return key_map, value_map, block_map
   
   
   def get_kv_relationship(key_map, value_map, block_map):
       kvs = {}
       for block_id, key_block in key_map.items():
           value_block = find_value_block(key_block, value_map)
           key = get_text(key_block, block_map)
           val = get_text(value_block, block_map)
           kvs[key] = val
       return kvs
   
   
   def find_value_block(key_block, value_map):
       for relationship in key_block['Relationships']:
           if relationship['Type'] == 'VALUE':
               for value_id in relationship['Ids']:
                   value_block = value_map[value_id]
       return value_block
   
   
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
                           if word['SelectionStatus'] == 'SELECTED':
                               text += 'X '    
   
                                   
       return text
   
   
   def print_kvs(kvs):
       for key, value in kvs.items():
           print(key, ":", value)
   
   
   def search_value(kvs, search_key):
       for key, value in kvs.items():
           if re.search(search_key, key, re.IGNORECASE):
               return value
   
   def main(file_name):
   
       key_map, value_map, block_map = get_kv_map(file_name)
   
       # Get Key Value relationship
       kvs = get_kv_relationship(key_map, value_map, block_map)
       print("\n\n== FOUND KEY : VALUE pairs ===\n")
       print_kvs(kvs)
   
       # Start searching a key value
       while input('\n Do you want to search a value for a key? (enter "n" for exit) ') != 'n':
           search_key = input('\n Enter a search key:')
           print('The value is:', search_value(kvs, search_key))
   
   if __name__ == "__main__":
       file_name = sys.argv[1]
       main(file_name)
   ```

1. At the command prompt, enter the following command\. Replace `file` with the document image file that you want to analyze\.

   ```
   textract_python_kv_parser.py file
   ```

1. When you're prompted, enter a key that's part of the input document\. If the key is detected, the program displays the value that's associated with the key\.