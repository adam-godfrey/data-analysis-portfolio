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

As you can see, we have some NAN values in the self_empoyed column.

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

fig.text(0,1.03,'Distribution of Gender', fontfamily='serif', fontsize=18, fontweight='bold')
fig.text(0,0.92,'We see vastly more males than females completing survery.', fontfamily='serif', fontsize=12) 

plt.axis('off')
fig.tight_layout()
plt.show()
plt.close()
```
</div>
</details>
<br/>

## Individuals Seeking Treatment by Gender
From the first chart below, we can see that individuals in both genders are about 5x more likely seek treatment.

In the second chart, we can see that males are 4x more likely to not seek treatment compared to females and there is a near even split between both genders seeking treatment;.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals seeking treatment by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# get counts by gender and if they are seeking treatment
treatment_by_gender = df.groupby(['gender', 'treatment'])['treatment'].count().unstack()
# get a propotion based on the above results
treatment_by_gender_proportion = treatment_by_gender.div(treatment_by_gender.sum(1), axis=0)

# chart settings
groups = ['No', 'Yes']
colors = ['pink', 'steelblue'] 
labels = ['Female', 'Male']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2 ,figsize=(10, 4))

# create a stacked bar on the left for counts
ax1.bar(groups, treatment_by_gender.No.values, color=colors[0])
ax1.bar(groups, treatment_by_gender.Yes.values, bottom = treatment_by_gender.No.values, color=colors[1])

# create a stacked bar on the right for proportion
ax2.bar(groups, treatment_by_gender_proportion.No, color=colors[0])
ax2.bar(groups, treatment_by_gender_proportion.Yes, bottom = treatment_by_gender_proportion.No, color=colors[1])

# treatment_by_gender.plot(kind='bar', stacked=True, color=colors)
fig.text(0,1.03,'Individuals Seeking Treatment By Gender', fontfamily='serif', fontsize=18, fontweight='bold')
fig.text(0,0.92,'We see a vast majority of both males and females seeking treatment.', fontfamily='serif', fontsize=12)
# common axis labels
fig.supxlabel('Individuals Seeking Treatment')
fig.supylabel('Count')

# set the location of the legend
ax1.legend(title='Gender', labels=labels, loc='upper left')

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.ylabel('Proportion')
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

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/gender-by-country.png){:.centered loading="lazy"}

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

# treatment_by_gender.plot(kind='bar', stacked=True, color=colors)
fig.text(0,1.03,'Individuals By Gender From Each Country', fontfamily='serif', fontsize=18, fontweight='bold')
fig.text(0,0.92,'We see the vast majority of individuals gtom the United Kingdom and United States.', fontfamily='serif', fontsize=12)

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






 It also shows 'housewives' are prone to mental health issues and are more likely to stay indoors.

```python
# Analyze the relationship between days indoors and occupation
days_indoors_by_occupation = pd.crosstab(df['days_indoors'], df['occupation'])

# Visualize the relationship between days indoors and occupation
days_indoors_by_occupation.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Days Indoors Behavior by Occupation')
plt.xlabel('Occupation')
plt.ylabel('Proportion Staying Indoors')
plt.legend(title='Occupation', labels=['Corporate', 'Student', 'Business', 'Housewife', 'Others'])
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/days_indoors_vs_occupation.png" alt="Days Indoors Behavior by Occupation" 
title="Days Indoors Behavior by Occupation" 
description="This shows consistency between all occupations across the board." %}

 Surprisingly it shows, even with mental health issues, a lot of people still enjoy their work.


```python
# Analyze the relationship between occupation and work interest
work_interest_by_occupation = pd.crosstab(df['occupation'], df['work_interest'])

# Visualize the relationship between occupation and work interest
work_interest_by_occupation.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Work Interest Behavior vs. Occupation')
plt.xlabel('Occupation')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.legend(title='Work Interest')
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/work-interest-by-occupation.png" alt="Work Interest by Occupation" 
title="Work Interest Behavior by Occupation" 
description="This shows consistency between all occupations across the board." %}

```python
# Analyze the relationship between coping struggle and treatment
coping_struggles_treatment = pd.crosstab(df['coping_struggles'], df['treatment'])

# Visualize the relationship between coping struggles and treatment
coping_struggles_treatment.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Coping Struugles vs. Seeking Treatment')
plt.xlabel('Coping Struggles')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.legend(title='Seeking Treatment')
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/coping-struggles-treatment.png" alt="Coping Struggles With Treatment" 
title="Coping Struggles With Treatment" 
description="This shows nearly an even split between people with coping struggles and another near even split for people who are seeking treatment." %}


 It's surprising that a lot of people dopn't have social weakness but h
```python
# Analyze the relationship between social weakness and mental health history
social_weakness_by_gender = pd.crosstab(df['social_weakness'], df['mental_health_history'])

# Visualize the relationship between social weakness and mental health history
social_weakness_by_gender.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Social Weakness vs. Mental Health History')
plt.xlabel('Social Weakness')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.legend(title='Mental Health History')
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/social-weakness.png" alt="Social Weakness by Mental Health History" 
title="Social Weakness by Mental Health History" 
description="This shows there is nearly an even spread for social weakness and the person having mental health history." %}

```python
# Analyze the relationship between social weakness and mental health history
mood_swings_with_treatment = pd.crosstab(df['mood_swings'], df['treatment'])

# Visualize the relationship between social weakness and mental health history
mood_swings_with_treatment.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Mood Swings vs. Treatment')
plt.xlabel('Mood Swings')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.legend(title='Treatment')
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/mood-swings-treatment.png" alt="Mood Swings by Treatment" 
title="Mood Swings by Treatment" 
description="This shows if there is a possible link to mood swings with treatment." %}

```python
# Analyze the relationship between social weakness and mental health history
growing_stress_with_chaing_habits = pd.crosstab(df['growing_stress'], df['changes_habits'])

# Visualize the relationship between social weakness and mental health history
growing_stress_with_chaing_habits.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Growing Stress vs. Changes Habits')
plt.xlabel('Growing Stress')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.legend(title='Changes Habits')
plt.show()
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/growing-stress-change-habits.png" alt="Growing Stress by Changing Habits" 
title="Growing Stress by Changing Habits" 
description="This shows if there is a possible link to mood swings with treatment." %}