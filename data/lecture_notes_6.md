# Data Science Programming:markdown
___:markdown
:markdown
## Week 7: Data Cleaning and Preparation:markdown
---:markdown
:markdown
**Agenda**:markdown
- Missing values are None, numpy.nan:markdown
- DataFrame.isnull() detects missing values:markdown
- DataFrame.notnull() detects non-missing values:markdown
- DataFrame.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)[source]:markdown
- DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None):markdown
- DataFrame.transform() performs operations element-wise:markdown
- duplicated() returns a boolean Series indicating whether each row is a duplicate:markdown
- drop_duplicates() returns a DataFrame where the duplicated array is removed::markdown
- Transform data by applying function:markdown
- data.replace([-999, -1000], np.nan): replace values:markdown
- data.index.map(transform): rename index:markdown
- data.rename(index=str.title, columns=str.upper): rename index and columns:markdown

import numpy as np:code
import pandas as pd:code
import matplotlib.pyplot as plt:code

from google.colab import data_table:code
data_table.enable_dataframe_formatter:code

# Messy Data:markdown

from google.colab import files:code
files.upload():code

df = pd.read_csv('daily-minimum-temperatures-in-me.csv', parse_dates=['Date']):code
df.shape:code

df.info():code

df = df.rename(columns={'Daily minimum temperatures': 'temperatures'}):code
df.head():code

df.plot(x='Date', y='temperatures'):code

df[df['temperatures'].apply(lambda x: not str(x).replace('.', '', 1).isdigit())]:code

# list the rows that temperature can't be evaluated as numeric:code
df[pd.to_numeric(df['temperatures'], errors='coerce').isnull()]:code

# Handling Missing Data:markdown
:markdown
Missing data occurs commonly in many data analysis applications.:markdown
All of the descriptive statistics on pandas objects exclude missing data by default. For numeric data, pandas uses the floating-point value NaN (Not a Number) to represent missing data. We call this a sentinel value that can be easily detected.:markdown

## `isnull()`: detects missing values:markdown
```:code
string_data = pd.Series(['aardvark', 'artichoke', np.nan, 'avocado']):code
:code
string_data.isnull():code
:code
string_data.isnull().sum():code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Reasons for Missing Values:markdown
:markdown
Before we start treating the missing values, it is important to understand the various reasons for the missingness in data. Broadly speaking, there can be three possible reasons: (references: Wikipedia: https://en.wikipedia.org/wiki/Missing_data ; Flexible Imputation of Missing Data by R: https://stefvanbuuren.name/fimd/sec-MCAR.html):markdown
:markdown
1. Missing Completely at Random (MCAR): The missing values on a given variable (Y) are not associated with other variables in a given data set or with the variable (Y) itself. In other words, there is no particular reason for the missing values. An example of MCAR is a weighing scale that ran out of batteries. Some of the data will be missing simply because of bad luck.:markdown
:markdown
2. Missing at Random (MAR): MAR occurs when the missingness is not random, but where missingness can be fully accounted for by variables where there is complete information. For example, when placed on a soft surface, a weighing scale may produce more missing values than when placed on a hard surface. Such data are thus not MCAR. If, however, we know surface type and if we can assume MCAR within the type of surface, then the data are MAR.:markdown
:markdown
3. Missing Not at Random (MNAR): Missingness depends on unobserved data or the value of the missing data itself. MNAR means that the probability of being missing varies for reasons that are unknown to us. For example, the weighing scale mechanism may wear out over time, producing more missing data as time progresses, but we may fail to note this.:markdown

## Dealing with Missing Data:markdown
Missing data are often denoted by a "na, n/a, N/a, N/A (not applicable)" or "Nan, NaN, NAN (not a number)". The denomination depends on the used progamming langauge (NaN is the most common in python language for machine learning). In general, there are two approaches to handle missing data: Deletion and Imputation.:markdown

## Deletions:markdown
:markdown
![](https://imgur.com/tBvdfyX.png):markdown
:markdown
Deletion means to delete the missing values from a dataset. Deletions are further categorized into three types::markdown
:markdown
### Pairwise Deletion:markdown
:markdown
>Parwise Deletion is used when values are missing completely at random i.e MCAR. During Pairwise deletion, only the missing values are deleted. All operations in pandas like mean,sum etc intrinsically skip missing values.:markdown
:markdown
### Listwise Deletion/ Dropping rows:markdown
:markdown
>During Listwise deletion, complete rows (which contain the missing values) are deleted. As a result, it is also called Complete Case deletion. Like Pairwise deletion, listwise deletions are also only used for MCAR values.:markdown
:markdown
### Dropping complete columns:markdown
:markdown
>If a column contains a lot of missing values, say more than 80%, and the feature is not significant, you might want to delete that feature. However, again, it is not a good methodology to delete data.:markdown

:markdown
## `dropna()`: Drops missing values; for Series, it returns non-null data.:markdown
:markdown
```:code
from numpy import nan as NA:code
data = pd.Series([1, NA, 3.5, NA, 7]):code
:code
data.dropna():code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## `notnull()`: returns list of boolean values indicating whether an item is null or not:markdown
:markdown
```:code
data.notnull():code
```:code


##  `dropna()` With DataFrame objects, things are a bit more complex.  You may want to drop rows or columns that are all NA or only those containing any NAs. `dropna` by default drops any row containing a missing value.:markdown
:markdown
```:code
data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA],:code
[NA, NA, NA], [NA, 6.5, 3.]]):code
:code
data.dropna():code
:code
data.dropna(axis=1):code
:code
data.isnull().sum():code
:code
data.isnull().sum().sum():code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Passing how='all' will only drop rows that are all NA::markdown
:markdown
```:code
data.dropna(how='all'):code
:code
data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA],:code
[NA, NA, NA], [NA, 6.5, 3.]]):code
:code
data[3] = NA:code
:code
data.dropna(axis=1, how='all'):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Suppose you want to keep only rows containing a certain number of observations. You can indicate this with the thresh argument as `dropna(thresh=2)`.:markdown

```:code
df = pd.DataFrame(np.random.randn(7, 3)):code
df.iloc[:4, 1] = NA:code
df.iloc[:2, 2] = NA:code
:code
df.dropna():code
df.dropna(thresh=2):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Imputations:markdown
:markdown
![](https://imgur.com/bL0iHde.png):markdown
:markdown
>Imputation refers to replacing missing data with substituted values. There are a lot of ways in which the missing values can be imputed depending upon the nature of the problem and data. Dependng upon the nature of the problem, imputation techniques can be broadly they can be classified as follows::markdown
:markdown
:markdown
### Basic Imputation Techniques:markdown
:markdown
- Imputating with a constant value:markdown
- Imputation using the statistics (mean, median or most frequent) of each column in which the missing values are located:markdown
:markdown
For this we shall use the `The SimpleImputer` class from sklearn.:markdown

### Filling In Missing Data:markdown
For most purposes, the `fillna` method is the workhorse function to use. Calling `fillna` with a constant replaces missing values with that value::markdown
:markdown
```:code
data = pd.DataFrame([[1., 2, 3.], [4., NA, NA],:code
[NA, NA, NA], [NA, 8, 9]]):code
:code
:code
data.fillna(0):code
:code
```:code

Calling fillna with a dict, you can use a different fill value for each column::markdown
:markdown
```:code
data.fillna({0: 5, 1: 0.5, 2: 0}):code
:code
data.fillna(data.mean(), inplace = True):code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


### fillna returns a new object, but you can modify the existing object in-place::markdown
:markdown
```:code
_ = df.fillna(0, inplace=True):code
:code
df.fillna({1:0.3, 2:0.5}, inplace=True):code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


### With fillna you can do lots of other things with a little creativity. For example, you might pass the mean or median value of a Series::markdown
:markdown
```:code
data = pd.Series([1., NA, 3.5, NA, 7]):code
:code
data.fillna(data.median()):code
:code
```:code


## We can also fill in the missing value using the previous or next value by `ffill` or `bfill`.:markdown
```:code
df = pd.DataFrame(np.random.randn(6, 3)):code
df.iloc[2:, 1] = NA:code
df.iloc[4:, 2] = NA:code
:code
data.fillna(method='bfill'):code
:code
df.fillna(method='ffill', limit=2):code
```:code

# Data Transformation:markdown

## The transform() function::markdown
- The transform() function in pandas is used to perform operations on each element of a series or column in a DataFrame.:markdown
- Unlike apply(), transform() returns an object that is the same size as the input.:markdown
- This makes it particularly useful for filling missing values.:markdown
- The transform() function is often used with groupby objects to perform operations within groups while maintaining the original DataFrame shape.:markdown
```:code
df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar', 'foo', 'bar', ],:code
'B': [1, np.nan, 2, 4, np.nan, 2]}):code
B_imputed = df.groupby('A')['B'].transform(lambda x: x.fillna(x.mean())):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Removing Duplicates:markdown

```:code
data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],:code
'k2': [1, 1, 2, 3, 3, 4, 4]}):code
:code
```:code

## The DataFrame method `duplicated()` returns a boolean Series indicating whether each row is a duplicate (has been observed in a previous row) or not::markdown
```:code
data.duplicated():code
:code
```:code


### drop_duplicates returns a DataFrame where the duplicated array is removed::markdown
```:code
data.drop_duplicates():code
```:code


### drop_duplicates can take a column as a parameter.:markdown
:markdown
```:code
data['v1'] = range(7):code
:code
data.drop_duplicates(['k1', 'k2']):code
:code
```:code


### duplicated and drop_duplicates by default keep the first observed value combination. Passing keep='last' will return the last one::markdown
:markdown
```:code
data.drop_duplicates(['k1', 'k2'], keep='last'):code
:code
data.drop_duplicates(keep='last'):code
:code
```:code
:markdown



## Transforming Data Using a Function or Mapping:markdown
### For many datasets, you may wish to perform some transformation based on the values in an array, Series, or column in a DataFrame. Consider the following data collected about various kinds of meat::markdown
:markdown
```:code
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon',:code
'Pastrami', 'corned beef', 'Bacon',:code
'pastrami', 'honey ham', 'nova lox'],:code
'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]}):code
:code
```:code

### Suppose we wanted to add a column indicating the type of animal that each food came from.:markdown
### Step 1: Write down a mapping of each distinct meat type to the kind of animal::markdown
:markdown
```:code
meat_to_animal = {:code
'bacon': 'pig',:code
'pulled pork': 'pig',:code
'pastrami': 'cow',:code
'corned beef': 'cow',:code
'honey ham': 'pig',:code
'nova lox': 'salmon':code
}:code
```:code

### Step 2: Normalize the text if the map only uses normalized lower-case strings.:markdown
:markdown
```:code
data['food'].str.lower():code
:code
lowercased = data['food'].str.lower():code
```:code

### Step 3: Apply the map method on a Series by accepting a function or dict-like object containing a mapping:markdown
:markdown
```:code
data['animal'] = data['food'].str.lower().map(meat_to_animal):code
```:code

## We could also have passed a function that does all the work::markdown
```:code
data['food'].map(lambda x: meat_to_animal[x.lower()]):code
```:code

# Replacing Values:markdown
Filling in missing data with the fillna method is a special case of more general value:markdown
replacement. As you’ve already seen, map can be used to modify a subset of values in:markdown
an object.:markdown
## `replace` provides a simpler and more flexible way to do so. Let’s consider this Series::markdown
:markdown
```:code
data = pd.Series([1., -999., 2., -999., -1000., 3.]):code
```:code

The -999 values might be sentinel values for missing data. To replace these with NA:markdown
values that pandas understands, we can use replace, producing a new Series (unless:markdown
you pass inplace=True)::markdown
:markdown
```:code
data1 = data.replace([-999, -1000], np.nan):code
:code
data1.fillna(data1.mean()):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


If you want to replace multiple values at once, you instead pass a list and then the:markdown
substitute value::markdown
:markdown
```:code
data.replace([-999, -1000], np.nan):code
```:code

To use a different replacement for each value, pass a list of substitutes::markdown
:markdown
```:code
data.replace([-999, -1000], [np.nan, 0]):code
```:code

The argument passed can also be a dict::markdown
:markdown
```:code
data.replace({-999: np.nan, -1000: 0}):code
```:code

## Renaming Axis Indexes:markdown
### Like values in a Series, axis labels can be similarly transformed by a function or mapping of some form to produce new, differently labeled objects.:markdown
### We can also modify the axes in-place without creating a new data structure. Here’s a simple example::markdown
:markdown
```:code
data = pd.DataFrame(np.arange(12).reshape((3, 4)),:code
index=['Ohio', 'Colorado', 'New York'],:code
columns=['one', 'two', 'three', 'four']):code
```:code

### We can modify the index of the DataFrame in-place::markdown
:markdown
```:code
transform = lambda x: x[:4].upper():code
data.index = data.index.map(transform):code
```:code

### If we want to create a transformed version of a dataset without modifying the original, a useful method is rename::markdown
:markdown
```:code
data.rename(index=str.title, columns=str.upper):code
:code
data.rename(columns={"one":'column1'}):code
```:code

### Notably, rename can be used in conjunction with a dict-like object providing new values for a subset of the axis labels::markdown
:markdown
```:code
data.rename(index={'OHIO': 'INDIANA'},:code
columns={'three': 'peekaboo'}):code
```:code

### rename saves you from the chore of copying the DataFrame manually and assigning to its index and columns attributes. Should you wish to modify a dataset in-place, pass inplace=True::markdown
:markdown
```:code
data.rename(index={'OHIO': 'INDIANA'}, inplace=True):code
```:code


