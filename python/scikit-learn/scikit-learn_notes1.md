# scikit-learn笔记1——Introduction to machine learning
## What is machine learning, and how does it work?
* One definition: "Machine learning is the semi-automated extraction of knowledge from data"
	- **Knowledge from data**: Starts with a question that might be answerable using data
	- **Automated extraction**: A computer provides the insight
	- **Semi-automated**: Requires many smart decisions by a human
## What are the two main categories of machine learning?
* **Supervised learning**: Making predictions using data
	- Example: Is a given email "spam" or "ham"?
	- There is an outcome we are trying to predict
* **Unsupervised learning**: Extracting structure from data
	- Example: Segment grocery store shoppers into clusters that exhibit similar behaviors
	- There is no "right answer"
## How does machine learning "work"?

High-level steps of supervised learning:

1. First, train a **machine learning model** using **labeled data**
	- "Labeled data" has been labeled with the outcome
	- "Machine learning model" learns the relationship between the attributes of the data and its outcome

2. Then, make **predictions** on **new data** for which the label is unknown

The primary goal of supervised learning is to build a model that "generalizes": It accurately predicts the **future** rather than the **past**!

## Questions about machine learning

- How do I choose **which attributes** of my data to include in the model?
- How do I choose **which model** to use?
- How do I **optimize** this model for best performance?
- How do I ensure that I'm building a model that will **generalize** to unseen data?
- Can I **estimate** how well my model is likely to perform on unseen data?

## Resources
- Book: [An Introduction to Statistical Learning](http://www-bcf.usc.edu/~gareth/ISL/) (section 2.1, 14 pages)
- Video: [Learning Paradigms](http://work.caltech.edu/library/014.html) (13 minutes)

