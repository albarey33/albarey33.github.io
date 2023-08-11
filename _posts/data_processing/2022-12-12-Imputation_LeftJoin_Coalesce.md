---
title: "DP12 Imputation using LeftJoin and Coalesce"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-12 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Imputation, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

Another example of imputation of missing values but in this case using the information located in the same data table, for the same MID but in other periods. This was used on demographic values as for example race, DOB, that we can assume don't change for the same individual. A table without missing values is created using dplyr::group_by. Then the original data is merged with dplyr::left_join(). Finally the replacing value is taken with coalesce.

## Link to the Complete Script in Github
[R Script - Inputation using LeftJoin and Coalesce](https://github.com/albarey33/Data_Analysis_R/blob/main/12%20Inputation%20using%20LeftJoin%20and%20Coalesce.R)


## Initial data with missing values
![12 Result](/images/DataProcess/12_Initial_Data_with_Missing_info.PNG){: width=100% }
_Initial data with missing values_

## Code to create a new data frame by excluding the rows with missing (NA) values:
```R
df %>% dplyr::arrange(Period) %>% 
  filter(!is.na(Type)) %>% select(MID, Type) %>% 
  group_by(MID, Type) %>% tally() %>% 
  arrange(MID, desc(n)) %>% rename(Type_wo_NA = Type)
```

## Final result with the option to take value from the new table using coalesce
![12 Result](/images/DataProcess/12_Final_Result_Replacing_NA_values.PNG){: width=100% }
_Result of the Script_


__

End of Post
