# Column Selector Component

A component which allows one or more columns to be selected interactively from a given selected table.

The initial view of the ColumnSelectorComponent is identical to that of the TableSelectorComponent. When the user selects a table from the previews, a request is made (defined in column_selector.js) which returns the full table, which the user then selects columns from. The previewed tables are the pandas `DataFrames` stored in the relevant `Tool.tables`. These selected columns are submitted when the stage's POST request is made. The selected columns are stored as a pandas `DataFrame`. 

### Attributes
- `label : str` - The label to paint onto this ColumnSelectorComponent.
- `num_columns : int` - The number of columns to be selected by the user.
- `table_name : str` - The name of the table selected by the user.
-  `column_names : list[str]` - The names of the columns selected by the user.
- `value : pd.DataFrame` - A pandas DataFrame containing the table columns selected by the user.

### Constructor
`ColumnSelectorComponent(label='', num_columns=1)`

Defines a ColumnSelectorComponent with the given attributes to be shown on the user interface for this stage.

- `label : str` - The label to paint onto this ColumnSelectorComponent.
- `num_columns : int` - The number of columns to be selected by the user.

### Example usage
An example of viewing selected columns from tables already saved in `.tables`.

```python
def column_viewer():
    """
    Stage to select and view a column of a table. 
    """
    TextComponent("Select a table and then select a column:")
    column_selector1 = ColumnSelectorComponent("Select a column:")
    column_selector2 = ColumnSelectorComponent("Select another column:")
    results.show_results((column_selector1, "Selected column: "), (column_selector2, "Selected column: "))

tool.add_stage('column_viewer', column_viewer)
```
