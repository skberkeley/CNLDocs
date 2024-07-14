# Selector Component

An input component which captures input from a choice of radio buttons.

### Attributes
- `label : str` - A label to prompt the user for input.
- `options : list[str]` - The list of options displayed to the user.
- `value : str` - The value inputted by the user.

### Constructor
`SelectorComponent(expected_type: type, options: list[str], label: str = "")`

Defines a SelectorComponent to be shown in the user interface for this stage. Once user input is provided through the radio buttons, it is cast to `expected_type` and stored in the `value` attribute of this Component.

- `expected_type : type` - The expected type of the user input. The user input is cast to this type before being stored in the `value` attribute.
- `options : list[str]` - The list of options displayed to the user.
- `label : str` - A label to prompt the user for input. Shows a default value if not provided.
