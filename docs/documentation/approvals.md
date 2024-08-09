# `hilt.approvals`
`approvals` is a module which allows programmers to request user approval before committing changes to the underlying database of a `Tool`. HiLT currently supports requesting user approvals for three types of changes: creating a new table, adding a new column to a table, and deleting a table. If approvals for any other change to a table are requested, such as a row being added, then the user will be shown an approval page as if the table were being newly added to the table.

User approvals can be collected by calling `approvals.get_user_approvals()` within an SDF. Any changes to the database made in the same SDF will be cached, and shown to the user for approval before they are committed. If the user approves the changes, they will be committed to the database. If the user rejects the changes, they will be discarded. An table creation example can be seen below.

```python
tool = hilt.Tool('demo')

def create_table():
    hilt.TextComponent("Creating and displaying a table to approve")

    if tool.user_input_received():
        df = pd.DataFrame([["Oski", "Bear", 1000], ["Carol", "Christ", 2000]], columns=["First Name", "Last Name", "Age"])
        tool.tables["newTable"] = df

        hilt.approvals.get_user_approvals()
        hilt.results.show_results((tool.tables["newTable"], "Created table: "))

tool.add_stage('create_table', create_table)

tool.run()
```