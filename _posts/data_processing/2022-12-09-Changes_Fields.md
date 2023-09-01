---
title: "DP09 Apply changes to Filtered Group of Records"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: Alejando BaRey          #layout: post
date: 2022-12-09 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

When dealing with situations where a common transformation needs to be applied to multiple fields, the following script applies a method, often referred to as Batch Transformation.

In this scenario, the script focuses on fields related to medical conditions within the dataset. It addresses the issue of empty "" or N/A values by replacing them with the value "No." This alteration serves the purpose of using "Yes" or "No" values for subsequent visualizations in a PowerBI chart. The transformations are executed using Dplyr functions mutate and Case_When.

## Link to the Complete Script in Github
[R Script - Apply changes to Medical Condition fields](https://github.com/albarey33/Data_Analysis_R/blob/main/09%20Apply%20changes%20to%20Medical%20Condition%20fields.R)


## Initial Data Format for the group of Fields

Identify the specific location within the dataset where the transformation needs to be performed. This applies specifically to the Medical Conditions fields.

![09 Input Format](/images/DataProcess/09_Medical_Conditional_original_format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Original Structure of values for Medical Conditions_

```R
# Replace empty value "" or NA by "No"
PPL_df[,RangeConditions][PPL_df[,RangeConditions]==""] <- "No"
PPL_df[,RangeConditions][is.na(PPL_df[,RangeConditions])] <- "No"
```

## Another approach for multiple replacements with mutate and case_when 

Example for only "Dual" field

```R
PPL_df %>% mutate(Dual = case_when(Dual == ""    ~ "No", 
                                   Dual == "YES" ~ "Yes"))
```

## Results
![09 Results](/images/DataProcess/09_Medical_Conditional_final_structure.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Final arranged values for Medical Conditions_



__

End of Post


