# Table Selector Component

A component used to display all existing tables to the user and allow them to select one

After user submission, TableSelectorComponents can be iterated over to yield the rows of the selected table. These rows can be indexed into using the column names of the selected table.

### Attributes
- `label : str` - The label to paint onto this TableSelectorComponent
- `only_user_tables : bool` - Whether only user-created tables should be selectable or CNL-created metadata tables can also be selected
- `value : pd.DataFrame` - The table selected by the user stored as a pandas DataFrame

### Constructor
`TableSelectorComponent(label: str = "", only_user_tables: bool = True)`

Defines a TableSelectorComponent with the given attributes to be shown on the user interface for this stage.

- `label : str` - The label with instructions for the user, displayed as part of this Component. Shows a default value if not provided
- `only_user_tables : bool` - Whether only user-created tables should be selectable or CNL-created metadata tables can also be selected
