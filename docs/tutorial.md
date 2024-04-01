# CNL Background
CNL is a domain-specific language meant to build data-transformation interfaces for non-technical users. The intended use case is for it to be used by programmers building bespoke data-processing GUIs for their non-programming colleagues. For example, a team of social scientists might have one teammate with programming experience. Ideally, this programmer would be able to use CNL to build data-processing interfaces for their teammates when faced with some data transformation task too large to be done manually.

Using CNL, programmers write each interface using a `Tool`. Each `Tool` has an underlying database which maintains state and data which users can interact with through `Stages`. Within each `Tool`, programmers can define multiple `Stages`, each of which can be used by the users to accomplish a specific data transformation task. For example, you (the programmer) could define one `Stage` to do some sort of filtering over a particular dataset, or a basic `Stage` where the user is just uploading a new dataset. What the user sees in each `Stage` is defined by its Components, which are the building blocks of the layout of the `Stage`. Components also provide fields where the user is able to provide inputs. What happens with this data is defined in the `Stage`'s `Processor`, whose results can be presented to the user for approval, or immediately saved.

# Using PyCharm
In this section, we present some useful feaetures of PyCharm that may be helpful in working with CNL.

## Running a Python program in PyCharm
<iframe src="https://drive.google.com/file/d/1vCWeAva81_PQ7wdTV2oCg6nvmRc5_6w0/preview" width="640" height="480" allow="autoplay"></iframe>

## Using PyCharm’s auto-import feature
<iframe src="https://drive.google.com/file/d/1A1MiecdldG-xzp32b3xnkyirqnmSN8h4/preview" width="640" height="480" allow="autoplay"></iframe>

# Creating a `Tool`
In this section, we present some video tutorials which demonstrate how to create a `Tool` using CNL.

## Create a `Tool`
<iframe src="https://drive.google.com/file/d/1DnTb5myMITBLdGyTB7cTXEu13LtvseiJ/preview" width="640" height="480" allow="autoplay"></iframe>

## Create a `Stage` to compute the sum of two numbers
### Adding `Components` to a `Stage`
<iframe src="https://drive.google.com/file/d/15WAqNLHYpuwjWDfz343a-CYuZMm7B8iA/preview" width="640" height="480" allow="autoplay"></iframe>

### Adding a `Processor` to a `Stage`
<iframe src="https://drive.google.com/file/d/1EzIoq6IrDa33FqZ5e2VRkGysB9_k7hLC/preview" width="640" height="480" allow="autoplay"></iframe>

### Adding `Results` to a `Stage`
<iframe src="https://drive.google.com/file/d/1CZDQPY7PAJzvvLuV3H5kdOv_ZOnyotDD/preview" width="640" height="480" allow="autoplay"></iframe>

## Using `Table` and `Column` `Selectors`:
<iframe src="https://drive.google.com/file/d/1gU9evV_nnoWAoqGfE5YE6fz3e2k3O4Ac/preview" width="640" height="480" allow="autoplay"></iframe>
