# Netflix Movies Data Analysis Project #

 ## Project Overview
 
 Netflix is widely recognized for its strong use of Data Science, Artificial Intelligence, and Machine Learning, especially in building powerful recommendation systems that understand user behavior and content trends.

In this project, we analyze a dataset containing 9,000+ Netflix movies to extract meaningful insights that can help in data-driven business decision-making.
The analysis focuses on genres, popularity, voting patterns, and release trends over the years.

### Objectives

The main goals of this project are to answer the following business questions:

1.What is the most frequent genre of movies released on Netflix?

2.Which movie has the highest vote average?

3.Which movie has the highest popularity and what is its genre?

4.Which movie has the lowest popularity and what is its genre?

5.Which year has the highest number of movies released on Netflix?

## Dataset Information

### Source: Netflix Movies Dataset

## #Number of Records: 9,000+ movies

### Key Columns Used:

1.Genre

2.Popularity

3.Vote_Count

4.Vote_Average

5.Release_Date

6.Release_Year

## Tools & Technologies Used

### 1.Python

   Pandas – data cleaning & manipulation

   NumPy – numerical operations

   Matplotlib & Seaborn – data visualization

### 2.Jupyter Notebook

## Data Cleaning & Preprocessing

1.Converted vote-related columns to numeric format

2.Handled missing and invalid values using coercion

3.Extracted Release Year from Release_Date

4.Split multi-genre values into individual rows for accurate genre analysis

5.Removed unnecessary columns to optimize the dataset

## importing liberies
```python
import pandas as pd 
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## loading the data
```python
df = pd.read_csv(r"C:\work_space\mymoviedb.csv")
df.head()
```

## data understanding
```python
df.info()
df.dtypes
df.isnull().sum()
df.duplicated().sum()
```

## data cleaning

### 1.changing the data type
```python
df["Vote_Count"] = pd.to_numeric(df["Vote_Count"] ,errors="coerce")
df["Vote_Average"] = pd.to_numeric(df["Vote_Average"],errors="coerce")

f["Vote_Count"] = df["Vote_Count"].astype("Int64")
df["Vote_Average"] = df["Vote_Average"].astype("float")
df.dtypes

df["Release_YEARS"] = pd.to_numeric(df["Release_YEARS"], errors="coerce")
print(df["Release_YEARS"].dtype)

df.head()
```

### 2.Check if there are ANY null values
```python
df.isnull().sum()
df.dropna(inplace=True)
```

### 3.checking the duplicated values
```python
df.duplicated().sum()
```
### 4.droping the unwanted values which are irrelevant for the analysis
```python
df.drop(columns=["Overview","Original_Language","Poster_Url","Release_Date"],inplace=True)

df.tail()

```
### 5.Making each Genre for row for proper analysis
```python
df["Genre"] = df["Genre"].str.split(", ")
df = df.explode("Genre").reset_index(drop=True)

df.tail()

df["Genre"].value_counts()
```

### 6.How can numerical features be categorized into meaningful groups using quartiles for Vote_Average?
```python
def categorized_col(df,col,lables):
    df[col] = pd.to_numeric(df[col],errors="coerce")
    df=df.dropna(subset=[col])

    edges = [df[col].describe()["min"],
             df[col].describe()["25%"],
             df[col].describe()["50%"],
             df[col].describe()["75%"],
             df[col].describe()["max"]
            ]
    df[col] = pd.cut(df[col],edges,labels = labels,
                     duplicates="drop")

    return df

labels=["not_popular","below_average","average","popular"]
df=categorized_col(df,"Vote_Average",labels)

df.head()

df["Vote_Average"].value_counts()

```

## Data Visualization

### 1.What is the most frequent genre of movies released on Netflix?
```python
sns.set_style("whitegrid")
sns.catplot(y="Genre",data=df,kind="count",
            order=df["Genre"].value_counts().index,
            color="#4287f5")
plt.title("Genre Column Distribution")
plt.show()

```

### 2.Which has highest votes in vote avg column?
```python
sns.catplot(y="Vote_Average",data=df,kind="count",
            order=df["Vote_Average"].value_counts().index,
            color="#4287f5")
plt.title("Highest_Votes")
plt.show()

```
### 3.What movie got the highest popularity? what's its genre?
```python
df[df["Popularity"] == df["Popularity"].max()]
```

### 4.What movie got the lowest popularity? what's its genre?
```python
df[df["Popularity"] == df["Popularity"].min()]
```

### 5.Which year has the most filmmed movies?
```python
df["Release_YEARS"].value_counts().sort_index().plot(kind="bar", figsize=(46,18))

plt.xlabel("Release Year")
plt.ylabel("Number of Movies")
plt.title("Movies Released Per Year")
plt.show()

```


## Key Insights

1.Identified the most common genre on Netflix

2.Found movies with highest and lowest popularity

3.Analyzed vote average trends

4.Determined the year with the highest number of movie releases

5.Visualized genre distribution and release trends
