---
title: "DP15 Examples BarPlots ggplot"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-15 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, ggplot]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

In this post, there are charts that show variations in data using bar plots. The first one utilizes the bar plot from the base package, and finally, the ggplot barplot is presented. In the present example, the data corresponds to population enrollment, Emergency Department (ED) visits, Inpatient (IP) visits, and readmissions over the past 12 months.

## Link to the Complete Script in Github
[R Script - Examples BarPlots ggplot](https://github.com/albarey33/Data_Analysis_R/blob/main/15%20Examples%20BarPlots%20ggplot.R)

## Initial Data

![15 Initial Data](/images/DataProcess/15_Data_to_use_in_BarPlots.PNG){: width=100% }
_Initial Data: Recap of Members, ED Visits, IP Visits, Readmissions, by Months_


15_Data_BarPlots
## Bar Plot (base)
```R
barplot(recaptable$IPVs, 
        main="IP Visits in the Last 12 months",
        xlab = "PPL",
        ylab = "IP Visits", col = 'lightgray')
```
![15 Result](/images/DataProcess/15_BarPlot_base.PNG){: width=100% }
_Final result: Bar Plot base_

## Bar Plot with ggplot 
```R
# ggplot2 plot with modified x-axis labels
# ggplot: With x-axis labels
ggplot(recaptable, aes(PPL, IPVs)) +    
  geom_bar(stat = "identity", fill = "#990000") +
  theme(axis.text.x = element_text(angle = 90, size = 10)) + 
  ggtitle("IP Visits in the Last 12 months")

```

![15 Result](/images/DataProcess/15_BarPlot_ggplot.PNG){: width=100% }
_Final result: Bar Plot using ggplot_




__

End of Post
