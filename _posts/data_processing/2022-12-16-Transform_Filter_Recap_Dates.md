---
title: "DP16 Transform, Filter and Recap of Date Fields"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-16 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Lubridate, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

<!--  PENDING ADD MORE INFORMATION ABOUT FILTERING, WORKING WITH WEEKDAYS, WEEKNUMS, as it is in the Script -->

## Description

The present post shows a practical applications of the lubridate package, focusing on manipulating date formats, extracting year and month information from date values, and summarizing interaction dates within specific time periods like weeks and weekdays. The post also covers the addition of weekdays and week numbers to enhance the temporal context of the data.


## Link to the Complete Script in Github
[R Script - Date type recap by time periods](https://github.com/albarey33/Data_Analysis_R/blob/main/16%20Date%20type%20recap%20by%20time%20periods%20Week%20Day%20Lubridate%20.R)



## Initial dataframe with POSIXct data formats

The next screenshot shows the structure of the initially uploaded data.

![16 Result](/images/DataProcess/16_Structure_Head_Initial_Data.PNG){: width=100% }
_Structure and Head Initial Data_


## Batch transformation of all POSIXct date format fields to standard date

The next function is convenient to convert all columns of date and time data with class POSIXct to the standard date format using the lubridate::as_date() function. 

```R

dfINT[] <- lapply(dfINT, function(x) {
  if (inherits(x, "POSIXt")) lubridate::as_date(x) else x
})


```

![16 Result](/images/DataProcess/16_Structure_Head_Transformed_Data.PNG){: width=100% }
_Structure and Head Transformed Date Fields_



## Random Sampling: 10 Records

Out of a dataset comprising 9,000 records of interaction dates, a random sample of 10 records was chosen.


```R
sampledates <- dfINT[sample(1:9000,10),"Date.of.Interaction"]
asinteger   <- as.integer(sampledates) # As numeric
asdate      <- sampledates  # Already converted to date POSIXct format
ascharmdy   <- format(sampledates, format="%m/%d/%Y") # as character
asdatelubri <- lubridate::mdy(ascharmdy)     # back as date using lubridate

data.frame(asinteger, asdate, ascharmdy, asdatelubri)
str(data.frame(asinteger, asdate, ascharmdy, asdatelubri))
```

![16 Result](/images/DataProcess/16_Changes_Data_Formats.PNG){: width=100% }
_Data frame with changes in date formats and structure of this data frame_



## Recap as a table Years vs Months 

Recap year ~ month for interactions for the period July 2020 - June 2021 - with Lubridate:

```R
# Recap year ~ month with Lubridate:
table(lubridate::year(dfINT$Date.of.Interaction),
      lubridate::month(dfINT$Date.of.Interaction), useNA = "ifany")
```

![16 tLubridate](/images/DataProcess/16_Table_Lubridate.PNG){: width=100% }
_Recap of number of Interactions per Year/Month as 2D Table_


## Recap year/month using format Date:

```R
## Recap year ~ month using format Date:
table(paste(format.Date(dfINT$Date.of.Interaction, "%Y"),
            format.Date(dfINT$Date.of.Interaction, "%m"), sep="/"))
```

![16 format Date](/images/DataProcess/16_Table_format_Date.PNG){: width=100% }
_Array Recap of number of Interactions per Year/Month_



__

End of Post
