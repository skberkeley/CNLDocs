# Tool

A data processing tool

### Attributes
- `tool_name : str` - The name of this `Tool`, can only contain alphanumeric characters or underscores.
- `stages : list[Stage]` - A list of the current `Stages` added to this `Tool`.
- `url : str` - The URL at which this Tool's landing page is accessed.
- `web_app : WebApp` - The WebApp driver used to host this Tool.
- `file_dir : Pathlib.Path` - A path to the directory in which to store files uploaded to this Tool.
-  `state : dict` - A dictionary programmers can use to share state between Stages.
-  `tables : Tables` - A dictionary mapping table names to pandas `DataFrames` in order to save, overwrite, and delete tables in the database underlying each `Tool`.

### Constructor
`Tool(tool_name: str, url: str = '', file_dir_path: str = '')`

Initializes a new tool.

- `tool_name : str` - The name of this tool, can only contain alphanumeric characters or underscores.
- `url : str` - The url path for this tool, to be used in the future for situations with multiple tools.
- `file_dir_path : str` - A path to the directory in which to store files uploaded to this Tool.

### `Tool.add_stage(stage_name: str, stage_func: Callable)`

Add a stage to this tool.

- `stage_name : str` - The name of this stage.
- `stage_func : Callable` The function used to define this stage.

### `Tool.run()`

Run this tool using aiohttp.

Add a landing page route, and the requisite routes for each stage.
