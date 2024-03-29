# File Upload Component

A component used to accept user file uploads

After user submission, writes a copy of the uploaded file, and sets this component's value to be the path to that copy

`expected_ext` is enforced by the browser by adding an accept attribute to the input component

File Upload Component is rendered as the browser's default file input component

### Attributes
- `label : str` - The label with instructions for the user, displayed as part of this Component
- `expected_ext : str` - The expected extension for files uploaded to this Component
- `value : str` - The path to the uploaded file

### Constructor
`FileUploadComponent(expected_ext: str, label: str = '', replace_existing: bool = True)`

Defines a FileUploadComponent with the given attributes to be shown on the user interface for this stage.

- `expected_ext : str` - The expected extension for files uploaded to this Component
- `label : str` - The label with instructions for the user, displayed as part of this Component. Shows a default value if not provided
- `replace_existing : bool` - Whether files uploaded will replace previously uploaded files with the same name. Default True.
