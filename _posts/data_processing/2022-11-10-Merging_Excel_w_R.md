---
title: "01 Merging Excel Data Files using R"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-11-10 10:34:00 -0500
categories: [Data Processing]    # , Merging Tables
tags: [R, Dplyr, Merging Tables, Excel]
# background: '/img/posts/01.jpg'
#pin: true
---

<!-- Reviewed 2023/08/08 -->
This post depicts situations when data is received in parts, for example by time period, users, practices. etc, and needs to be merged into a single table, stacking each one after the other. Merging data is crucial for comprehensive data processing and getting results that encompass all the original files individually.

<!-- folders and files, and are required to merge because of the way they are provided, by chunks of months, or by users and daily work, etc. So the data needs to be merged as one data table to be further processed. Sometimes the process incorporates the data file name  into the data as an added field, to identify the exact source of the record.-->

<!--This post explains procedures to process data using R, which is a necessary step required before generating reports, recaps or to use as input for machine learning models. -->

Contents
========

1. Merging Excel files with equal structure with inherits 
    <!--//(#01-merging-excel-files-with-equal-structure-with-inherits)[link to part 1 ->](#part-1)-->
2. Merging Excel files with equal structure with mutate across

## 1. Merging Excel files with equal structure with inherits

<!--- #### Brief Description:  {: width="832" height="505" } -->

The purpose of this script is to import data on parts (whenever are limitations to get data) and integrate them into one CSV file. 

![Mergin Files](/images/DataProcess/01_Merging_Excel_Filespng.PNG){: width="832" height="505" }    
<center>_Merging Excel Files_</center>


The script first create a list of the required Excel files names located in the source folder by using list.files and regex to define a pattern. 

```R
filenames_list <- list.files(path= path, full.names=TRUE, 
                 pattern=paste0("^Patient.*?",currPPL,"_1000pts.xlsx"))
```

Then a function that includes readxl::read_excel(filename) is defined to read the data inside the Excel files, creating tibbles (data tables). For convenience, the defined function also converts the logical type fields to character type using "inherits". 

```R
# * 2.1 Function: Read Excel files with equal structure --------

fx_readfiles <- function(filename){   
  print(paste("Merging",filename,sep = " "))
  xlfile <- readxl::read_excel(filename)#, col_types = vectortypes)
  print(paste("Number of records in ",
              filename," = ",nrow(xlfile),
              " ; columns = ",length(xlfile),sep=""))
  xlfile[] <- lapply(xlfile, function(x) {    # Change field types
    if (inherits(x, "logical")) as.character(x) else x
  })
  dfXLfile <- data.frame(xlfile)
  dfXLfile
  }
```

Apply the defined function to the list containing the file names: 

```R
# Apply defined function to list
tibbles <- lapply(filenames_list,fx_readfiles)
```

Finally the tibbles are merged into one CSV using do.call bind_rows.

```R
PPL_df <- data.frame(do.call(dplyr::bind_rows, tibbles))
```

### Script with Excel files: [R Script - Merge Excel Files with Inherits](https://github.com/albarey33/Data_Analysis_R/blob/main/01%20Merging%20Excel%20files%20with%20equal%20structure%20with%20inherit.R)


___

## 2. Merging Excel files with equal structure with mutate across

In some situations when the data is imported, and the fields contain no values, R mistakenly assumes that they are of 'logical' type instead of 'character', causing a conflict when merging the files. To avoid this, the 'where - across' function is used to check all the fields and convert any logical types into character types. The result is a vector containing all the types. 

```R
vectortypes <- read_excel(filenames_list[1]) %>% 
  mutate(across(where(is.logical), as.character)) %>%   # as.character
  summarise_all(class) %>% slice(1) %>% unlist(., use.names=FALSE)
vectortypes
# Convert character and logical to text
vectortypes[vectortypes %in% c("character", "logical")] <- "text"
#vectortypes[vectortypes == "character" | vectortypes == "logical"] <- "text"  # Option
vectortypes
```
Finally the vector of types is used when reading the Excel files.

```R

# * 2.2 Function: Read Excel files with equal structure --------

fx_readfiles <- function(filename){
  print(paste("Merging",filename,sep = " "))
  xlfile <- readxl::read_excel(filename, col_types = vectortypes)
  print(paste("Number of records in ",
              filename," = ",nrow(xlfile),
              " ; columns = ",length(xlfile),sep=""))
  dfXLfile <- data.frame(xlfile)
  dfXLfile 
  }

```


### Script with Excel files: [R Script - Merge Excel Files with Mutate Across](https://github.com/albarey33/Data_Analysis_R/blob/main/02%20Merging%20Excel%20files%20with%20equal%20structure%20with%20mutate%20across.R)
___


_____

END OF POST
___


