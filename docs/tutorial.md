# HiLT Background
HiLT is a domain-specific language designed for constructing data-transformation interfaces for non-programmers.

Using HiLT, programmers can construct data processing interfaces by instantiating and adding to a `Tool`. Each `Tool` has an underlying database which maintains state and data which users can interact with.

Typically, data transformation flows are composed of multiple steps, each of which require some sort of input from users. Within each `Tool`, programmers can define multiple `Stages`, to help guide users through the data transformation process. For example, you (the programmer) could define one `Stage` where users upload datasets, and then another `Stage` where the user is filtering over those datasets. Stages are defined using Stage-Defining Functions (SDFs) which we discuss in more detail later in this tutorial.

# Creating a `Tool`
In this section, we walk through creating a `Tool` in HiLT.

## Create a `Tool`

All HiLT programs start by creating a `Tool`. When creating a `Tool`, we provide it with a name. Once the `Tool` is created, we can run it using the `run` method. Typically, when you a `Tool` is run, its landing page will show links allowing users to navigate to the different `Stages` you've defined. However, since we haven't added any `Stages` yet, the landing page will be empty.

```python
# Instantiate a new tool
tool = hilt.Tool('demo')

# Run the tool
tool.run()
```

Try running this program.  The output will include a link, which you can follow to see the UI you've built so far.

## Create a `Stage` to compute the sum of two numbers
In this section, we'll walk through creating a `Stage` which computes the sum of two numbers input by the user.

### Using an SDF to add a `Stage` to a `Tool`

To define a `Stage`, we use a Stage-Defining Function (SDF). At a high level, we do this by defining a Python function, which contains the logic for the `Stage`. Then, we use the `Tool.add_stage` method to add the `Stage` to the `Tool`, passing the name we want to give to the `Stage` and the SDF as arguments. In this case, we define a function called `adder`, which we leave empty for now, and add it to the `Tool` as a `Stage` called "adder".

At this point, when we run the program, the `Tool`'s landing page will now show a link to the "adder" `Stage`. However, since we haven't yet defined the interface or any logic inside the SDF, clicking on the link will show an empty page with just a "Submit" button.

```python
# Instantiate a new tool
tool = hilt.Tool('demo')

# Define an SDF
def adder():
    pass

# Add the SDF as a Stage to the Tool
tool.add_stage("adder", adder)

# Run the tool
tool.run()
```

### Using `Components` to add widgets to a `Stage`
To define the interface for a `Stage`, we use `Components`. `Components` are widgets that users can interact with to provide input or view output. `Components` should be instantiated *within* an SDF for a particular `Stage`. In this case, we use two `UserInputComponents` to prompt for and collect the two numbers we want to add from the user.

Now, when you run the `Tool` and navigate to the "adder" `Stage`, you'll see two input boxes prompting you to enter the two numbers you want to add. However, clicking "Submit" won't do anything yet, since we haven't added any logic to the `Stage`.

```python
# Instantiate a new tool
tool = hilt.Tool('demo')

# Define an SDF
def adder():
    # Create interface to collect two numbers from the user
    input1 = hilt.UserInputComponent(int, "Enter the first number to add:")
    input2 = hilt.UserInputComponent(int, "Enter the second number to add:")

# Add the SDF as a Stage to the Tool
tool.add_stage("adder", adder)

# Run the tool
tool.run()
```

### Adding logic to a `Stage`
Now we get to reveal once of HiLT's key secrets: A Stage-Defining Function (SDF) for a particular `Stage` is run twice. The first time, HiLT runs the SDF to build the user interface and put all the widgets on the page.  The second time, `Tool.user_input_received()` returns true, and  HiLT runs the SDF to execute anything that should only happen after users have given input. This means that in the same SDF, we can both (1) add widgets and (2) add logic that depends on what users input to those widgets!

To let HiLT know what part of the SDF depends on user input, we check whether the user has submitted their input using the `Tool.user_input_received` method. We can use this to wrap input-dependent code inside an if statement. In this case, we can only add the two numbers together once the user has submitted their input.

Now, when we run this program, navigate to the "adder" `Stage`, and enter two numbers, we can calculate the sum of the two numbers, although the user won't see it. To access the user's inputs, we access each `UserInputComponent`'s `value` attribute. Every `Component` which captures user input has a `value` attribute we can use to access that input once user input ahs been received.

```python
# Instantiate a new tool
tool = hilt.Tool('demo')

# Define an SDF
def adder():
    # Create interface to collect two numbers from the user
    input1 = hilt.UserInputComponent(int, "Enter the first number to add:")
    input2 = hilt.UserInputComponent(int, "Enter the second number to add:")

    if tool.user_input_received():
        # User input has been received, so now we can
        # add inputs by accessing each component's value attribute
        result = input1.value + input2.value

# Add the SDF as a Stage to the Tool
tool.add_stage("adder", adder)

# Run the tool
tool.run()
```

### Adding `Results` to a `Stage`
Ideally, we'd like to show the user the sum of the two numbers they've inputted for us. To do this, we can use HiLT's `results.show_results` function, which specifies what to show to the user once we've finished running the SDF the second time. The easiest way to use it is to pass it a tuple of the result we want to show and a string with a label for it.

Now, when you run this program and provide input to the "adder" `Stage`, you'll see the sum of the two numbers you've inputted.

```python
# Instantiate a new tool
tool = hilt.Tool('demo')

# Define an SDF
def adder():
    # Create interface to collect two numbers from the user
    input1 = hilt.UserInputComponent(int, "Enter the first number to add:")
    input2 = hilt.UserInputComponent(int, "Enter the second number to add:")

    if tool.user_input_received():
        # User input has been received, so now we can
        # add inputs by accessing each component's value attribute
        result = input1.value + input2.value
        # Use show_results to display the result to the user
        hilt.results.show_results((result, "The sum of the two numbers is:"))

# Add the SDF as a Stage to the Tool
tool.add_stage("adder", adder)

# Run the tool
tool.run()
```

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

## Using `approvals` to allow users to approve or reject changes
In this section, we walk through using the `approvals` API, which allows users to approve or reject changes to the underlying database of a `Tool`. The `approvals` API is designed to allow users to review changes to the database before they are committed.

User approvals can be collected by calling `approvals.get_user_approvals()` within an SDF. Any changes to the database made in the same SDF will be cached, and shown to the user for approval before they are committed. If the user approves the changes, they will be committed to the database. If the user rejects the changes, they will be discarded. An example modifying the table creation example can be seen below.

```python
tool = hilt.Tool('demo')

def create_table():
    hilt.TextComponent("Creating and displaying a table to approve")

    if tool.user_input_received():
        # Use a DataFrame to create a table
        df = pd.DataFrame([["Oski", "Bear", 1000], ["Carol", "Christ", 2000]], columns=["First Name", "Last Name", "Age"])
        tool.tables["newTable"] = df

        # Use get_user_approvals to allow the user to inspect the table and approve or reject it
        hilt.approvals.get_user_approvals()
        # Use show_results to display the approved version of the table to the user
        hilt.results.show_results((tool.tables["newTable"], "Created table: "))

tool.add_stage('create_table', create_table)

tool.run()
```