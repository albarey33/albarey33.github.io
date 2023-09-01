---
title: "DP18 Merge multiple fields of Labels"
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-18 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, RegEx]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

The script takes each record representing a patient, scan across a group of fields, in this case medical conditions, and creates a compounded field with the names of the conditions that the patient has. This process is required for reporting purposes.

In order to create the compounded field the script merges the names using apply-collapse. Finally, all the resulting consecutive commas are removed with a regular expression.

<!--The purpose of this script is the inverse of the previous one. 
Having multiple fields, in this case for medical conditions. 
the field, which have the label "Yes". The script takes the names of the fields where those "Yes" values are present and merges them using apply-collapse-->

## Link to the Complete Script in Github
[R Script - Merge multiple fields of labels](https://github.com/albarey33/Data_Analysis_R/blob/main/18%20Merge%20multiple%20fields%20of%20labels%20into%20one.R)


## Initial Data: Medical Conditions for 11 patients (sample)

|row| Asthma | CVA.Stroke | Chr.Kidney.Dis | Chronic.Pain | COPD | Diabetes | CHF |
|---|--------|------------|----------------|--------------|------|----------|-----|
|192| Yes    | Yes        | No             | Yes          | No   | No       | Yes |
|111| No     | Yes        | Yes            | No           | No   | Yes      | No  |
|128| No     | No         | No             | Yes          | Yes  | Yes      | Yes |
|163| No     | No         | Yes            | Yes          | No   | Yes      | No  |
| 87| No     | Yes        | No             | Yes          | No   | No       | No  |
| 74| Yes    | No         | No             | No           | No   | No       | No  |
| 37| Yes    | Yes        | No             | No           | Yes  | No       | No  |
|  7| No     | Yes        | No             | No           | Yes  | Yes      | No  |
|396| No     | No         | No             | Yes          | Yes  | Yes      | Yes |
|311| No     | No         | No             | Yes          | No   | Yes      | Yes |
|306| No     | Yes        | No             | No           | No   | No       | No  |


![18 Initial Data](/images/DataProcess/18_SomeMedConditions_InitialData.PNG){: width=100% }
_Some Medical Conditions Initial Data in R console_

## Transforming "Yes" labels using a function

The next function converts the values "Yes" into the name of the condition, and assign blank for the "No" values.

```R
# Defined function to convert the "Yes" values into the name of the condition
fxonlyfieldnamestochangevalues <- function(df){
  for(ii in seq_along(df)){
    df[,ii] <- ifelse(df[,ii]  == "Yes", names(df)[ii],"")
  }
  df
}
```
Applying the function

```R
dataselected <- fxonlyfieldnamestochangevalues(dataselected)
```

Partial Results

![18 Partial Results](/images/DataProcess/18_Transformed_Med_Conditions_Sample.PNG){: width=100% }
_Partial Results_



## Merge the values

Merge the values into a compounded field and remove the unnecessary commas.


```R
# Code to merge the labels and remove commas
allconditions <- apply( dfclinicalconditions,1,paste,collapse = ",")
allconditions <- gsub("^,*|(?<=,),|,*$","", allconditions, perl=T)
allconditions <- gsub(",", ", ", allconditions)
# Add to the data frame as one column
dfclinicalconditions$onecolumn <- allconditions
```

## Final Result

The conditions were merged into one field.

![18 Final Result](/images/DataProcess/18_SomeMedConditions_FinalResult.PNG){: width=100% }
_Some Medical Conditions Merged into one Field. Final Result_



__

End of Post
