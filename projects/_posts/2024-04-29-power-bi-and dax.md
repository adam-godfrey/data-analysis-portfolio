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

### Formatting the Sales column
I noticed from my initial inspection of the data, the Sales column was using an integer data type. I wanted to convert this to a currency format.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/change-type.png){:.centered loading="lazy"}

Converting data types from integer to fixed width decimal number (currency).
{:.figcaption}

### Creating a Date column for time intelligence
I also needed a Date column because it is a necessity if I wanted to do any type of time intelligence analysis in Power BI. I had to select the columns in the order I wanted the date column to be formatted as and I merged the columns with a separator. This removed the Day, Month and Year columns and replaced it with a Date column.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/date-column.png){:.centered loading="lazy"}

Merging columns to create new Date column.
{:.figcaption}

For the Date column, I needed to convert that from a text data type to a date data type.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/date-type.png){:.centered loading="lazy"}

Converting data types from text to date.
{:.figcaption}

### Splitting columns
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

### Sales by Category
One of the charts I wanted to display was a sales by category. I chose a chart and selected some columns of data I wanted to plot. I also had to format the chart slightly to show the values and rename the axis labels.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/sales-by-category.png){:.centered loading="lazy"}

Chart showing Sales by Category.
{:.figcaption}

### Adding Time Intelligence
The chart is okay if I wanted to view all the data vy category but I wanted to be able to select a year and see the chart update.

This was achieved by adding a Slicer to the report so when I select a year from the slicer, the chart is updated automaticlly. For the slicer, I had to drill down into the Date heirachy and select Year. I also changed the slicer from the default slider to a dropdown.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/slicer.png){:.centered loading="lazy"}

Slicer.
{:.figcaption}

### Adding a Table of Sales

#### Sales by Month
I wanted to add a table that shows the sales value by month and does a year on year percentage calculation and also a year to date calculation.

I added a table to the report and from the data fields, I selected Sales and Month.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/table-of-sales.png){:.centered loading="lazy"}

Table showing sales by month.
{:.figcaption}

#### Sales Year on Year
To add the year on year percentage, I needed to create a new quick measure. 

From the list of calculations, I had to go down to Time Intelligence and choose 'Year-over-year change'.

For the Base value, I had selected Sales and the Date I selected Date from the field options.

After I clicked Add, a new measure was added to the data fields so it could be dragged onto the existing table.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/quick-measure.png){:.centered loading="lazy"}

Quick measure.
{:.figcaption}

#### Sales Year To Date
For this step it was very similar to Sales Year on Year with the only difference being that I needed to select 'Year-to-date' from the Time Intelligence option from the Calculations dropdown.

I was then able to drag that onto my table.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/table.png){:.centered loading="lazy"}

Completed table.
{:.figcaption}

### Sales by County
The next visual I wanted on my dashboard was a map which I could select an area and the other charts are updated.

Because my locale was set to United Kingdom and I could see my sales values were in GBP, it made sense that I needed to change the Province column to County. I updated the data source for the customer data and replaced the values in Province with counties in the UK. This would help with the map as the software knows that the counties I'm using are in the UK.

I selected Filled Map as a chart type and dragged the County data field onto the chart. I was able to zoom into the map to select the Devon for example, and I could see all the sales values for Devon get updated in the other charts.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/map.png){:.centered loading="lazy"}

Map showing Devon as selected county.
{:.figcaption}

### Quantity by Month
I wanted to display a visual that shows the quantity sold by month. I added a line chart to the dashboard and select the Month for the X-axis and Quantity for the Y-axis.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/line-chart.png){:.centered loading="lazy"}

Line chart showing quantity sold by month.
{:.figcaption}

### KPI
The last visual I wanted to see was a KPI. I chose to add the sum of sales by quantity.  By selecting a different category, this chart updates with the sales by quantity using the category as a filter.

![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/KPI.png){:.centered loading="lazy"}

KPI showing Sales by Quantity.
{:.figcaption}

### My Completed Dashboard
![Full-width image](/data-analysis-portfolio/assets/img/mental-health/2024-04-29/dashboard.png){:.centered loading="lazy"}

Finished dashboard.
{:.figcaption}