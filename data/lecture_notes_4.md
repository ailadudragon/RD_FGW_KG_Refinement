# Data Science Programming:markdown
___:markdown
:markdown
## Week 5: Lecture::markdown
### 1. Pandas Cut:markdown
### 2. Hierarchical Indexing:markdown
---:markdown
:markdown
## Agenda:markdown
:markdown
### Cut and Hierarchical Indexing::markdown
- pd.cut() for categorization:markdown
- data.stack():markdown
- data.unstack():markdown
- frame.swaplevel('key1', 'key2'):markdown
- frame.sum(level='key2'):markdown
- frame.sum(level='color', axis=1):markdown
- frame.set_index(['c', 'd']):markdown
- frame.reset_index():markdown
- frame.reindex(['a', 'b']):markdown

# import libraries:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code

# Pandas cut() function:markdown
:markdown
The cut() function in pandas is used to transform continuous data into categorical ones, often referred to as "binning.":markdown
:markdown
The key parameters of the cut() function are::markdown
- x: Array-like input data.:markdown
- bins: Defines the bin edges for the segmentation. Can be an int (defining the number of equal-width bins in the range of x) or a sequence of scalars.:markdown
- right: Bool, default True. Indicates whether the bins include the rightmost edge or not.:markdown
- labels: Specifies the labels for the returned bins. If set to False, only integer indicators of the bins are returned.:markdown
- retbins: Bool, default False. Whether to return the bins or not.:markdown

```:code
data = {'Name': ['Anna', 'Ben', 'Charlie', 'David', 'Ella', 'Frank', 'Grace', 'Hannah', 'Ivy', 'Jack'],:code
'Age': [15, 22, 34, 45, 29, 67, 54, 89, 20, 32]}:code
:code
df = pd.DataFrame(data):code
:code
bins = [0, 18, 35, 50, 70, 100]:code
labels = ['<18', '18-34', '35-49', '50-69', '70+']:code
df['Age Group'] = pd.cut(df['Age'], bins=bins, labels=labels:code
```:code

We can assign custom labels to the bins::markdown
:markdown
```:code
bins = [0, 20, 40, 60, 80, 100]:code
labels = ['Very Young', 'Young', 'Middle Aged', 'Senior', 'Elderly']:code
df['Age Group'] = pd.cut(df['Age'], bins=bins, labels=labels):code
```:code

We can exclude the rightmost edge of the bin::markdown
:markdown
```:code
bins = [0, 20, 40, 60, 80, 100]:code
df['Age Group'] = pd.cut(df['Age'], bins=bins, right=False):code
```:code

If we specify an integer for bins, it'll create that many equal-width bins::markdown
:markdown
```:code
df['Age Group'] = pd.cut(df['Age'], bins=5):code
```:code

## Exercise::markdown
Load the housing.csv as a DataFrame. Plot the median house price. Categorize the median prices into cheap, regular, and expensive. Plot the distribution of the categories.:markdown


# Indexing with a DataFrame's columns:markdown
It’s not unusual to want to use one or more columns from a DataFrame as the row:markdown
index; alternatively, you may wish to move the row index into the DataFrame’s columns.:markdown
Here’s an example DataFrame::markdown

:markdown
```:code
frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),:code
'c': ['one', 'one', 'one', 'two', 'two',:code
'two', 'two'],:code
'd': [0, 1, 2, 0, 1, 2, 3]}):code
frame:code
````:code

## set_index() and reset_index():markdown
DataFrame’s set_index function will create a new DataFrame using one or more of:markdown
its columns as the index::markdown
:markdown
```:code
frame.set_index('c'):code
```:code

we can also set multiple columns as index. The result has an **hierarchical index!**:markdown
```:code
frame2 = frame.set_index(['c', 'd']):code
frame2:code
```:code

By default the columns are removed from the DataFrame, though you can leave them:markdown
in::markdown

```:code
frame.set_index(['c', 'd'], drop=False):code
```:code

reset_index, on the other hand, does the opposite of set_index; the hierarchical:markdown
index levels are moved into the columns::markdown

```:code
frame2.reset_index():code
```:code

## Exercise::markdown
For the housing DataFrame, create the column `value_cat` using the categories cheap, regular, and expensive based on the value range in previous exercise. Set the columns `ocean_proximity` and `value_cat` as the index for the DataFrame. To group the houses in `ocean_proximity`, ensure the DataFrame is first sorted on `ocean_proximity`. Store the result in df1.:markdown


# Hierarchical Indexing:markdown

Hierarchical indexing is an important feature of pandas that enables you to have multiple:markdown
(two or more) index levels on an axis. Somewhat abstractly, it provides a way for:markdown
you to work with higher dimensional data in a lower dimensional form.:markdown

```:code
frame2:code
```:code

What you’re seeing is a prettified view of a DataFrame with a MultiIndex as its index. The:markdown
“gaps” in the index display mean “use the label directly above”::markdown

```:code
frame2.index:code
```:code

Hierarchical index has levels. The level numbers start from 0 at outmost and increment inward.:markdown

With a hierarchically indexed object, so-called partial indexing is possible, enabling:markdown
you to concisely select subsets of the data::markdown

```:code
frame2.loc['one']:code
frame2.loc['one'].iloc[1]:code
```:code


## Exercise::markdown
Show df1'index and its levels. Select all hourses 'INLAND' and plot the distribution of value_cat for those hourses. Similarly, Select all hourses 'ISLAND' and plot the distribution of value_cat for those hourses.:markdown


Hierarchical indexing plays an important role in reshaping data group-based:markdown
operations like forming a pivot table (next in aggregation). It also provides a convenient way to work with higher dimensional data in a lower dimensional form. For example, we can convert the DataFrame to a 1-D Series.:markdown
```:code
frame3 = frame2.stack():code
```:code

The inverse operation of unstack is stack::markdown

```:code
frame2.stack().unstack():code
```:code

The DataFrame xs() function takes a key argument to select data at a particular level of a MultiIndex.:markdown
```:code
frame2.xs(1, level=1):code
```:code

## Exercise::markdown
Select all 'expensive' hourses and plot distribution by `ocean_proximity`.:markdown


### Reordering and Sorting Levels:markdown
At times you will need to rearrange the order of the levels on an axis or sort the data:markdown
by the values in one specific level. The swaplevel takes two level numbers or names:markdown
and returns a new object with the levels interchanged (but the data is otherwise:markdown
unaltered)::markdown

```:code
frame2.swaplevel('c', 'd'):code
```:code

sort_index, on the other hand, sorts the data using only the values in a single level.:markdown
When swapping levels, it’s not uncommon to also use sort_index so that the result is:markdown
lexicographically sorted by the indicated level::markdown

```:code
frame2.sort_index(level=1):code
frame2.swaplevel(0, 1).sort_index(level=0):code
```:code

## Exercise::markdown
Swap the index of df1 so that `value_cat` is at the level 0. Ensure the houses are grouped by the value_cat. Select all expensive houses located in INLAND.:markdown


