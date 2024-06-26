---
layout:     post
title:      Power BI Mental Health Analysis
categories: [projects]
image: 
  path: /assets/img/mental-health/2024-04-29/power-bi-icons-1.jpg
description: >
  
hide_description: true
sitemap: false
permalink: /projects/2024-04-29-power-bi-mental-health-analysis/
---
# Power BI Mental Health Analysis

* this unordered seed list will be replaced by the toc
{:toc}

## Introduction


## Data Preparation
Having already completed an [analysis](https://adam-godfrey.github.io/data-analysis-portfolio/projects/2024-04-06-mental-health/) on a [dataset](https://www.kaggle.com/datasets/bhavikjikadara/mental-health-dataset "Your home for data science"), I wanted to use the same dataset to see the difference in visualisations.

I already knew there were formatting issues with the column names as there was a mixture of title case, unserscores etc and I wanted to tidy this up before I start any analysis on the data.

I started by selecting a data source for my data and this was using text/csv source. Once the data had been imported, I had to tell Power BI that the first row is column header values. From there I had to make changes to the column names and I chose to use Title Capitalisation to keep the column names in the same format. It is more humanly readable for the user.

I had to transform the data and select the Advanced Editor within Power BI and enter the following code:

```
let
    Source = Csv.Document(File.Contents("C:\Users\Ad\Pandas\data\Mental Health Dataset.csv"),[Delimiter=",", Columns=17, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Changed Type" = Table.TransformColumnTypes(Source, {
        {"Column1", type text}, {"Column2", type text}, {"Column3", type text}, {"Column4", type text}, {"Column5", type text}, {"Column6", type text}, {"Column7", type text}, {"Column8", type text}, {"Column9", type text}, {"Column10", type text}, {"Column11", type text}, {"Column12", type text}, {"Column13", type text}, {"Column14", type text}, {"Column15", type text}, {"Column16", type text}, {"Column17", type text}
    }),
    #"Promoted Headers" = Table.PromoteHeaders(#"Changed Type", [PromoteAllScalars=true]),
    #"Changed Type1" = Table.TransformColumnTypes(#"Promoted Headers", {
        {"Timestamp", type text}, {"Gender", type text}, {"Country", type text}, {"Occupation", type text}, {"self_employed", type text}, {"family_history", type text}, {"treatment", type text}, {"Days_Indoors", type text}, {"Growing_Stress", type text}, {"Changes_Habits", type text}, {"Mental_Health_History", type text}, {"Mood_Swings", type text}, {"Coping_Struggles", type text}, {"Work_Interest", type text}, {"Social_Weakness", type text}, {"mental_health_interview", type text}, {"care_options", type text}
    }),
    #"GET column names" = Table.ColumnNames(#"Changed Type1"),
    #"REPLACE underscore in colnames" = List.Transform(#"GET column names", each Text.Replace(_, "_", " ")),
    #"CAPITALIZE first letter in colnames" = List.Transform(#"REPLACE underscore in colnames", each Text.Proper(_)),
    #"APPLY colnames to table" = Table.RenameColumns(#"Changed Type1",List.Zip({Table.ColumnNames(#"Changed Type1"), #"CAPITALIZE first letter in colnames"}))
in
    #"APPLY colnames to table"
```

I also knew there were issues with the data in that there were a lot of rows with missing values for the Self Employed column. To get an idea of how many rows had missing values by each gender I used the following code to create a table of counts.

```
EVALUATE
SUMMARIZE (
    MentalHealthDataset,
    ROLLUP ( 'MentalHealthDataset'[Gender] ),
    "Blanks", COUNTBLANK ( 'MentalHealthDataset'[Self Employed] ) + 0,
    "Non Blanks", CALCULATE (
            COUNTROWS ( MentalHealthDataset ),
            NOT ISBLANK ( MentalHealthDataset[Self Employed] )
        )
)
```

This produced a table like this.

| MentalHealthDataset[Gender] | [Blanks] | [Non Blanks] |
|-----------------------------|----------|--------------|
| Female                      | 1302     | 51212        |
| Male                        | 3900     | 235950       |
|                             | 5202     | 287162       |

After removing rows that have blank values, I reran the code above and got the following result.

| MentalHealthDataset[Gender] | [Blanks] | [Non Blanks] |
|-----------------------------|----------|--------------|
| Female                      |          | 51212        |
| Male                        |          | 235950       |
|                             |          | 287162       |

## Data Visualisations

### Distribution of Country
I already knew from my previous analysis about the country distribution, however I wanted to see if I could visualise this better.

I chose a similar chart and added a card to the chart to show the values. By selecting a country from the chart, the card values update to show the selected country and the gender distribution within that country.

I also added another chart to show the overall gender distribution for each country relative to the gender distribution of the whole dataset which again update to show the selected country.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/gender-by-country.png){:.centered loading="lazy"}

Chart showing gender distribution by country.
{:.figcaption}

```html
DEFINE
    MEASURE MentalHealthDataset[Male] = CALCULATE ( COUNTROWS ( MentalHealthDataset ), MentalHealthDataset[Gender] = "Male" )
    MEASURE MentalHealthDataset[Female] = CALCULATE ( COUNTROWS ( MentalHealthDataset ), MentalHealthDataset[Gender] = "Female" )
EVALUATE
SUMMARIZECOLUMNS (
    MentalHealthDataset[Country],
    "Male", [Male],
    "Female", [Female]
)
```