---
layout:     post
title:      Mental Health Analysis - Introduction and Overview
categories: [projects]
image: 
  path: /assets/img/mental-health/mental-health.png
description: >
  This is an analysis of mental health data I used to complete my portfolio
hide_description: true
sitemap: false
permalink: /projects/2024-04-06-mental-health/
---
# Mental Health Analysis - Introduction and Overview

* this unordered seed list will be replaced by the toc
{:toc}

## Introduction

Welcome to my first project published on my portfolio website! Having been learning data analysis, I decided to use my skills to learn about a topic that is very important to both myself but the Unitied Kingdom iteslf especially in the wake of the Covid 19 pandamic. The mental health of people around the world.

As we all know from the months of isolation and the massive changes the world has overcome, this can have a negative impact on a person's mental health. Although the data I have used in my findings were pre Covid, I still wanted to studay how mental health affects the daily lives of people suffering with mental health issues.

## Data Preparation

### Data Source
**Mental health dataset** The dataset about mental health was collected from [kaggle.com](https://www.kaggle.com/datasets/bhavikjikadara/mental-health-dataset "Your home for data science") and was downloaded on 2024-04-04.
### Data
It is good practice to have a look at the data before any manipulation and cleaning to get an idea of what the structure of the data looks like. As we can see, there are some missing values in the data which could potentially skew the results we are looking for.

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
import pandas as pd
import numpy as np
from scipy import stats
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib.axes._axes import _log as matplotlib_axes_logger
matplotlib_axes_logger.setLevel('ERROR')
import plotly.express as px

# Specify the path of the CSV file to read
filepath = "data/Mental Health Dataset.csv"

# Read the file into a dataframe mh_df
df = pd.read_csv(filepath, parse_dates=['Timestamp'])

df.head()
```
</div>
</details>
<br/>

As you can see, we have some NaN values in the self_empoyed column.

![Pandas dataframe showing first 5 rows](/assets/img/mental-health/pandas-head.png "Pandas dataframe showing first 5 rows")

I first find out how many missing values are in each column.

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
```
</div>
</details>

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/missing-values.png" alt="Pandas showing columns with missing values" 
title="Pandas showing columns with missing values" 
description="This shows that there are 5202 rows with missing values in the self_employed column." %}

I drop any rows which have empty values and do the check again. This clearly shows the rows have been removed.

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Drop rows with missing self_employed values
df = df.dropna(subset=['self_employed'])
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
```
</div>
</details>
<br/>

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/zero-missing-values.png" alt="Pandas showing columns with zero missing values" 
title="Pandas showing columns with zero missing values" 
description="This shows that there are 0 rows with missing values in the self_employed column." %}

The data is now clean but there is inconsistency with the column headings naming conventions. There are title case headings and lower case headings. To keep things consistent, I rename the column headings to all lower case.

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
df = df.rename(columns={'Timestamp': 'timestamp', 
                        'Gender': 'gender', 
                        'Country': 'country', 
                        'Occupation': 'occupation', 
                        'Days_Indoors': 'days_indoors', 
                        'Growing_Stress': 'growing_stress', 
                        'Changes_Habits': 'changes_habits', 
                        'Mental_Health_History': 'mh_history', 
                        'Mood_Swings': 'mood_swings', 
                        'Coping_Struggles': 'coping_struggles', 
                        'Work_Interest': 'work_interest', 
                        'Social_Weakness': 'social_weakness' })
```
</div>
</details>
<br/>

## Data Visualisations

### Distribution of Gender
I first wanted to find out the distribution of gender taking part in the survery. 

From the following chart, I was able to see that the distribution was heavily led to males and it shows males are 4 times more likely to suffer with mental health than females. Could this be because males are more likely to suffer with mental health than feamles OR are females more reluctant to complete the survey?

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/gender-pie-chart.png){:.centered loading="lazy"}

Pie chart showing gender distribution.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Defining colors for the pie chart 
colors = ['pink', 'steelblue'] 
labels = ['Female', 'Male']
  
# Define the ratio of gap of each fragment in a tuple 
explode = (0.05, 0.05) 

# Plotting the pie chart for dataframe 
genders = df.groupby(['gender'])['gender'].count()

# set the fig size for the titles
fig, ax = plt.subplots(1, 1 ,figsize=(4, 4))
ax.pie(genders, colors=colors, explode=explode, labels=labels, autopct='%1.0f%%')

plt.axis('off')
fig.tight_layout()
plt.show()
plt.close()
```
</div>
</details>
<br/>

### Distribution of Country
I also wanted to find out the distribution of gender from each country taking part in the survery. 

From the following chart, I was able to see that the majority of individuals completing the survery was from the United States with 168,056 and United Kingdom with 50,624.

Most of the other countries were around a few hundred or few thousand with the exception of Canada.

From this information, it made me curious as to why the United States and United Kingdom have the vastly the highest amounts in the world. What I found interesting is the amount in the United Kingdom. For such a small country compared to the majority of other countries, there was a high percentage. 

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/gender-by-country.png){:.left loading="lazy"}

Chart showing gender distribution by country.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# get counts by gender and country
# replace any possible NaNs with zero
gender_by_country = df.groupby(['country', 'gender'])['country'].count().unstack().fillna(0)

# chart settings
colors = ['pink', 'steelblue'] 
labels = ['Female', 'Male']

# set the fig size for the titles
fig, ax = plt.subplots(1, 1 ,figsize=(10, 10))
bottom = np.zeros(len(gender_by_country))

# create the bars for each country
for i, col in enumerate(gender_by_country.columns):
  p = ax.barh(gender_by_country.index, gender_by_country[col], left=bottom, label=col,
         color=colors[i])
  bottom += np.array(gender_by_country[col])

# Sum up the rows of our data to get the total value of each bar.
totals = gender_by_country.sum(axis=1)
# Set an offset that is used to bump the label up a bit above the bar.
y_offset = 4

# Add labels to each bar.
for i, total in enumerate(totals):
  ax.bar_label(p, label_type='edge', fontweight='ultralight')

# set the location of the legend
ax.legend(title='Gender', labels=labels, loc='center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.show()
plt.close()
````
</div>
</details>
<br/>