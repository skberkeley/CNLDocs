# File Upload Component

A component used to accept user file uploads.

After user submission, writes a copy of the uploaded file, and sets this component's value to be the path to that copy.

`expected_ext` is enforced by the browser by adding an accept attribute to the input component.

File Upload Component is rendered as the browser's default file input component.

### Attributes
- `label : str` - The label with instructions for the user, displayed as part of this Component.
- `expected_ext : str` - The expected extension for files uploaded to this Component.
- `value : str` - The path to the uploaded file.

### Constructor
`FileUploadComponent(expected_ext, label='', replace_existing=True)`

Defines a FileUploadComponent with the given attributes to be shown on the user interface for this stage.

- `expected_ext : str` - The expected extension for files uploaded to this Component.
- `label : str` - The label with instructions for the user, displayed as part of this Component. Shows a default value if not provided.
- `replace_existing : bool` - Whether files uploaded will replace previously uploaded files with the same name. Default True.

### Example usage
An example of how to use a FileUploadComponent to capture a file path.

```python
def file_upload():
    file_path = FileUploadComponent(expected_ext = "txt", label = "Input a text file")
    if tool.user_input_received():
        results.show_results((file_path.value, "Uploaded file path: "))

tool.add_stage('file_upload', file_upload)
```

On the user interface, this stage will look like the image below. Here the user has already uploaded a file named "requirements.txt". 

<img src="/docs/images/file_upload_1.png" alt="File upload component input">

The `results` page will surface `file_path`'s stored file path. This relative path can be utilized in file reading functions such as `pandas.read_json` (if the code above was edited to accept files with the .json extension). Below is the `results` page that corresponds with the example stage above. 

<img src="/docs/images/file_upload_2.png" alt="File upload component resutls">

