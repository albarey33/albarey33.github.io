---
title: "DP04 Stacking CSV Data Files with Different Structure"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-04 10:34:00 -0500
categories: [Data Processing]    # , Merging Tables
tags: [R, Dplyr, Stacking, Comparing, CSV]         # Data Processing, 
# background: '/img/posts/01.jpg'
#pin: true
mermaid: true
---

## Description 

This post depicts situations when data is collected in separate fragments, such as different time periods, places, or other subsets, and needs to be consolidated into a unified table for comprehensive analysis. This process is also known as "stacking" tables. Combining these individual data parts is essential to perform in-depth data processing and obtain insights that encompass the entirety of the original files.

The stacking process requires the same field names across all files. In cases where data includes varying names, the current script aids in automating the comparison process with a defined function that shows the fields that differs in names or is present in only one of the data sources. This results in significant time savings and error prevention whenever wide tables, meaning with many fields, are involved.


## Link to the Complete Script in Github

[R Script - Stacking CSV files with different structure](https://github.com/albarey33/Data_Analysis_R/blob/main/04%20Merging%20CSV%20files%20different%20structure.R)


## Example: Two DataTables with different fields. Check before concatenate
![04 Two DataTables with different fields. Check before merge](/images/DataProcess/04_Two_DataTables_with_different_fields_check_before_merge.PNG){: width="500" height="505" }
_Two DataTables with different fields. Check before merge_


## Function: Comparing two data tables with different named fields.

```R
fx_comparison_two_DFs_Fields <- function(df_x, df_y){
  LeftW <- data.frame(field=names(df_x),match=match(names(df_x), names(df_y)), "L in R")
  RightW <- data.frame(field=names(df_y),match=match(names(df_y), names(df_x)),"R in L")
  compardf <- LeftW %>% full_join(RightW, by= 'field') %>% filter(is.na(match.x) | is.na(match.y) )
  print(compardf)
}
```
_<center>Comparing two data tables with different named fields.</center>_


## Result of the comparison by using the script

![04 Comparison of Fields with Differences](/images/DataProcess/04_Comparison_Differences.PNG){: width="350" height="350" }
_Result: Comparison of Differences_

## Fixing the discrepancies

By using `dplyr::rename`, it is possible to rename the field names to match each other, allowing the fields to concatenate correctly.

```R

tibble_1mo <- tibble_1mo %>% dplyr::rename(Medicaid.ID = Current.MID,
                      Name = Patient.Name,
                      DOB = Patient.DOB,
                      Age = Patient.Age,
                      Gender = Patient.Gender,
                      Phone = Patient.Phone,
                      Address = Patient.Address,
                      City = Patient.City,
                      Zip = Patient.Zip
                      )

fx_comparison_two_DFs_Fields(tibble_1mo, tibble_2mo)

```

__

End of Post


