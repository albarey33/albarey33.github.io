---
title: "DP02 Stacking Excel with Mutate Cross"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-02 10:34:00 -0500
categories: [Data Processing]    # , Merging Tables
tags: [R, Dplyr, Stacking, Excel]
# background: '/img/posts/01.jpg'
#pin: true
---

<!-- Reviewed 2023/08/08 -->

## Description 

This post depicts situations when data is collected in separate fragments, such as different time periods, places, or other subsets, and needs to be consolidated into a unified table for comprehensive analysis. This process is also known as "stacking" tables. Combining these individual data parts is essential to perform in-depth data processing and obtain insights that encompass the entirety of the original files.

The script automates the stacking process, resulting in significant time savings and error prevention, particularly when managing a substantial number of files.

The provided R script begins by collecting Excel files with the same structure (fields) from a specific folder. The script also enhances data integrity by modifying column names and introducing a "PPL" column that indicates the period for each record. The process concludes by exporting the consolidated dataset for further utilization in a CSV format.


## Link to the Complete Script in Github
[R Script - Merge Excel Files with Mutate Across](https://github.com/albarey33/Data_Analysis_R/blob/main/02%20Merging%20Excel%20files%20with%20equal%20structure%20with%20mutate%20across.R)


## Merging Excel files with equal structure with mutate across

In some situations when the data is imported, and the fields contain no values, R mistakenly assumes that they are of 'logical' type instead of 'character', causing a conflict when merging the files. To avoid this, the 'where - across' function is used to check all the fields and convert any logical types into character types. The result is a vector containing all the types. 


## Create a vector with the field types.

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

## Function Definition

The vector of types is applied in the function.

```R

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





__

End of Post
