# pandas笔记3——pandas基础3
## How do I make my pandas DataFrame smaller and faster?
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# exact memory usage is unknown because object columns are references elsewhere
drinks.info()

# force pandas to calculate the true memory usage
drinks.info(memory_usage='deep')

# calculate the memory usage for each Series (in bytes)
drinks.memory_usage(deep=True)
# 如果没有deep=True,则并不是精确值，只是大于等于那个值

# use the 'category' data type (new in pandas 0.15) to store the 'continent' strings as integers
drinks['continent'] = drinks.continent.astype('category')
drinks.dtypes
# 对continent的内容unique之后升序排列后，从0开始编码为整数


# 'continent' Series appears to be unchanged
drinks.continent.head()
# 内容呈现不变，但实际上内部存储是按整数来存，通过一个lookup table来提取内容

# strings are now encoded (0 means 'Africa', 1 means 'Asia', 2 means 'Europe', etc.)
drinks.continent.cat.codes.head()

# memory usage has been drastically reduced
drinks.memory_usage(deep=True)

# repeat this process for the 'country' Series
drinks['country'] = drinks.country.astype('category')
drinks.memory_usage(deep=True)

# memory usage increased because we created 193 categories
drinks.country.cat.categories
# pandas的Index type，其中元素就是这些categories

# create a small DataFrame from a dictionary
df = pd.DataFrame({'ID':[100, 101, 102, 103], 'quality':['good', 'very good', 'good', 'excellent']})
df

# sort the DataFrame by the 'quality' Series (alphabetical order)
df.sort_values('quality')

# define a logical ordering for the categories
from pandas.api.types import CategoricalDtype
quality_cat = CategoricalDtype(['good','very good','excellent'],ordered=True)
df['quality']=df['quality'].astype(quality_cat)
df.quality

# sort the DataFrame by the 'quality' Series (logical order)
df.sort_values('quality')

# comparison operators work with ordered categories
df.loc[df.quality > 'good', :]
```
* Documentation for [**`info`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.info.html) and [**`memory_usage`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.memory_usage.html)
* The **category** data type should only be used with a string Series that has a **small number of possible values**.
## How do I use pandas with scikit-learn to create Kaggle submissions?
* **Goal:** Predict passenger survival aboard the Titanic based on [passenger attributes](https://www.kaggle.com/c/titanic/data)
```python
# read the training dataset from Kaggle's Titanic competition into a DataFrame
train = pd.read_csv('http://bit.ly/kaggletrain')
train.head()

# create a feature matrix 'X' by selecting two DataFrame columns
feature_cols = ['Pclass', 'Parch']
X = train.loc[:, feature_cols]
X.shape

# create a response vector 'y' by selecting a Series
y = train.Survived
y.shape

# fit a classification model to the training data
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()
logreg.fit(X, y)

# read the testing dataset from Kaggle's Titanic competition into a DataFrame
test = pd.read_csv('http://bit.ly/kaggletest')
test.head()

# create a feature matrix from the testing data that matches the training data
X_new = test.loc[:, feature_cols]
X_new.shape

# use the fitted model to make predictions for the testing set observations
new_pred_class = logreg.predict(X_new)

# create a DataFrame of passenger IDs and testing set predictions
pd.DataFrame({'PassengerId':test.PassengerId, 'Survived':new_pred_class}).head()

# ensure that PassengerID is the first column by setting it as the index
pd.DataFrame({'PassengerId':test.PassengerId, 'Survived':new_pred_class}).set_index('PassengerId').head()

# write the DataFrame to a CSV file that can be submitted to Kaggle
pd.DataFrame({'PassengerId':test.PassengerId, 'Survived':new_pred_class}).set_index('PassengerId').to_csv('sub.csv')

# save a DataFrame to disk ("pickle it")
train.to_pickle('train.pkl')

# read a pickled object from disk ("unpickle it")
pd.read_pickle('train.pkl').head()
```
* **Note:** There is no need to convert these pandas objects to NumPy arrays. scikit-learn will understand these objects as long as they are entirely numeric and the proper shapes.
* Documentation for the [**`DataFrame`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) constructor
* Documentation for [**`to_csv`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_csv.html)
* Documentation for [**`to_pickle`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_pickle.html) and [**`read_pickle`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_pickle.html)
## More of your pandas questions answered!
* **Question:** Could you explain how to read the pandas documentation?
	* 例子: Google 搜索 `pandas read_csv` 即可找到文档
	* 读文档，看是什么level对应的方法，比如`pd.xxx`, `pd.DataFrame.xxx`, `pd.Series.xxx`,然后阅读参数等详细内容
* **Question:** What is the difference between **`ufo.isnull()`** and **`pd.isnull(ufo)`**?
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.head()

# use 'isnull' as a top-level function
pd.isnull(ufo).head()

# equivalent: use 'isnull' as a DataFrame method
ufo.isnull().head()
```
* **Question:** Why are DataFrame slices inclusive when using **`.loc`**, but exclusive when using **`.iloc`**?
```python
# label-based slicing is inclusive of the start and stop
ufo.loc[0:4, :]

# position-based slicing is inclusive of the start and exclusive of the stop
ufo.iloc[0:4, :]

# 'iloc' is simply following NumPy's slicing convention...
ufo.values[0:4, :]

# ...and NumPy is simply following Python's slicing convention
'python'[0:4]

# 'loc' is inclusive of the stopping label because you don't necessarily know what label will come after it
ufo.loc[0:4, 'City':'State']
# 其实这个设计和matlab的slicing就是一样的了
```
* **Question:** How do I randomly sample rows from a DataFrame?
```python
# sample 3 rows from the DataFrame without replacement (new in pandas 0.16.1)
ufo.sample(n=3)

# use the 'random_state' parameter for reproducibility
ufo.sample(n=3, random_state=42)

# sample 75% of the DataFrame's rows without replacement
train = ufo.sample(frac=0.75, random_state=99)

# store the remaining 25% of the rows in another DataFrame
test = ufo.loc[~ufo.index.isin(train.index), :]
```
* Documentation for [**`sample`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sample.html)
* Documentation for [**`isin`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Index.isin.html)
## How do I create dummy variables in pandas?
```python
# read the training dataset from Kaggle's Titanic competition
train = pd.read_csv('http://bit.ly/kaggletrain')
train.head()

# create the 'Sex_male' dummy variable using the 'map' method
train['Sex_male'] = train.Sex.map({'female':0, 'male':1})
train.head()

# alternative: use 'get_dummies' to create one column for every possible value
pd.get_dummies(train.Sex).head()

# drop the first dummy variable ('female') using the 'iloc' method
pd.get_dummies(train.Sex).iloc[:, 1:].head()

# add a prefix to identify the source of the dummy variables
pd.get_dummies(train.Sex, prefix='Sex').iloc[:, 1:].head()

# use 'get_dummies' with a feature that has 3 possible values
pd.get_dummies(train.Embarked, prefix='Embarked').head(10)

# drop the first dummy variable ('C')
pd.get_dummies(train.Embarked, prefix='Embarked').iloc[:, 1:].head(10)

# save the DataFrame of dummy variables and concatenate them to the original DataFrame
embarked_dummies = pd.get_dummies(train.Embarked, prefix='Embarked').iloc[:, 1:]
train = pd.concat([train, embarked_dummies], axis=1)
train.head()

# reset the DataFrame
train = pd.read_csv('http://bit.ly/kaggletrain')
train.head()

# pass the DataFrame to 'get_dummies' and specify which columns to dummy (it drops the original columns)
pd.get_dummies(train, columns=['Sex', 'Embarked']).head()

# use the 'drop_first' parameter (new in pandas 0.18) to drop the first dummy variable for each feature
pd.get_dummies(train, columns=['Sex', 'Embarked'], drop_first=True).head()
```
* Documentation for [**`map`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.map.html)
* Generally speaking:
	- If you have **"K" possible values** for a categorical feature, you only need **"K-1" dummy variables** to capture all of the information about that feature.
	- One convention is to **drop the first dummy variable**, which defines that level as the "baseline".
* How to translate these values back to the original 'Embarked' value:
	- **0, 0** means **C**
	- **1, 0** means **Q**
	- **0, 1** means **S**
* Documentation for [**`concat`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html)
* Documentation for [**`get_dummies`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.get_dummies.html)
## How do I work with dates and times in pandas?
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.head()

# 'Time' is currently stored as a string
ufo.dtypes
# dtype是object

# hour could be accessed using string slicing, but this approach breaks too easily
ufo.Time.str.slice(-5, -3).astype(int).head()

# convert 'Time' to datetime format
ufo['Time'] = pd.to_datetime(ufo.Time)
# 注意这里一般的字符串转datetime需要指定%Y%M这种表明位置，才能转化，这里它的数据很“well formatted”，所以可以直接转化
ufo.head()

ufo.dtypes
# 现在Time是datetime类型

# convenient Series attributes are now available
ufo.Time.dt.hour.head()

# 返回日期所对应的星期几
ufo.Time.dt.day_name()

# 返回在一年中的第几天1-365/366
ufo.Time.dt.dayofyear.head()

# convert a single string to datetime format (outputs a timestamp object)
ts = pd.to_datetime('1/1/1999')
ts
# 只给了精确到天数，默认自动补全为00:00:00这个时间点

# compare a datetime Series with a timestamp
ufo.loc[ufo.Time >= ts, :].head()

# perform mathematical operations with timestamps (outputs a timedelta object)
ufo.Time.max() - ufo.Time.min()

# timedelta objects also have attributes you can access
(ufo.Time.max() - ufo.Time.min()).days

# allow plots to appear in the notebook
%matplotlib inline

# count the number of UFO reports per year
ufo['Year'] = ufo.Time.dt.year
ufo.Year.value_counts().sort_index().head()

# plot the number of UFO reports per year (line plot is the default)
ufo.Year.value_counts().sort_index().plot()
```
* Documentation for [**`to_datetime`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html)
* API reference for [datetime properties and methods](http://pandas.pydata.org/pandas-docs/stable/api.html#datetimelike-properties)
## How do I find and remove duplicate rows in pandas? 
```python
# read a dataset of movie reviewers into a DataFrame
user_cols = ['user_id', 'age', 'gender', 'occupation', 'zip_code']
users = pd.read_table('http://bit.ly/movieusers', sep='|', header=None, names=user_cols, index_col='user_id')
users.head()

# detect duplicate zip codes: True if an item is identical to a previous item
users.zip_code.duplicated().tail()

# count the duplicate items (True becomes 1, False becomes 0)
users.zip_code.duplicated().sum()

# detect duplicate DataFrame rows: True if an entire row is identical to a previous row
users.duplicated().tail()

# count the duplicate rows
users.duplicated().sum()

# 查看重复行
users.loc[users.duplicated(),:]

# examine the duplicate rows (ignoring the first occurrence)
users.loc[users.duplicated(keep='first'), :]

# examine the duplicate rows (ignoring the last occurrence)
users.loc[users.duplicated(keep='last'), :]

# examine the duplicate rows (including all duplicates)
users.loc[users.duplicated(keep=False), :]
# 只要出现2次及以上都算

# drop the duplicate rows (inplace=False by default)
users.drop_duplicates(keep='first').shape

users.drop_duplicates(keep='last').shape

users.drop_duplicates(keep=False).shape

# only consider a subset of columns when identifying duplicates
users.duplicated(subset=['age', 'zip_code']).sum()

users.drop_duplicates(subset=['age', 'zip_code']).shape
```
* Logic for [**`duplicated`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.duplicated.html):
	- **`keep='first'`** (default): Mark duplicates as True except for the first occurrence.
	- **`keep='last'`**: Mark duplicates as True except for the last occurrence.
	- **`keep=False`**: Mark all duplicates as True.
* Documentation for [**`drop_duplicates`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.drop_duplicates.html)
## How do I avoid a SettingWithCopyWarning in pandas?
* **Goal:** Mark the 'NOT RATED' values as missing values, represented by 'NaN'.
* **Goal:** Fix the 'duration' for 'The Shawshank Redemption'.
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')
movies.head()

# count the missing values in the 'content_rating' Series
movies.content_rating.isnull().sum()

# examine the DataFrame rows that contain those missing values
movies[movies.content_rating.isnull()]

# examine the unique values in the 'content_rating' Series
movies.content_rating.value_counts()

# first, locate the relevant rows
movies[movies.content_rating=='NOT RATED'].head()

# then, select the 'content_rating' Series from those rows
movies[movies.content_rating=='NOT RATED'].content_rating.head()

# finally, replace the 'NOT RATED' values with 'NaN' (imported from NumPy)
import numpy as np
movies[movies.content_rating=='NOT RATED'].content_rating = np.nan

# the 'content_rating' Series has not changed
movies.content_rating.isnull().sum()

# replace the 'NOT RATED' values with 'NaN' (does not cause a SettingWithCopyWarning)
movies.loc[movies.content_rating=='NOT RATED', 'content_rating'] = np.nan

# this time, the 'content_rating' Series has changed
movies.content_rating.isnull().sum()

# create a DataFrame only containing movies with a high 'star_rating'
top_movies = movies.loc[movies.star_rating >= 9, :]
top_movies

# overwrite the relevant cell with the correct duration
top_movies.loc[0, 'duration'] = 150
# 抛出警告，但是对top_movies修改有效,对movies没有修改

# 'top_movies' DataFrame has been updated
top_movies

# 'movies' DataFrame has not been updated
movies.head(1)

# explicitly create a copy of 'movies'
top_movies = movies.loc[movies.star_rating >= 9, :].copy()

# pandas now knows that you are updating a copy instead of a view (does not cause a SettingWithCopyWarning)
top_movies.loc[0, 'duration'] = 150

# 'top_movies' DataFrame has been updated
top_movies


```
* **Problem:** That statement involves two operations, a **`__getitem__`** and a **`__setitem__`**. pandas can't guarantee whether the **`__getitem__`** operation returns a view or a copy of the data.
	- If **`__getitem__`** returns a view of the data, **`__setitem__`** will affect the 'movies' DataFrame.
	- But if **`__getitem__`** returns a copy of the data, **`__setitem__`** will not affect the 'movies' DataFrame.
* **Solution:** Use the **`loc`** method, which replaces the 'NOT RATED' values in a single **`__setitem__`** operation.
* **Summary:** Use the **`loc`** method any time you are selecting rows and columns in the same statement.
* **More information:** [What’s up with the SettingWithCopy warning](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy)
* 第二个问题的**Solution:** Any time you are attempting to create a DataFrame copy, use the [**`copy`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.copy.html) method.
* Documentation on indexing and selection: [Returning a view versus a copy](http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy)
* Stack Overflow: [What is the point of views in pandas if it is undefined whether an indexing operation returns a view or a copy?](http://stackoverflow.com/questions/34884536/what-is-the-point-of-views-in-pandas-if-it-is-undefined-whether-an-indexing-oper)
## How do I change display options in pandas?
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')

# only 60 rows will be displayed when printing
drinks

# check the current setting for the 'max_rows' option
pd.get_option('display.max_rows')

# overwrite the current setting so that all rows will be displayed
pd.set_option('display.max_rows', None)
drinks

# reset the 'max_rows' option to its default
pd.reset_option('display.max_rows')

# 还有其他一些参数
# the 'max_columns' option is similar to 'max_rows'
pd.get_option('display.max_columns')

# read the training dataset from Kaggle's Titanic competition into a DataFrame
train = pd.read_csv('http://bit.ly/kaggletrain')
train.head()

# an ellipsis is displayed in the 'Name' cell of row 1 because of the 'max_colwidth' option
pd.get_option('display.max_colwidth')

# overwrite the current setting so that more characters will be displayed
pd.set_option('display.max_colwidth', 1000)
train.head()

# overwrite the 'precision' setting to display 2 digits after the decimal point of 'Fare'
pd.set_option('display.precision', 2)
train.head()

# add two meaningless columns to the drinks DataFrame
drinks['x'] = drinks.wine_servings * 1000
drinks['y'] = drinks.total_litres_of_pure_alcohol * 1000
drinks.head()

# use a Python format string to specify a comma as the thousands separator
pd.set_option('display.float_format', '{:,}'.format)
drinks.head()

# 'y' was affected (but not 'x') because the 'float_format' option only affects floats (not ints)
drinks.dtypes

# view the option descriptions (including the default and current values)
pd.describe_option()

# search for specific options by name
pd.describe_option('rows')

# reset all of the options to their default values
pd.reset_option('all')
# 这里会抛出警告，可以忽略
```
* Documentation for [**`set_option`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.set_option.html) and [**`reset_option`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.reset_option.html)
* Documentation for [**`describe_option`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.describe_option.html)
## How do I create a pandas DataFrame from another object?
```python
# create a DataFrame from a dictionary (keys become column names, values become data)
pd.DataFrame({'id':[100, 101, 102], 'color':['red', 'blue', 'red']})

# optionally specify the order of columns and define the index
df = pd.DataFrame({'id':[100, 101, 102], 'color':['red', 'blue', 'red']}, columns=['id', 'color'], index=['a', 'b', 'c'])
df

# create a DataFrame from a list of lists (each inner list becomes a row)
pd.DataFrame([[100, 'red'], [101, 'blue'], [102, 'red']], columns=['id', 'color'])

# create a NumPy array (with shape 4 by 2) and fill it with random numbers between 0 and 1
import numpy as np
arr = np.random.rand(4, 2)
arr

# create a DataFrame from the NumPy array
pd.DataFrame(arr, columns=['one', 'two'])

# create a DataFrame of student IDs (100 through 109) and test scores (random integers between 60 and 100)
pd.DataFrame({'student':np.arange(100, 110, 1), 'test':np.random.randint(60, 101, 10)})

# 'set_index' can be chained with the DataFrame constructor to select an index
pd.DataFrame({'student':np.arange(100, 110, 1), 'test':np.random.randint(60, 101, 10)}).set_index('student')

# create a new Series using the Series constructor
s = pd.Series(['round', 'square'], index=['c', 'b'], name='shape')
s

# concatenate the DataFrame and the Series (use axis=1 to concatenate columns)
pd.concat([df, s], axis=1)
# 有公共index，则会自动对齐，没有数据的以nan填充
```
* Documentation for [**`DataFrame`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)
* Documentation for [**`np.arange`**](http://docs.scipy.org/doc/numpy/reference/generated/numpy.arange.html) and [**`np.random`**](http://docs.scipy.org/doc/numpy/reference/routines.random.html)
*  For `concat` **Notes:**
	- The Series name became the column name in the DataFrame.
	- The Series data was aligned to the DataFrame by its index.
	- The 'shape' for row 'a' was marked as a missing value (NaN) because that index was not present in the Series.
* Documentation for [**`concat`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html)
## How do I apply a function to a pandas Series or DataFrame?
* **Goal:** Map the existing values of a Series to a different set of values
	* **Method:** [**`map`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.map.html) (Series method)
* **Goal:** Apply a function to each element in a Series
	* **Method:** [**`apply`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.apply.html) (Series method)
* **Note:** **`map`** can be substituted for **`apply`** in many cases, but **`apply`** is more flexible and thus is recommended
* **Goal:** Apply a function along either axis of a DataFrame
	* **Method:** [**`apply`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html) (DataFrame method)
* **Goal:** Apply a function to every element in a DataFrame
	* **Method:** [**`applymap`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.applymap.html) (DataFrame method)
```python
# read the training dataset from Kaggle's Titanic competition into a DataFrame
train = pd.read_csv('http://bit.ly/kaggletrain')
train.head()

# map 'female' to 0 and 'male' to 1
train['Sex_num'] = train.Sex.map({'female':0, 'male':1})
train.loc[0:4, ['Sex', 'Sex_num']]

# calculate the length of each string in the 'Name' Series
train['Name_length'] = train.Name.apply(len)
train.loc[0:4, ['Name', 'Name_length']]

# round up each element in the 'Fare' Series to the next integer
import numpy as np
train['Fare_ceil'] = train.Fare.apply(np.ceil)
train.loc[0:4, ['Fare', 'Fare_ceil']]

# we want to extract the last name of each person
train.Name.head()

# use a string method to split the 'Name' Series at commas (returns a Series of lists)
train.Name.str.split(',').head()

# define a function that returns an element from a list based on position
def get_element(my_list, position):
    return my_list[position]

# apply the 'get_element' function and pass 'position' as a keyword argument
train.Name.str.split(',').apply(get_element, position=0).head()

# alternatively, use a lambda function
train.Name.str.split(',').apply(lambda x: x[0]).head()

# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# select a subset of the DataFrame to work with
drinks.loc[:, 'beer_servings':'wine_servings'].head()

# apply the 'max' function along axis 0 to calculate the maximum value in each column
drinks.loc[:, 'beer_servings':'wine_servings'].apply(max, axis=0)

# apply the 'max' function along axis 1 to calculate the maximum value in each row
drinks.loc[:, 'beer_servings':'wine_servings'].apply(max, axis=1).head()

# use 'np.argmax' to calculate which column has the maximum value for each row
drinks.loc[:, 'beer_servings':'wine_servings'].apply(np.argmax, axis=1).head()

# convert every DataFrame element into a float
drinks.loc[:, 'beer_servings':'wine_servings'].applymap(float).head()

# overwrite the existing DataFrame columns
drinks.loc[:, 'beer_servings':'wine_servings'] = drinks.loc[:, 'beer_servings':'wine_servings'].applymap(float)
drinks.head()
```