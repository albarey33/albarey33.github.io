---
title: "DP18 Merge multiple fields of Labels"
#author: AlBaRey          #layout: post
date: 2022-12-18 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

The purpose of this script is the inverse of the previous one. With multiple medical conditions in the data, with one field per condition, the script takes per each MID the conditions on which each patient is flagged as "Yes". The script take the names of those flags and merge all those fields using apply-collapse. Finally all the resulting consecutive commas are removed with a regular expression.

## Link to the Complete Script in Github
[R Script - Merge multiple fields of labels](https://github.com/albarey33/Data_Analysis_R/blob/main/18%20Merge%20multiple%20fields%20of%20labels%20into%20one.R)


## Graph
![18 Initial Data](/images/DataProcess/18_SomeMedConditions_InitialData.PNG){: width=100% }
_Some Medical Conditions Initial Data_

## Defined function to convert the values "Yes" into the name of the condition
```R
# Defined function to convert the values "Yes" into the name of the condition
fxonlyfieldnamestochangevalues <- function(df){
  for(ii in seq_along(df)){
    df[,ii] <- ifelse(df[,ii]  == "Yes", names(df)[ii],"")
  }
  df
}
```

```R
# Code to merge the labels and remove commas
allconditions <- apply( dfclinicalconditions,1,paste,collapse = ",")
allconditions <- gsub("^,*|(?<=,),|,*$","", allconditions, perl=T)
allconditions <- gsub(",", ", ", allconditions)
# Add to the data frame as one column
dfclinicalconditions$onecolumn <- allconditions
```

## Graph
![18 Final Result](/images/DataProcess/18_SomeMedConditions_FinalResult.PNG){: width=100% }
_Some Medical Conditions Merged into one Field. Final Result_



__

End of Post
