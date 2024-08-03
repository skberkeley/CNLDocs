# `hilt.TextComponent`

A component used to render some text within the interface of a Stage. Doesn't take user input.

### Attributes
- `text : str` - The text to display.

### Constructor
`TextComponent(text)`

Defines a TextComponent to be shown with the given text.

- `text : str` - The text to display.

### Example usage
An example of how to use a TextComponent. 

```python
def text_component():
    text = hilt.TextComponent("This will show up as a message to the user.")

tool.add_stage('text_component', text_component)
```

On the user interface, this stage will look like the image below. Since there are no `results` being surfaced, there is no corresponding `results` page. 

<img src="https://github.com/skberkeley/CNLDocs/blob/main/docs/images/text_component_1.png?raw=true?" alt="Text component"> </img>

