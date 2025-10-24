# Data Science Programming:markdown
___:markdown
:markdown
## Week 4: Lecture: Pandas, Plot, and Visualization:markdown
---:markdown
:markdown
**Agenda:**:markdown
- Introduction to data visualization:markdown
- About Figures, Axes, and Axis:markdown
- Univariate plots:markdown
- Bivariate Plots:markdown
- Colors, markers, and line styles:markdown
- Ticks and legends:markdown
- Plotting with seaborn:markdown
- An application on map:markdown

# import libraries:code
import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code
import seaborn as sns:code

# Pandas (continue from last lecture):markdown

from google.colab import files:code
files.upload():code

# pd.read_csv("train.csv"):code

# df.apply(f) - Applying Functions:markdown
:markdown
Apply a function along the axis of the DataFrame (rows or columns). apply() is a powerful way to manipulate pandas data structures, making it a key method in data transformation and preparation tasks.:markdown
:markdown
- When used on a Series, apply() allows you to apply a function to each element in the Series.:markdown
- When used on a DataFrame, you can apply a function along each column or row. The axis is specified using axis parameter::markdown
- axis=0 or axis='index' (default) applies the function to each column.:markdown
- axis=1 or axis='columns' applies the function to each row.:markdown
- apply() is commonly used with lambda functions for quick operations without defining a separate function.:markdown

## Exercise::markdown
Use apply() to create a new column 'AgeCategory' so that the passengers are categoried into 'child' if Age <= 18, 'youth' if 18 < Age < 30, and 'adult' if Age >= 30.:markdown
:markdown
```:code
def age_category(age)::code
if age <= 18::code
return 'Child':code
elif age < 30::code
return 'Youth':code
else::code
return 'Adult':code
:code
df['AgeCategory'] = df['Age'].apply(age_category):code
```:code


# df.sort_values(by) - Sorting by Values:markdown
:markdown
Sorts the rows based on the values in a particular column.:markdown
```:code
df.sort_values(by='Age'):code
```:code

## Exercise::markdown
Sort the passengers in the DataFrame by their ticket fares in ascending and descending order.:markdown


# df.sort_index(axis) - Sorting by Index:markdown
:markdown
Sorts the DataFrame based on its index (row or column names).:markdown
```:code
df.sort_index(axis=1):code
```:code

## Exercise::markdown
Sort the columns by their names by ascending and descending from left to right.:markdown


# Introduction to Data Visualization:markdown
- Importance of Informative Visualizations::markdown
* Central task in data analysis:markdown
* Aids in the exploratory process:markdown
* Helps identify outliers & data transformations:markdown
* Generates ideas for models:markdown
- End Goal for Some::markdown
* Building interactive web visualizations:markdown
- Python's Capabilities::markdown
* Numerous add-on libraries:markdown
* Supports both static and dynamic visualizations:markdown
:markdown
![](https://i.imgur.com/h2wyqTx.png):markdown

# Visualization in Plots::markdown
## To create meaningful data visualizations, you should consider two key aspects::markdown
:markdown
- What to plot: Identify the specific data points or variables that best convey your insights or support your analysis.:markdown
- How to plot: Choose the most appropriate type of plot (e.g., bar charts, line graphs, scatter plots) that effectively represents your data and communicates the intended message to your audience.:markdown

# Summary of Plotting Methods:markdown
Given a DataFrame, df, with a numerical column, num, and a categorical column, cat, we can visualize the data with the following functions::markdown
- `df['num'].plot.hist()`: histogram of num.:markdown
- `df['cat'].value_counts().plot.bar()`: bar plot of cat.:markdown
- `df['num'].plot.box()`: box plot of num.:markdown
- `df.boxplot(column='num', by='cat')`: Box plot of num based on categories in cat.:markdown

# Create a sample DataFrame, df, with a numerical column, num, and a categorical column, cat:code
np.random.seed(0):code
df = pd.DataFrame({'num': np.random.randn(1000),:code
'cat': np.random.choice(['A', 'B', 'C'], 1000)}):code

fig, axes = plt.subplots(2, 2, figsize=(12, 8)):code
:code
axes[0, 0].hist(df['num']):code
axes[0, 1].bar(df['cat'].value_counts().index, df['cat'].value_counts()):code
axes[1, 0].boxplot(df['num']):code
df.boxplot(column = 'num', by = 'cat', ax=axes[1, 1]):code
:code
plt.tight_layout():code
plt.show():code

# Why is Data Visualization so Important?:markdown

## Complete the following practice exercise::markdown
In this exercise you will find the importance of data visualization vs. numerical summary. Anscombe's quartet comprises four data sets that have nearly identical simple descriptive statistics, yet have very different distributions and appear very different when graphed.:markdown

### Step 1: Load the anscombes.csv as a DataFrame.:markdown


### Step 2: Show the column names of the DataFrame.:markdown


### Step 3: List the unique values in dataset column.:markdown


### Step 4: Show the distribution of data by dataset.:markdown
Hint: Use value_counts() to list the number of records for each unique dataset.:markdown


### Step 5: Compute the mean, std of x and y for each unique dataset.:markdown
Hint: use groupby("dataset"):markdown



## What do you observe from the means and stds?:markdown

### Step 6: For each dataset, scatter plot the values in x and y.:markdown


## What do you observe from the visualization?:markdown

# About Figures, Axes, and Axis:markdown
Matplotlib graphs your data on `Figures` (e.g., windows, Jupyter widgets, etc.), each of which can contain one or more `Axes`, an area where points can be specified in terms of x-y coordinates (or theta-r in a polar plot, x-y-z in a 3D plot, etc.). The simplest way of creating a Figure with an Axes is using pyplot.subplots. We can then use Axes.plot to draw some data on the Axes.:markdown
- Figure: The whole figure. Think of it as the 'canvas' for your painting.:markdown
- Axes: This is what you think of as 'a plot', it is the region of the image with the data space (bounded by the x-axis and y-axis).:markdown
- Axis: These are the number-line-like objects that take care of setting the graph limits and generating the ticks and ticklabels.:markdown

fig, ax = plt.subplots()  # Create a figure containing a single axes.:code
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])  # Plot some data on the axes.:code

## Parts of a Figure:markdown
Here are the components of a Matplotlib Figure. Figure, Axes, and Axis refer to different parts of a plot. Understanding these components is crucial for effectively using the library to create and customize visualizations.:markdown
:markdown
![matplotlib figure](https://i.imgur.com/SIHDY22.png):markdown

## Figures:markdown
The whole figure. The Figure keeps track of all the child Axes, a group of 'special' Artists (titles, figure legends, colorbars, etc), and even nested subfigures. The easiest way to create a new Figure is with pyplot::markdown

fig = plt.figure()  # an empty figure with no Axes:code
fig, ax = plt.subplots()  # a figure with a single Axes:code
fig, axs = plt.subplots(2, 2)  # a figure with a 2x2 grid of Axes:code

## Axes:markdown
An Axes is an Artist attached to a Figure that contains a region for plotting data, and usually includes two (or three in the case of 3D) Axis objects (be aware of the difference between Axes and Axis) that provide ticks and tick labels to provide scales for the data in the Axes. Each Axes also has a title (set via set_title()), an x-label (set via set_xlabel()), and a y-label set via set_ylabel()).:markdown
:markdown
## Axis:markdown
These objects set the scale and limits and generate ticks (the marks on the Axis) and ticklabels (strings labeling the ticks).:markdown

# We can plot using Pandas:markdown
- Pandas can load and transform data:markdown
- Pandas also provides easy-to-use and expressive plotting API.:markdown
:markdown
# We can plot using special library such as seaborn:markdown
- seaborn is a statistical graphics library.:markdown
- seaborn simplifies creating many common visualization types.:markdown
- With data that requires aggregation or summarization before making a plot, using the seaborn package can make things much simpler.:markdown

# Load a Dataset:markdown

# Cardiovascular Disease dataset:code
file = "/content/drive/MyDrive/Colab Notebooks/courses/INFO212/CVD_cleaned.csv":code

# Show the shape and head:code

# show the columns:code

# show info:code

# show summary statistics:code

# show summary statistics of object columns:code

# Univariate Plot:markdown
- Univariate plots visualize data one variable at a time. They help in understanding the distribution, central tendencies, and variability of the data.:markdown
:markdown
- Importance: Before delving deep into complex multivariate visualizations, understanding the basics of each variable is essential. Univariate plots offer insights into the nature of a variable (whether it's normally distributed, skewed, has outliers, etc.).:markdown
:markdown
- Numerical Data: This refers to data that is quantifiable and is typically represented as numbers.:markdown
* Visualization Techniques: Histograms are great for understanding the distribution of the data. Boxplots, on the other hand, provide a summary of the central tendency, variability, and outliers in the dataset.:markdown
:markdown
- Categorical Data: This type of data represents categories or labels. These can't be measured in quantitative terms but can be counted or grouped.:markdown
* Visualization Techniques: Bar charts are suitable for comparing the frequency of different categories. Pie charts provide a proportionate representation of each category in a whole.:markdown

## Histogram:markdown
- A histogram visualizes the distribution of a continuous numerical dataset by grouping the data into 'bins' or intervals.:markdown
- The width of the bins can be consistent or variable. The height of each bar represents the frequency of data points within that bin. For example, a histogram could show how many people fall into specific age groups (0-10, 10-20, etc.).:markdown

# plot histogram for BMI:code

## Bar charts and categorical data:markdown
- Bar charts map categories to numbers: for example, the amount of eggs consumed for breakfast (a category) to a number breakfast-eating Americans; or, in our case, wine-producing provinces of the world (category) to the number of labels of wines they produce (number).:markdown

# plot bar charts of Age_Category categories sorted by index:code

## Box plot:markdown
- Box plots offer a summarized view of the data distribution, highlighting central tendency, spread, and skewness.:markdown
- Components of Box Plot::markdown
* Median: Central value:markdown
* IQR: Middle 50% of data:markdown
* Whiskers: Data spread outside the IQR:markdown
* Outliers: Unusual data points:markdown
- Purpose & Use::markdown
* Summarizes data distribution:markdown
* Easily compare multiple groups:markdown
* Identifies skewness and potential outliers:markdown

# box plot for Fruit_Consumption:code

# box plot Fruit_Consumption < 60:code

## Pie plot:markdown

# pie plot for General_Health:code

## Line charts:markdown
- For the points values as real number, line charts should be the first choice for visualizing the distribution.:markdown

# line plot for Age_Category categories sorted by index:code

## Area charts:markdown
- Area charts are just line charts, but with the bottom shaded in. That's it!:markdown

# area plot Diabeties:code

# Density plots:markdown
Kernel density estimate (KDE) plot is type of a smoothed histogram.:markdown
:markdown
To make a KDE plot, we use the sns.kdeplot command. Setting shade=True colors the area below the curve (and data= chooses the column we would like to plot).:markdown

# Use seaborn kdeplot for Height_(cm) with shape=True:code

# kdeplot for BMI:code

# Bivariate Plot:markdown
- Two Variables: Bivariate plots provide insights into the relationship between two different variables.:markdown
- Relationships or Disparities: They can highlight whether there's a connection between the variables, whether they move in tandem (positive correlation), move in opposite directions (negative correlation), or if there's no discernible pattern.:markdown
:markdown
- Common Types of Bivariate Plots::markdown
:markdown
* Scatter Plots: Points plotted in two dimensions; often used to see if two variables have a linear relationship.:markdown
* Line Graphs: Usually displays a series of data points connected by straight line segments; can be useful in observing trends over continuous data.:markdown
* Hexbin Plots: Used when dealing with a lot of data points; it groups points in hexagonal bins and then color-codes those bins based on the number of points they contain.:markdown
* Contour Plots: Useful to represent three-dimensional data in two dimensions; shows areas where there is a high density of data points.:markdown
:markdown
- Usage of Bivariate Plots::markdown
:markdown
* Identifying Trends: Spot patterns between two variables.:markdown
Spotting Outliers: Identify data points that don't follow the general trend.:markdown
* Assessing Correlation: Understand if and how strongly two variables are related.:markdown

## Scatter plot:markdown
- Scatter plots use dots to represent values of two numerical variables. The position of each dot on the horizontal (X) and vertical (Y) axis shows the value for an individual data point.:markdown

# scatter plot BMI vs. Weight_(kg):code

## Bivariate line plot:markdown
- Let us illustrate the relationship between the BMI and FriedPotato_Consumption for a random subset of 1000 people::markdown
:markdown
* The x-axis represents the BMI of the people.:markdown
* The y-axis represents the FriedPotato_Consumption of the people.:markdown

# randomly sample 1000 people:code

# line plot BMI vs. FriedPotato_Consumption for the 1000 samples:code

# Plotting:code
plt.figure(figsize=(10, 6)):code
sns.lineplot(x='BMI', y='FriedPotato_Consumption', data=sample_people, sort=False):code
plt.title('BMI vs. FriedPotato_Consumption'):code
plt.xlabel('BMI'):code
plt.ylabel('FriedPotato_Consumption'):code
plt.grid(True, which='both', linestyle='--', linewidth=0.5):code
plt.tight_layout():code
:code
plt.show():code

## Hexbin plot:markdown
- A hexbin plot is a bivariate histogram where the plane is divided into hexagons. Each hexagon represents a 'bin' and the number of data points that fall into each bin is represented by the color of the hexagon. Hexbin plots are useful for visualizing the relationship between two numerical variables, especially when there are many data points, which could lead to overplotting in scatter plots.:markdown

# Plotting:code
plt.figure(figsize=(10, 6)):code
plt.hexbin(df['Height_(cm)'], df['Weight_(kg)'], gridsize=50, cmap='Blues', mincnt=1):code
plt.colorbar(label='Count'):code
plt.title('Hexbin Plot of Heights vs. Weights'):code
plt.xlabel('Heights (CM)'):code
plt.ylabel('Weights (KG)'):code
plt.grid(True, which='both', linestyle='--', linewidth=0.5, alpha=0.5):code
plt.tight_layout():code
:code
plt.show():code

## Contour Plot:markdown
- A contour plot, also known as a contour map or level plot, is a graphical technique used to represent a 3-dimensional surface in two dimensions. It's commonly used to display the patterns of response as two predictor variables change. Essentially, it visualizes regions of constant values (contours) on a 2D plane.:markdown

# Plotting:code
plt.figure(figsize=(10, 6)):code
sns.jointplot(x=df['Alcohol_Consumption'], y=df['Fruit_Consumption'], kind='kde', fill=True):code
plt.title('Contour Plot of Alcohol_Consumption vs. Fruit_Consumption'):code
plt.xlabel('Alcohol_Consumption'):code
plt.ylabel('Fruit_Consumption'):code
plt.grid(True, which='both', linestyle='--', linewidth=0.5, alpha=0.5):code
plt.tight_layout():code
:code
plt.show():code

# Colors, Markers, and Line Styles:markdown
- Python visualization libraries provide powerful toosl to change styles of plots:markdown

```:code
ax = df.plot.scatter(x = 'BMI', y = 'Weight_(kg)', \:code
figsize = (12, 6), \:code
color = 'mediumvioletred', \:code
linestyle = '--', \:code
fontsize = 16):code
ax.set_title('BMI vs. Weights'):code
```:code

- We can set markers on plotted lines for data points.:markdown

```:code
from numpy.random import randn:code
plt.plot(randn(30).cumsum(), color = 'r', linestyle = '--', marker = 'o'):code
```:code

# Ticks and Legends:markdown
- We can set ticks and legends for plots.:markdown

```:code
fig = plt.figure():code
ax = fig.add_subplot(1, 1, 1):code
ax.plot(np.random.randn(1000).cumsum()):code
```:code

```:code
fig = plt.figure():code
ax = fig.add_subplot(1, 1, 1):code
ax.plot(np.random.randn(1000).cumsum()):code
ticks = ax.set_xticks([0, 250, 500, 750, 1000]):code
labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'],:code
rotation=30, fontsize='small'):code
```:code

```:code
fig = plt.figure():code
ax = fig.add_subplot(1, 1, 1):code
blue_lines = ax.plot(np.random.randn(1000).cumsum(), label ="Random Numbers"):code
ticks = ax.set_xticks([0, 250, 500, 750, 1000]):code
labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'],:code
rotation=30, fontsize='small'):code
ax.set_title('My first matplotlib plot'):code
ax.set_ylabel("Random Number"):code
ax.set_xlabel('Stages'):code
ax.legend(loc='best'):code
```:code

- Add legends to a plot.:markdown

```:code
from numpy.random import randn:code
fig = plt.figure(); ax = fig.add_subplot(1, 1, 1):code
ax.plot(randn(1000).cumsum(), 'k', label='one'):code
ax.plot(randn(1000).cumsum(), 'k--', label='two'):code
ax.plot(randn(1000).cumsum(), 'k.', label='three'):code
ax.legend(loc='best'):code
```:code

# How to Save Plots to File?:markdown

```:code
plt.savefig('figpath.svg'):code
```:code

```:code
plt.savefig('figpath.png', dpi=400, bbox_inches='tight'):code
```:code

```:code
plt.plot(randn(30).cumsum(), color = 'r', linestyle = '--', marker = 'o'):code
plt.savefig('figpath.png', dpi=400, bbox_inches='tight'):code
```:code

# Plot the Titanic Data:markdown

### Step 1: Load train.csv as a DataFrame.:markdown


### Step 2: Plot the Survived column into two bars, one for 0 and one for 1.:markdown


### Step 3: Plot a histogram for Age.:markdown


### Step 4: Plot the Age distribution in box plot.:markdown


### Step 5: Plot the age distribution for each Pclass in box plot.:markdown
Hint: use seaborn's boxplot().:markdown


### Step 6: Visualize the chances of survival related to Age and Pclass.:markdown

```:code
grid = sns.FacetGrid(df, col='Survived', row='Pclass', aspect=1.6):code
grid.map(sns.boxplot, data=df, y='Age'):code
grid.add_legend();:code
```:code

```:code
grid = sns.FacetGrid(df, col='Survived', row='Pclass', aspect=1.6):code
grid.map(plt.hist, 'Age', alpha=.5, bins=20):code
grid.add_legend();:code
```:code



# Plot the US States:markdown

import pandas as pd:code
import numpy as np:code
import matplotlib.pyplot as plt:code

# upload US-states.txt:code

```:code
# Read in the US-states.txt and make a list of longitudinal and latitudinal data:code
segments = []:code
points = []:code
:code
lat_long_regex = r"<point lat=\"(.*)\" lng=\"(.*)\"":code
:code
with open("US-states.txt", "r") as f::code
lines = [line for line in f]:code
:code
for line in lines::code
if line.startswith("</state>")::code
for p1, p2 in zip(points, points[1:])::code
segments.append((p1, p2)):code
points = []:code
s = re.search(lat_long_regex, line):code
if s::code
lat, lon = s.groups():code
points.append((float(lon), float(lat))):code
```:code

```:code
plt.figure(figsize = (12, 8)):code
for ((lon1, lat1), (lon2, lat2)) in segments::code
plt.plot([lon1, lon2], [lat1, lat2], color = '0.8'):code
plt.plot(-75.165222, 39.952583, 'bo', label = 'Philadelphia'):code
plt.legend(loc = 'best'):code
```:code

**Summary:**:markdown
- df.plot.bar():markdown
- df.plot.hist():markdown
- df.plot.box():markdown
- df.plot.pie():markdown
- df.plot.line():markdown
- df.plot.area():markdown
- df.plot.scatter():markdown
- fig = plt.figure():markdown
- ax = fig.add_subplot(1, 1, 1):markdown
- ax.plot(np.random.randn(1000).cumsum(), label ="Random Numbers"):markdown
- ax.set_xticks([0, 250, 500, 750, 1000]):markdown
- ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'], rotation=30, fontsize='small'):markdown
- ax.set_title('My first matplotlib plot'):markdown
- ax.set_ylabel("Random Number"):markdown
- ax.set_xlabel('Stages'):markdown
- ax.legend(loc='best'):markdown
- sns.boxplot():markdown
- grid = sns.FacetGrid(df, col = 'Survived', row='Pclass'):markdown
- grid.map(sns.boxplot, data=df, x='Embarked', y='Age', hue='Sex'):markdown
- grid.add_legend():markdown
- sns.kdeplot(data=df['Age'], shade=True):markdown
- sns.jointplot(x=df['Age'], y=df['Fare'], kind="kde", shade=True):markdown


