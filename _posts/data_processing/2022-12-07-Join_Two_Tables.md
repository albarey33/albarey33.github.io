---
title: "DP07 Joining two Tables - Left Join"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-07 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr, Joining]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

This script provides an example of working with relational data tables and demonstrates a method for imputing values. Unlike previous cases where data was hardcoded within the script, this example involves importing a table containing data to be updated, such as practices type: Family, Pediatric, etc. Imputation of values is achieved using the left_join function.


## Link to the Complete Script in Github
[R Script - Join two Tables](https://github.com/albarey33/Data_Analysis_R/blob/main/07%20Join%20two%20Tables%20for%20Classification%20by%20Type%20of%20Practices.R)


## Data: List of Patients and Practices. View of first 10 from a list of 3000.
![07 Results](/images/DataProcess/07_First_10_Pts_w_Practices_Previous.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_List of Patients and Practices_

## List of Practices and Type.
![07 Results](/images/DataProcess/07_First_10_Practices_with_Type.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_List of Practices and Type_

## Code: Left_join Two Tables. Common key: 'Practice.Name'
```R
PPL_df <- PPL_df %>% left_join(practices_type, 
              by = c('Practice_Name' = 'Practice.Name'))
```

## Final result. Recap of Patients by Type of Practice.

```R
PPL_df %>% dplyr::group_by(Practice.Type) %>% 
                        tally() %>% arrange(desc(n))
```
## Results.
![07 Results](/images/DataProcess/07_Final_Count_of_Patient_by_Type_of_Practice.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Recap of Patients by Practice Type_



__

End of Post


