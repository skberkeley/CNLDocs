# Lambda Processor

A Processor which handles user input by running a function passed to the Processor on creation.

### Attributes
- `func : Callable` - The function to be run as part of this processor
- `num_return_vals : int` - The number of return values expected from `func`. Defaults to 1
- `result` - The result of running `func` on the user input. Is a tuple if the function returns more than one value.

### Constructor
`LambdaProcessor(func: Callable, num_return_vals: int = 1)`

Defines a LambdaProcessor to handle user input by running `func`. The result of running `func` is stored in the `result` attribute of this Processor.

- `func : Callable` - The function to be run to process user input.
- `num_return_vals : int` - The number of return values expected from `func`. Defaults to 1
