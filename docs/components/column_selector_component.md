# Column Selector Component

A component which allows one or more columns to be selected interactively from a given selected table

The initial view of the ColumnSelectorComponent is identical to that of the TableSelectorComponent. When the user selects a table from the previews, a request is made (defined in column_selector.js) which returns the full table, which the user then selects columns from. These selected columns are submitted when the stage's POST request is made. 

### Attributes
- `label : str` - The label to paint onto this ColumnSelectorComponent
- `num_columns : int` - The number of columns to be selected by the user
- `emulated_columns : list[str]` - The names of the columns selected by the user
- `expected_val_types : list[type]` - The expected types of the values contained in the selected columns

### Constructor
`ColumnSelectorComponent(label: str = "", num_columns: int = 1, expected_val_types: Optional[list[type]] = None)`

Defines a ColumnSelectorComponent with the given attributes to be shown on the user interface for this stage.

- `label : str` - The label to paint onto this ColumnSelectorComponent
- `num_columns : int` - The number of columns to be selected by the user
- `expected_val_types : list[type]` - The expected types of the values contained in the selected columns
