# HiLT Example Programs
Below are some example program snippets written in HiLT. Each example can be utilized as a State-Defining Function (SDF) to create a `Stage` in a HiLT program. Assumes the instantiated `Tool` variable is named "tool". If using external packages such as `numpy` or `pandas`, make sure to import them into the program before calling, and don't forget to add the `Stage` to the `Tool` you're building!

## Saving a Table

A `Stage` which demonstrates how to save a DataFrame to the underlying `Tool` database using `.tables`.

```python
def create_table():
    """
    Stage to allow users to save a hard-coded DataFrame to the tool's Tables with a user-inputted name.
    """
    table_name = hilt.UserInputComponent(str, label="Enter table name: ")

    if tool.user_input_received():
        df = pd.DataFrame({'col1': [1, 2], 'col2': [3, 4]})
        tool.tables[table_name.value] = df
        hilt.results.show_results((df, "Created table"))
```

## Table Dropper

A `Stage` which allows users to delete a table from the underlying database.

```python
def delete_table():
    """
    Stage to allow users to delete a table from the database.
    Performed using a TableSelectorComponent to allow the user to select the table of interest.
    """
    table = hilt.TableSelectorComponent(label="Select a table that you want to delete from the database.")

    if tool.user_input_received():
        del tool.tables[table.table_name]
```

## Column Dropper

A `Stage` which allows users to copy a table with a dropped column.

```python
def drop_column():
    """
    Stage which allows users to select a table and associated column. A new table with the inputted name
    will be created in the database as the selected table with the selected column dropped.
    """
    col = hilt.ColumnSelectorComponent(label="Select column to drop")
    new_name = hilt.UserInputComponent(str,
                                label="Enter table name, entering a name that already exists will REWRITE the original table.")

    if tool.user_input_received():
        df = tool.tables[col.table_name]
        df.drop(labels=col.column_names, axis=1)
        tool.tables[new_name.value] = df
        hilt.results.show_results((df, "Modified Table: "))
```


## Metric Aggregator

A `Stage` to display various metrics from a column of data. Make sure to `import numpy as np`!

```python
def metrics():
    """
    Stage to get various metrics of a column of data. To test, select a table in the database,
    select the column of interest, and select the metric of interest. If mean, median, or standard
    deviation are selected, the entries of the column must be castable to integers (otherwise 
    the program will error). Demonstration on how to utilize Numpy in the context of a HiLT program.
    """
    table = hilt.TableSelectorComponent(label="Select a table")
    col = hilt.ColumnSelectorComponent(label="Select the column that you want your metric on")
    metric_selector = hilt.SelectorComponent(str, options=["mean", "median", "mode", "standard deviation"], label="Select the metric you want to see! Mean, median, and standard deviation only work for numerical data.")

    if tool.user_input_recieved():
        desired_metric = metric_selector.value
        metric = None

        if desired_metric == "mean":
            metric = round(np.mean(col.value), 3)
        elif desired_metric == "median":
            metric = round(np.median(col.value), 3)
        elif desired_metric == "standard deviation":
            metric = return round(np.std(col.value), 3)
        else:
            metric = mode(values)

        hilt.results.show_results(hilt.results.Result(metric, "Desired metric:"))
```

## Numerical Filter with New Table 

A `Stage` which displays the results of a numerical filtering operation done on a column of choice and saves the new table to the database. Requires `numpy` and `statistics`. 

```python
    def numerical_filter_new_table():
        """
        Stage to filter the rows of a selected table based on the numerical values in the selected column.
        The selected column and boundary should both be numerical values. The Stage displays the results
        as a new table and saves said table to the database under the inputted table_name.
        """
        table = hilt.TableSelectorComponent(label="Select the table you want to filter")
        col = hilt.ColumnSelectorComponent(label="Select the numerical column to filter on")
        relationship = hilt.SelectorComponent(str, options=["Greater than", "Less than", "Equal to"],
                                     label="Select the relationship you want to filter on")
        boundary = hilt.UserInputComponent(int,
                                  label="Input the value you want to compare the values of your selected column to")
        table_name = hilt.UserInputComponent(str,
                                    label="Enter table name, entering a name that already exists will REWRITE the original table with newly merged table.")

        if tool.user_input_received():

            df = tool.tables[table.table_name]

            if relationship.value == "Greater than":
                filtered_df = df[df[col.column_names[0]] > boundary.value]
            elif relationship.value == "Less than":
                filtered_df = df[df[col.column_names[0]] < boundary.value]
            else:
                filtered_df = df[df[col.column_names[0]] == boundary.value]

            tool.tables[table_name.value] = filtered_df
            hilt.results.show_results((filtered_df, "Filtered Table"))

    tool.add_stage('process_numerical_filter_new_table', numerical_filter_new_table)
```

## Merging two tables

More comprehensive `Stage` which performs merging with `pandas`. Allows for inner, outer, left, right, and cross-merging. Make sure to `import pandas as pd`!

```python
def pandas_merge():
    """
    Stage to perform a merge with specified tables and columns. This implementation utilizes Panda's implementation
    of merge(), which naturally extends off tables being internally stored as DataFrames in HiLT.
    """
    left_on = hilt.ColumnSelectorComponent(label="Select the column for the left table to merge on, expects merge keys to be unique (implementation only merges on one column)")
    right_on = hilt.ColumnSelectorComponent(label="Select the column for the right table to merge on (implementation only merges on one column)")
    merge_type = hilt.SelectorComponent(str, options=['left', 'right', 'outer', 'inner', 'cross'],
                                label="Select the merge type desired. Missing values will be reported as \"None\" in the merged table.")
    new_table_name = hilt.UserInputComponent(str,
                                    label="Enter table name, entering a name that already exists will REWRITE the original table with newly merged table.")

    if tool.user_input_received():
        left_df = tool.tables[left_on.table_name]
        right_df = tool.tables[right_on.table_name]
        left_key = left_on.column_names[0]
        right_key = right_on.column_names[0]
        merged_dataframe = left_df.merge(right_df, how=merge_type.value, left_on=left_key, right_on=right_key)

        tool.tables[new_table_name.value] = merged_dataframe
        hilt.results.show_results((merged_dataframe, "Merged table:"))
```
