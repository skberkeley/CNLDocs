# `hilt.FileUploadComponent`

_class_ `hilt.FileUploadComponent(expected_ext, label='', replace_existing=True)`

A component used to accept user file uploads.

After user submission, writes a copy of the uploaded file locally, and sets this component's value to be the path to that copy.

`expected_ext` is enforced by the browser by adding an accept attribute to the input component.

File Upload Component is rendered as the browser's default file input component.

### Attributes
- `expected_ext : str` - The expected extension for files uploaded to this Component.
- `label : str` - The label with instructions for the user, displayed as part of this Component.
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

On the user interface, this stage will look like the image below. Here the user has already uploaded a file named "requirements.txt". 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/file_upload_1.png?raw=true?" alt="File upload component input"> </img>

The `results` page will surface `file_path`'s stored file path. This relative path can be utilized in file reading functions such as `pandas.read_json` (if the code above was edited to accept files with the .json extension). Below is the `results` page that corresponds with the example stage above. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/file_upload_2.png?raw=true?" alt="File upload component output"> </img>

