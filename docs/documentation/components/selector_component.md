# `hilt.SelectorComponent`

_class_ `hilt.SelectorComponent(expected_type, options, label='')`

An input component which captures input from a choice of radio buttons.

### Attributes
- `label : str` - A label to prompt the user for input.
- `options : list[str]` - The list of options displayed to the user.
- `value : str` - The value chosen by the user.

### Example usage
An example of how to use a Selector Component. 

```python
def selector_component():
    choice = hilt.SelectorComponent(str, options=["Option 1", "Option 2", "Option 3"], label="Pick your option")
    if tool.user_input_received():
        hilt.results.show_results((choice.value, "Selected option: "))

tool.add_stage('selector_component', selector_component)
```

On the user interface, this stage will look like the image below. Here the user has already chosen the "Option 2" radio button.

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/selector_1.png?raw=true?" alt="Selector component input"> </img>

The `results` page will surface `input`'s stored option. Below is the `results` page that corresponds with the example stage above. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/selector_2.png?raw=true?" alt="Selector component output"> </img>

