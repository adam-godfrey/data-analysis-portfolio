---
layout:     post
title:      Mental Health Analysis - Treatment
categories: [projects]
image: 
  path: /assets/img/mental-health/mental-health.png
description: >
  This section breaks down each of the variables by treatment to see if there is correlation between them.
hide_description: true
sitemap: false
permalink: /projects/2024-04-11-mental-health-treatment-analysis/
---
# Mental Health Analysis - Treatment

## Seeking Treatment by Gender
From the first chart below, we can see that individuals in both genders are about 5x more likely seek treatment.

In the second chart, we can see that males are 4x more likely to not seek treatment compared to females and there is a near even split between both genders seeking treatment;.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/treatment-by-gender.png){:.left loading="lazy"}

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
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2 ,figsize=(10, 4))

# create a stacked bar on the left for counts
ax1.bar(groups, treatment_by_gender.No.values, color=colors[0])
ax1.bar(groups, treatment_by_gender.Yes.values, bottom = treatment_by_gender.No.values, color=colors[1])

# set the location of the legend
ax1.legend(title='Gender', labels=labels, loc='upper left')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [v.get_height() if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])
    
# create a stacked bar on the right for proportion
ax2.bar(groups, treatment_by_gender_proportion.No, color=colors[0])
ax2.bar(groups, treatment_by_gender_proportion.Yes, bottom = treatment_by_gender_proportion.No, color=colors[1])

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.1f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])
    
# common axis labels
fig.supxlabel('Individuals Seeking Treatment')
fig.supylabel('Count')

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

## Coping Struggles

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/coping-struggles-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with coping struggles.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between coping struggles and treatment
coping_struggles_by_treatment = df.groupby(['treatment', 'coping_struggles'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['No', 'Yes']
label_colors = ['white', 'white']

ax = coping_struggles_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Coping Struggles', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

### Coping Struggles by Gender

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/coping-struggles-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with coping struggles by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between coping struggles and treatment
coping_struggles_by_treatment_yes = df[df['coping_struggles'] == 'Yes'].groupby(['treatment', 'gender'])['gender'].count().unstack().fillna(0)
coping_struggles_by_treatment_no = df[df['coping_struggles'] == 'No'].groupby(['treatment', 'gender'])['gender'].count().unstack().fillna(0)

# chart settings
colors = ['pink', 'steelblue'] 
gender = ['Female', 'Male']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 4))

fig.supxlabel('Coping Struggles')

ax1 = coping_struggles_by_treatment_no.plot(kind='bar', ax=ax1, width=1, xlabel='No', ylabel='Seeking Treatment', color=colors)

# set the location of the legend
ax1.legend(title='Gender', labels=gender, loc='upper left')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# rotation of xlabels
for label in ax1.get_xticklabels():
  label.set_rotation(0)
  label.set_ha('right')
    
# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2 = coping_struggles_by_treatment_yes.plot(kind='bar', ax=ax2, width=1, xlabel='Yes', ylabel='Seeking Treatment', color=colors)

# set the location of the legend
ax2.legend(title='Gender', labels=gender, loc='upper left')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# rotation of xlabels
for label in ax2.get_xticklabels():
  label.set_rotation(0)
  label.set_ha('right')

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

## Mood Swings

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mood-swings-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with mood swings.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between mood swings and treatment
mood_swings_by_treatment = df.groupby(['treatment', 'mood_swings'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between mood swings and treatment
ax = mood_swings_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Mood Swings', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

### Mood Swings by Gender

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mood-swings-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with mood swings by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Mood Swings')

ax1 = sns.countplot(x='mood_swings', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='mood_swings', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Mmale')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

## Social Weakness
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/social-weakness-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with social weakness.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between social weakness and treatment
social_weakness_by_treatment = df.groupby(['treatment', 'social_weakness'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between social weakness and treatment
ax = social_weakness_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Social Weakness', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

### Social Weakness by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/social-weakness-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with social weakness by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Social Weakness')

ax1 = sns.countplot(x='social_weakness', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='social_weakness', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/>   

## Number of Days Indoors
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/days-indoors-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individual's number of days indoors.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between occupation and treatment
days_indoors_by_treatment = df.groupby(['treatment', 'days_indoors'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['1-14 days', '15-30 days', '31-60 days', 'Go out everyday', 'More than 2 months']
colors = ['red', 'yellow', 'limegreen', 'dodgerblue', 'rebeccapurple']
label_colors = ['white', 'black', 'black', 'white', 'white']

# Visualize the relationship between occupation and treatment
ax = days_indoors_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Days Indoors', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Female Number of Days Indoors
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/days-indoors-by-treatment-female.png){:.left loading="lazy"}

Chart showing female individual's number of days indoors.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Correlation between days idoors and treatment
pallete = sns.color_palette(['#FFC0CB', '#E37383'])
label_colors = ['black', 'white']

plt.figure(figsize=(10, 6))

ax = sns.countplot(x='days_indoors', data=df[df['gender'] == 'Female'], hue='treatment', palette=pallete)

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper left')

# set the xlabel and ylabelfor females chart
ax.set_xlabel('Days Indoors')
ax.set_ylabel('Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Male Number of Days Indoors
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/days-indoors-by-treatment-male.png){:.left loading="lazy"}

Chart showing male individual's number of days indoors.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Correlation between days idoors and treatment
pallete = sns.color_palette(['#90E0EF', '#4682b4'])
label_colors = ['black', 'white']

plt.figure(figsize=(10, 6))

ax = sns.countplot(x='days_indoors', data=df[df['gender'] == 'Male'], hue='treatment', palette=pallete)

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper left')

# set the xlabel and ylabelfor females chart
ax.set_xlabel('Days Indoors')
ax.set_ylabel('Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/occupation-by-treatment.png){:.left loading="lazy"}

Chart showing individual's occupation.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
## Occupation
```python
# Analyze the relationship between occupation and treatment
days_indoors_by_treatment = df.groupby(['treatment', 'days_indoors'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['1-14 days', '15-30 days', '31-60 days', 'Go out everyday', 'More than 2 months']
colors = ['red', 'yellow', 'limegreen', 'dodgerblue', 'rebeccapurple']
label_colors = ['white', 'black', 'black', 'white', 'white']

# Visualize the relationship between occupation and treatment
ax = days_indoors_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Occupation', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Occupation by Female
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/occupation-by-treatment-female.png){:.left loading="lazy"}

Chart showing female individual's occupation.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Correlation between family history and mh history
pallete = sns.color_palette(['#FFC0CB', '#E37383'])

plt.figure(figsize=(10, 6))

ax = sns.countplot(x='occupation', data=df[df['gender'] == 'Female'], hue='treatment', palette=pallete)

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper left')

# set the xlabel and ylabelfor females chart
ax.set_xlabel('Occupation')
ax.set_ylabel('Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Occupation by Male
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/occupation-by-treatment-male.png){:.left loading="lazy"}

Chart showing male individual's occupation.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Correlation between family history and mh history
pallete = sns.color_palette(['#90E0EF', '#4682b4'])

plt.figure(figsize=(10, 6))

ax = sns.countplot(x='occupation', data=df[df['gender'] == 'Male'], hue='treatment', palette=pallete)

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper right')

# set the xlabel and ylabelfor females chart
ax.set_xlabel('Occupation')
ax.set_ylabel('Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

## Work Interest

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/work-interest-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individual's work interest.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between work interest and treatment
work_interest_by_treatment = df.groupby(['treatment', 'work_interest'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between social weakness and treatment
ax = work_interest_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Work Interest', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

### Work Interest by Gender Distrubtion

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/work-interest-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individual's work interest by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Work Interest')

ax1 = sns.countplot(x='work_interest', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='work_interest', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
</div>
</details>
<br/>

## Growing Stress
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/growing-stress-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with growing stress.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between growing stress history and treatment
growing_stress_by_treatment = df.groupby(['treatment', 'growing_stress'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between growing stress and treatment
ax = mental_health_history_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Growing Stress', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Growing Stress by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/growing-stress-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with growing stress by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Growing Stress')

ax1 = sns.countplot(x='growing_stress', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='growing_stress', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

## Family History
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/family-history-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with family history.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between family history and treatment
family_history_by_treatment = df.groupby(['treatment', 'family_history'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['No', 'Yes']
label_colors = ['white', 'white']

# Visualize the relationship between family history and treatment
ax = family_history_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Family History', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Family History by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/family-history-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with family history by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Family History')

ax1 = sns.countplot(x='family_history', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='family_history', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

## Mental Health History
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mental-health-history-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with mental health history.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between mental health history and treatment
mental_health_history_by_treatment = df.groupby(['treatment', 'mental_health_history'])['treatment'].count().unstack().fillna(0)

# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between mental health = history and treatment
ax = mental_health_history_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Mental Health History', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Mental Health History by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mental-health-history-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with mental health history by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Mental Health History')

ax1 = sns.countplot(x='mental_health_history', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='mental_health_history', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

## Changing Habits
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/changes-habits-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with changing habits.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between changing habits and treatment
changes_habits_by_treatment = df.groupby(['treatment', 'changes_habits'])['treatment'].count().unstack().fillna(0)

# chart settings
# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between changing habits and treatment
ax = changes_habits_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Changes Habits', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

### Changing Habits by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/changes-habits-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with changing habits by gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Changes Habits')

ax1 = sns.countplot(x='changes_habits', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='growing_stress', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

## Care Options
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/care-options-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with care options.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Analyze the relationship between care options and treatment
changes_habits_by_treatment = df.groupby(['treatment', 'care_options'])['treatment'].count().unstack().fillna(0)

# chart settings
# chart settings
legend = ['Maybe', 'No', 'Yes']
label_colors = ['white', 'white', 'white']

# Visualize the relationship between changing habits and treatment
ax = changes_habits_by_treatment.plot(kind='bar', stacked=True, figsize=(10, 6), rot=0, xlabel='Care Options', ylabel='Count')

for i, c in enumerate(ax.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# set the location of the legend
ax.legend(title='Seeking Treatment', labels=legend, loc='upper center')

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 


### Care Options by Gender
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/care-options-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with care options gender.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# chart settings
female_pallete = sns.color_palette(['#FFC0CB', '#E37383'])
male_pallete = sns.color_palette(['#90E0EF', '#4682b4'])
legend = ['No', 'Yes']
label_colors = ['black', 'white']

# set the fig size for the titles
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

fig.supxlabel('Care Options')

ax1 = sns.countplot(x='care_options', data=df[df['gender'] == 'Female'], hue='treatment', ax=ax1, palette = female_pallete)

# set the location of the legend
ax1.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor females chart
ax1.set_xlabel('Female')
ax1.set_ylabel('Count')

for i, c in enumerate(ax1.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax1.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax1.spines['top'].set_visible(False)
ax1.spines['right'].set_visible(False)

ax2  = sns.countplot(x='care_options', data=df[df['gender'] == 'Male'], hue='treatment', ax=ax2, palette=male_pallete)

# set the location of the legend
ax2.legend(title='Treatment', labels=legend, loc='upper left')
# set the xlabel and ylabelfor males chart
ax2.set_xlabel('Male')
ax2.set_ylabel('Count')

for i, c in enumerate(ax2.containers):
    # Optional: if the segment is small or 0, customize the labels
    labels = [f'{v.get_height():.0f}' if v.get_height() > 0 else '' for v in c]
    # remove the labels parameter if it's not needed for customized labels
    ax2.bar_label(c, labels=labels, label_type='center', color=label_colors[i])

# Remove the top and right spines
ax2.spines['top'].set_visible(False)
ax2.spines['right'].set_visible(False)

plt.show()
plt.close()
```
 </div>
</details>
<br/> 

