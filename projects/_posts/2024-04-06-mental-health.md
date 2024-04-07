---
layout:     post
title:      Mental Health Study
categories: [projects]
image: 
  path: /assets/img/mental-health/mental-health.png
description: >
  Here you should be able to find everything you need to know to accomplish the most common tasks when blogging with Hydejack.
hide_description: true
sitemap: false
permalink: /projects/2023-04-06-mental-health/
---
# Mental health study and behaviours

## Introduction

Welcome to my first project published on my portfolio website! Having been learning data analysis, I decided to use my skills to learn about a topic that is very important to both myself but the Unitied Kingdom iteslf especially in the wake of the Covid 19 pandamic. The mental health of people around the world.

As we all know from the months of isolation and the massive changes the world has overcome, this can have a negative impact on a person's mental health. Although the data I have used in my findings were pre Covid, I still wanted to studay how mental health affects the daily lives of people suffering with mental health issues.

## Data Preparation

### Data Source
**Mental health dataset** The dataset about mental health was collected from [kaggle.com](https://www.kaggle.com/datasets/bhavikjikadara/mental-health-dataset "Your home for data science") and was downloaded on 2024-04-04.
### Data
It is good practice to have a look at the data before any manipulation and cleaning to get an idea of what the structure of the data looks like. As we can see, there are some missing values in the data which could potentially skew the results we are looking for.

~~~python
import pandas as pd

# Specify the path of the CSV file to read
filepath = "data/Mental Health Dataset.csv"

# Read the file into a dataframe mh_df
df = pd.read_csv(filepath, parse_dates=['Timestamp'])

df.head()
~~~

As you can see, we have some NAN values in the self_empoyed column.

![Pandas dataframe showing first 5 rows](/assets/img/mental-health/pandas-head.png "Pandas dataframe showing first 5 rows")
Pandas dataframe showing first 5 rows.
{:.figure}

~~~python
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
~~~
<img align="center" width="250" height="300" src="/data-analysis-portfolio/assets/img/mental-health/missing-values.png">
5202 missing value counts.
{:.figure}

~~~python
# Drop rows with missing self_employed values
df = df.dropna(subset=['self_employed'])
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
~~~
<img align="center" width="250" height="300" src="/data-analysis-portfolio/assets/img/mental-health/zero-missing-values.png">
0 missing value counts.
{:.figure}
