---
title: "DP14 Add Index column to data frame"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-14 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

An index column with unique values was created to use insted of the patients identifier MID or the name. It can be created either by using stringr::str_pad() or stringf (base). An index is necessary in some situations to replace and don't reveal the patient personal information.  

## Link to the Complete Script in Github
[R Script - Add Index column to data frame](https://github.com/albarey33/Data_Analysis_R/blob/main/14%20Add%20Index%20column%20to%20data%20frame.R)



## Generated Index column
Forcing the number of digits to 7. The array length is calculated by the number of rows of the data frame this array will be attached to. 

```R
PPL_df$Index <- paste0('E',stringr::str_pad(1:10, 7, side="left", pad="0"))
```

## Final result
![14 Result](/images/DataProcess/14_Generated_Index_field_attached_to_dataframe.PNG){: width=100% }
_Final result: Attached generated Index field_


__

End of Post
