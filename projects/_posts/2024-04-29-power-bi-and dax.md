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
After completing my initial analysis using [Python](https://www.python.org/) and [Pandas](https://pandas.pydata.org/), I went on to explore other tools used for Data Analytics.

I found a lot of businesses use [Microsoft Power BI](https://app.powerbi.com/) for their business intelligence and thought I would investigate this further. Upon initial inspection, I found that some of the syntax for creating queries on the data is also used in Microsoft Excel 2016 onwards. Already having used Excel quite extensively in previous roles, I decided to learn other techniques for data analytics and business intelligence using Power BI.

A big part of Power BI that I found the most interesting and is also used in Excel, is DAX or Data Analysis Expressions. At first it can be quite complicated but once I was used to how it worked, things didn't seem so bad. I still have a way to go with learning DAX but seeing the result I was looking for makes the time and effort learning DAX well worth it.

> Data Analysis Expressions (DAX) is a formula expression language used in Analysis Services, Power BI, and Power Pivot in Excel. DAX formulas include functions, operators, and values to perform advanced calculations and queries on data in related tables and columns in tabular data models.

Before I perform some analysis on my [Mental Health Dataset](https://adam-godfrey.github.io/data-analysis-portfolio/projects/2024-04-06-mental-health/), I wanted to familarise myself with Power BI and the reports and visualisations I can create using this software. For this section I got through the steps I used to create a dashboard showing some visualisations.

## Data Preparation
The first step I had to perform was to import some data and clean it up. I might need to add/remove some columns, change columnn types or even rename column headers.

I noticed from my initial inspection of the data, the Sales column was using an integer data type. I wanted to convert this to a currency format.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/change-type.png){:.centered loading="lazy"}

Converting data types from integer to fixed width decimal number (currency).
{:.figcaption}

I also needed a Date column because it is a necessity if I wanted to do any type of time intelligence analysis in Power BI. I had to select the columns in the order I wanted the date column to be formatted as and I merged the columns with a separator. This removed the Day, Month and Year columns and replaced it with a Date column.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/date-column.png){:.centered loading="lazy"}

Merging columns to create new Date column.
{:.figcaption}

For the Date column, I needed to convert that from a text data type to a date data type.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/date-type.png){:.centered loading="lazy"}

Converting data types from text to date.
{:.figcaption}

Another formatting step was to split the CityProvince column into 2 columns for City and Province. I needed to choose the delimiter to split on and it was easy to see from the data that the province was in brackets so that became my delimiter

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/delimiter.png){:.centered loading="lazy"}

Splitting the CityProvince column into separate columns.
{:.figcaption}

After that was done I needed to rename the columns and also replace the values in the new Province column to remove the closing bracket.
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/replace-values.png){:.centered loading="lazy"}

Cleanup of column names and replacing values.
{:.figcaption}

I also removed any columns that I didn't need such as customer data.

## Creating the Dashboard Visualisation
One of the charts I wanted to display was a sales by category. I chose a chart and selected some columns of data I wanted to plot. I also had to format the chart slightly to show the values and rename the axis labels.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/sales-by-category.png){:.centered loading="lazy"}

Chart showing Sales by Category.
{:.figcaption}