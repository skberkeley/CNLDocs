# Tables
A class to represent the tables in a tool. Programmers can treat it as a dictionary mapping table names to pandas `DataFrames` in order to save, overwrite, and delete tables in the database underlying each `Tool`. 
Changes to underlying tables are cached until approvals are handled. Some notes on cached changes and ordering:
- If a table is added/modified and then deleted, then it is ultimately deleted.
- If a table is deleted and then added/modified, then it is ultimately added/modified.
- If a table is added/modified multiple times, then only the last change is saved.
- If a table is added/modified and then accessed, the modified version is returned.
- If a table is deleted and then accessed, a KeyError is raised.

