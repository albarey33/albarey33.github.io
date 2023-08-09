---
title: "02 Merging CSV Data Files using R"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-11-11 10:34:00 -0500
categories: [Data Processing]    # , Merging Tables
tags: [R, Dplyr, Merging Tables, CSV]         # Data Processing, 
# background: '/img/posts/01.jpg'
#pin: true
---

<!-- Reviewed 2023/08/08 -->
This post depicts situations when data is received in parts, for example by time period, users, practices. etc, and needs to be merged into a single table, stacking each one after the other. Merging data is crucial for comprehensive data processing and getting results that encompass all the original files individually.

<!-- folders and files, and are required to merge because of the way they are provided, by chunks of months, or by users and daily work, etc. So the data needs to be merged as one data table to be further processed. Sometimes the process incorporates the data file name  into the data as an added field, to identify the exact source of the record.-->

<!--This post explains procedures to process data using R, which is a necessary step required before generating reports, recaps or to use as input for machine learning models. -->

Contents
========

1. Merging CSV files with equal structure
2. Merging CSV files different structure


## 1. Merging CSV files with equal structure

This procedure uses the function _read.csv_ to merge the CSV files.

First the next function is developed. It takes three variables: 
* list_of_csv_files: List of file names to be imported.
* PPLini: Number of character where the period begins whithin the name.
* PPLfin: Number of character where the period finishes whithin the name.

```R

######### FUNCTION APPEND PPL CSV FILES  <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
fx_append_csvfiles <- function(list_of_csv_files, PPLini, PPLfin){
  mytypelist <- list() # Creation of empty list
  for(i in seq_along(list_of_csv_files)){
    df.list <- lapply(list_of_csv_files[i], read.csv)  ######### Convert list to Data frame 
    df.list[[1]]$PPL <- substr(list_of_csv_files[i], PPLini, PPLfin)
    print(paste(i," - Number of records in ",list_of_csv_files[i]," = ",
                nrow(df.list[[1]])," ; columns = ",length(df.list[[1]]),sep=""))
    mytypelist <- append(mytypelist, df.list)
  }
  mytypelist

}
```

Read the downloaded csv files using full path and regex: 

```R
filenames_list <- list.files(path= path, full.names=TRUE, 
                             pattern=paste0("^Patient.*?",currPPL,"_1000pts.csv"))
filenames_list

# Locate the data month on the name
locPPL <- regexpr(currPPL,filenames_list[1])[1]  # Locate pattern in string

# Apply function
tibbles <- fx_append_csvfiles(filenames_list, locPPL, locPPL+5)
```


### 1.1 Add Field with the Data Source File Name

Extract name from paths: basename

```R

for( i in seq_along(filenames_list)){
  tibbles[[i]]$FileName <- basename(filenames_list[i]) # list_files[i]
  }

```

### Code: [R Script - Merge CSV files with equal structure](https://github.com/albarey33/Data_Analysis_R/blob/main/03%20Merging%20CSV%20files%20equal%20structure.R)

___

## 2. Merging CSV files with different structure

This script is applied whenever there are two data sources in csv with different structure of fields. It contains a function that shows the fields that differs in names or is present in only one of the data sources. 

### Example: Two DataTables with different fields. Check before merge
![04 Two DataTables with different fields. Check before merge](/images/DataProcess/04_Two_DataTables_with_different_fields_check_before_merge.PNG){: width="500" height="505" }
_Two DataTables with different fields. Check before merge_

### Function: Matching two data tables with different named fields.
```R
fx_comparison_two_DFs_Fields <- function(df_x, df_y){
  LeftW <- data.frame(field=names(df_x),match=match(names(df_x), names(df_y)), "L in R")
  RightW <- data.frame(field=names(df_y),match=match(names(df_y), names(df_x)),"R in L")
  compardf <- LeftW %>% full_join(RightW, by= 'field') %>% filter(is.na(match.x) | is.na(match.x) )
  print(compardf)
}
```
_Matching two data tables with different named fields._


### Result of the comparison by using the script

![04 Comparison of Fields with Differences](/images/DataProcess/04_Comparison_Differences.PNG){: width="350" height="350" }
_Result: Comparison of Differences_

Using dplyr::rename is possible to match back the names so the fields can merge.


### Code: [R Script - Merge CSV files with different structure](https://github.com/albarey33/Data_Analysis_R/blob/main/04%20Merging%20CSV%20files%20different%20structure.R)



_____

END OF POST
___


