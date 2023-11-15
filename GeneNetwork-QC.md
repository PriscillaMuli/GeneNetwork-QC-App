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

The tests directory contains 4 subdirectories and 2 Python modules. Within the qc subdirectory are 6 Python modules:
 ```markdown
1. Init.py
-Implement tests and stubs for functions under test
```
```markdown 
2. test_cells.py
-Checks that the cell values meet and fulfil required criteria.
-Modules include random, hypothesis and quality control
-Functions include: test_cell_value_errors_with_invalid_inputs2(num_str), test_cell_average_value_errors_if_not_three_decimal_places2(num_str), test_cell_average_value_pass_if_three_decimal_places(num_str), test_cell_standard_error_value_errors_if_less_than_six_decimal_places2(num_str), test_cell_standard_error_value_pass_if_six_or_more_decimal_places(num_str)

```
```markdown 
3. test_cells_average.py
-Tests average values
-Modules include: random, hypothesis and quality control
-Functions include: test_cell_average_value_pass_if_no_decimal_places(num_str), 
```
```markdown 
4. test_cells_standard_error.py
-Tests standard error values
-Modules include: random, hypothesis and quality control
-Functions include: test_cell_standard_error_value_errors_if_less_than_six_decimal_places2(num_str)
```
```markdown
5. test_error_collection
-Inspects that the error collection works as it should
-Modules include: quality control
-Functions include: test_take(sample, num, expected), test_collect_errors(filepath, filetype, strains, count), test_collect_inconsistent_column_errors(filepath, filetype, strains, expected)
```
```markdown
6. test_header.py
-Tests the parsing of headers
-Modules include: hypothesis, quality control
-Functions include:test_invalid_header_with_list_of_one_value(headers), test_invalid_headings_with_invalid_inputs(headings), test_invalid_header_with_valid_headers(headers), test_invalid_headings_with_valid_headings(strains, headings), test_duplicate_headers_with_repeated_column_headings(headers, repeated), test_duplicate_headers_with_unique_column_headings(headers)
```

The quality control directory contains 8 Python modules as follows:
 ```markdown
1. Init.py
-Implement tests and stubs for functions under test. Has the logic for testing validity of files.
```
```markdown 
2. Average.py
-Checks validity of average files.
-Modules used include typing, utils, errors.
-Function used is invalid_value
```
```markdown 
3. errors.py
-Checks for errors such as invalid values, duplicate headings and inconsistent columns
-Modules used include collections
-Function used is namedtuple
```
```markdown 
4. file_utils.py 
-Opens TSV and ZIP files
-Modules used include typing, pathlib, zipfile
-Function used is open_file
```
```markdown 
5. headers.py
-Validates headers and each header must contain 2 column names
-Modules used include functools, typing
-Functions used include invalid_header, invalid_headings, duplicate_headings
```
```markdown 
6. standard_errors.py
-Checks validity of standard error files
-Module used is typing
-Function used is invalid_value
```
```markdown 
7. parsing.py
-Provides tools  to parse uploaded files 
-Modules used include collections, enum, functools, typing, quality control
-Functions used include strain_names(filepath), header_errors(line_number, fields, strains), empty_value(line_number, column_number, value), average_errors(line_number, fields), se_errors(line_number, fields), make_column_consistency_checker(header_row), take(iterable: Iterable, num: int)
```
```markdown 
8. utils.py
-As the application checks data quality of uploaded files, utils handles and tracks progress of cell errors 
```
```markdown 

