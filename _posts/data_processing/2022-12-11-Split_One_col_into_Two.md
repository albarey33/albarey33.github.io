---
title: "DP11 Split One Column into Two Fields"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-11 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

Split one field with compounded data ('Practice...NPI') into two by using the function str_split_fixed (stringr) and a separator. In this example, per each practice it was required to separate the National Provider Identifier (NPI) from the name.

## Link to the Complete Script in Github
[R Script - Split One Column into Two Fields](https://github.com/albarey33/Data_Analysis_R/blob/main/11%20Split%20One%20Column%20into%20Two%20Fields.R)


## Initial Data
![11 Input](/images/DataProcess/11_Initial_Data_with_the_field_required_to_split.PNG){: width=100% }
_Initial Data with the field required to split_

## Code to split 
Split into two fields using the separator ": "

```R
stringr::str_split_fixed(PPL_df$Practice...NPI, ": ", n = 2) 
```

The new data frame is then appended to the original data frame

## Final Result
![11 Result](/images/DataProcess/11_Final_Result.PNG){: width=100% }
_Result with only the practice name_



__

End of Post
