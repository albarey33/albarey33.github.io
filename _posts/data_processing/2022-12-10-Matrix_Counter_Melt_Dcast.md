---
title: "DP10 Matrix Counter using Melt Dcast"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-10 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr, Melt - Dcast]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description 

**Script:** Matrix Counter using Melt DCast (equivalent to Pivot tables)

**Example:** Enrollment Member-Months

This script computes the count of months each patient was enrolled in during the 12 months preceding a specific month. For illustrative purposes, the script focuses on calculating member months for the enrollment month and the one immediately prior. As a result, each patient can have either one or two member-months. The dataset consists of two crucial fields: "Period" and "MID" (patient identifier). Leveraging the functionalities of the melt and dcast functions (similar to Excel pivot tables), the data transformation occurs, converting the data into a structured table that compares patients (MID) against their enrollment periods (Period).



## Link to the Complete Script in Github

[R Script - Count of Member Months Matrix Melt DCast](https://github.com/albarey33/Data_Analysis_R/blob/main/10%20Count%20of%20Member%20Months%20Matrix%20Melt%20DCast%20.R)


## Initial Data
![10 Input Format](/images/DataProcess/10_Initial_Data_Simulated_Enrollment_in_Four_Periods.PNG){: width=100% }
_Initial Data Simulated Enrollment in Four Periods_

## Convert to Matrix Member ~ Period
```R
# 1 or 0 if the pt is enrolled in each Period
castedMIDPeriod <- data.table::dcast(melt(data.table(df), 
                           id.vars = c("MID", "Period"), 
                           measure.vars = c("Period")),
                           MID ~ Period, fun.agg = length)
```
![10 Matrix MbPeriod](/images/DataProcess/10_Matrix_Enrollment_Member_Periods.PNG){: width=100% }
_Matrix Enrollment Member Periods_

## Counter of Two Months
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

## Final Result of Script

![10 Results](/images/DataProcess/10_Final_Results_Calculated_MemberMonths.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Calculated Member Months_


The resulting table shows the MbMos, reflecting the initial matrix source. For patient 7004R, the 2 MbMos mean that they were enrolled in 202102 and in the previous month, 202101. The patient 9265T, in period 202103, has 1 MbMos because they were enrolled in 202102 but not in 202103... and so on.

__

End of Post



