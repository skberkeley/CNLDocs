# `hilt.FileUploadComponent`

_class_ `hilt.FileUploadComponent(expected_ext, label='', replace_existing=True)`

A component used to accept user file uploads.

After user submission, writes a copy of the uploaded file locally, and sets this component's value to be the path to that copy.

`expected_ext` is enforced by the browser by adding an accept attribute to the input component.

File Upload Component is rendered as the browser's default file input component.

### Attributes
- `expected_ext : str` - The expected extension for files uploaded to this Component.
- `label : str` - The label with instructions for the user, displayed as part of this Component.
- `replace_existing : bool` - What to do if a file with the same name has been uploaded previously. If `True`, silently replaces the pre-existing file, otherwise raises a `ValueError`. Default `True`.
- `value : str` - The path to the uploaded file.

### Example usage
An example of how to use a FileUploadComponent to capture a file path.

```python
def file_upload():
    file_path = hilt.FileUploadComponent(expected_ext = "txt", label = "Input a text file")
    if tool.user_input_received():
        hilt.results.show_results((file_path.value, "Uploaded file path: "))

tool.add_stage('file_upload', file_upload)
```

### Adding table using a FileUploadComponent and `read_csv` 

An example of using a FileUploadComponent to read and save a file using HiLT's `Tables`.

```python
def create_table_from_csv_path():
    # Use a TextComponent to show a message to the user
    hilt.TextComponent("Creating and displaying a table through file upload")
    filePath = hilt.FileUploadComponent("csv", "Upload your file you want to save!")

    if tool.user_input_received():
        # Create a DataFrame through reading an inputted file path, captured by the FileUploadComponent.  
        df = pd.read_csv(filePath.value)

        # Add the DataFrame to the tool's tables with name "newTable"
        tool.tables["newTable"] = df

        # Use show_results to display the table to the user
        hilt.results.show_results((tool.tables["newTable"], "Created table: "))

tool.add_stage('create_table', create_table_from_csv_path)
```

### User facing display
The basic example usage Stage will look like the image below on the user interface. Here the user has already uploaded a file named "requirements.txt". 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/file_upload_1.png?raw=true?" alt="File upload component input"> </img>

The `results` page will surface `file_path`'s stored file path. Below is the `results` page that corresponds with the example stage above. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/file_upload_2.png?raw=true?" alt="File upload component output"> </img>

