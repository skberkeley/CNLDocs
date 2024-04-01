# db_utils
db_utils is a module that provides a set of utilities to work with the database backing a given Tool.

#### `create_table_from_csv(table_name: UserInputComponent | str, csv_file: FileUploadComponent, has_header: bool = True, overwrite_existing: bool = True) -> sqlalchemy.Table`

Create a table in the database of the tool, using the passed csv file to populate it. Returns the created table.

- `table_name : UserInputComponent | str` - The name to use for the table being inserted. If a UserInputComponent is passed, the input provided by the user into that Component is used for the table name.
- `csv_file : FileUploadComponent` - The file to be used to populate the table. The file uploaded by the user to the FileUploadComponent is used.
- `has_header : bool` - Whether the passed csv file has a header row. Defaults to True.
- `overwrite_existing : bool` - Whether to overwrite an existing table with the same name. Defaults to True.

#### `create_table_from_lists(table_name: str, data: list[list], return_existing_table: bool = True, overwrite_existing_table: bool = True, get_user_approvals: bool = False) -> sqlalchemy.Table`

Create and commit a table in the database of the currently running tool, populating it with data passed in as a list of lists. Assumes the first row contains the column names for the table. Returns the created table.

- `table_name : str` - The name to use for the table being inserted.
- `data : list[list]` - The data to be used to populate the table. The first list in the list of lists is assumed to contain the column names for the table.
- `return_existing_table : bool` - Whether to return an existing table with the same name instead of creating a new one. Defaults to False.
- `overwrite_existing_table : bool` - Whether to overwrite an existing table with the same name. Defaults to True.
- `get_user_approvals : bool` - Whether to prompt the user for approval before committing the created table to the backing database. Defaults to False.

#### `get_table_names_from_tool(tool: Tool, only_user_tables: bool = True) -> List[str]`

Get the names of the tables in the backing database of the passed tool. Returns a list of table names.

- `tool : Tool` - The Tool from which to get associated table names.
- `only_user_tables : bool` - Whether to only return user-created tables and ignore any CNL-created metadata tables. Defaults to True.

#### `get_column_names_from_table_name(tool: Tool, table_name: str, only_user_columns: bool = True) -> List[str]`

Get the column names of the passed table in the backing database of the passed tool. Returns a list of column names.

- `tool : Tool` - The Tool associated with the table being queried.
- `table_name : str` - The name of the table from which to get column names.
- `only_user_columns : bool` - Whether to only return user-created columns and ignore any CNL-created metadata columns. Defaults to True.

#### `get_table_from_table_name(tool: Tool, table_name: str) -> Optional[sqlalchemy.Table]`

Get the sqlaclchemy Table which has the given name associated with the passed Tool. Returns the Table if it exists, otherwise returns None.

- `tool : Tool` - The Tool associated with the table being queried.
- `table_name : str` - The name of the table being requested.

#### `get_rows_of_table(tool: Tool, table: sqlalchemy.Table) -> Sequence[sqlalchemy.Row]`

Get the rows of the specified table. The returned rows can be indexed into using column names. Returns a sequence of rows.

- `tool : Tool` - The Tool associated with the table being queried.
- `table_name : str` - The name of the table being requested.


#### `get_row(tool: Tool, table_name: str, row_id: int) -> 'Row':`

Get the row of the passed table with the passed row id. Returns the row as a Row object, which can be indexed into using column names.

- `tool : Tool` - The Tool associated with the table being queried.
- `table_name : str` - The name of the table being queried.
- `row_id : int` - The id of the row being requested.