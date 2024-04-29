---
layout:     post
title:      MS Power BI and DAX
categories: [projects]
image: 
  path: /assets/img/mental-health/2024-04-29/power-bi-icons-1.jpg
description: >
  
hide_description: true
sitemap: false
permalink: /projects/2024-04-29-power-bi-and-dax/
---
# Power BI and DAX - Introduction and Overview

* this unordered seed list will be replaced by the toc
{:toc}

## Introduction
After completing my initial analysis using [Python](https://www.python.org/) and [Pandas](https://pandas.pydata.org/), I went on to explore other toold used for Data Analytics.

I found a lot of businesses use [Microsoft Power BI](https://app.powerbi.com/) for their business intelligence and thought I would investigate this further. Upon initial inspection, I found that some of the syntax for creating queries on the data is also used in Microsoft Excel 2016 onwards. Already having used Excel quite extensively in previous roles, I decided to learn other techniques for data analytics and business intelligence using Power BI.

A big part of Power BI that I found the most interesting and is also used in Excel, is DAX or Data Analysis Expressions. At first it can be quite complicated but once I was used to how it worked, things didn't seem so bad. I still have a way to go with learning DAX but seeing the result I was looking for makes the time and effort learning DAX well worth it.

> Data Analysis Expressions (DAX) is a formula expression language used in Analysis Services, Power BI, and Power Pivot in Excel. DAX formulas include functions, operators, and values to perform advanced calculations and queries on data in related tables and columns in tabular data models.

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