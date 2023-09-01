---
title: "DP14 Add Index column to data frame"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-14 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, StringR]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

This post guides you through the process of adding an index column to a data frame. This index acts as a unique label, serving as a substitute for patient identifiers like MID or names. It's like giving each row a special ID. Creating this index  and can be done using either stringr::str_pad() or stringf functions. This index is quite handy in situations where we need to replace patient details with something less revealing.


## Link to the Complete Script in Github
[R Script - Add Index column to data frame](https://github.com/albarey33/Data_Analysis_R/blob/main/14%20Add%20Index%20column%20to%20data%20frame.R)



## Generated Index column

Enforcing a consistent length of 7 digits, the size of the array is determined by the number of rows in the data frame to which the index will be added. To enhance distinction and prevent the index from being misconstrued as a plain number, the letter 'E' is prefixed at the beginning of each index value. This safeguard ensures that the index retains its unique identifier format.


```R
PPL_df$Index <- paste0('E',stringr::str_pad(1:10, 7, side="left", pad="0"))
```

## Final result
![14 Result](/images/DataProcess/14_Generated_Index_field_attached_to_dataframe.PNG){: width=100% }
_Final result: Attached generated Index field_


__

End of Post
