# CNL Example Programs
Below are some example programs written in CNL. Each example can be utilized as a `Stage` in a CNL program. If using external packages such as `numpy` or `pandas`, make sure to import them into the program before calling!

## Table Dropper

A `Stage` which allows users to delete a table from the database.

```python
    def delete_table():
        """
        Stage to allow users to delete a table from the database.
        """
        table = TableSelectorComponent(label="Select a table that you want to delete from the database.")

        def process_delete_table():
            table.delete()
            return "Successfully deleted table"

        processor = LambdaProcessor(process_delete_table)
        results.show_results([results.Result(processor.result)])

    tool.add_stage('delete_table', delete_table)
```

## Column Dropper

A `Stage` which allows users to copy a table with a dropped column.

```python
    def drop_column():
        """
        Stage which allows users to select a table and associated column. A new table with the inputted name
        will be created in the database as the selected table with the selected column dropped. 
        """
        table = TableSelectorComponent(label="Select the table you want to filter")
        col = ColumnSelectorComponent(label="Select column to drop")
        table_name = UserInputComponent(str, label="Enter table name, entering a name that already exists will REWRITE the original table.")


        def process_drop_column():
            columns = []
            new_rows = []
            for row in table:
                if not columns:
                    columns = list(row.keys())[1:]
                    columns.remove(col.value[0])
                new_row = [row[column].val for column in columns]
                new_rows.append(new_row)

            new_table = [columns]
            new_table.extend(new_rows)

            return create_table_from_lists(table_name.value, new_table, return_existing_table=False, overwrite_existing_table=True)

        processor = LambdaProcessor(process_drop_column)
        results.show_results([results.Result(processor.result)])

    tool.add_stage('drop_column', drop_column)
```


## Metric Aggregator

A `Stage` to display various metrics from a column of data. Make sure to `import numpy as np`!

```python
    def metrics():
        """
        Stage to get various metrics of a column of data. To test, select a table in the database,
        select the column of interest, and select the metric of interest. If mean, median, or standard
        deviation are selected, the entries of the column must be castable to integers (otherwise 
        the program will error). Demonstration on how to utilize Numpy in the context of a CNL program.
        """
        table = TableSelectorComponent(label="Select a table")
        col = ColumnSelectorComponent(label="Select the column that you want your metric on")
        metric_selector = SelectorComponent(str, options=["mean", "median", "mode", "standard deviation"], label="Select the metric you want to see! Mean, median, and standard deviation only work for numerial data.")

        def process_metrics():
            desired_metric = metric_selector.value
            column_name = col.value[0]
            values = []

            for row in table:
                value = row[column_name].val
                if desired_metric != "mode":
                    try:
                        value = int(value)
                    except:
                        raise Exception("Expected value of column to be a number, since mean, median, or standard deviation was selected.")
                values.append(value)

            if desired_metric == "mean":
                return round(np.mean([value for value in values]), 3)
            elif desired_metric == "median":
                return round(np.median([value for value in values]), 3)
            elif desired_metric == "standard deviation":
                return round(np.std([value for value in values]), 3)
            else:
                return mode(values)

        processor = LambdaProcessor(process_metrics)
        results.show_results([results.Result(processor.result, "Desired metric:")])

    tool.add_stage('metrics', metrics)
```
## Numerical Filter Results

A `Stage` which displays the results of a numerical filtering operation done on a column of choice.

```python
    def numerical_filter():
        """
        Stage to filter the rows of a selected table based on the numerical values in the selected column.
        The selected column and boundary should both be numerical values. The Stage displays the results
        as a list of rows. 
        """
        table = TableSelectorComponent(label="Select the table you want to filter")
        col = ColumnSelectorComponent(label="Select the numerical column to filter on")
        relationship = SelectorComponent(str, options=["Greater than", "Less than", "Equal to", "Not equal to"], label="Select the relationship you want to filter on")
        boundary = UserInputComponent(int, label="Input the value you want to compare the values of your selected column to")

        def process_numerical_filter():
            var = relationship.value
            if var == "Greater than":
                return [row for row in table if int(row[col.value[0]].val) > boundary.value]
            elif var == "Less than":
                return [row for row in table if int(row[col.value[0]].val) < boundary.value]
            elif var == "Equal to":
                return [row for row in table if int(row[col.value[0]].val) == boundary.value]
            else:
                return [row for row in table if int(row[col.value[0]].val) != boundary.value]

        processor = LambdaProcessor(process_numerical_filter)
        results.show_results([results.Result(processor.result, "Filtered results:")])

    tool.add_stage('numerical_filter', numerical_filter)
```
## Numerical Filter with New Table 

A `Stage` which displays the results of a numerical filtering operation done on a column of choice and saves the new table to the database.

```python
    def numerical_filter_new_table():
        """
        Stage to filter the rows of a selected table based on the numerical values in the selected column.
        The selected column and boundary should both be numerical values. The Stage displays the results
        as a new table and saves said table to the database under the inputted table_name.
        """
        table = TableSelectorComponent(label="Select the table you want to filter")
        col = ColumnSelectorComponent(label="Select the numerical column to filter on")
        relationship = SelectorComponent(str, options=["Greater than"], label="Select the relationship you want to filter on")
        boundary = UserInputComponent(int, label="Input the value you want to compare the values of your selected column to")
        table_name = UserInputComponent(str, label="Enter table name, entering a name that already exists will REWRITE the original table with newly merged table.")

        def process_numerical_filter_new_table():
            columns = []
            valid_rows = []
            for row in table:
                if not columns:
                    columns = list(row.keys())[1:]
                if int(row[col.value[0]].val) > boundary.value:
                    row_values = []
                    for column in columns:
                        row_values.append(row[column].val)
                    valid_rows.append(row_values)

            new_table = [columns]
            new_table.extend(valid_rows)

            return create_table_from_lists(table_name.value, new_table, return_existing_table=False, overwrite_existing_table=True)

        processor = LambdaProcessor(process_numerical_filter_new_table)
        results.show_results([results.Result(processor.result, "Filtered table:")])

    tool.add_stage('process_numerical_filter_new_table', numerical_filter_new_table)
```

## Inner Merge Without `Pandas`

A `Stage` which creates a new table as a result of an inner merge between two selected tables. Only merges on one column of keys for each selected table.

```python
    def inner_merge():
        """
        Stage to perform an inner merge with specified tables and columns. This implementation only
        merges on one column of keys for each table. Values of left keys must be unique. Does not utilize any external
        packages.
        """
        left_table = TableSelectorComponent(label="Select the left table for the merge")
        right_table = TableSelectorComponent(label="Select the right table for the merge")
        left_on = ColumnSelectorComponent(label="Select the column for the left table to merge on, expects merge keys to be unique (implementation only merges on one column)")
        right_on = ColumnSelectorComponent(label="Select the column for the right table to merge on (implementation only merges on one column)")
        table_name = UserInputComponent(str, label="Enter table name, entering a name that already exists will REWRITE the original table with newly merged table.")

        def process_inner_merge():
            left_key = left_on.value[0]
            right_key = right_on.value[0]
            left_cols = []
            right_cols = []
            left_merge_information = {}
            new_rows = []
            for row in left_table:
                if not left_cols:
                    left_cols = list(row.keys())[1:]
                left_merge_information[row[left_key].val] = [row[col].val for col in left_cols]
            for row in right_table:
                if not right_cols:
                    right_cols = list(row.keys())[1:]
                    right_cols.remove(right_key)
                if row[right_key].val in left_merge_information:
                    right_row = [row[col].val for col in right_cols]
                    new_rows.append(left_merge_information[row[right_key].val] + right_row)

            left_cols.extend(right_cols)

            new_table = [left_cols]
            new_table.extend(new_rows)

            return create_table_from_lists(table_name.value, new_table, return_existing_table=False, overwrite_existing_table=True)

        processor = LambdaProcessor(process_inner_merge)
        results.show_results([results.Result(processor.result, "Merged table:")])

    tool.add_stage('inner merge', inner_merge)
```

## Merge with `Pandas`

More comprehensive `Stage` which performs merging with `Pandas`. Allows for inner, outer, left, right, and cross merging. Make sure to `import pandas as pd`!

```python
    def pandas_merge():
        """
        Stage to perform a merge with specified tables and columns. This implementation utilizes Panda's implementation
        of merge(). To accomplish this, the data in the selected tables are aggregated into DataFrames. Then, merge()
        is utilized with the merge_type inputted by the user. Finally, the DataFrames are converted into CNL usable
        tables and committed to the database. Demonstration on how to utilize Pandas in context of a CNL program.
        """
        left_table = TableSelectorComponent(label="Select the left table for the merge")
        right_table = TableSelectorComponent(label="Select the right table for the merge")
        left_on = ColumnSelectorComponent(label="Select the column for the left table to merge on, expects merge keys to be unique (implementation only merges on one column)")
        right_on = ColumnSelectorComponent(label="Select the column for the right table to merge on (implementation only merges on one column)")
        merge_type = SelectorComponent(str, options = ['left', 'right', 'outer', 'inner', 'cross'], label="Select the merge type desired. Missing values will be reported as \"None\" in the merged table.")
        table_name = UserInputComponent(str, label="Enter table name, entering a name that already exists will REWRITE the original table with newly merged table.")

        def process_pandas_merge():
            left_key = left_on.value[0]
            right_key = right_on.value[0]
            left_table_dict = {}
            right_table_dict = {}
            for row in left_table:
                if not left_table_dict:
                    for col in list(row.keys())[1:]:
                        left_table_dict[col] = []
                for key in left_table_dict:
                    left_table_dict[key].append(row[key].val)
            for row in right_table:
                if not right_table_dict:
                    for col in list(row.keys())[1:]:
                        right_table_dict[col] = []
                for key in right_table_dict:
                    right_table_dict[key].append(row[key].val)

            left_df = pd.DataFrame(left_table_dict)
            right_df = pd.DataFrame(right_table_dict)

            merged_dataframe = left_df.merge(right_df, how=merge_type.value, left_on=left_key, right_on=right_key)

            new_table = [list(merged_dataframe.columns)]
            new_table.extend(merged_dataframe.values.tolist())

            return create_table_from_lists(table_name.value, new_table, return_existing_table=False, overwrite_existing_table=True)

        processor = LambdaProcessor(process_pandas_merge)
        results.show_results([results.Result(processor.result, "Merged table:")])

    tool.add_stage('pandas merge', pandas_merge)
```
