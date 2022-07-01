# Exporting Tables into a CSV File<a name="examples-export-table-csv"></a>

This Python example shows how to export tables into a comma\-separated values \(CSV\) file\. Table information is returned as [Block](API_Block.md) objects from a call to [AnalyzeDocument](API_AnalyzeDocument.md)\. For more information, see [Tables](how-it-works-tables.md)\. The `Block` objects are stored in a map structure that's used to export the table data into a CSV file\. 

The Amazon Textract Helper tool greatly simplifies the process.

**Note**  
You can download the source code from [amazon-textract](https://github.com/aws-samples/amazon-textract-textractor/blob/master/helper/bin/amazon-textract)\.

**To export tables into a CSV file**

1. Configure your environment\. For more information, see [Prerequisites](examples-blocks.md#examples-prerequisites)\.

1. At the command prompt, install the Amazon Textract Helper tool

   ```bash
   python -m pip install amazon-textract-helper
   ```

1. Then enter the following command\. 

```
export AWS_DEFAULT_REGION=us-east-2; amazon-textract --input-document "s3://amazon-textract-public-content/blogs/employmentapp.png" --feature TABLES --pretty-print TABLES --pretty-print-table-format csv > output.csv
```

When you run the example, the CSV output is saved to a file named `output.csv`\.

The ```export AWS_DEFAULT_REGION=us-east-2``` was added because the S3 bucket `amazon-textract-public-content` is in the us-east-2 region. 


1. To use your own file

Point `--input-document` to your file. This can be a path to a file on the local filesystem or on S3.

You can also add FORMS as `--feature` and `--pretty-print` option and get the keys and values in a CSV file format.


** Generate CSV in your code **

The [amazon-textract-prettyprinter](https://github.com/aws-samples/amazon-textract-textractor/tree/master/prettyprinter) does simplify generating a CSV from TABLES or FORMS.

This package is used by the Amazon Textract Helper tool under the hood.

The following sample uses the amazon-textract-caller and the amazon-textract-prettyprinter packages.

```bash
python -m pip install amazon-textract-caller amazon-textract-prettyprinter
```

Code
```python
from textractcaller.t_call import call_textract, Textract_Features
from textractprettyprinter.t_pretty_print import Pretty_Print_Table_Format, Textract_Pretty_Print, get_string

textract_json = call_textract(input_document=input_document, features=[Textract_Features.FORMS, Textract_Features.TABLES])
print(get_string(textract_json=textract_json,
               table_format=Pretty_Print_Table_Format.csv,
               output_type=[Textract_Pretty_Print.TABLES, Textract_Pretty_Print.FORMS]))
```
