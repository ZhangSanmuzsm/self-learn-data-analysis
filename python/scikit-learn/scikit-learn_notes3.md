# scikit-learn笔记3——Getting started in scikit-learn with the famous iris dataset
## Agenda
- What is the famous iris dataset, and how does it relate to machine learning?
- How do we load the iris dataset into scikit-learn?
- How do we describe a dataset using machine learning terminology?
- What are scikit-learn's four key requirements for working with data?

## Introducing the iris dataset
- 50 samples of 3 different species of iris (150 samples total)
- Measurements: sepal length, sepal width, petal length, petal width

## Machine learning on the iris dataset
- Framed as a **supervised learning** problem: Predict the species of an iris using the measurements
- Famous dataset for machine learning because prediction is **easy**
- Learn more about the iris dataset: [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Iris)

## Loading the iris dataset into scikit-learn
```python
# import load_iris function from datasets module
from sklearn.datasets import load_iris

# save "bunch" object containing iris dataset and its attributes
iris = load_iris()
type(iris)

# print the iris data
print(iris.data)

# print the names of the four features
print(iris.feature_names)

# print integers representing the species of each observation
print(iris.target)

# print the encoding scheme for species: 0 = setosa, 1 = versicolor, 2 = virginica
print(iris.target_names)

# check the types of the features and response
print(type(iris.data))
print(type(iris.target))

# check the shape of the features (first dimension = number of observations, second dimensions = number of features)
print(iris.data.shape)

# check the shape of the response (single dimension matching the number of observations)
print(iris.target.shape)

# store feature matrix in "X"
X = iris.data

# store response vector in "y"
y = iris.target
```


## Machine learning terminology
- Each row is an **observation** (also known as: sample, example, instance, record)
- Each column is a **feature** (also known as: predictor, attribute, independent variable, input, regressor, covariate)
- Each value we are predicting is the **response** (also known as: target, outcome, label, dependent variable)
- **Classification** is supervised learning in which the response is categorical
- **Regression** is supervised learning in which the response is ordered and continuous

## Requirements for working with data in scikit-learn
1. Features and response are **separate objects**
2. Features and response should be **numeric**
3. Features and response should be **NumPy arrays**(这一条不一定需要，只要数据大小能够对得上，之前pandas里面就没有转numpy也可以直接用scikit-learn)
4. Features and response should have **specific shapes**

## Resources
- scikit-learn documentation: [Dataset loading utilities](http://scikit-learn.org/stable/datasets/)
- Jake VanderPlas: Fast Numerical Computing with NumPy ([slides](https://speakerdeck.com/jakevdp/losing-your-loops-fast-numerical-computing-with-numpy-pycon-2015), [video](https://www.youtube.com/watch?v=EEUXKG97YRw))
- Scott Shell: [An Introduction to NumPy](http://www.engr.ucsb.edu/~shell/che210d/numpy.pdf) (PDF)

## Training a machine learning model with scikit-learn

## Agenda
- What is the **K-nearest neighbors** classification model?
- What are the four steps for **model training and prediction** in scikit-learn?
- How can I apply this pattern to **other machine learning models**?

## K-nearest neighbors (KNN) classification
1. Pick a value for K.
2. Search for the K observations in the training data that are "nearest" to the measurements of the unknown iris.
3. Use the most popular response value from the K nearest neighbors as the predicted response value for the unknown iris.

## scikit-learn 4-step modeling pattern
**Step 1:** Import the class you plan to use
```python
from sklearn.neighbors import KNeighborsClassifier
```
**Step 2:** "Instantiate" the "estimator"
- "Estimator" is scikit-learn's term for model
- "Instantiate" means "make an instance of"
```python
knn = KNeighborsClassifier(n_neighbors=1)
print(knn)
```
- Name of the object does not matter
- Can specify tuning parameters (aka "hyperparameters") during this step
- All parameters not specified are set to their defaults

**Step 3:** Fit the model with data (aka "model training")
- Model is learning the relationship between X and y
- Occurs in-place
```python
knn.fit(X, y)
```
**Step 4:** Predict the response for a new observation

- New observations are called "out-of-sample" data
- Uses the information it learned during the model training process
```python
knn.predict([[3, 5, 4, 2]])
```
- Returns a NumPy array
- Can predict for multiple observations at once
```python
X_new = [[3, 5, 4, 2], [5, 4, 3, 2]]
knn.predict(X_new)
```
## Using a different value for K
```python
# instantiate the model (using the value K=5)
knn = KNeighborsClassifier(n_neighbors=5)

# fit the model with data
knn.fit(X, y)

# predict the response for new observations
knn.predict(X_new)
```
## Using a different classification model
```python
# import the class
from sklearn.linear_model import LogisticRegression

# instantiate the model (using the default parameters)
logreg = LogisticRegression()

# fit the model with data
logreg.fit(X, y)

# predict the response for new observations
logreg.predict(X_new)
```
## Resources
- [Nearest Neighbors](http://scikit-learn.org/stable/modules/neighbors.html) (user guide), [KNeighborsClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html) (class documentation)
- [Logistic Regression](http://scikit-learn.org/stable/modules/linear_model.html#logistic-regression) (user guide), [LogisticRegression](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) (class documentation)
- [Videos from An Introduction to Statistical Learning](http://www.dataschool.io/15-hours-of-expert-machine-learning-videos/)
    - Classification Problems and K-Nearest Neighbors (Chapter 2)
    - Introduction to Classification (Chapter 4)
    - Logistic Regression and Maximum Likelihood (Chapter 4)
