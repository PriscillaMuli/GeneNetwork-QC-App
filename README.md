# GeneNetwork-QC-App
Here is an overview of the quality control application whose objective is to evaluate data files for the right syntax before the code is uploaded and it has specific criteria that each of the files must meet.

The project development uses guix and the application runs as the command-line interface and as a web application under the flask application.
The link that contains the quality control application source code: https://gitlab.com/fredmanglis/gnqc_py.

The requirements that must be met are as follows:
1. The files must be tab-separated files(TSV) not comma-separated files(CSV)
2. There should be no empty data cells
3. There should be	no data cells with spurious characters like `eeeee`, `5.555iloveguix`.
4. Average files must contain exactly three places to the right side of the dot.
5. Standard error files must ontain decimal numbers six or greater places to the right side of the dot.
6. There must be a number to the left side of the dot.
7. Line endings should be Unix and not DOS
8. Strain headers should be checked against a source of truth 

**Suggestions on what I would do differently if I were to write an uploader myself:**
1. To enable other file formats to be accept, I would provide an option to convert file formats into TSV files so that the uploader accepts them.

The tests folder makes sure the code is doing the right thing while the scripts folder provides ways to invoke the code and the quality control folder verifies that the data is correct. The quality control folder is the "core" of the application.

The quality control directory contains 8 Python modules as follows:
 ```markdown
1. Init.py
Implement tests and stubs for functions under test. Has the logic for testing validity of files.
```
```markdown 
2. Average.py
Checks validity of average files.
Modules used include typing, utils, errors.
Function used is invalid_value
```
```markdown 
3. errors.py
Checks for errors such as invalid values, duplicate headings and inconsistent columns
Modules used include collections
Function used is namedtuple
```
```markdown 
4. file_utils.py 
Opens TSV and ZIP files
Modules used include typing, pathlib, zipfile
Function used is open_file
```
```markdown 
5. headers.py
Validates headers and each header must contain 2 column names
Modules used include functools, typing
Functions used include invalid_header, invalid_headings, duplicate_headings
```
```markdown 
6. standard_errors.py
Checks validity of standard error files
Module used is typing
Function used is invalid_value
```
```markdown 
7. parsing.py
Provides tools  to parse uploaded files 
Modules used include collections, enum, functools, typing, quality control
Functions used include strain_names(filepath), header_errors(line_number, fields, strains), empty_value(line_number, column_number, value), average_errors(line_number, fields), se_errors(line_number, fields), make_column_consistency_checker(header_row), take(iterable: Iterable, num: int)
```
```markdown 
8. utils.py
As the application checks data quality of uploaded files, utils handles and tracks progress of cell errors 
```
```markdown 

