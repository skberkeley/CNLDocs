# `hilt.results`
`results` is a module containing the `show_results` method and the `Result` class, which are used to display results to the user for a particular `Stage`.

#### `hilt.show_results(*results, results_title='')`

Shows the results for a particular Stage to the user. The desired values to be shown can be passed by themselves, in a tuple with a string label, or wrapped in a `Result` object.

- `results : Result | Tuple[Any, str] | Any` - The results to show to the user. Each result can be wrapped in a `Result` object, in a tuple with a string label, or passed by itself.
- `results_title : str` - The title to display above the results. Defaults to a generic title if not passed.

#### `class hilt.Result`
An object-based representation of a result, label pairing.
##### Attributes
- `label : str` - The label to display for this result.
- `value : Any` - The value to display for this result.
##### Constructor
`Result(value: Any, label: str = '')`
- `value : Any` - The value to display for this result.
- `label : str` - The label to display for this result.

#### Example usage
Below is an example stage that demonstrates how to use `results` to surface internal changes to state. Here, when the user selects the `create_table` stage, the results page will show the user the hard-coded DataFrame as well as the number 3 (to demonstrate how to surface multiple results). 

```python
def create_table():
        """
        Stage to allow users to save a hard-coded DataFrame to the tool's Tables with a user-inputted name. 
        """
        table_name = hilt.UserInputComponent(str, label="Enter table name: ")

        if tool.user_input_received():
            df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
            tool.tables[table_name.value] = df
            hilt.results.show_results((df, "Created table: "), (3, "Second number"))

    tool.add_stage('create_table', create_table)
```

Alternatively, the stage below demonstrates an alternative usage of `show_results` by creating individual `Result` objects. Surfaces the same information as the program above.
```python
def create_table():
        """
        Stage to allow users to save a hard-coded DataFrame to the tool's Tables with a user-inputted name. 
        """
        table_name = hilt.UserInputComponent(str, label="Enter table name: ")

        if tool.user_input_received():
            df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
            tool.tables[table_name.value] = df
            hilt.results.show_results(hilt.results.Result(df, "Created table: "), hilt.results.Result(3, "Second number"))

    tool.add_stage('create_table', create_table)
```
