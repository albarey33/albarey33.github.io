---
title: "Matrix Counter of Member Months"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-10 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

Procedures to process data using R, necessary steps required before generating reports, recaps or to use as input for machine learning models. By the next cases, sometimes the data is taken from different sources, folders and files, sometimes the data is processed using a pattern on the name to include it as a field #<-- necessary for recap of info.-->

___

## 10 Matrix Counter of Member Months using Melt DCast (equivalent to Pivot tables)

This code calculates the number of months each patient was enrolled in the 12 months previous to an specific months. For ilustration, this script shows a calculation for only the enrollment month and the previous one. So a patient can have one or two member-months. The data comes in just two fields: Period and MID (patient identifier). The functions melt and dcast (equivalent of pivot tables) are used to convert the data into a table MID vs Period.

### Initial Data
![10 Input Format](/images/DataProcess/10_Initial_Data_Simulated_Enrollment_in_Four_Periods.PNG){: width=100% }
_Initial Data Simulated Enrollment in Four Periods_

### Convert to Matrix Member ~ Period
```R
# 1 or 0 if the pt is enrolled in each Period
castedMIDPeriod <- data.table::dcast(melt(data.table(df), 
                           id.vars = c("MID", "Period"), 
                           measure.vars = c("Period")),
                           MID ~ Period, fun.agg = length)
```
![10 Matrix MbPeriod](/images/DataProcess/10_Matrix_Enrollment_Member_Periods.PNG){: width=100% }
_Matrix Enrollment Member Periods_

### Counter of Two Months
Fields of member-months are created per ending period. The code returns since the first period that have a previous one (202102) 
Code: Counter of Current + Previous month. 
```R
# Counter of Current + Previous month
for(i in 2:nrow(dfPeriod)){   
  #  i <- 12
  dfPeriodfincol <- dfPeriod$Period[i]
  dfPeriodfincol     # Name of Month example "201809"
  C3fincol <- match(dfPeriodfincol, names(castedMbMos))
  C3fincol        # Index of Month example "13"
  dfPeriodinicol <- dfPeriod$Period[i-1]
  dfPeriodinicol
  C3inicol <- match(dfPeriodinicol, names(castedMbMos))
  C3inicol
  castedMbMos[,C3inicol:C3fincol]
  castedMbMos <- castedMbMos %>% 
    mutate( !!paste0(dfPeriodfincol, 'MbMo') := 
              rowSums(castedMbMos[,C3inicol:C3fincol]))
}
```

### Final Result of Script
![10 Results](/images/DataProcess/10_Final_Results_Calculated_MemberMonths.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Calculated Member Months_

### Code: [R Script - Count of Member Months Matrix Melt DCast](https://github.com/albarey33/Data_Analysis_R/blob/main/10%20Count%20of%20Member%20Months%20Matrix%20Melt%20DCast%20.R)

_____

### END OF POST
___


