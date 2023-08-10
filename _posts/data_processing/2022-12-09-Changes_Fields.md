---
title: "DP09 Apply changes to Filtered Group of Records"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-09 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

For only the fields related to medical conditions in the data, the empty or N/A values are replaced by the value "No". The reason of this change is to use the "Yes"/"No" for a later presentation in a PowerBI chart. These changes are applied using dplyr functions mutate and case_when.

## Link to the Complete Script in Github
[R Script - Apply changes to Medical Condition fields](https://github.com/albarey33/Data_Analysis_R/blob/main/09%20Apply%20changes%20to%20Medical%20Condition%20fields.R)


## Initial Values
![09 Input Format](/images/DataProcess/09_Medical_Conditional_original_format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Original Structure of values for Medical Conditions_

```R
# Replace empty value "" or NA by "No"
PPL_df[,RangeConditions][PPL_df[,RangeConditions]==""] <- "No"
PPL_df[,RangeConditions][is.na(PPL_df[,RangeConditions])] <- "No"
```

Other method for multiple replacement. Example for only "Dual" field
```R
PPL_df %>% mutate(Dual = case_when(Dual == ""    ~ "No", 
                                   Dual == "YES" ~ "Yes"))
```

## Results
![09 Results](/images/DataProcess/09_Medical_Conditional_final_structure.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Final arranged values for Medical Conditions_



__

End of Post


