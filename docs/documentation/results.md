# results
`results` is a module containing the `show_results` method and the `Result` class, which are used to display results to the user for a particular `Stage`.

#### `show_results(results: list[Result], results_title: str = '') -> None`

Shows the results for a particular Stage to the user. The desired values to be shown should be wrapped in Result objects.

- `results : list[Result]` - The results to show to the user, passed as a list of Result objects.
- `results_title : str` - The title to display above the results. Defaults to a generic title if not passed.

#### `class Result`
An object-based representation of a result, label pairing.
##### Attributes
- `label : str` - The label to display for this result.
- `value : Any` - The value to display for this result.
##### Constructor
`Result(value: Any, label: str = '')`
- `value : Any` - The value to display for this result.
- `label : str` - The label to display for this result.

#### Example usage
Below is an example stage that demonstrates how to use `results` to surface internal changes to state. Here, when the user selects the `create_table` stage, the results page will show the user the hard-coded DataFrame. 

```python
def create_table():
        """
        Stage to allow users to save a hard-coded DataFrame to the tool's Tables with a user-inputted name. 
        """
        table_name = UserInputComponent(str, label="Enter table name: ")

        if tool.user_input_recieved():
            df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
            tool.tables[table_name.value] = df
            results.show_results([results.Result(df, "Created table: "]))

    tool.add_stage('create_table', create_table)
