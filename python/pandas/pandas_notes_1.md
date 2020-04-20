# pandas笔记1——pandas基础1
## pandas简介
- pandas是python中常用的数据分析包之一，可以方便地和数据库交互
- pandas is a fast, powerful, flexible and easy to use open source data analysis and manipulation tool,
built on top of the Python programming language.
- [pandas main page](http://pandas.pydata.org/)
- [pandas installation instructions](http://pandas.pydata.org/pandas-docs/stable/install.html)
- [Anaconda distribution of Python](https://www.continuum.io/downloads) (includes pandas)
- [How to use the IPython/Jupyter notebook](https://youtu.be/IsXXlYVBt1M?t=5m17s) (video)
## How do I read a tabular data file into pandas?
* 例子
```python
import pandas as pd
# read a dataset of Chipotle orders directly from a URL and store the results in a DataFrame
orders = pd.read_table('http://bit.ly/chiporders')
#读取table，url也可以

# examine the first 5 rows
orders.head()

# read a dataset of movie reviewers (modifying the default parameter values for read_table)
user_cols = ['user_id', 'age', 'gender', 'occupation', 'zip_code']
users = pd.read_table('http://bit.ly/movieusers', sep='|', header=None, names=user_cols)
#以'|'作为分隔符读取，header=None表明没有表头，最后names=user_cols，表明用自动定义的字段作为表头字段

# examine the first 5 rows
users.head()
```
* Documentation for [**`read_table`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_table.html)
## How do I select a pandas Series from a DataFrame?
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_table('http://bit.ly/uforeports', sep=',')

# read_csv is equivalent to read_table, except it assumes a comma separator
ufo = pd.read_csv('http://bit.ly/uforeports')

# select the 'City' Series using bracket notation
ufo['City']

# or equivalently, use dot notation
ufo.City
```
**Bracket notation** will always work, whereas **dot notation** has limitations:

- Dot notation doesn't work if there are **spaces** in the Series name
- Dot notation doesn't work if the Series has the same name as a **DataFrame method or attribute** (like 'head' or 'shape')
- Dot notation can't be used to define the name of a **new Series** (see below)
```python
# create a new 'Location' Series (must use bracket notation to define the Series name)
ufo['Location'] = ufo.City + ', ' + ufo.State
ufo.head()
```
* 所以，好的习惯是总是使用`ufo['City']`这种方式来取字段，而不是点号`ufo.City`这种方式
## describe函数/方法，方法和属性的区别
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')

# example method: show the first 5 rows
movies.head()

# example method: calculate summary statistics
movies.describe()
# 默认只统计数值型的字段


# example attribute: number of rows and columns
movies.shape

# example attribute: data type of each column
movies.dtypes

# use an optional parameter to the describe method to summarize only 'object' columns
movies.describe(include=['object'])
```
* Documentation for [**`describe`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.describe.html)

## How do I rename columns in a pandas DataFrame
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')

# examine the column names
ufo.columns

# rename two of the columns by using the 'rename' method
ufo.rename(columns={'Colors Reported':'Colors_Reported', 'Shape Reported':'Shape_Reported'}, inplace=True)
ufo.columns

# replace all of the column names by overwriting the 'columns' attribute
ufo_cols = ['city', 'colors reported', 'shape reported', 'state', 'time']
ufo.columns = ufo_cols
ufo.columns

# replace the column names during the file reading process by using the 'names' parameter
ufo = pd.read_csv('http://bit.ly/uforeports', header=0, names=ufo_cols)
ufo.columns

# replace all spaces with underscores in the column names by using the 'str.replace' method
ufo.columns = ufo.columns.str.replace(' ', '_')
ufo.columns
```
* Documentation for [**`rename`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html)
* Documentation for [**`read_csv`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)
* Documentation for [**`str.replace`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.str.replace.html)
## How do I remove columns from a pandas DataFrame
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.head()

# remove a single column (axis=1 refers to columns,按列操作)
ufo.drop('Colors Reported', axis=1, inplace=True)#inplace=True代表要用修改后的数据替换掉原数据
ufo.head()

# remove multiple columns at once
ufo.drop(['City', 'State'], axis=1, inplace=True)
ufo.head()

# remove multiple rows at once (axis=0 refers to rows)
ufo.drop([0, 1], axis=0, inplace=True)#表明去掉0行和1行
ufo.head()
```
* Documentation for [**`drop`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.drop.html)
## How do I sort a pandas DataFrame or a Series?
* **Note:** None of the sorting methods below affect the underlying data. (In other words, the sorting is temporary).
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')
movies.head()

# sort the 'title' Series in ascending order (returns a Series)
movies.title.sort_values().head()
# 默认按升序排序

# sort in descending order instead
movies.title.sort_values(ascending=False).head()

# sort the entire DataFrame by the 'title' Series (returns a DataFrame)
movies.sort_values('title').head()
# 默认按升序排序

# sort in descending order instead
movies.sort_values('title', ascending=False).head()

# sort the DataFrame first by 'content_rating', then by 'duration'
movies.sort_values(['content_rating', 'duration']).head()
```
* Documentation for [**`sort_values`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sort_values.html) for a **Series**.
* Documentation for [**`sort_values`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html) for a **DataFrame**.
## How do I filter rows of a pandas DataFrame by column value
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')
movies.head()

# examine the number of rows and columns
movies.shape

#**Goal:** Filter the DataFrame rows to only show movies with a 'duration' of at least 200 minutes.

# create a list in which each element refers to a DataFrame row: True if the row satisfies the condition, False otherwise
booleans = []
for length in movies.duration:
    if length >= 200:
        booleans.append(True)
    else:
        booleans.append(False)

# confirm that the list has the same length as the DataFrame
len(booleans)

# examine the first five list elements
booleans[0:5]

# convert the list to a Series
is_long = pd.Series(booleans)
is_long.head()

# use bracket notation with the boolean Series to tell the DataFrame which rows to display
movies[is_long]


# simplify the steps above: no need to write a for loop to create 'is_long' since pandas will broadcast the comparison
is_long = movies.duration >= 200
movies[is_long]

# or equivalently, write it in one line (no need to create the 'is_long' object)
movies[movies.duration >= 200]

# select the 'genre' Series from the filtered DataFrame
movies[movies.duration >= 200].genre

# or equivalently, use the 'loc' method
movies.loc[movies.duration >= 200, 'genre']
```
* Documentation for [**`loc`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html)
## How do I apply multiple filter criteria to a pandas DataFrame? 
* Understanding **logical operators:**
	- **`and`**: True only if **both sides** of the operator are True
	- **`or`**: True if **either side** of the operator is True
* Rules for specifying **multiple filter criteria** in pandas:
	- use **`&`** instead of **`and`** 用and会报错
	- use **`|`** instead of **`or`** 用or会报错
	- add **parentheses** around each condition to specify evaluation order
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')
movies.head()

# filter the DataFrame to only show movies with a 'duration' of at least 200 minutes
movies[movies.duration >= 200]

# **Goal:** Further filter the DataFrame of long movies (duration >= 200) to only show movies which also have a 'genre' of 'Drama'
# CORRECT: use the '&' operator to specify that both conditions are required
movies[(movies.duration >=200) & (movies.genre == 'Drama')]

# **Goal:** Filter the original DataFrame to show movies with a 'genre' of 'Crime' or 'Drama' or 'Action'
# use the '|' operator to specify that a row can match any of the three criteria
movies[(movies.genre == 'Crime') | (movies.genre == 'Drama') | (movies.genre == 'Action')].head(10)

# or equivalently, use the 'isin' method
movies[movies.genre.isin(['Crime', 'Drama', 'Action'])].head(10)
```
* Documentation for [**`isin`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.isin.html)
## Your pandas questions answered! 
* **Question:** When reading from a file, how do I read in only a subset of the columns?
```python
# read a dataset of UFO reports into a DataFrame, and check the columns
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.columns

# specify which columns to include by name
ufo = pd.read_csv('http://bit.ly/uforeports', usecols=['City', 'State'])

# or equivalently, specify columns by position
ufo = pd.read_csv('http://bit.ly/uforeports', usecols=[0, 4])
ufo.columns
```
* **Question:** When reading from a file, how do I read in only a subset of the rows?
```python
# specify how many rows to read
ufo = pd.read_csv('http://bit.ly/uforeports', nrows=3)
ufo
```
* **Question:** How do I iterate through a Series?
```python
# Series are directly iterable (like a list)
for c in ufo.City:
    print(c)
```
* **Question:** How do I iterate through a DataFrame?
```python
# various methods are available to iterate through a DataFrame
for index, row in ufo.iterrows():
    print(index, row.City, row.State)
```
* Documentation for [**`iterrows`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iterrows.html)
* **Question:** How do I drop all non-numeric columns from a DataFrame?
```python
# read a dataset of alcohol consumption into a DataFrame, and check the data types
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.dtypes

# only include numeric columns in the DataFrame
import numpy as np
drinks.select_dtypes(include=[np.number]).dtypes
```
* Documentation for [**`select_dtypes`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.select_dtypes.html)
* **Question:** How do I know whether I should pass an argument as a string or a list?
```python
# describe all of the numeric columns
drinks.describe()

# pass the string 'all' to describe all columns
drinks.describe(include='all')

# pass a list of data types to only describe certain types
drinks.describe(include=['object', 'float64'])

# pass a list even if you only want to describe a single data type
drinks.describe(include=['object'])
```