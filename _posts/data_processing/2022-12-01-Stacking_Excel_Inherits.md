---
title: "DP01 Stacking Excel files with Inherits"      # subtitle: "Description of R Scripts for data processing."
# author: A01 #author: AlBaRey          #layout: post
date: 2022-12-01 10:34:00 -0500
categories: [Data Processing]    # , Merging Tables
tags: [R, Dplyr, Stacking, RegEx, Excel]
# background: '/img/posts/01.jpg'
#pin: true
mermaid: true
---

<!-- Reviewed 2023/08/08 -->

## Description 

This post depicts situations when data is collected in separate fragments, such as different time periods, places, or other subsets, and needs to be consolidated into a unified table for comprehensive analysis. This process is also known as "stacking" tables. Combining these individual data parts is essential to perform in-depth data processing and obtain insights that encompass the entirety of the original files.

The script automates the stacking process, resulting in significant time savings and error prevention, particularly when managing a substantial number of files.

The provided R script begins by collecting Excel files with the same structure (fields) from a specific folder. The script also enhances data integrity by modifying column names and introducing a "PPL" column that indicates the period for each record. The process concludes by exporting the consolidated dataset for further utilization in a CSV format.

## Link to the Complete Script in Github

[R Script - Stacking Excel Files with Inherits](https://github.com/albarey33/Data_Analysis_R/blob/main/01%20Merging%20Excel%20files%20with%20equal%20structure%20with%20inherit.R)



<!--- #### Brief Description:  {: width="832" height="505" } -->

## Graphical Description of the Process

```mermaid

flowchart LR
Excel_File_01-- Stacking -->id1((R_Script))
Excel_File_02-- Stacking -->id1((R_Script))
Excel_File_03-- Stacking -->id1((R_Script))
Excel_File_N-- Stacking  -->id1((R_Script))
id1((R_Script))-->Resulting_DataFrame_CSV_file

```



## Import files from source folder

The script first create a list of the required Excel files names located in the source folder by using list.files and regular expression (RegEx) to define a pattern. 

```R
filenames_list <- list.files(path= path, full.names=TRUE, 
                 pattern=paste0("^Patient.*?",currPPL,"_1000pts.xlsx"))   # pattern using RegEx
```

## Merging Excel files with equal structure with Inherits

In certain situations, when data is imported and fields have no values, R may mistakenly assume them to be of 'logical' type instead of 'character', causing conflicts during stacking. To circumvent this, for convenience, the defined function employs the 'inherits' function to inspect all fields and convert any logical types to character types. The outcome is a vector containing all the types.

## Function Definition

A function that includes readxl::read_excel(filename) is defined to read the data inside the Excel files, creating tibbles (data tables).  

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

## Apply the defined function to the list containing the file names

```R
# Apply defined function to list
tibbles <- lapply(filenames_list,fx_readfiles)
```

## Concatenate the tibbles
Finally the tibbles are stacked into one CSV using do.call bind_rows.

```R
PPL_df <- data.frame(do.call(dplyr::bind_rows, tibbles))
```


__

End of Post


