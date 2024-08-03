# hilt.Tool.add_stage

`hilt.Tool.add_stage(stage_name, stage_func)`

Uses the `stage_func` provided to define and add a `Stage`  with name `stage_name` to this `Tool`.

### Parameters
- `stage_name : str`

    The name of the `Stage` being added.

- `stage_func : Callable`

    A function defining the `Stage` being added. Stage-defining functions (SDFs) are discussed in more detail __TODO__. Should be a function which can be called with no arguments.