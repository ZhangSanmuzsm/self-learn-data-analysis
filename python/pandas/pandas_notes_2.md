# pandas笔记2——pandas基础2
## How do I use the "axis" parameter in pandas?
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# drop a column (temporarily)
drinks.drop('continent', axis=1).head()

# drop a row (temporarily)
drinks.drop(2, axis=0).head()

# calculate the mean of each numeric column
drinks.mean()

# or equivalently, specify the axis explicitly
drinks.mean(axis=0)

# calculate the mean of each row
drinks.mean(axis=1).head()

# 'index' is an alias for axis 0
drinks.mean(axis='index')

# 'columns' is an alias for axis 1
drinks.mean(axis='columns').head()
```
* When **referring to rows or columns** with the axis parameter:
	- **axis 0** refers to rows
	- **axis 1** refers to columns
* When performing a **mathematical operation** with the axis parameter:
	- **axis 0** means the operation should "move down" the row axis
	- **axis 1** means the operation should "move across" the column axis
* Documentation for [**`mean`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.mean.html)
## How do I use string methods in pandas?
```python
# read a dataset of Chipotle orders into a DataFrame
orders = pd.read_table('http://bit.ly/chiporders')
orders.head()

# normal way to access string methods in Python
'hello'.upper()

# string methods for pandas Series are accessed via 'str'
orders.item_name.str.upper().head()

# string method 'contains' checks for a substring and returns a boolean Series
orders.item_name.str.contains('Chicken').head()

# use the boolean Series to filter the DataFrame
orders[orders.item_name.str.contains('Chicken')].head()

# string methods can be chained together
orders.choice_description.str.replace('[', '').str.replace(']', '').head()

# many pandas string methods support regular expressions (regex)
orders.choice_description.str.replace('[\[\]]', '').head()
```
* [String handling section](http://pandas.pydata.org/pandas-docs/stable/api.html#string-handling) of the pandas API reference
## How do I change the data type of a pandas Series
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# examine the data type of each Series
drinks.dtypes
# object type其实就是代表字符串类型

# change the data type of an existing Series
drinks['beer_servings'] = drinks.beer_servings.astype(float)
drinks.dtypes

# alternatively, change the data type of a Series while reading in a file
drinks = pd.read_csv('http://bit.ly/drinksbycountry', dtype={'beer_servings':float})
drinks.dtypes

# read a dataset of Chipotle orders into a DataFrame
orders = pd.read_table('http://bit.ly/chiporders')
orders.head()

# examine the data type of each Series
orders.dtypes

# convert a string to a number in order to do math
orders.item_price.str.replace('$', '').astype(float).mean()

# string method 'contains' checks for a substring and returns a boolean Series
orders.item_name.str.contains('Chicken').head()

# convert a boolean Series to an integer (False = 0, True = 1)
orders.item_name.str.contains('Chicken').astype(int).head()
```
* Documentation for [**`astype`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.astype.html)
## When should I use a "groupby" in pandas?
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# calculate the mean beer servings across the entire dataset
drinks.beer_servings.mean()

# calculate the mean beer servings just for countries in Africa
drinks[drinks.continent=='Africa'].beer_servings.mean()

# calculate the mean beer servings for each continent
drinks.groupby('continent').beer_servings.mean()
# drinks.groupby('continent')是DataFrameGroupBy object
# drinks.groupby('continent').beer_servings是SeriesGroupBy object
# drinks.groupby('continent').beer_servings.mean()是pandas Series


# other aggregation functions (such as 'max') can also be used with groupby
drinks.groupby('continent').beer_servings.max()

# multiple aggregation functions can be applied simultaneously
drinks.groupby('continent').beer_servings.agg(['count', 'mean', 'min', 'max'])

# specifying a column to which the aggregation function should be applied is not required
drinks.groupby('continent').mean()
# 默认对所有数值型的字段都分组求平均

# allow plots to appear in the notebook
%matplotlib inline
# 需要安装matplotlib包

# side-by-side bar plot of the DataFrame directly above
drinks.groupby('continent').mean().plot(kind='bar')
```
* Documentation for [**`groupby`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html)
* Documentation for [**`agg`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.core.groupby.DataFrameGroupBy.agg.html)
* Documentation for [**`plot`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html)
## How do I explore a pandas Series?
* **Exploring a non-numeric Series:**
* **Exploring a numeric Series:**
```python
# read a dataset of top-rated IMDb movies into a DataFrame
movies = pd.read_csv('http://bit.ly/imdbratings')
movies.head()

# examine the data type of each Series
movies.dtypes

## **Exploring a non-numeric Series:**
# count the non-null values, unique values, and frequency of the most common value
movies.genre.describe()

# count how many times each value in the Series occurs
movies.genre.value_counts()

# display percentages instead of raw counts
movies.genre.value_counts(normalize=True)

# 'value_counts' (like many pandas methods) outputs a Series
type(movies.genre.value_counts())

# thus, you can add another Series method on the end
movies.genre.value_counts().head()

# display the unique values in the Series
movies.genre.unique()

# count the number of unique values in the Series
movies.genre.nunique()

# compute a cross-tabulation of two Series
pd.crosstab(movies.genre, movies.content_rating)
# 计算属于每个(movies.genre,movies.content_rating)的总个数作为表格对应(movies.genre,movies.content_rating)位置的value

## **Exploring a numeric Series:**
# calculate various summary statistics
movies.duration.describe()

# many statistics are implemented as Series methods
movies.duration.mean()

# 'value_counts' is primarily useful for categorical data, not numerical data 也能用，但一般是对categorical data才计数
movies.duration.value_counts().head()

# allow plots to appear in the notebook
%matplotlib inline

# histogram of the 'duration' Series (shows the distribution of a numerical variable)
movies.duration.plot(kind='hist')

# bar plot of the 'value_counts' for the 'genre' Series
movies.genre.value_counts().plot(kind='bar')
```
* Documentation for [**`value_counts`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html)
* Documentation for [**`unique`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.unique.html) and [**`nunique`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.nunique.html)
* Documentation for [**`crosstab`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.crosstab.html)

## How do I handle missing values in pandas?
* **What does "NaN" mean?**
	- "NaN" is not a string, rather it's a special value: **`numpy.nan`**.
	- It stands for "Not a Number" and indicates a **missing value**.
	- **`read_csv`** detects missing values (by default) when reading the file, and replaces them with this special value.
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.tail()#默认显示后5行数据，和head()对应

# 'isnull' returns a DataFrame of booleans (True if missing, False if not missing)
ufo.isnull().tail()

# 'nonnull' returns the opposite of 'isnull' (True if not missing, False if missing)
ufo.notnull().tail()

# count the number of missing values in each Series
ufo.isnull().sum()#等价于ufo.isnull().sum(axis=0),即沿着行求和，也就是每列进行求和，默认只对数值型，bool型进行计算，字符串型不计算
# 对bool值求sum，默认把True转化为1,False转化为0,然后计算

# use the 'isnull' Series method to filter the DataFrame rows
ufo[ufo.City.isnull()].head()

#### **How to handle missing values** depends on the dataset as well as the nature of your analysis. Here are some options:
## 1. 丢掉缺失值
# examine the number of rows and columns
ufo.shape

# if 'any' values are missing in a row, then drop that row
ufo.dropna(how='any').shape

# 'inplace' parameter for 'dropna' is False by default, thus rows were only dropped temporarily
ufo.shape

# if 'all' values are missing in a row, then drop that row (none are dropped in this case)
ufo.dropna(how='all').shape

# if 'any' values are missing in a row (considering only 'City' and 'Shape Reported'), then drop that row
ufo.dropna(subset=['City', 'Shape Reported'], how='any').shape

# if 'all' values are missing in a row (considering only 'City' and 'Shape Reported'), then drop that row
ufo.dropna(subset=['City', 'Shape Reported'], how='all').shape


## 2. 以特定值替代缺失值，即缺失值填补
# 'value_counts' does not include missing values by default
ufo['Shape Reported'].value_counts().head()

# explicitly include missing values
ufo['Shape Reported'].value_counts(dropna=False).head()

# fill in missing values with a specified value
ufo['Shape Reported'].fillna(value='VARIOUS', inplace=True)

# confirm that the missing values were filled in
ufo['Shape Reported'].value_counts().head()
```
* Documentation for [**`isnull`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.isnull.html) and [**`notnull`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.notnull.html)
* Documentation for [**`dropna`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.dropna.html)
* Documentation for [**`fillna`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.fillna.html)
## What do I need to know about the pandas index? (Part 1)
* The **DataFrame index** is **core(核心)** to the functionality of pandas
* **What is the index used for?**
	1. identification
	2. selection
	3. alignment (covered in the next section)
```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# every DataFrame has an index (sometimes called the "row labels")
drinks.index

# column names are also stored in a special "index" object
drinks.columns

# neither the index nor the columns are included in the shape
drinks.shape

# index and columns both default to integers if you don't define them 且都是默认从0开始
pd.read_table('http://bit.ly/movieusers', header=None, sep='|').head()

# identification: index remains with each row when filtering the DataFrame
drinks[drinks.continent=='South America']

# selection: select a portion of the DataFrame using the index
drinks.loc[23, 'beer_servings']

# set an existing column as the index
drinks.set_index('country', inplace=True)
drinks.head()

# 'country' is now the index
drinks.index

# 'country' is no longer a column
drinks.columns

# 'country' data is no longer part of the DataFrame contents
drinks.shape

# country name can now be used for selection
drinks.loc['Brazil', 'beer_servings']

# index name is optional
drinks.index.name = None
drinks.head()

# restore the index name, and move the index back to a column
drinks.index.name = 'country'
drinks.reset_index(inplace=True)
drinks.head()

# many DataFrame methods output a DataFrame
drinks.describe()

# you can interact with any DataFrame using its index and columns
drinks.describe().loc['25%', 'beer_servings']
```
* Documentation for [**`set_index`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.set_index.html)
* Documentation for [**`reset_index`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reset_index.html)
## What do I need to know about the pandas index? (Part 2)

```python
# read a dataset of alcohol consumption into a DataFrame
drinks = pd.read_csv('http://bit.ly/drinksbycountry')
drinks.head()

# every DataFrame has an index
drinks.index

# every Series also has an index (which carries over from the DataFrame)
drinks.continent.head()

# set 'country' as the index
drinks.set_index('country', inplace=True)

# Series index is on the left, values are on the right 此时已经变成了country对应的index
drinks.continent.head()

# another example of a Series (output from the 'value_counts' method)
drinks.continent.value_counts()

# access the Series index
drinks.continent.value_counts().index

# access the Series values
drinks.continent.value_counts().values

# elements in a Series can be selected by index (using bracket notation or dot notation) 不过最好用中括号，原因之前说过，用点号可能出现各种问题
drinks.continent.value_counts()['Africa']
# 这里说下Series和DataFrame的区别，Series[index]=value, DataFrame.loc[index,column]=value，也就是Series类似向量，DataFrame类似矩阵，或者说Series是1维张量，DataFrame是2维张量

# any Series can be sorted by its values
drinks.continent.value_counts().sort_values()
# 默认升序，对values排序，SQL也是如此，python list也是如此，很多语言都是默认升序

# any Series can also be sorted by its index
drinks.continent.value_counts().sort_index()
# 默认升序，对index排序

# 'beer_servings' Series contains the average annual beer servings per person
drinks.beer_servings.head()

# create a Series containing the population of two countries
people = pd.Series([3000000, 85000], index=['Albania', 'Andorra'], name='population')
people

# calculate the total annual beer servings for each country
(drinks.beer_servings * people).head()
#- The two Series were **aligned** by their indexes.
#- If a value is missing in either Series, the result is marked as **NaN**.
#- Alignment enables us to easily work with **incomplete data**.

# concatenate the 'drinks' DataFrame with the 'population' Series (aligns by the index)
pd.concat([drinks, people], axis=1).head()


```
* Documentation for [**`sort_values`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sort_values.html) and [**`sort_index`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.sort_index.html)
* Documentation for [**`Series`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html)
* Documentation for [**`concat`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html)
## How do I select multiple rows and columns from a pandas DataFrame?
* The [**`loc`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.loc.html) method is used to select rows and columns by **label**. You can pass it:
	- A single label
	- A list of labels
	- A slice of labels
	- A boolean Series
	- A colon (which indicates "all labels")
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.head(3)

# row 0, all columns
ufo.loc[0, :]
# 返回一个Series，index是列名，name是所在行的index，values当然就是相应的values

# rows 0 and 1 and 2, all columns
ufo.loc[[0, 1, 2], :]
# 返回dataframe，相当于原表的子表，矩阵的子矩阵

# rows 0 through 2 (inclusive), all columns
ufo.loc[0:2, :]#前后切片值都包括，类似matlab
# 返回dataframe，相当于原表的子表，矩阵的子矩阵

# this implies "all columns", but explicitly stating "all columns" is better
ufo.loc[0:2]

# rows 0 through 2 (inclusive), column 'City'
ufo.loc[0:2, 'City']

# rows 0 through 2 (inclusive), columns 'City' and 'State'
ufo.loc[0:2, ['City', 'State']]

# accomplish the same thing using double brackets - but using 'loc' is preferred since it's more explicit
ufo[['City', 'State']].head(3)

# rows 0 through 2 (inclusive), columns 'City' through 'State' (inclusive)
ufo.loc[0:2, 'City':'State']

# accomplish the same thing using 'head' and 'drop'
ufo.head(3).drop('Time', axis=1)

# rows in which the 'City' is 'Oakland', column 'State'
ufo.loc[ufo.City=='Oakland', 'State']

# accomplish the same thing using "chained indexing" - but using 'loc' is preferred since chained indexing can cause problems
ufo[ufo.City=='Oakland'].State
```
* The [**`iloc`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.iloc.html) method is used to select rows and columns by **integer position**. You can pass it:
	- A single integer position
	- A list of integer positions
	- A slice of integer positions
	- A colon (which indicates "all integer positions")
```python
# rows in positions 0 and 1, columns in positions 0 and 3
ufo.iloc[[0, 1], [0, 3]]

# rows in positions 0 through 2 (exclusive), columns in positions 0 through 4 (exclusive)
ufo.iloc[0:2, 0:4]

# rows in positions 0 through 2 (exclusive), all columns
ufo.iloc[0:2, :]

# accomplish the same thing - but using 'iloc' is preferred since it's more explicit
ufo[0:2]


```
* The [**`ix`**](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.ix.html) method is used to select rows and columns by **label or integer position**, and should only be used when you need to mix label-based and integer-based selection in the same call.
* **Rules for using numbers with `ix`:**
	- If the index is **strings**, numbers are treated as **integer positions**, and thus slices are **exclusive** on the right.
	- If the index is **integers**, numbers are treated as **labels**, and thus slices are **inclusive**.
```python
# read a dataset of alcohol consumption into a DataFrame and set 'country' as the index
drinks = pd.read_csv('http://bit.ly/drinksbycountry', index_col='country')
drinks.head()

# row with label 'Albania', column in position 0
drinks.ix['Albania', 0]

# row in position 1, column with label 'beer_servings'
drinks.ix[1, 'beer_servings']

# rows 'Albania' through 'Andorra' (inclusive), columns in positions 0 through 2 (exclusive)
drinks.ix['Albania':'Andorra', 0:2]

# rows 0 through 2 (inclusive), columns in positions 0 through 2 (exclusive)
ufo.ix[0:2, 0:2]
```
* 注记：这是以前pandas才有ix这个方法，现在ix已经被删除了
## When should I use the "inplace" parameter in pandas?
```python
# read a dataset of UFO reports into a DataFrame
ufo = pd.read_csv('http://bit.ly/uforeports')
ufo.head()

ufo.shape

# remove the 'City' column (doesn't affect the DataFrame since inplace=False)
ufo.drop('City', axis=1).head()

# confirm that the 'City' column was not actually removed
ufo.head()

# remove the 'City' column (does affect the DataFrame since inplace=True)
ufo.drop('City', axis=1, inplace=True)

# confirm that the 'City' column was actually removed
ufo.head()

# drop a row if any value is missing from that row (doesn't affect the DataFrame since inplace=False)
ufo.dropna(how='any').shape

# confirm that no rows were actually removed
ufo.shape

# use an assignment statement instead of the 'inplace' parameter
ufo = ufo.set_index('Time')#这样做等价于inplace=True
ufo.tail()

# fill missing values using "backward fill" strategy (doesn't affect the DataFrame since inplace=False)
ufo.fillna(method='bfill').tail()

# compare with "forward fill" strategy (doesn't affect the DataFrame since inplace=False)
ufo.fillna(method='ffill').tail()
```