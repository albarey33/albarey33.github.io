---
title: "DP13 Match Replace Pattern ReGex"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-13 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

One example of application of regular expressions to change the date format.

## Link to the Complete Script in Github
[R Script - Match Replace Pattern ReGex](https://github.com/albarey33/Data_Analysis_R/blob/main/13%20Match%20Replace%20Pattern%20ReGex.R)


## Initial data Date + Time
![13 Initial Date + Time](/images/DataProcess/13_Initial_Data_Regex.PNG){: width=100% }
_Initial data: Date + Time_

## Code with Regular expression. Take the characters before the first space
```R
gsub('([0-9]+) .*', '\\1', df$Int_Date)
```
## Final result: Only date
![13 Result](/images/DataProcess/13_Resulting_Data_Regex.PNG){: width=100% }
_Final results: Only date_


__

End of Post
