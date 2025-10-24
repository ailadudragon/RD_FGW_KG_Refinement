# Data Science Programming:markdown
___:markdown
:markdown
## Lecture: Pandas:markdown
---:markdown
:markdown
**Agenda:**:markdown
- Introduction to Pandas:markdown
* What is Pandas?:markdown
* Importance in data analysis:markdown
- Exploring DataFrame Structure:markdown
* df.shape: Dimensions of DataFrame:markdown
* df.describe(): Descriptive statistics of DataFrame:markdown
- Manipulating DataFrames:markdown
* df.reindex(newIndex): Adjusting data to new indices:markdown
* df.drop(columns, axis): Dropping columns from DataFrame:markdown
- DataFrame Indexing:markdown
* df.iloc[]: Integer-location based indexing:markdown
* df.loc[]: Label-based indexing:markdown
- Function Application on DataFrames:markdown
* df.apply(f): Applying functions to DataFrame:markdown
- Sorting in DataFrames:markdown
* df.sort_values(by): Sorting rows by column values:markdown
* df.sort_index(axis): Sorting by DataFrame index:markdown
- Unique Values and Counts:markdown
* df.Pclass.unique(): Extracting unique values from a column:markdown
* df.Pclass.value_counts(): Counting unique value occurrences:markdown
- Read a CSV file as a DataFrame::markdown
* pd.read_csv():markdown
---:markdown

## Titanic Data Set - Machine Learning for Disaster:markdown

from IPython.display import HTML:code
HTML('<iframe width="800" height="450" src="https://www.youtube.com/embed/bYOn3-PhA9c?si=ZFuiA_6MBXKp3d_c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>'):code

Kaggle competition: https://www.kaggle.com/competitions/titanic:markdown

# import libraries:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code

from google.colab import files:code
files.upload():code

titanic = pd.read_csv('train.csv'):code
titanic.shape:code

titanic.head():code

titvals = titanic.values:code
titvals:code

# What is Pandas?:markdown
- Pandas: High-performance, easy-to-use data structures and data analysis tools.:markdown
- Fundamental for data manipulation in Python.:markdown
- Contains data structures and data manipulation tools designed to make data cleaning and analysis fast and easy in Python.:markdown
- Often used togeter with numerical computing tools like NumPy and SciPy, analytical libraries like statsmodels and scikit-learn, and data visualization libraries like matplotlib.:markdown
- Adopts significant parts of NumPy’s idiomatic style of array-based computing, especially array-based functions and a preference for data processing without 'for' loops.:markdown

# Series and DataFrame:markdown

## Series:markdown
- A Series is a one-dimensional array-like object containing a sequence of values (of similar types to NumPy types) and an associated array of data labels, called its index.:markdown
:markdown
![](https://i.imgur.com/4hGYKrX.png):markdown
:markdown
Key Features::markdown
- Homogeneous data:markdown
- Size immutable:markdown
- Data values can be mutable:markdown
- Axis labels (can be of any type: string, integer, date-time, etc.):markdown

## Exercise::markdown
Creating a Series from an array::markdown
```:code
s1 = pd.Series([1, 2, 3, 4, 5]):code
```:code
What are the index and values?:markdown
:markdown


## Exercise::markdown
Create a series from a dictionary::markdown
```:code
data = {'a': 0., 'b': 1., 'c': 2.}:code
s2 = pd.Series(data):code
```:code
:markdown
What are the index and values?:markdown


## Exercise::markdown
Create a series from a scalar::markdown
```:code
s3 = pd.Series(5, index=[0, 1, 2, 3]):code
```:code
:markdown
What are the index and values?:markdown


### Accessing Data in a Series::markdown
:markdown
- Using position::markdown
```:code
s1[0]  # Output: 1:code
```:code
:markdown
- Using the label/index::markdown
```:code
s2['b']  # Output: 1.0:code
```:code

## DataFrame:markdown
:markdown
- A DataFrame represents a rectangular table of data and contains an ordered collection:markdown
of columns, each of which can be a different value type (numeric, string,:markdown
boolean, etc.). The DataFrame has both a row and column index; it can be thought of:markdown
as a dict of Series all sharing the same index.:markdown
:markdown
![](https://i.imgur.com/3MUvN7R.png):markdown
:markdown
Key Features::markdown
- Heterogeneous data (can store columns of different types):markdown
Size mutable:markdown
- Data can be set and fetched using row labels and column names:markdown
:markdown

## Exercise::markdown
Creating a DataFrame from a dictionary of Series or dictionaries::markdown
```:code
data = {'Name': pd.Series(['Alice', 'Bob', 'Chris']),:code
'Age': pd.Series([25, 26, 24])}:code
df1 = pd.DataFrame(data):code
```:code
:markdown
What are the index and column labels of the dataframe?:markdown


## Exercise::markdown
Create a DataFrame from a list of dictionaries::markdown
```:code
data = [{'Name': 'Alice', 'Age': 25}, {'Name': 'Bob', 'Age': 26}]:code
df2 = pd.DataFrame(data):code
```:code
:markdown
What are the index and column labels of the dataframe?:markdown


## Exercise::markdown
Create a DataFrame from an array or list::markdown
```:code
data = [['Alex', 10], ['Bob', 15], ['Charlie', 14]]:code
df3 = pd.DataFrame(data, columns=['Name', 'Age']):code
```:code
:markdown
What are the index and column labels of the dataframe?:markdown


## Series and DataFrame:markdown
:markdown
![](https://i.imgur.com/4QEW9Dd.png):markdown

### Each column or row of a DataFrame is a Series:markdown
![](https://i.imgur.com/rHJsq6F.png):markdown

# Pandas Essential Functions:markdown
Let us load a CSV file into a Pandas DataFrame.:markdown
:markdown
We can access the CSV file from mounted Google Drive or using files.upload().:markdown
:markdown
The CSV file we will load is the train.csv available on the course shell.:markdown

# pd.read_csv() to load a titanic CSV file as a pandas DataFrame:code

# df.shape - Dimensions of a DataFrame:markdown
:markdown
Returns a tuple representing the dimensionality of the DataFrame.:markdown
Format: (number of rows, number of columns):markdown
```:code
df.shape  # Output: (891, 12):code
```:code


# df.describe() - Describing the DataFrame:markdown
:markdown
Provides descriptive statistics of the DataFrame's columns.:markdown
Includes: count, mean, std, min, 25th percentile, median (50th percentile), 75th percentile, max.:markdown
```:code
df.describe():code
```:code


# Wednesday, 10/9/2024:markdown

import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code

# df.reindex(newIndex) - Reindexing:markdown
:markdown
Adjusts data to conform to a new index.:markdown
Might introduce NaN values if the new index doesn't match existing data.:markdown
```:code
data = {'Name': pd.Series(['Alice', 'Bob', 'Chris']),:code
'Age': pd.Series([25, 26, 24])}:code
df1 = pd.DataFrame(data):code
new_index = [2, 3, 5, 7]:code
df1.reindex(new_index):code
```:code


## Load train.csv as df:markdown


# df.drop(columns, axis) - Dropping Columns:markdown
:markdown
Removes columns or rows from the DataFrame.:markdown
axis=1 signifies the operation is column-wise.:markdown
```:code
df.drop(['PassengerId', 'Survived'], axis=1):code
```:code


# df.iloc[] - Integer-location based indexing:markdown
:markdown
Purely integer-location based indexing for selection by position.:markdown
```:code
df.iloc[:10, :]['Age']:code
```:code


# df.loc[] - Label-based indexing:markdown
:markdown
Primarily label based indexing. Can also be used with a boolean array.:markdown
```:code
df.loc[:10, :]['Age']:code
```:code


df.apply(f) - Applying Functions:markdown
:markdown
Apply a function along the axis of the DataFrame (rows or columns).:markdown
```:code
def age_category(age)::code
if age < 18::code
return 'Child':code
else::code
return 'Adult':code
df['AgeCategory'] = df['Age'].apply(age_category):code
```:code


# df.sort_values(by) - Sorting by Values:markdown
:markdown
Sorts the rows based on the values in a particular column.:markdown
```:code
df.sort_values(by='Age'):code
```:code


# df.sort_index(axis) - Sorting by Index:markdown
:markdown
Sorts the DataFrame based on its index (row or column names).:markdown
```:code
df.sort_index(axis=1):code
```:code


# df.Pclass.unique() - Unique Values in a Column:markdown
:markdown
Gets the unique values from a particular column.:markdown
```:code
df.Pclass.unique()  # e.g., Output: [3, 1, 2]:code
```:code


# df.Pclass.value_counts() - Value Counts in a Column:markdown
:markdown
Counts the occurrences of unique values in a particular column.:markdown
```:code
df.Pclass.value_counts():code
```:code


# df.info() - Overview of the DataFrame:markdown
:markdown
- Provides a concise summary of the DataFrame.:markdown
- Shows the number of non-null values in each column.:markdown
- Displays the data type (dtype) of each column.:markdown
- Helpful for quickly assessing the completeness and type of our data.:markdown
```:code
df.info():code
```:code

**Summary:**:markdown
- df.shape: find the dimensions of a DataFrame:markdown
- df.describe(): describe the DataFrame:markdown
- df.reindex(newIndex): create a new DataFrame with the data conformed to a new index.:markdown
- df.drop(['PassengerId', 'Survived'], axis = 1): drop columns (axis=1):markdown
- df.iloc[:10, :]['Age']: find the Age for row at index 0-9:markdown
- df.loc[:10, :]['Age']: find the Age for row with labels 0-9:markdown
- df.apply(f): apply function f to each row of the DataFrame df.:markdown
- df.sort_values(by='Age'): sort the rows by the values in the column Age:markdown
- df.sort_index(axis=1): sort the columns by the column names.:markdown
- df.Pclass.unique(): get the unique values in the column Pclass.:markdown
- df.Pclass.value_counts(): get the count for each unique value in the column Pclass.:markdown
- pd.read_csv(): read a CSV file as a DataFrame:markdown
---:markdown


