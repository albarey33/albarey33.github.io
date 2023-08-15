---
title: "SA01 Statistical Analysis Small Samples T-Test"  # subtitle: "Methods to Verify Hipothesis about samples vs populationsDescription of R Scripts for data processing."
# author: BABR
date: 2023-01-07 10:34:00 -0500
categories: [Statistical Analysis]            # , R
tags: [R]          # layout: post
background: '/img/posts/01.jpg'
---

## SA01 Statistical Analysis Small Samples T-Test.R

[R Script - Merge Excel Files with Inherits](https://github.com/albarey33/Data_Analysis_R/blob/main/01%20Merging%20Excel%20files%20with%20equal%20structure%20with%20inherit.R)

<!--- #### Brief Description: --->

The purpose of this script is to import data on parts (whenever there are limitations to get data) and integrate them into one CSV file. The script first create a list of the required Excel files names located in the source folder by using list.files and regex to define a pattern. Then a function that includes readxl::read_excel(filename) is defined to read the data inside the Excel files, creating tibbles (data tables). For convenience, the defined function also converts the logical fields to character using "inherits". Finally the tibbles are merged into one CSV using do.call bind_rows.


___

## 02 Statistical Analysis Binomial Random Sample.R

[R Script - Merge Excel Files with Mutate Across](https://github.com/albarey33/Data_Analysis_R/blob/main/02%20Merging%20Excel%20files%20with%20equal%20structure%20with%20mutate%20across.R)

Similar to the previous script but using where - accross instead of inherit to read and merge excel data files.


___