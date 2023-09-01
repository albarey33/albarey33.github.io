---
title: "DP11 Split One Column into Two Fields"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-11 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Splitting, StringR]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

This script is designed to split a combined field containing a pattern that is used to separate the information. In this example, the field 'Practice...NPI' is compounded. The primary objective is to separate the National Provider Identifier (NPI) from the practice name in each case. The str_split_fixed function from the stringr package is applied for that purpose.


## Link to the Complete Script in Github
[R Script - Split One Column into Two Fields](https://github.com/albarey33/Data_Analysis_R/blob/main/11%20Split%20One%20Column%20into%20Two%20Fields.R)


## Initial Data
![11 Input](/images/DataProcess/11_Initial_Data_with_the_field_required_to_split.PNG){: width=100% }
_Initial Data with the field required to split_

Table: 

| Medicaid.ID | Practice...NPI                              |
|-------------|--------------------------------------------|
| MID_10000   | CAPE FEAR VALLEY PRIMARY CARE - FAYETTEVILLE FAMILY: 1548704042-003 |
| MID_10001   | A BRIGHTER FUTURE HEALTHCARE: 1538491642-003 |
| MID_10002   | BIRTH AND WOMEN'S CARE: 1124089040-003       |
| MID_10003   | ALPHA MEDICAL CENTER: 1285719997-003         |
| MID_10004   | CAROLINA PEDIATRIC GROUP: 1467595959-003     |
| MID_10005   | CAROLINA PEDIATRIC GROUP: 1467595959-003     |
| MID_10006   | CAROLINA URGENT AND FAMILY CARE: 1467679712-003 |
| MID_10007   | BIRTH AND WOMEN'S CARE: 1124089040-003       |
| MID_10008   | A WOMAN'S PLACE IN FAYETTEVILLE: 1326006438-003 |
| MID_10009   | CAPE FEAR VALLEY PEDIATRICS: 1295738920-003   |


## Code to split 
Split into two fields using the separator ": "

```R
stringr::str_split_fixed(PPL_df$Practice...NPI, ": ", n = 2) 
```

The new dataframe is then appended to the original data frame

## Final Result
![11 Result](/images/DataProcess/11_Final_Result.PNG){: width=100% }
_Result with only the practice name_



__

End of Post
