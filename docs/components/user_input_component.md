# User Input Component

An input component which captures input typed by the user

### Attributes
- `label : str` - A label to prompt the user for input.
- `value : str` - The value inputted by the user

### Constructor
`UserInputComponent(expected_type: type, label: str = "")`

Defines a UserInputComponent to be shown in the user interface for this stage. Once user input is provided, it is cast to `expected_type` and stored in the `value` attribute of this Component.

- `expected_type : type` - The expected type of the user input. The user input is cast to this type before being stored in the `value` attribute.
- `label : str` - A label to prompt the user for input. Shows a default value if not provided.