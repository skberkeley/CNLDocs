# hilt.Tables

A `Tables` instance is a view of the tables in the underlying database backing a particular `Tool`. Individual tables can be accessed using dictionary-like notation, with string table names as keys (i.e. `tool.tables['myTable']`). This notation is supported for getting tables, in which case a pandas DataFrame is returned, and for storing or updating tables in the underlying database, in which case a DataFrame is expected. Tables can be deleted using the `del` keyword.

Each `Tool` creates its own  `Tables` instance upon declaration, so you should never need to instiantiate a `Tables` object yourself.

### Some notes on ordering
Due to the nature of how `Stages` are implemented, database changes made using a `Tables` instance are cached until after an SDF has finished running. As a result, the following ordering rules are used:
- If a table is added/modified and then deleted, then it is ultimately deleted.
- If a table is deleted and then added/modified, then it is ultimately added/modified.
- If a table is added/modified multiple times, then only the last change is saved.
- If a table is added/modified and then accessed, the modified version is returned.
- If a table is deleted and then accessed, a KeyError is raised.

## Using the `Tables` API:
In this section, we walk through using the `Tables` API to interact with the underlying database of a `Tool`. Individual tables can be accessed using dictionary-like notation, with string table names as keys (i.e. `tool.tables['myTable']`). This notation is supported for getting tables, in which case a pandas DataFrame is returned, and for storing or updating tables in the underlying database, in which case a DataFrame is expected. Tables can be deleted using the `del` keyword.

### Adding a table using the `Tables` API

At a high level, we can think of a `Tables` instance as a dictionary of tables, mapping table names to pandas DataFrames. To add a table to the `Tool`, we can use the same notation as adding an entry to a dictionary.

```python
tool = hilt.Tool('demo')

def create_table():
    # Use a TextComponent to show a message to the user
    hilt.TextComponent("Creating and displaying a table to approve")

    if tool.user_input_received():
        # Create a DataFrame
        df = pd.DataFrame([["Oski", "Bear", 1000], ["Carol", "Christ", 2000]], columns=["First Name", "Last Name", "Age"])
        # Add the DataFrame to the tool's tables with name "newTable"
        tool.tables["newTable"] = df

        # Use show_results to display the table to the user
        hilt.results.show_results((tool.tables["newTable"], "Created table: "))

tool.add_stage('create_table', create_table)

tool.run()
```
### Adding table using `read_csv`

Besides adding a hardcoded DataFrame, `Tables` can be updated using slightly more complex methods. In this example, a table is added through a hard-coded file path and `pd.read_csv`. 

```python
tool = hilt.Tool('demo')

def create_table_from_csv_path():
    # Use a TextComponent to show a message to the user
    hilt.TextComponent("Creating and displaying a table through referencing a file path")

    if tool.user_input_received():
        # Create a DataFrame through a hard-coded arbitrary directory path "directory/file.csv".  
        df = pd.read_csv("directory/file.csv")
        # Add the DataFrame to the tool's tables with name "newTable"
        tool.tables["newTable"] = df

        # Use show_results to display the table to the user
        hilt.results.show_results((tool.tables["newTable"], "Created table: "))

tool.add_stage('create_table', create_table)

tool.run()
```

### Modifying a table using the `Tables` API
We can use the same notation as table creation to modify a table. In this case, we'll add a new row to the table we created in the previous example. Note how we handle the case where a table named "newTable" doesn't exist yet.

```python
tool = hilt.Tool('demo')

def modify_table():
    # Use a TextComponent to show a message to the user
    hilt.TextComponent("Modifying a table")

    if tool.user_input_received():
        try: 
            # Try to append to the table named "newTable"
            # Get a df containing the table
            df = tool.tables["newTable"]
            # Append a new row to the table
            df.append({"First Name": "Alice", "Last Name": "Wonderland", "Age": 3000})
            # Update the table in the tool's tables
            tool.tables["newTable"] = df
        except KeyError:
            # If the table doesn't exist, create it
            df = pd.DataFrame([["Oski", "Bear", 1000], ["Carol", "Christ", 2000]], columns=["First Name", "Last Name", "Age"])
            tool.tables["newTable"] = df

        # Use show_results to display the table to the user
        hilt.results.show_results((tool.tables["newTable"], "Modified table: "))

tool.add_stage('modify_table', modify_table)

tool.run()
```

### Deleting a table using the `Tables` API
We can use `del` notation used for normal dictionaries to delete tables as well.

```python
tool = hilt.Tool('demo')

def modify_table():
    # Use a TextComponent to show a message to the user
    hilt.TextComponent("Modifying a table")

    if tool.user_input_received():
        try: 
            # Try to the delete the table named "newTable"
            del tool.tables["newTable"]
        except KeyError:
            # If the table doesn't exist, create it
            df = pd.DataFrame([["Oski", "Bear", 1000], ["Carol", "Christ", 2000]], columns=["First Name", "Last Name", "Age"])
            tool.tables["newTable"] = df

        # Use show_results to display whether the table exists to the user
        hilt.results.show_results(("newTable" in tool.tables, "newTable table exists: "))

tool.add_stage('modify_table', modify_table)

tool.run()
```
