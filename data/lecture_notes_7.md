# Data Science Programming:markdown
___:markdown
:markdown
## Week 8: Data Loading and Merging:markdown
---:markdown
**Agenda:**:markdown
- Read and parse tables from HTML files:markdown
- pd.read_html(html_source.text):markdown
- Read and write JSON data:markdown
- json.loads():markdown
- json.dumps():markdown
- pd.read_json():markdown
- Combining and merging datasets:markdown
- numpy.concatenate(list of narrays, axis):markdown
- pandas.concat(list of DataFrames, axis):markdown
- pd.merge(df1, df2, left_on='key1', right_on='key2', how='outer'):markdown
- pd.merge(left1, right1, left_on='key', right_index=True):markdown
- left.join(right, on='key'):markdown

# import:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code
:code
import json:code
import requests:code

# Loading Data:markdown
## Accessing data is a necessary first step for data analysis. We are going to be focused on data input and output using pandas, though there are numerous tools in other libraries to help with reading and writing data in various formats. Input and output typically falls into a few main categories: reading text files and other more efficient on-disk formats, loading data from databases, and interacting with network sources like web APIs.:markdown

# HTML: Web Scraping:markdown
:markdown
### Python has many libraries for reading and writing data in the ubiquitous HTML and XML formats. Examples include lxml, Beautiful Soup, and html5lib. While lxml is comparatively much faster in general, the other libraries can better handle malformed HTML or XML files.:markdown
:markdown
### Pandas has a built-in function, read_html, which uses libraries like lxml and Beautiful Soup to automatically parse tables out of HTML files as DataFrame objects. To show how this works, read in an HTML file  from the United States FDIC government agency showing bank failures: (https://www.fdic.gov/resources/resolutions/bank-failures/failed-bank-list/).:markdown

```:code
import requests:code
:code
target_url = 'https://www.fdic.gov/resources/resolutions/bank-failures/failed-bank-list/':code
:code
html_source = requests.get(target_url):code
:code
html_source.status_code:code
:code
html_source.text:code
:code
tables = pd.read_html(html_source.text):code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


### Explore the tables.:markdown


### Change the column labels:markdown
```:code
df.columns = ['Bank', 'City', 'State', 'Cert',:code
'AI', 'ClosingDate', 'Fund']:code
```:code

## Exercise::markdown
Run the above code.:markdown


### Parse the datetime column:markdown
```:code
close_timestamps = pd.to_datetime(df['ClosingDate']):code
```:code


### Extract some interesting data:markdown
```:code
close_timestamps.dt.year.value_counts():code
:code
close_timestamps.dt.year.value_counts().sort_index():code
```:code

## Exercise::markdown
Run the above code.:markdown


# JSON Data:markdown
## JSON (short for JavaScript Object Notation) has become one of the standard formats for sending data by HTTP request between web browsers and other applications. It is a much more free-form data format than a tabular text form like CSV. Here is an example.:markdown

```:code
:code
:code
obj = """:code
{"name": "Wes",:code
"places_lived": ["United States", "Spain", "Germany"],:code
"pet": null,:code
"siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},:code
{"name": "Katie", "age": 38,:code
"pets": ["Sixes", "Stache", "Cisco"]}]:code
}:code
```:code

Create a DataFrame::markdown
```:code
pd.DataFrame([obj]):code
```:code

## How to parse a JSON structure into a Python object?:markdown
```:code
import json:code
:code
json.loads(obj):code
:code
pd.DataFrame([json.loads(obj)]):code
:code
result = json.loads(obj):code
:code
result['siblings']:code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Extract the nested information and create a flatten DataFrame.:markdown
```:code
sibs = pd.DataFrame(result['siblings']):code
:code
rows = []:code
for idx, row in sibs.iterrows()::code
pets = row['pets']:code
for pet in pets::code
cols = {}:code
cols['name'] = row['name']:code
cols['age']= row['age']:code
cols['pet'] = pet:code
rows.append(cols):code
:code
pd.DataFrame(rows):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## How you convert a JSON object or list of objects to a DataFrame or some other data structure for analysis will be up to you. Conveniently, you can pass a list of dicts (which were previously JSON objects) to the DataFrame constructor and select a subset of the data fields. For example, we did:markdown
```:code
result_df = pd.DataFrame(result['siblings']):code
:code
siblings = pd.DataFrame(result['siblings'], columns=['name', 'age']):code
```:code

## To flatten the entire dictionary as a DataFrame, we should merge the top-level DataFrame with the nested DataFrame based on the shared column, in this case, the name "Wes".:markdown

```:code
obj = """:code
{"name": "Wes",:code
"places_lived": ["United States", "Spain", "Germany"],:code
"pet": null,:code
"siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},:code
{"name": "Katie", "age": 38,:code
"pets": ["Sixes", "Stache", "Cisco"]}]:code
}:code
""":code
```:code

result = json.loads(obj):code

## Extract and Flatten the Nested Dictionary:markdown

sibs = pd.DataFrame(result['siblings']):code
:code
rows = []:code
for idx, row in sibs.iterrows()::code
pets = row['pets']:code
for pet in pets::code
cols = {}:code
cols['name'] = row['name']:code
cols['age']= row['age']:code
cols['pet'] = pet:code
rows.append(cols):code
:code
sibs_flat = pd.DataFrame(rows):code

sibs_flat = sibs_flat.rename(columns={'name':'sib_name', 'age':'sib_age', 'pet':'sib_pet'}):code
sibs_flat:code

## Add the Top-Level Name to the DataFrame from the Nested Dictionary:markdown

sibs_flat['name'] = result['name']:code
sibs_flat:code

## Extract and Flatten the Top-Level Dictionary:markdown

per = pd.DataFrame([result], columns=['name', 'places_lived', 'pet']):code
per:code

rows = []:code
for idx, row in per.iterrows()::code
places = row['places_lived']:code
for place in places::code
cols = {}:code
cols['name'] = row['name']:code
cols['place_lived']= place:code
cols['pet'] = row['pet']:code
rows.append(cols):code
:code
per_flat = pd.DataFrame(rows):code

per_flat:code

## Merge the top-level and nested DataFrames:markdown

pd.merge(per_flat, sibs_flat, on='name'):code

## The pandas `read_json` can automatically convert JSON datasets in specific arrangements into a Series or DataFrame. For example, let us load:markdown
```:code
example.json:code
```:code

from google.colab import files:code
files.upload():code

# show the content of the file:code
with open('example.json') as f::code
print(f.read()):code

pd.read_json('example.json'):code

# Combining and Merging Datasets:markdown

## Concatenating Along an Axis:markdown
## A kind of data combination operation is referred to interchangeably as concatenation, binding, or stacking.:markdown

```:code
df1 = pd.DataFrame([{'name':'Alice', 'Subject':'math', 'Score':95}, {'name':'Bob', 'Subject':'English', 'Score':96}]):code
:code
df2 = pd.DataFrame([['Alice', 'Frank', 'Charlie'], [19, 20, 18], ['alice@drexel', 'frank@drexel', 'charlie@drexel']]):code
:code
df2 = df2.T:code
:code
df2.columns = ['name', 'age', 'email']:code
:code
df3  = pd.concat([df1, df2], axis=1, keys=['df1', 'df2']):code
:code
pd.concat([df1, df2]):code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## In the context of pandas objects such as Series and DataFrame, having labeled axes enable you to further generalize array concatenation. In particular, you have a number of additional things to think about::markdown
* If the objects are indexed differently on the other axes, should we combine the:markdown
distinct elements in these axes or use only the shared values (the intersection)?:markdown
* Do the concatenated chunks of data need to be identifiable in the resulting:markdown
object?:markdown
* Does the “concatenation axis” contain data that needs to be preserved? In many:markdown
cases, the default integer labels in a DataFrame are best discarded during:markdown
concatenation.:markdown
:markdown
## The concat function in pandas provides a consistent way to address each of these concerns. Here is a list of examples to illustrate how it works. Suppose we have three Series with no index overlap::markdown
```:code
s1 = pd.Series([0, 1], index=['a', 'b']):code
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e']):code
s3 = pd.Series([5, 6], index=['f', 'g']):code
```:code

## Calling concat with these objects in a list glues together the values and indexes::markdown
```:code
pd.concat([s1, s2, s3], axis = 1):code
```:code


## By default concat works along axis=0, producing another Series. If you pass axis=1, the result will instead be a DataFrame (axis=1 is the columns)::markdown
```:code
pd.concat([s1, s2, s3]):code
```:code


## In this case there is no overlap on the other axis, which as you can see is the sorted union (the 'outer' join) of the indexes. You can instead intersect them by passing join='inner'::markdown
```:code
s4 = pd.concat([s1, s3]):code
pd.concat([s1, s4], axis=1, join='inner'):code
```:code


### Combine DataFrames::markdown
We can concatenate dataframes on axis to combine them vertically or horizontally.:markdown
```:code
df1 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'],:code
columns=['one', 'two']):code
df2 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'],:code
columns=['three', 'four']):code
:code
pd.concat([df1, df2], axis=0):code
:code
pd.concat([df1, df2], axis=1):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Database-Style DataFrame Joins:markdown
## Merge or join operations combine datasets by linking rows using one or more keys. These operations are central to relational databases (e.g., SQL-based). The merge function in pandas is the main entry point for using these algorithms on your data. Let’s start with a simple example::markdown

```:code
df1 = pd.DataFrame({'key1': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],:code
'data1': range(7)}):code
df2 = pd.DataFrame({'key2': ['a', 'b', 'd'],:code
'data2': range(3)}):code
:code
pd.merge(df1, df2, left_on='key1', right_on='key2', how='outer'):code
:code
df1.merge(df2, left_on='key1', right_on='key2'):code
:code
df1.join(df2):code
```:code

## Exercise::markdown
Run and test the above code.:markdown


## Note that I didn't specify which column to join on. If that information is not specified, merge uses the overlapping column names as the keys. It's a good practice to specify explicitly, though::markdown
```:code
pd.merge(df1, df2, left_on='key', right_on='key'):code
```:code


## By default merge does an 'inner' join; the keys in the result are the intersection, or the common set found in both tables. Other possible options are 'left', 'right', and 'outer'. The outer join takes the union of the keys, combining the effect of applying both left and right joins::markdown
```:code
pd.merge(df1, df2):code
:code
pd.merge(df1, df2, how='outer'):code
```:code


## Merging on Index:markdown
## In some cases, the merge key(s) in a DataFrame will be found in its index. In this case, you can pass left_index=True or right_index=True (or both) to indicate that the index should be used as the merge key::markdown

```:code
left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],:code
'value': range(6)}):code
right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b']):code
:code
pd.merge(left1, right1.reset_index(), left_on='key', right_on='index'):code
:code
pd.merge(left1, right1, left_on='key', right_index=True):code
:code
:code
```:code

## Exercise::markdown
Run and test the above code.:markdown


