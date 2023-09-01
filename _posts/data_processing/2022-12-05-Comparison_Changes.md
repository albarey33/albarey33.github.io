---
title: "DP05 Comparison and Changes between Two Periods"    #  05  # subtitle: "SUBTITLE it doesn't work for chirpy."
# author: BABR #author: Alejando BaRey          #layout: post
date: 2022-12-05 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr, Comparing, Mining]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

The present example shows a comparison between two periods: 'previous' and 'current'. In an enrolled population, there can be new patients (present in the current period but not in the previous), unenrolled patients (present in the previous period but not in the current), or patients present in both periods. Using the R Dplyr functions, it's possible to tally the number of patients in these three groups and further categorize these groups based on another characteristic, such as the practices they were linked to.

## Link to the Complete Script in Github

[R Script - Comparison and Changes between Two Periods](https://github.com/albarey33/Data_Analysis_R/blob/main/05%20Comparison%20and%20Changes%20between%20Two%20Periods.R)


## Rename the Periods as Previous and Current. Join both tables.

```R

dfMonth1 <- dfMonth1 %>% rename(Practice_Previous_Month = Practice_Name)
dfMonth2 <- dfMonth2 %>% rename(Practice_Current_Month = Practice_Name)

# Join using the common fields
comparison <- dfMonth1 %>% full_join(dfMonth2, #by=c('Medicaid.ID'), 
                    by = c('Medicaid.ID','Name','DOB','Age','Gender','Race'))
```

## Rename the Periods as Previous and Current. Join both tables.


## Add Fields to Identify groups 'New' or 'Unenrolled'

```R
comparison$Prev_Month <- ifelse(is.na(comparison$Practice_Previous_Month), 'NewPt', 'Practice')
comparison$Curr_Month <- ifelse(is.na(comparison$Practice_Current_Month), 'Unenrolled', 'Practice')

comparison %>% group_by(Prev_Month, Curr_Month) %>% tally()
```
## Resulting Table

The next recap is obtained from comparing these two periods: 

| Prev_Month | Curr_Month |    n |
|------------|------------|------|
| NewPt      | Practice   |  541 |
| Practice   | Practice   | 4459 |
| Practice   | Unenrolled |  534 |


## Comparison and Changes of Practices between Two Periods

For the patients that were in both periods, it is possible to identify who of them had a change of Practice, and group by combination of practices previous and current.

The following code will achieve this outcome, sorted by the most frequent combination at the top:

```R
comparison %>% 
  filter(Practice_Previous_Month != Practice_Current_Month) %>% 
  group_by(Practice_Previous_Month, Practice_Current_Month) %>% 
  tally() %>% arrange(desc(n)) %>% filter(n>0)

```

### Example Top Results by Groups of Changes

![05 Main Results](/images/DataProcess/05_Changes_of_Practices_per_Patient.PNG){: width=100% }<!--{: width="350" height="350" }-->
_<center>Result: Comparison of Differences</center>_

### Detail of the Main Combination of Changed Practices

![05 Results](/images/DataProcess/05_Result_Comparison.PNG){: width=100% }<!--{: width="350" height="350" }-->
_<center>Result: Comparison of Differences</center>_

<!--
## Detail of First Result. ![05 Detail](/images/DataProcess/05_Detail_First_Result.PNG){: width=100% }<!--{: width="350" height="350" }-->

__

End of Post

