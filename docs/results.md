# results
results is a module containing the show_results method and the Result class, which are used to display results to the user for a particular Stage.

#### `show_results(results: list[Result], results_title: str = '') -> None`

Shows the results for a particular Stage to the user. The desired values to be shown should be wrapped in Result objects.

- `results : list[Result]` - The results to show to the user, passed as a list of Result objects
- `results_title : str` - The title to display above the results. Defaults to a generic title if not passed.

#### `class Result`
An object-based representation of a result, label pairing
##### Attributes
- `label : str` - The label to display for this result
- `value : Any` - The value to display for this result
##### Constructor
`Result(value: Any, label: str = '')`
- `value : Any` - The value to display for this result
- `label : str` - The label to display for this result