# scikit-learn笔记9——Encoding categorical features
## Agenda

- Why should you use a Pipeline?
- How do you encode categorical features with OneHotEncoder?
- How do you apply OneHotEncoder to selected columns with ColumnTransformer?
- How do you build and cross-validate a Pipeline?
- How do you make predictions on new data using a Pipeline?
- Why should you use scikit-learn (rather than pandas) for preprocessing?

## Step 1: Load the dataset
```python
import pandas as pd

df = pd.read_csv('https://bit.ly/kaggletrain')

df.shape
```
## Step 2: Select features
```python
df.columns

df.isna().sum()

df = df.loc[df.Embarked.notna(), ['Survived', 'Pclass', 'Sex', 'Embarked']]

df.shape

df.isna().sum()

df.head()
```
## Step 3: Cross-validate a model with one feature
```python
X = df.loc[:, ['Pclass']]
y = df.Survived

print(X.shape)
print(y.shape)

from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression(solver='lbfgs')
#默认也是lbfgs这个优化求解算法

from sklearn.model_selection import cross_val_score
cross_val_score(logreg, X, y, cv=5, scoring='accuracy').mean()

y.value_counts(normalize=True)
```
## Step 4: Encode categorical features
```python
df.head()

# dummy encoding of categorical features
from sklearn.preprocessing import OneHotEncoder
ohe = OneHotEncoder(sparse=False)
#sparse=True时会以稀疏矩阵的方式储存，sparse=False则以常规方式储存

ohe.fit_transform(df[['Sex']])

ohe.categories_

ohe.fit_transform(df[['Embarked']])

ohe.categories_

ohe.fit_transform(df[['Sex', 'Embarked']])

ohe.categories_
```
## Step 5: Cross-validate a Pipeline with all features
```python
X = df.drop('Survived', axis='columns')

X.head()

# use when different features need different preprocessing
from sklearn.compose import make_column_transformer

column_trans = make_column_transformer(
    (OneHotEncoder(), ['Sex', 'Embarked']),
    remainder='passthrough')

column_trans.fit_transform(X)

# chain sequential steps together
from sklearn.pipeline import make_pipeline

pipe = make_pipeline(column_trans, logreg)

# cross-validate the entire process
# thus, preprocessing occurs within each fold of cross-validation
cross_val_score(pipe, X, y, cv=5, scoring='accuracy').mean()
```
## Step 6: Make predictions on "new" data
```python
X_new = X.sample(5, random_state=99)
X_new

pipe.fit(X, y)

pipe.predict(X_new)
```
## Recap
```python
import pandas as pd
from sklearn.compose import make_column_transformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.model_selection import cross_val_score

df = pd.read_csv('http://bit.ly/kaggletrain')
df = df.loc[df.Embarked.notna(), ['Survived', 'Pclass', 'Sex', 'Embarked']]
X = df.drop('Survived', axis='columns')
y = df.Survived

column_trans = make_column_transformer(
    (OneHotEncoder(), ['Sex', 'Embarked']),
    remainder='passthrough')
logreg = LogisticRegression(solver='lbfgs')

pipe = make_pipeline(column_trans, logreg)

cross_val_score(pipe, X, y, cv=5, scoring='accuracy').mean()

X_new = X.sample(5, random_state=99)

pipe.fit(X, y)

pipe.predict(X_new)
```