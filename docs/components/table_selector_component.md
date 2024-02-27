# Table Selector Component

A component used to display all existing tables to the user and allow them to select one

After user submission, TableSelectorComponents can be iterated over to yield the rows of the selected table. These rows can be indexed into using the column names of the selected table.

### Attributes
- `label : str` - The label to paint onto this TableSelectorComponent
- `only_user_tables : bool` - Whether only user-created tables should be selectable or CNL-created metadata tables can also be selected
- `value : str` - The table selected by the user

### Constructor
`TableSelectorComponent(label: str = "", only_user_tables: bool = True)`

Defines a TableSelectorComponent with the given attributes to be shown on the user interface for this stage.

- `label : str` - The label with instructions for the user, displayed as part of this Component. Shows a default value if not provided
- `only_user_tables : bool` - Whether only user-created tables should be selectable or CNL-created metadata tables can also be selected

### `TableSelectorComponent.append(other: dict, get_user_approvals: bool = False)`

Appends other as a row to the table selected by the user by emitting an insert statement with values gathered from `other`. Assumes that the field names present in the dict exactly match the column names in the selected table. If the value in a dict is a UserInputComponent instance, uses that Component's value attribute as the actual value to insert into the selected table.

- `other : dict` - A dictionary representing the row to be inserted into the selected table. Maps column names to values.
- `get_user_approvals : bool` - Whether to get user approvals before committing the append to the database backing the Tool being used. Default False.

### `TableSelectorComponent.delete()`

Deletes the selected table from the database backing the Tool being used.