---
title: "DP17 Split String of Values"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-17 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

Split string of values into multiple fields and summarize.R

In the example there is a field with the reasons for service each interaction had. The most cases have grouped by commas two or more reasons. In order to split this values so that we can tally the total number of reasons for all the patients in a period, by using: with(dfRecapNoUTR, strsplit(Reason.s..for.Service, ',')). It splits the field into a list of lists. These lists are then converted into a sparse matrix to summarize it.


## Link to the Complete Script in Github
[R Script - Split string of values](https://github.com/albarey33/Data_Analysis_R/blob/main/17%20Split%20string%20of%20values%20into%20multiple%20fields%20and%20summarize.R)



## Split a String of Values into Multiple Columns per Value
Using comma as separator

## Initial Data
![17 format Date](/images/DataProcess/17_Data_Reasons_for_Service.PNG){: width=100% }
_Initial Data: Reasons for Service merged with commas_

```R
dat <- with(dfRecapNoUTR, strsplit(Reason.s..for.Service, ','))  # Result: Lists within lists
df2 <- data.frame(ID = factor(rep(dfRecapNoUTR$ID,               # ID ~ Reasons   
                                  times = lengths(dat)),
                              levels = dfRecapNoUTR$ID),
                  Reason.s..for.Service = unlist(dat))
# Sparse Matrix. Convert table to data frame
df3 <- as.data.frame(cbind(ID = dfRecapNoUTR$ID,
                   table(df2$ID, df2$Reason.s..for.Service)))  
head(df3)
```
## Graph
![17 Sparse Matrix](/images/DataProcess/17_Result_Sparse_Matrix.PNG){: width=100% }
_Result: Sparse Matrix_

## Graph
![17 Summarized Values](/images/DataProcess/17_Result_Summarized.PNG){: width=100% }
_Result: Summarized values_


__

End of Post
