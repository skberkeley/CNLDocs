# Tables
A class to represent the tables in a tool. Programmers can treat it as a Python dictionary mapping table names to pandas `DataFrames` in order to save, overwrite, and delete tables in the database underlying each `Tool`. 
Changes to underlying tables are cached until approvals are handled. Some notes on cached changes and ordering:
- If a table is added/modified and then deleted, then it is ultimately deleted.
- If a table is deleted and then added/modified, then it is ultimately added/modified.
- If a table is added/modified multiple times, then only the last change is saved.
- If a table is added/modified and then accessed, the modified version is returned.
- If a table is deleted and then accessed, a KeyError is raised.

Each `Tool` automatically instantiates its own empty `Tables` attribute upon declaration. 

### `Tables.get_table_names(only_user_tables: bool = True)`

Returns the names of the tables in the tool. 

- `only_user_tables : bool` - If True, only returns the names of user-created tables.

### `Tables.get_columns_of_table(table_name: str, only_user_columns: bool = True)`

Returns a list of the column names of the passed table name. 

- `table_name : str` - The name of the table from which to get the column names
- `only_user_columns : bool` - If True, only includes user columns
