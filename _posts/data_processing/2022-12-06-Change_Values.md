---
title: "DP06 Change Values based on External Table"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-06 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

Similar to Match Index in Excel, with this script data is taken from another table to apply to specific records using a common key on these two tables. In this example we update the names of practices which come with outdated names from the source. The new names are kept in a specific table, created inside the script (hardcoded). This is also known as imputation of values. In this script a specific function was coded with the function "match" (base).

## Link to the Complete Script in Github
[R Script - Change Values based on External Table](https://github.com/albarey33/Data_Analysis_R/blob/main/06%20Change%20Values%20based%20on%20External%20Table%20-%20Match%20Index.R)



## Detail of info Previous to Imputation.

![06 Pts per Practice before Inputation](/images/DataProcess/06_Pts_per_Practice_before_Imputation.PNG){: width=100% } <!--{: width="350" height="350" }-->
_Result: Comparison of Differences_


## Function for Imputation.
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

