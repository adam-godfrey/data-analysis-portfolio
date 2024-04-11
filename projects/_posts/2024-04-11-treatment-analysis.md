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
permalink: /projects/2024-04-11-mental-health-treatment-analysis/
---
# Mental health study and behaviours

## Individuals Seeking Treatment by Gender
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

## Individuals With Coping Struggles Seeking Treatment

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/coping-struggles-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with coping struggles seeking treatment.
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

### Individuals With Coping Struggles Seeking Treatment by Gender Distribution

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/coping-struggles-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals seeking treatment by gender.
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

## Individuals With Mood Swings by Seeking Treatment

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mood-swings-by-treatment.png){:.left loading="lazy"}

Chart showing individuals with mood swings by seeking treatment by gender distribution.
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

# Visualize the relationship between social weakness and mental health history
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

### Individuals With Mood Swings Seeking Treatment by Gender Distrubtion

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/mood-swings-by-treatment-by-gender.png){:.left loading="lazy"}

Chart showing individuals with mood swings by seeking treatment by gender distribution.
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