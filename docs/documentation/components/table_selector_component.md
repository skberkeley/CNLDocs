# `hilt.TableSelectorComponent`

_class_ `hilt.TableSelectorComponent(label='', only_user_tables=True)`

A component used to display all existing tables to the user and allow them to select one. The previewed tables are the pandas `DataFrames` stored in the relevant `Tool.tables`.

After user submission, TableSelectorComponents stores the selected table as a pandas `DataFrame`.

### Attributes
- `label : str` - The label to paint onto this TableSelectorComponent.
- `only_user_tables : bool` - Whether only user-created tables should be selectable or HiLT-created metadata tables can also be selected.
- `table_name : str` - The selected table's name as stored in the underlying database.
- `value : pd.DataFrame` - The table selected by the user stored as a pandas DataFrame.

### Example usage
An example of how to use a TableSelectorComponent to view a table.

```python
def table_viewer():
    """
    Stage to select and view a table. To test, access this stage after uploading a csv, and view a table. Verify
    all uploaded tables are viewable, and no hidden ones are.
    :return:
    """
    table = hilt.TableSelectorComponent(label="Select a table: ")
    if tool.user_input_received():
        hilt.results.show_results((table.value, "Selected table: "))

tool.add_stage('table_viewer', table_viewer)

```

On the user interface, this stage will look like the image below. Here the internal database already has two tables saved, `documentation` and `other table`. The user has selected `documentation`. Below is what the UI will look like while the user has selected the table.

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/table_selector_1.png?raw=true?" alt="Table selector component input"> </img>

Below shows how `results` display tables from a TableSelectorComponent. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/table_selector_2.png?raw=true?" alt="Table upload component results"> </img>
