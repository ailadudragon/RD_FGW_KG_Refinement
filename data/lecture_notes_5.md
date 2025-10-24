# Data Science Programming:markdown
___:markdown
:markdown
## Week 5-6: Lecture::markdown
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


# INFO 212: Data Science Programming 1:markdown
___:markdown
:markdown
## Week 6: Lecture: Data Aggregation and Group Operations:markdown
---:markdown
:markdown
**Agenda:**:markdown
- frame.groupby(list).agg:markdown
- groupby by a function:markdown
- groupby by a dictionary:markdown
- groupby by a list:markdown
- iterate over groups:markdown
- split-apply-combine:markdown
- pivotal tables and cross-tabulation:markdown

# import libraries:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code

# GroupBy Mechanics:markdown
:markdown
Categorizing a dataset and applying a function to each group, whether an aggregation:markdown
or transformation, is often a critical component of a data analysis workflow. After:markdown
loading, merging, and preparing a dataset, you may need to compute group statistics:markdown
or possibly pivot tables for reporting or visualization purposes.:markdown

## The split-apply-combine Principle for GroupBy:markdown
In the first stage of the groupby process, data contained in a pandas object, whether a Series, Data‐:markdown
Frame, or otherwise, is split into groups based on one or more keys that you provide.:markdown
The splitting is performed on a particular axis of an object. For example, a DataFrame:markdown
can be grouped on its rows (axis=0) or its columns (axis=1). Once this is done, a:markdown
function is applied to each group, producing a new value. Finally, the results of all:markdown
those function applications are combined into a result object. The form of the resulting:markdown
object will usually depend on what’s being done to the data.:markdown
:markdown
![](https://i.imgur.com/89PD9om.png):markdown

## pandas provides a flexible groupby interface, enabling you to slice, dice, and summarize datasets in a natural way.:markdown

## Each grouping key can take many forms, and the keys do not have to be all of the same type::markdown
- A list or array of values that is the same length as the axis being grouped:markdown
- A value indicating a column name in a DataFrame:markdown
- A dict or Series giving a correspondence between the values on the axis being:markdown
grouped and the group names:markdown
- A function to be invoked on the axis index or the individual labels in the index:markdown

Create a DataFrame example::markdown
:markdown
```:code
df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],:code
'key2' : ['one', 'two', 'one', 'two', 'one'],:code
'data1' : np.random.randn(5),:code
'data2' : np.random.randn(5)}):code
:code
df.info():code
```:code

## Frequently, the grouping information is found in the same DataFrame as the data you want to work on. In that case, you can pass column names (whether those are strings, numbers, or other Python objects) as the group keys::markdown
```:code
df.groupby('key1')[['data1', 'data2']].mean():code
:code
df.groupby('key1'):code
:code
```:code

## Exercise::markdown
Compute the average total_bill and tip_pct for each unique value in the day column.:markdown


## The group keys could be any arrays of the right length::markdown

```:code
states = np.array(['Ohio', 'California', 'California', 'Ohio', 'Ohio']):code
years = np.array([2005, 2005, 2006, 2005, 2006]):code
:code
:code
df['data1'].groupby(states).mean():code
:code
df['data2'].groupby(years).sum():code
:code
df.groupby([states, years]).mean():code
```:code

# DataSet: Meals and Tips:markdown
Download the `tips.csv` dataset and load it as a DataFrame here.:markdown


## Explore the tips dataset:markdown


## Add a tip_pct column:markdown
Compute the percentage of tips for each meal:markdown


## Exercise::markdown
Extract the column `time` as a list called `meal_type`. Compute the maximum total_bill, tip, and tip_pct of each type of meal.:markdown


## Exercise::markdown
Group by the tips data using the combinations of days and smoker types. Sort the results by the maximums of tip_pct in descending order.:markdown


# Iterating Over Groups:markdown
## The GroupBy object supports iteration, generating a sequence of 2-tuples containing the group name along with the chunk of data. Consider the following::markdown
```:code
for key, group in df.groupby('key1')::code
print(key):code
print(group):code
```:code

## In the case of multiple keys, the first element in the tuple will be a tuple of key values::markdown

```:code
for key, chunk in df.groupby(['key1', 'key2'])::code
print(key[0], key[1]):code
print(chunk):code
```:code

## Of course, you can choose to do whatever you want with the pieces of data. A recipe you may find useful is computing a dict of the data pieces as a one-liner::markdown

```:code
pieces = dict(list(df.groupby('key1'))):code
for k in pieces::code
print('key = ', k):code
print('chunk =\n', pieces[k]):code
:code
:code
pieces['b']:code
```:code

## Exercise::markdown
Group by the tips dataset by the unique values in day column;  sort the all Sunday's records by the sizes in descending order.:markdown


## By default, groupby groups on axis=0, but you can group on any of the other axes. For example, we could group the columns of our example df here by dtype like so::markdown

```:code
grouped = df.groupby(df.dtypes, axis=1):code
:code
:code
for dtype, group in grouped::code
print(dtype):code
print(group):code
```:code

## Exercise::markdown
Group by the tips dataset on the data types of the columns.:markdown


# How to Group by Two Consecutive Rows:markdown

```:code
df = pd.DataFrame([1, 2, 3, 4, 1, 2,3], columns=['val']):code
:code
for key, data in df.groupby((np.arange(len(df)) // 3))::code
print(key):code
print(data):code
```:code

## Exercise::markdown
Group by the tips dataset by every 4 consecutive rows.:markdown


# Grouping with Dicts and Series:markdown
## Grouping information may exist in a form other than an array. Let’s consider another example DataFrame::markdown

```:code
people = pd.DataFrame(np.random.randn(5, 5),:code
columns=['a', 'b', 'c', 'd', 'e'],:code
index=['Joe', 'Steve', 'Wes', 'Jim', 'Travis']):code
people.iloc[2:3, [1, 2]] = np.nan # Add a few NA values:code
people:code
```:code

## Now, suppose I have a group correspondence for the columns and want to sum together the columns by group::markdown

```:code
mapping = {'a': 'red', 'b': 'red', 'c': 'blue',:code
'd': 'blue', 'e': 'red', 'f' : 'orange'}:code
```:code

## Now, you could construct an array from this dict to pass to groupby, but instead we can just pass the dict::markdown

```:code
by_column = people.groupby(mapping, axis=1):code
by_column.sum():code
```:code

## The same functionality holds for Series, which can be viewed as a fixed-size mapping::markdown

```:code
map_series = pd.Series(mapping):code
map_series:code
:code
:code
people.groupby(map_series, axis=1).sum():code
```:code

## Exercise::markdown
For the day column, map 'Sun' and 'Sat' to 'weekend' and 'Thur' and 'Fri' to weekday. Compute the mean total_bill and tip_pct of weekend and weekday.:markdown


# Grouping with Functions:markdown
Using Python functions is a more generic way of defining a group mapping compared:markdown
with a dict or Series. Any function passed as a group key will be called once per index:markdown
value, with the return values being used as the group names. More concretely, consider:markdown
the example DataFrame above, which has people’s first:markdown
names as index values. Suppose you wanted to group by the length of the names;:markdown
while you could compute an array of string lengths, it’s simpler to just pass the len:markdown
function::markdown

```:code
for k, chunk in people.groupby(len)::code
print(k):code
print(chunk):code
```:code

## Exercise::markdown
Define a function that converts the tip_pct to percentage (100%) and round the percentage to the nearest number multiplied by 10 (i.e, 10, 20, 30, ...). For each rounded percentage, compute the average total_bill and tip_pct.:markdown


# Grouping by Index Levels:markdown
## A convenience for hierarchically indexed datasets is the ability to aggregate using one of the levels of an axis index. Let’s look at an example::markdown

```:code
columns = pd.MultiIndex.from_arrays([['US', 'US', 'US', 'JP', 'JP'],:code
[1, 3, 5, 1, 3]],:code
names=['cty', 'tenor']):code
hier_df = pd.DataFrame(np.random.randn(4, 5), columns=columns):code
hier_df:code
```:code

## To group by level, pass the level number or name using the level keyword::markdown

```:code
hier_df.groupby(level='cty', axis=1).count():code
```:code

# Data Aggregation:markdown
Aggregations refer to any data transformation that produces scalar values from:markdown
arrays. The preceding examples have used several of them, including mean, count,:markdown
min, and sum. However, you are not limited to only this set of:markdown
methods.:markdown

```:code
df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],:code
'key2' : ['one', 'two', 'one', 'two', 'one'],:code
'data1' : np.random.randn(5),:code
'data2' : np.random.randn(5)}):code
df:code
```:code

You can use aggregations of your own devising and additionally call any method that:markdown
is also defined on the grouped object. For example, you might recall that quantile:markdown
computes sample quantiles of a Series or a DataFrame’s columns.:markdown
While quantile is not explicitly implemented for GroupBy, it is a Series method and:markdown
thus available for use. Internally, GroupBy efficiently slices up the Series, calls:markdown
piece.quantile(0.9) for each piece, and then assembles those results together into:markdown
the result object::markdown

```:code
grouped = df.groupby('key1'):code
grouped['data1'].quantile(0.9):code
```:code

## To use your own aggregation functions, pass any function that aggregates an array to the aggregate or agg method::markdown

```:code
def peak_to_peak(arr)::code
return arr.max() - arr.min():code
:code
df[['data1', 'data2']].apply(peak_to_peak):code
:code
df.groupby('key1').agg(peak_to_peak):code
```:code

## You may notice that some methods like describe also work, even though they are not aggregations, strictly speaking::markdown

```:code
df.describe():code
:code
grouped.describe():code
```:code

## Exercise::markdown
For each unique value in the day column, compute the maximum, mininum, and the difference between the max and min for total_bill and tip_pct.:markdown


## Select Top Results on Aggregation:markdown
Suppose we wanted to select the top five tip_pct values by group. First, write a function that selects the rows with the largest values in a particular column::markdown

```:code
def top(df, n=5, column='tip_pct')::code
return df.sort_values(by=column, ascending=False)[:n]:code
:code
```:code

Now, if we group by smoker, say, and call apply with this function, we get the following::markdown

```:code
tips.groupby('smoker').apply(top):code
```:code

The top function is called on each row group from the:markdown
DataFrame, and then the results are glued together using pandas.concat, labeling the:markdown
pieces with the group names. The result therefore has a hierarchical index whose:markdown
inner level contains index values from the original DataFrame.:markdown

## Exercise::markdown
For each unique value in the day column, extract the top 3 total bills.:markdown


In the preceding examples, you see that the resulting object has a hierarchical index formed from the group keys along with the indexes of each piece of the original object. You can disable this by passing group_keys=False to groupby::markdown

```:code
tips.groupby('smoker', group_keys=False).apply(top):code
```:code

# What is a Pivot Table and How to Use It?:markdown
- A pivot table is a data summarization tool frequently found in spreadsheet programs:markdown
and other data analysis software.:markdown
- It aggregates a table of data by one or more keys,:markdown
arranging the data in a rectangle with some of the group keys along the rows and:markdown
some along the columns.:markdown
- Pivot tables in Python with pandas are made possible:markdown
through the groupby facility combined with reshape operations:markdown
utilizing hierarchical indexing.:markdown
- DataFrame has a pivot_table method, and:markdown
there is also a top-level pandas.pivot_table function.:markdown
- In addition to providing a:markdown
convenience interface to groupby, pivot_table can add partial totals, also known as:markdown
margins.:markdown
:markdown
## Returning to the `tips` dataset, suppose you wanted to compute a table of group means (the default pivot_table aggregation type) arranged by day and smoker on the rows::markdown
```:code
tips[['day', 'smoker', 'total_bill', 'tip']].pivot_table(index=['day', 'smoker']).mean():code
```:code

## This could have been produced with groupby directly. Now, suppose we want to aggregate only tip_pct and size, and additionally group by time. I’ll put smoker in the table columns and day in the rows::markdown
```:code
tips.pivot_table(['tip_pct', 'size'], index=['time', 'day'],:code
columns='smoker'):code
```:code

## We could augment this table to include partial totals by passing margins=True. This has the effect of adding All row and column labels, with corresponding values being the group statistics for all the data within a single tier::markdown
```:code
tips.pivot_table(['size', 'tip_pct'], index=['time', 'day'],:code
columns='smoker', margins=True):code
```:code

## Here, the All values are means without taking into account smoker versus nonsmoker (the All columns) or any of the two levels of grouping on the rows (the All row).:markdown
:markdown
## To use a different aggregation function, pass it to aggfunc. For example, 'count' or len will give you a cross-tabulation (count or frequency) of group sizes::markdown
```:code
tips.pivot_table('tip_pct', index=['time', 'smoker'], columns='day',:code
aggfunc=len, margins=True):code
```:code

## If some combinations are empty (or otherwise NA), you may wish to pass a fill_value::markdown
```:code
tips.pivot_table('tip_pct', index=['time', 'size', 'smoker'],:code
columns='day', aggfunc='mean', fill_value=0):code
```:code

# What is Cross-Tabulation? How to Use Crosstab?:markdown
## A cross-tabulation (or crosstab for short) is a special case of a pivot table that computes group frequencies. Here is an example::markdown

```:code
import pandas as pd:code
from io import StringIO:code
data = """\:code
Sample  Nationality  Handedness:code
1   USA  Right-handed:code
2   Japan    Left-handed:code
3   USA  Right-handed:code
4   Japan    Right-handed:code
5   Japan    Left-handed:code
6   Japan    Right-handed:code
7   USA  Right-handed:code
8   USA  Left-handed:code
9   Japan    Right-handed:code
10  USA  Right-handed""":code
data = pd.read_table(StringIO(data), sep='\s+'):code
```:code

## As part of some survey analysis, we might want to summarize this data by nationality and handedness. You could use pivot_table to do this, but the pandas.crosstab function can be more convenient::markdown

```:code
pd.crosstab(data.Nationality, data.Handedness, margins=True):code
```:code

## The first two arguments to crosstab can each either be an array or Series or a list of arrays. As in the tips data::markdown

```:code
pd.crosstab([tips.time, tips.day], tips.smoker, margins=True).plot.bar():code
```:code


