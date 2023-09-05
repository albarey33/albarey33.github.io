---
title: "DP08 Apply changes to Group of Fields using RegEx"      # "Description of R Scripts for data processing."
# author: BABR #author: Alejando BaRey          #layout: post
date: 2022-12-08 10:34:00 -0500
categories: [Data Processing]             # , Other Procedures
tags: [R, Dplyr, RegEx]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

Financial data often arrives in a text-based format, with dollar signs and commas incorporated for ease of reading. However, numerical analyses and computations are more effective when the data is presented in numeric format.

This script focuses on addressing a common data transformation task involving currency values. It applies a regular expression (RegEx), defined within a dedicated function, to convert currency representations within a text (e.g., "$0,000") into a numeric format.


## Link to the Complete Script in Github
[R Script - Apply changes to Cost fields](https://github.com/albarey33/Data_Analysis_R/blob/main/08%20Apply%20changes%20to%20Cost%20fields.R)


## Original Data with Cost Fields as Characters 

The next graph shows the original data containing cost fields in the format "$0,000.00":

![08 Results](/images/DataProcess/08_Structure_of_Imported_Data_Cost_Fields_as_Characters.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields as Characters_

## Function to Change the Format of Cost as character to numeric

Using Regular Expression to remove the $ signs and the commas. Then converting to numeric format.

```R
fx_convmoney <- function(x){as.numeric(gsub("[\\$,]", "", x))}
```

## Transformed Data

The result of applying the script is the transformation of currency-based cost fields from their initial character format to a more analytically useful numeric representation.

![08 Results](/images/DataProcess/08_Structure_of_Cost_Fields_after_the_Change_of_Format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields after the Change of Format_

__

End of Post



