---
title: "Data Processing with R (Part 3)"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-11-30 10:24:00 -0500
categories: [Data Processing, R]
tags: [R, Dplyr, Data Processing, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

Procedures to process data using R, necessary steps required before generating reports, recaps or to use as input for machine learning models. By the next cases, sometimes the data is taken from different sources, folders and files, sometimes the data is processed using a pattern on the name to include it as a field .

___

# 21 Read data Interactions_WEEK_26_2021.R

Automatically read all the files containing the interactions per care manager in the last week. A tibble is created per file. All the tibbles are merged with dplyr::bind_rows to create a unique data frame for the week.  

## Code: [R Script - Merge Excel Files](https://github.com/albarey33/Data_Analysis_R/blob/main/21%204C%20Interactions_WEEK_26_2021.R)

___

# 22 Merge data Interactions_All_Periods.R

It collects all the weeks saved with the previous script. It collects and applies all the previous scripts. The resulting CSV files serve as sources for other procedures and for PowerBI dashboards.

## Code: [R Script - Merge Excel Files](https://github.com/albarey33/Data_Analysis_R/blob/main/22%204C%20Interactions_All_Periods.R)


## 22.1 Explanations Merge data Interactions_All_Periods.R

[R Script weeks saved with the previous script. It collects and applies all the previous scripts. The resulting CSV files serve as sources for other procedures and for PowerBI dashboards.

### Code: 

___


