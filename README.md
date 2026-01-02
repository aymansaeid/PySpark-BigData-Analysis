# Big Data Processing with Apache Spark
## MapReduce, Machine Learning, and SQL Applications

---

## 1. Introduction

The main goal of this project is not only to implement different algorithms, but also to understand how Apache Spark works as an integrated system when processing data in different formats.

During the development of this project, an important difference between Apache Spark and traditional Python-based tools was observed. Libraries such as Pandas and Scikit-learn perform well on small datasets but do not scale efficiently. These tools attempt to load the entire dataset into memory, which can quickly increase RAM usage and may lead to system instability or crashes.

Apache Spark handles this problem differently. When memory becomes insufficient, Spark automatically spills part of the data to disk instead of failing. This mechanism allows applications to complete successfully while using memory more efficiently. Observing this behavior highlights why Spark is widely used in Big Data environments and why this project focuses on Spark-based processing rather than standalone algorithm implementation.

---

## 2. Datasets Used in the Project

To evaluate different components of Apache Spark, three separate datasets were used:

### • MapReduce Application
A compiled text file related to Artificial Intelligence was used as an unstructured dataset. This dataset is suitable for testing text preprocessing, splitting, and RDD-based transformations.

### • Machine Learning Application
The **Mobile Price Classification** dataset was used. It contains numerical hardware features such as RAM, battery power, and screen resolution, along with a target price category.

Dataset source:  
https://www.kaggle.com/datasets/iabhishekofficial/mobile-price-classification

### • SQL Application
An **HR Analytics** database containing multiple related tables (employees, departments, salaries, managers, and titles) was selected. This dataset enables complex SQL queries involving multi-table joins.

Dataset source:  
https://www.kaggle.com/datasets/priyankbarbhaya/sql-analytics-case-study-employees-database/data

---

## 3. MapReduce Application

### 3.1 Understanding the MapReduce Concept

MapReduce is a two-phase programming model designed for processing large datasets in parallel:

- **Map Phase:** The input data is split into independent elements. In the word count example, each word is mapped to a key-value pair such as `(word, 1)`.
- **Shuffle and Sort Phase:** Spark automatically groups identical keys together across the dataset.
- **Reduce Phase:** Aggregation is performed by summing the values for each key to compute final word counts.

### 3.2 Implementation Details

Spark RDDs (Resilient Distributed Datasets) were used instead of DataFrames to allow low-level control over text processing.

- `flatMap()` was used to split text lines into individual words.
- Text cleaning operations such as `lower()` and punctuation removal were applied to ensure consistent word counting.
- `reduceByKey()` was used to aggregate word frequencies.

The output confirmed correct functionality, with meaningful and clean word counts corresponding to the text content.

---

## 4. Machine Learning Application

The objective of this section was to predict the price range of mobile phones (0–3) based on hardware specifications. Two classification models were implemented:

- Logistic Regression
- Random Forest

### 4.1 Data Preparation

Spark ML requires features to be stored in a single vector column. Therefore, a `VectorAssembler` was used to combine relevant feature columns into a single `features` vector. The dataset was split into 80% training and 20% testing data.

### 4.2 Logistic Regression

Logistic Regression was selected as a baseline model due to its efficiency and suitability for linearly separable data. Features such as RAM and battery power were expected to have a direct linear relationship with the price range.

### 4.3 Random Forest

A Random Forest classifier was also implemented to capture more complex and non-linear feature interactions. Default hyperparameters were used to evaluate out-of-the-box performance.

### 4.4 Results and Comparison

Model evaluation was performed using `MulticlassClassificationEvaluator` with accuracy as the metric:

- **Logistic Regression Accuracy:** 96%  
- **Random Forest Accuracy:** 84%

Despite expectations, Logistic Regression outperformed Random Forest. This indicates that the dataset is largely linearly separable, with RAM being the dominant factor influencing price. Random Forest overcomplicated the problem, while Logistic Regression effectively captured the simple underlying pattern.

Feature importance analysis from the Random Forest model confirmed that RAM was overwhelmingly the most influential feature.

---

## 5. Spark SQL Application

Spark was used as a distributed SQL engine by loading CSV files and registering them as temporary views. This enabled execution of complex SQL queries.

### Query 1: Department Salary Analysis

This query joined departments, employee, and salary tables to calculate the average salary and employee count per department. Results showed that the Sales department had the highest average salary, while Development had the largest workforce.

### Query 2: High Earner Profile

The top 20 highest-paid employees were identified using multi-table joins across employees, departments, titles, and salaries. This query demonstrated Spark SQL’s efficiency in handling complex joins.

### Query 3: Average Salary by Job Title

Average salaries were computed for each job title by joining titles and salaries tables, then grouping and sorting the results to identify the highest-paid roles.

---

## 6. Conclusion

This project demonstrated Apache Spark’s ability to handle multiple Big Data processing tasks within a single unified framework. Text processing using RDDs, machine learning model training, and complex SQL queries were all executed efficiently.

One key takeaway is that more complex models do not always yield better results. Logistic Regression outperformed Random Forest due to strong linear patterns in the dataset. Additionally, observing Spark’s memory management behavior emphasized why it is a preferred solution for Big Data processing compared to traditional local Python environments.



