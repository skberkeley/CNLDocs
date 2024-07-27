# Table Selector Component

A component used to display all existing tables to the user and allow them to select one. The previewed tables are the pandas `DataFrames` stored in the relevant `Tool.tables`.

After user submission, TableSelectorComponents stores the selected table as a pandas `DataFrame`.

### Attributes
- `label : str` - The label to paint onto this TableSelectorComponent.
- `only_user_tables : bool` - Whether only user-created tables should be selectable or HiLT-created metadata tables can also be selected.
- `table_name` - The selected table's name as stored in the underlying database.
- `value : pd.DataFrame` - The table selected by the user stored as a pandas DataFrame.

### Constructor
`TableSelectorComponent(label='', only_user_tables=True)`

Defines a TableSelectorComponent with the given attributes to be shown on the user interface for this stage.

- `label : str` - The label with instructions for the user, displayed as part of this Component. Shows a default value if not provided.
- `only_user_tables : bool` - Whether only user-created tables should be selectable or HiLT-created metadata tables can also be selected.

### Example usage
An example of how to use a TableSelectorComponent to view a table.

```python
def table_viewer():
    """
    Stage to select and view a table. To test, access this stage after uploading a csv, and view a table. Verify
    all uploaded tables are viewable, and no hidden ones are.
    :return:
    """
    TextComponent("Select a table: ")
    table = TableSelectorComponent()
    results.show_results((table, "Selected table: "))

tool.add_stage('table_viewer', table_viewer)

```
