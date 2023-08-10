---
title: "DP16 Date type recap by time periods Week Day"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-16 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Lubridate, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

This script shows some applications of the lubridate package, to change the date formats, or to extract the year and months of the date values to summarize interactions. Also to add the weekdays or number of week on the year. 

## Link to the Complete Script in Github
[R Script - Date type recap by time periods](https://github.com/albarey33/Data_Analysis_R/blob/main/16%20Date%20type%20recap%20by%20time%20periods%20Week%20Day%20Lubridate%20.R)


## Taken randomly interaction dates from 10 records 

```R
sampledates <- dfINT[sample(1:9000,10),"Date.of.Interaction"]
asinteger   <- as.integer(sampledates) # As numeric
asdate      <- sampledates  # Already converted to date
ascharmdy   <- format(sampledates, format="%m/%d/%Y") # as character
asdatelubri <- lubridate::mdy(ascharmdy)     # back as date using lubridate

data.frame(asinteger, asdate, ascharmdy, asdatelubri)
str(data.frame(asinteger, asdate, ascharmdy, asdatelubri))
```

![16 Result](/images/DataProcess/16_Changes_Data_Formats.PNG){: width=100% }
_Data frame with changes in date formats and structure of this data frame_

## Recap per Year - Month using Tables

Recap year ~ month with Lubridate:

```R
# Recap year ~ month with Lubridate:
table(lubridate::year(dfINT$Date.of.Interaction),
      lubridate::month(dfINT$Date.of.Interaction), useNA = "ifany")
```

![16 tLubridate](/images/DataProcess/16_Table_Lubridate.PNG){: width=100% }
_Recap of number of Interactions per Year/Month as 2D Table_

```R
# Recap year ~ month using format Date:
table(paste(format.Date(dfINT$Date.of.Interaction, "%Y"),
            format.Date(dfINT$Date.of.Interaction, "%m"), sep="/"))
```

![16 format Date](/images/DataProcess/16_Table_format_Date.PNG){: width=100% }
_Array Recap of number of Interactions per Year/Month_



__

End of Post
