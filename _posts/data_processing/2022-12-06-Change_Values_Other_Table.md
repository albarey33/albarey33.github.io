---
title: "DP06 Change Values based on External Table"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-06 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr, Imputation]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

There are situations when certain fields require updates using information gathered from another table. Similar to Excel's Match & Index feature, data is extracted from another table and applied to specific records through a common key shared by both tables. In this example, we update the names of practices that have outdated entries from the source. These new names are stored in a dedicated table, which is created within the script (hardcoded). This process is commonly referred to as value imputation. Within this script, a specific function has been coded utilizing the 'match' function from the base package.

## Link to the Complete Script in Github
[R Script - Change Values based on External Table](https://github.com/albarey33/Data_Analysis_R/blob/main/06%20Change%20Values%20based%20on%20External%20Table%20-%20Match%20Index.R)

## Table with the Names to Change

| OriginalName                                 | ChangedName                                     |
|-----------------------------------------------|-------------------------------------------------|
| CAPE FEAR FAMILY MEDICAL CARE 2344 WALTER REED | CAPE FEAR FAMILY MED CARE                       |
| CAPE FEAR FAMILY MEDICAL CARE 543 OWEN DR      | CAPE FEAR FAMILY MED CARE                       |
| CAPE FEAR FAMILY MEDICAL CARE 465 OWEN DR      | CAPE FEAR FAMILY MED CARE                       |
| CAROLINA RHEUMATOLOGY AND INTERNAL MEDICINE    | CAPE FEAR VALLEY PRIMARY CARE                   |
| FAYETTEVILLE GERIATRIC & INTERNAL MEDICINE     | CAROLINA PRIMARY & INTERNAL MEDICINE            |
| WADE FAMILY MEDICAL CENTER                     | WADE HEALTH SERVICES                            |

## Detail of info Previous to Imputation.

![06 Pts per Practice before Inputation](/images/DataProcess/06_Pts_per_Practice_before_Imputation.PNG){: width=100% } <!--{: width="350" height="350" }-->
_Result: Comparison of Differences_


## Function for Imputation.

For each value in the OriginalName, the function iterates through the DataFrame to find matching values and replaces each of these values with its corresponding ChangedName value.

```R
fx_change_Practice_Names <- function(PCP_field_name, df, dfChanges){
  LocPCP <- match(PCP_field_name,names(df))
  for(i in 1:nrow(dfChanges)){
    print(paste0("Records with ",dfChanges[i,1],": ", df %>% 
                   filter(df[ ,LocPCP] == dfChanges[i,1]) %>% tally()))
    df[ ,LocPCP][df[ ,LocPCP] == dfChanges[i,1]] <- dfChanges[i,2]
    print(paste0("  -------->   CHANGED TO :", dfChanges[i,2]))
  }
  df
}
```

## Function executed. Change Practice names showing number of changed records.
![06 Function Executed](/images/DataProcess/06_Function_Executed_with_number_of_Records_changed.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Result: Function applied_

## Final Results. Practice with changed names and their records.
![06 Results](/images/DataProcess/06_Final_Results_only_Practices_w_applied_Imputation.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Result: Results After imputation_



__

End of Post

