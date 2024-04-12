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

* this unordered seed list will be replaced by the toc
{:toc}

## Introduction
For this section on treatment, I wanted to visualise if seeking treatment for mental health has any effect on other categories.

- **Coping struggles** - Do individuals have reduced coping struggles having treatment?
- **Mood swings** - Are individuals having moods swings with treatment?
- **Social weakness** - Do individuals have social weakness with treatment?
- **Number of days indoors** - Is the number of days indoors reduced having seeking streatment?
- **Occupation** - An overview of occupation and number of individuals seeking treatment
- **Work interest** - Does having treatment increase work interest?
- **Growing stress** - Has treatment had an impact on growing stress?
- **Family history** - Is there a link to family history of mental health issues?
- **Mental health history** - Does the individual have a history of mental health?
- **Changing habits** - Does seeking treatment alter a change in habits?

I believe a lot of these factors are related such as **Occupation**, **Growing Stress** and **Work Interest**. Some occupations can be very stressful, with some occupations the individual may have a serious lack of work interest.

I want to complete some further analysis to see if there are links between the categories.


## Seeking Treatment
The first task was to get an idea of the percentage of individuals who are seeking treatment.

From the chart, it is easy to see there that is very nearly a 50/50 split between seeking treatment and not seeking treatment.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/seeking-treatment.png){:.left loading="lazy"}

Chart showing seeking streatement percentages.
{:.figcaption}

<details>
<summary>Expand to see code used</summary>
<div markdown="1">
```python
# Plotting the pie chart for dataframe 
treatment = df.groupby(['treatment'])['treatment'].count()

fig, ax = plt.subplots(figsize=(4, 4))

# Capture each of the return elements.
patches, texts, pcts = ax.pie(
    treatment, labels=labels, autopct='%.1f%%',
    wedgeprops={'linewidth': 3.0, 'edgecolor': 'white'})

# Style just the percent values.
plt.setp(pcts, color='white', fontweight='bold')
plt.tight_layout()
plt.show()
plt.close()
```
</div>
</details>
<br/>

### Seeking Treatment by Gender
First of all, I wanted to get an idea of the distribution between genders who are seeking treatment. I already know from the [initial analysis](https://adam-godfrey.github.io/data-analysis-portfolio/projects/2024-04-06-mental-health/) that the gender ratio of individuals who completed the study was 4:1 males vs. females. so I was expecting a larger amount of values for males.

From the first chart below, we can see that individuals in both genders are about 5x more likely seek treatment.

In the second chart, we can see that males are 4x more likely to not seek treatment compared to females and there is a near even split between both genders seeking treatment. While we can see that bother genders are evenly split seeking treatment, it makes me wonder why 70% of males don't seek treatment. 

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
For this section I wanted to see if seeking treatment has any effect on a person's coping struggles.

Here, I found this very interesting. There is a 50/50 split for individuals with coping struggles. 

For the half that doesn't have coping struggles, half of that again aren't seeking treatment so what could affect this? Could it be their work interest in that they enjoy their occupation, or have they changed habits? For the other half who are seeking treatment, it could be seeking treatment helps with their coping abilities.

Back to the individuals with coping struggles, 50% aren't seeking treatment. Why could this be? And for the other 50% who are seeking treatment who are still struggling to cope, what could be the cause? Could it be their occupation or another one of the categories?

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
The ratio between genders, both seeking and not seeking treatment are very similar which you can clearly see based onj the previous chart.

What is interesting however, is that females are more likely to seek treatment than males and that females who are haveing coping struggles are seeking treatment.
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
For this section I wanted to see if seeking treatment has any effect on a person's mood swings.

It's interesting to see that it is nearly very evenly split between all possible values being High, Low and Medium and seeking treatment doesn't seem to have any effect on mood swings.
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
What is very intersting indeed is females who aren't seeking treatment are more likely to get mood swings by up to 50% compared to females who are seeking treatment.

Males on the other hand are prettu well balanced when it comes to mood swings but there are slightly more prone to individuals who are not seeking treatment.

This goes to show both genders are more likely to suffer with mood swings if they don't seek treatment.

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
For this section I wanted to see if social weakness is affected by indiviudals seeking treatment.

This chart looks very similar to the mood swings chart except Maybe having the highest counts across the board.

Again, it appears that seeking treatment doesn't seem to impact on social weakness.

For people who don't have social weakness and aren't seeking treatment, what factors are affecting this....occupation enjoyment? For those who are seeking treatment, is the treatment helping with the social anxiety?

For those who have social weakness, is there another factor influencing this?

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

Again, the chart looks very similar to the mood swings gender chart where a vast majority of females aren't seeking treatment. The males on the other hand, about 46% are seeking treatment

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
For this section, I wanted to see if seeking treatment has any effect on the amount of days individuals are spending indoors.

it is very interesting to see that there isn't much variation between the number of days indoors for both seeking and not seeking treatment.

People who go out every day are still quite high in terms of value counts whether they have treatment or not but we can see that mostly people are staying indoors between 1-14 days.

The difference between seeking treatment or not is so marginal, it appears that seeking treatment doesn't affect the number of days people are staying indoors.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-11/days-indoors-by-treatment.png){:.left loading="lazy"}

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
There is no surprise here that females are not seeking teatment but it does affect how many days they stay indoors. 

It is pretty evely levelled out across the board however you can see, those who are not seeking treatment are more likely to stay indoors for long periods of time.

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
Unlike the females, males are more likely to seek treatment anbd this clearly shows.

What I would've expected to see is a higher increase in lower number of days indoors such as 'Go out every day' althought 1-14 days has a high value count.

For those who are seeking treatment, it would be good to see if any other factors are affecting this.

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

