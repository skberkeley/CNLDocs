# Text Component

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
    input = TextComponent("This will show up as a message to the user when they pick the text_component stage")

tool.add_stage('text_component', text_component)
```
