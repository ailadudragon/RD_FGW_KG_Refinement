# Data Science Programming:markdown
___:markdown
:markdown
## Lecture: Numpy: Arrays and Vectorized Computation:markdown
___:markdown
:markdown
**Agenda:**:markdown
- Introduction to NumPy:markdown
* What is NumPy?:markdown
* Why use NumPy?:markdown
- Getting Started with NumPy:markdown
* Installation and Setup:markdown
* Creating Arrays:markdown
- Diving Deep: Array Basics:markdown
* Array Properties:markdown
* Reshaping and Flattening:markdown
* Basic Operations:markdown
- Advanced Array Operations:markdown
* Broadcasting:markdown
* Indexing and Slicing:markdown
* Fancy Indexing:markdown
- Mathematical Capabilities:markdown
* Universal Functions (ufuncs):markdown
* Aggregation Functions:markdown
* Aggregation on Axes:markdown
* Matrix Operations:markdown
* Linear Algebra with NumPy:markdown
- Utility and Functionalities:markdown
* Random Number Generation:markdown
* File I/O:markdown
* Advanced Features (Optional):markdown
- Practical Applications:markdown
* Real-world uses of NumPy:markdown

# What did we do last week?:markdown

## Import Numpy:markdown
```import numpy as np```:code


# Reshaping Arrays:markdown
Changing the structure (number of rows, columns, dimensions) of an array without changing its data.:markdown
```:code
arr = np.arange(6):code
arr.reshape(2, 3):code
arr.ravel()  # Flattens the array:code
```:code

## Exercise::markdown
Reshape the 3x4 array as 2x6. Can it be reshaped as a 3x5 array?:markdown


# Broadcasting:markdown
Broadcasting defines how NumPy handles operations on arrays of different shapes. It extends smaller arrays to match the shape of larger ones.:markdown
```:code
a = np.array([1, 2, 3]):code
b = 2:code
a * b  # array([2, 4, 6]):code
```:code

## Exercise::markdown
Can you add a 3x4 array and a 1x4 array? Can you add a 3x4 array and a 2x4 array? Can you add a 3x1 array and a 2x4 array? Can you add a 2x1 array and a 2x4 array? Can you add a 3x4 array and a 3x1 array? Explain what is broadcasted in each permissiable operation.:markdown


# Indexing and Slicing:markdown
Accessing parts of the array based on conditions or positions.:markdown
```:code
arr = np.array([[1, 2, 3], [4, 5, 6]]):code
arr[0, 1]      # 2:code
arr[:, 1:3]    # array([[2, 3], [5, 6]]):code
```:code

$m\times n$ array's index starts from 0, 0 to m, n.:markdown
![](https://i.imgur.com/2zwzXCw.png):markdown


# Fancy Indexing:markdown
We also can use integer arrays or boolean values for array indexing.:markdown
```:code
arr = np.array([10, 20, 30, 40, 50]):code
idx = np.array([1, 3]):code
arr[idx]  # array([20, 40]):code
```:code

## Exercise::markdown
Given an array. Extract all the items that have an odd index.:markdown


# Aggregate Functions:markdown
Functions that summarize values in arrays.:markdown
```:code
arr = np.array([1, 2, 3, 4, 5]):code
np.sum(arr)   # 15:code
np.mean(arr)  # 3.0:code
```:code

# Aggregation on Axes:markdown
Aggregating on an axis means performing a computation along the specified dimension, reducing its size.:markdown
- For example, when you sum over axis=0, you're summing across rows (resulting in a column vector).:markdown
- Summing over axis=1 sums across columns (resulting in a row vector).:markdown
:markdown
Common Aggregations: sum(), mean(), std(), etc., can all take an axis argument.:markdown
```:code
arr = np.array([[1, 2, 3], [4, 5, 6]]):code
arr.sum(axis=0)  # Output: array([5, 7, 9]):code
arr.mean(axis=1) # Output: array([2., 5.]):code
```:code
:markdown
The concept of axes extends beyond 2D arrays. For a 3D array, you'd also have axis=2, and so forth for higher dimensions:markdown

# Random Number Generation:markdown
Generate arrays of random numbers from various distributions.:markdown
```:code
np.random.rand(3, 2)     # Uniform distribution between [0, 1):code
np.random.randn(5)      # Standard normal distribution:code
np.random.randint(1, 10, size=4)  # 4 random integers between 1 (inclusive) a:code
```:code

## Exercise::markdown
Create a 4x5 array with random values in [0, 1]. Calculate the mean of each row, mean of each column, and the overall mean.:markdown


# Write High Quality Code at Any Point of Your Programming Journey:markdown
:markdown
## Code quality is defined by a set of attributes such as:markdown
- Maintainability:markdown
- Reusability:markdown
- Readability:markdown
- Efficiency:markdown
- Error proneness:markdown
- Modularity.:markdown
:markdown
## Adopt a Coding Convention:markdown
- Use camelCase and PascalCase:markdown
- camelCase::markdown
- aVariables:markdown
- aFunctionName:markdown
- PascalCase::markdown
- ClassName:markdown
- ConstantName:markdown
- EnumSet:markdown
- Meaningful names:markdown
- Short lines:markdown
:markdown
## Write comments:markdown
- Add comments to explain your functions:markdown
- Add comments to explain your logic:markdown
- Add comments to explain your thoughts:markdown
- Add comments to explain the pre- and post- conditions:markdown
- Add comments to explain the usage:markdown
- It is all for your own benefits and others.:markdown
:markdown
## Adopt test-driven development:markdown
- Test and verify the code geneated by AI tools, that is where you separate yourself from a machine.:markdown
- Think about all the things that could trip your code up and write tests to ensure that your code caters for all scenarios.:markdown
- Murphy’s Law: Anything can go wrong, will go wrong.:markdown
- You test should achieve 100% coverage.:markdown
- Test all extreme cases and boundary conditions.:markdown

# File I/O:markdown
:markdown
Reading from and writing to files with NumPy functionalities.:markdown
```:code
arr = np.array([[1, 2], [3, 4]]):code
np.save('my_array.npy', arr)           # Save to a .npy binary file:code
loaded_arr = np.load('my_array.npy')   # Load from a .npy binary file:code
np.savetxt('my_array.txt', arr)        # Save to a text file:code
loaded_txt = np.loadtxt('my_array.txt')# Load from a text file:code
```:code

# Practical Applications:markdown

## Motivation Scenario:markdown
#### Imagine you work in a health services center. The Center recently started to monitor the daily body temperatures of their patients. The temperature data is stored as a spreadsheet/tabular format/matrix/2-D array::markdown
- each row records a patient's temperatures:markdown
- each column corresponds to a day:markdown
:markdown
#### You want to quickly answer the following questions using Python::markdown
- daily mean:markdown
- patient mean:markdown
- daily max/min:markdown
- patient max/min:markdown

## Data We will Use:markdown
Go to the Week 2 module on the course shell, download the zipped file "inflammation-data.zip" and unzip it in the "datasets" folder under "info212." You should see a directory "inflammation-data" containing data files.:markdown
:markdown
To use the files,:markdown
- you can upload the entire folder to your Google drive; mount your drive and access them through their absolute paths.:markdown
- or use files.upload() to upload the files from your local disk to the notebook.:markdown


What is the middle value in the array? value at row 30 and column 20.:markdown


How to select part of the data? For example, the inflammation of first 10 days for patients 3-6?:markdown


What if we need the maximum inflammation for each patient over all days (as in the next diagram on the left) or the average for each day (as in the diagram on the right)? As the diagram below shows, we want to perform the operation across an axis.:markdown
![](https://i.imgur.com/T8tMG7M.png):markdown

How to calculate the daily average inflammation?:markdown

How to calculate the average inflammations for all the patients?:markdown

How to visualize the inflammation values in a whole picture?:markdown

```:code
plt.imshow(data):code
```:code

How do the average inflammation values look like across days?:markdown

```:code
plt.plot(daily_averages):code
```:code

How to put all the figures side by side?:markdown

fig = plt.figure(figsize = (10, 3)):code
axe1 = fig.add_subplot(1,3,1):code
axe2 = fig.add_subplot(132):code
axe3 = fig.add_subplot(133):code
:code
axe1.plot(arr.mean(axis=0)):code
axe1.plot(arr.max(axis=0)):code
axe1.plot(arr.min(axis=0)):code

```:code
fig = plt.figure(figsize=(10, 3)):code
axes1 = fig.add_subplot(131):code
axes2 = fig.add_subplot(132):code
axes3 = fig.add_subplot(133):code
:code
axes1.plot(np.mean(data, axis = 0)):code
axes1.set_ylabel("Average"):code
axes2.plot(np.max(data, axis = 0)):code
axes2.set_ylabel("Max"):code
axes3.plot(np.min(data, axis = 0)):code
axes3.set_ylabel("Min"):code
# axes2.set_xlim(-1, 50):code
:code
plt.tight_layout():code
```:code

files = ["inflammation-01.csv", "inflammation-02.csv"]:code
list_arrs = []:code
list_arrs.append(np.loadtxt(files[0], delimiter = ",")):code
list_arrs.append(np.loadtxt(files[1], delimiter = ",")):code
for arr in list_arrs::code
fig = plt.figure(figsize=(15,6)):code
ax1 = fig.add_subplot(131):code
ax2 = fig.add_subplot(132):code
ax3 = fig.add_subplot(133):code
ax1.plot(np.mean(arr, axis=1)):code
ax1.set_ylabel("Average"):code
ax2.plot(np.max(arr, axis=1)):code
ax2.set_ylabel("Maximum"):code
ax3.plot(np.min(arr, axis=1)):code
ax3.set_ylabel("Minimum"):code

What is the largest change in inflammation for each patient?:markdown

```:code
np.max(np.diff(data, axis = 1), axis = 1):code
```:code

```:code
plt.scatter(np.arange(0, 60), np.max(np.diff(data, axis = 1), axis = 1)):code
plt.grid():code
```:code

```:code
plt.figure(figsize = (12, 9)):code
plt.scatter(np.arange(0, 60), np.max(np.diff(data, axis = 1), axis = 1), color = 'r'):code
plt.title('Max Inflammation Change for Patient'):code
plt.xlabel("patient Index"):code
plt.ylabel("Max Inflammatin Change"):code
plt.xticks(np.arange(0, 60, 2)):code
```:code

### Boolean Indexing:markdown
How to get the patients whose average inflammation is great than 7?:markdown

```:code
data[patient_averages > 7]:code
```:code

### Methods for Boolean Arrays:markdown
Boolean values are coerced to 1 (True) and 0 (False) in the preceding methods. Thus,:markdown
sum is often used as a means of counting True values in a boolean array.:markdown
:markdown
How many inflammation values greater than 15 for each patient?:markdown

```:code
np.sum(data > 15, axis = 1):code
```:code

## File Input and Output with Arrays:markdown
np.save and np.load are the two workhorse functions for efficiently saving and loading:markdown
array data on disk. Arrays are saved by default in an uncompressed raw binary:markdown
format with file extension .npy.:markdown

```:code
np.save('datasets/inflamation.npy', data):code
```:code

```:code
data_load = np.load('datasets/inflamation.npy'):code
```:code

```:code
data_load == data:code
```:code

**Summary:**:markdown
- arr = np.loadtxt('inflammation-01.csv', delimiter=","): load data as np array.:markdown
- data.shape: get the dimensions of an array:markdown
- data.mean(axis=0): compute means along row, axis=0, which are column means:markdown
- data.mean(axis=1): compute means along columns, axis=1, which are row means:markdown
- plt.plot(np.mean(arr, axis=0)): plot column means:markdown
- subsetting::markdown
- data[6]: row at index=6:markdown
- data[[3, 5, 7]]: rows at index 3, 5, 7:markdown
- data[:-1]: all rows except the last one:markdown
---:markdown

