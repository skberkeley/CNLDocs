# Selector Component

An input component which captures input from a choice of radio buttons.

### Attributes
- `label : str` - A label to prompt the user for input.
- `options : list[str]` - The list of options displayed to the user.
- `value : str` - The value inputted by the user.

### Constructor
`SelectorComponent(expected_type, options, label='')`

Defines a SelectorComponent to be shown in the user interface for this stage. Once user input is provided through the radio buttons, it is cast to `expected_type` and stored in the `value` attribute of this Component.

- `expected_type : type` - The expected type of the user input. The user input is cast to this type before being stored in the `value` attribute.
- `options : list[str]` - The list of options displayed to the user.
- `label : str` - A label to prompt the user for input. Shows a default value if not provided.

### Example usage
An example of how to use a Selector Component. 

```python
def selector_component():
    input = SelectorComponent(str, options=["Option 1", "Option 2", "Option 3"], label="Pick your option")
    if tool.user_input_received():
        results.show_results((input.value, "Selected option: "))

tool.add_stage('selector_component', selector_component)
```

On the user interface, this stage will look like the image below. Here the user has already chosen the "Option 2" radio button.

<img src="/docs/images/selector_1.png" alt="Selector component input">

The `results` page will surface `input`'s stored option. Below is the `results` page that corresponds with the example stage above. 

<img src="/docs/images/selector_2.png" alt="Selector component results">

