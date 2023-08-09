---
title: "Apply changes to Group of Fields using Regular Expressions"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-12-08 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

Procedures to process data using R, necessary steps required before generating reports, recaps or to use as input for machine learning models. By the next cases, sometimes the data is taken from different sources, folders and files, sometimes the data is processed using a pattern on the name to include it as a field #<-- necessary for recap of info.-->

___

## 08 Apply changes to Group of Fields using Regular Expressions

This script applies a regular expression defined in a function to convert currency values in a text with a dollar sign "$0,000" into numeric format.

![08 Results](/images/DataProcess/08_Structure_of_Imported_Data_Cost_Fields_as_Characters.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields as Characters_

### Function to Change the Format of Cost as character to numeric
Using Regular Expression to remove the $ signs and the commas. Then converting to numeric format.

```R
fx_convmoney <- function(x){as.numeric(gsub("[\\$,]", "", x))}
```
### Results
![08 Results](/images/DataProcess/08_Structure_of_Cost_Fields_after_the_Change_of_Format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields after the Change of Format_

### Code: [R Script - Apply changes to Cost fields](https://github.com/albarey33/Data_Analysis_R/blob/main/08%20Apply%20changes%20to%20Cost%20fields.R)

___
_____

### END OF POST
___


