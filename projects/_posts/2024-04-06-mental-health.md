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
~~~

As you can see, we have some NAN values in the self_empoyed column.

![Pandas dataframe showing first 5 rows](/assets/img/mental-health/pandas-head.png "Pandas dataframe showing first 5 rows")

I first find out how many missing values are in each column.

```python
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/missing-values.png" alt="Pandas showing columns with missing values" 
title="Pandas showing columns with missing values" 
description="This shows that there are 5202 rows with missing values in the self_employed column." %}

I drop any rows which have empty values and do the check again. This clearly shows the rows have been removed.

```python
# Drop rows with missing self_employed values
df = df.dropna(subset=['self_employed'])
# Check for missing values in each column
missing_values_count = df.isnull().sum()
# Print the count of missing values for each column
print(missing_values_count)
```

{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/zero-missing-values.png" alt="Pandas showing columns with zero missing values" 
title="Pandas showing columns with zero missing values" 
description="This shows that there are 0 rows with missing values in the self_employed column." %}

The data is now clean but there is inconsistency with the column headings naming conventions. There are title case headings and lower case headings. To keep things consistent, I rename the column headings to all lower case.

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


```python
# Defining colors for the pie chart 
colors = ['pink', 'steelblue'] 
  
# Define the ratio of gap of each fragment in a tuple 
explode = (0.05, 0.05) 
  
# Plotting the pie chart for above dataframe 
df.groupby(['gender'])['gender'].count().plot( 
    kind='pie', y='gender', autopct='%1.0f%%', 
  colors=colors, explode=explode)

plt.axis('off')
plt.show()
plt.close()
```
{% include img-left-box.html path="/data-analysis-portfolio/assets/img/mental-health/gender-pie-chart.png" alt="Pie chart showing gender distribution" 
title="Pie chart showing gender distribution" 
description="This shows that the distribution between males and females suffering with mental health is very far apart as males are 4 times more likely to suffer with mental health than females." %}

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
description="This shows consistency between all occupations across the board. It also shows 'housewives' are prone to mental health issues and are more likely to stay indoors." %}