# `Tool`

_class_ `Tool(tool_name, file_dir_path='')`

A `Tool` containing `Stage`s for doing data transformation work. Tabular data for a `Tool` is stored in its underlying database, which is accessible using the `Tool.tables` attribute. Non-tabular data, such as strings or numbers, can be shared between `Stage`s using `Tool.state`. More information about these attributes can be found below.

### Attributes
- `tool_name : str`
    
    The name of the `Tool`. Expected to be a string containing only alphanumeric characters and underscores.

- `file_dir_path : pathlike`

    A path to the directory where files uploaded to this `Tool` should be stored.

- `tables : Tables`

    A view of the tables in the underlying database backing this `Tool`. Individual tables can be accessed using dictionary-like notation, with string table names as keys (i.e. `tool.tables['myTable']`). This notation is supported for getting tables, in which case a pandas DataFrame is returned, as well as for storing or updating tables in the underlying database, in which case a DataFrame is expected. Tables can be deleted using the `del` keyword.

- `state : dict`

    A dictionary of values associated with this `Tool`, allowing variables to be shared across `Stage`s.