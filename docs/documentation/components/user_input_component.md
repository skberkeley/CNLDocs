# User Input Component

An input component which captures input typed by the user.

### Attributes
- `label : str` - A label to prompt the user for input.
- `value : str` - The value inputted by the user.

### Constructor
`UserInputComponent(expected_type, label='')`

Defines a UserInputComponent to be shown in the user interface for this stage. Once user input is provided, it is cast to `expected_type` and stored in the `value` attribute of this Component.

- `expected_type : type` - The expected type of the user input. The user input is cast to this type before being stored in the `value` attribute.
- `label : str` - A label to prompt the user for input. Shows a default value if not provided.

### Example usage
An example of how to use a UserInputComponent. 

```python
def user_input():
    input = UserInputComponent(str, "Input your string below: ")
    if tool.user_input_received():
        results.show_results((input, "Inputted string: "))

tool.add_stage('user_input', user_input)
```

On the user interface, this stage will look like the image below. Here the user has inputted "Hello HiLT users!". 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/user_input_1.png?raw=true?" alt="User input component input"> </img>

The `results` page will surface `input`'s stored file path. Below is the `results` page that corresponds with the example stage above. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/user_input_2.png?raw=true?" alt="User input component results"> </img>
