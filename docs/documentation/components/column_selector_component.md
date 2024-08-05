# `hilt.ColumnSelectorComponent`

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
    column_selector = hilt.ColumnSelectorComponent("Choose your column(s):")
    if tool.user_input_received():
        hilt.results.show_results((column_selector.num_columns, "Number of selected columns: "),
                             (column_selector.table_name, "Table name of selected column(s): "),
                             (" ".join(column_selector.column_names), "Column names of selected column(s): "),
                             (column_selector.value, "Actual columns selected in the form of a DataFrame: "))

tool.add_stage('column_viewer', column_viewer)
```

On the user interface, this stage will look like the image below. Here the internal database already has two tables saved, `documentation` and `other table`. The user has selected `documentation`. From `documentation` they only selected `col_1`. Below is what the UI will look like while the user has selected the table and is selecting columns.

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/column_selector_1.png?raw=true?" alt="Column selector component input"> </img>

Below is the UI after the user has selected "Confirm your column choices". Notice that the selected columns will be displayed to the user.

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/column_selector_2.png?raw=true?" alt="Column selector component input"> </img>

Below shows how each attribute of the ColumnSelectorComponent will be displayed through `results`. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/column_selector_3.png?raw=true?" alt="Column selector component results"> </img>
