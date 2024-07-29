# Tables

A `Tables` instance is a view of the tables in the underlying database backing a particular `Tool`. Individual tables can be accessed using dictionary-like notation, with string table names as keys (i.e. `tool.tables['myTable']`). This notation is supported for getting tables, in which case a pandas DataFrame is returned, and for storing or updating tables in the underlying database, in which case a DataFrame is expected. Tables can be deleted using the `del` keyword.

Each `Tool` creates its own  `Tables` instance upon declaration.

### Some notes on ordering
Due to the nature of how `Stages` are implemented, database changes made using a `Tables` instance are cached until after an SDF has finished running. As a result, the following ordering rules are used:
- If a table is added/modified and then deleted, then it is ultimately deleted.
- If a table is deleted and then added/modified, then it is ultimately added/modified.
- If a table is added/modified multiple times, then only the last change is saved.
- If a table is added/modified and then accessed, the modified version is returned.
- If a table is deleted and then accessed, a KeyError is raised.
