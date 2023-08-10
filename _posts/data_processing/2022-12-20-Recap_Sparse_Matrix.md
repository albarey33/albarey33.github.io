---
title: "DP20 Recap of Sparse Matrix Melt Dcast Conditions vs Periods"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-20 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

To process a big matrix of periods and medical conditions so to export the summarized values to Power BI and visualize them dinamically. The script also calculates the number of visits performed more than a year before (> 365 days), by reading the date of the last visit per patient. It contains a function to convert to a list the matrix periods - conditions using reshape2::melt. It uses dcast or group_by to get the final results. 

## Link to the Complete Script in Github
[R Script - Recap of Sparse Matrix](https://github.com/albarey33/Data_Analysis_R/blob/main/20%20Recap%20of%20Sparse%20Matrix%20Melt%20Dcast%20Conditions%20vs%20Periods.R)


## Source Data and Convert to a List format.

### Graph: Data Source. View of the CSV file in Excel.
![20 Data Source](/images/DataProcess/20_Enrollees_with_Medical_Conditions_Six_Months2.PNG){: width=100% }
_Matrix of Patients, Demographic info, Medical Conditions and Last Date of Visit._



### Use of Melt to Convert the matrix into a list
All the conditions fields will be listed into a single column "variable"
```R
# Function Melt Each Group of Conditions by Month and Practices
fx_melttables <- function(vectorconditions, dfdata){
  dffinal <- data.frame()
  dfdata <- data.table(dfdata)
  for(i in seq_along(vectorconditions)){
    print(vectorconditions[i])
    melted1 <- data.table::melt(dfdata, id.vars = c("MIDIndex", "Month", "Practice_Name"), 
                    measure.vars = vectorconditions[i]) 
    dffinal <- bind_rows(dffinal, melted1)
  }
  dffinal
}

### Applying the fx and summarizing the results to data "dtRECAP" and to group of medical conditions in vectorcond. 
results <- fx_melttables(vectorcond, dtRECAP)
```

### Graph: Melted Data
The resulting data frame has 157,980 rows resulting from 7899 rows from data multiplied by 20 variables.
![20 Result of Melt Data](/images/DataProcess/20_Melted_Data_20_variables.PNG){: width=100% }
_<center>View of some of the 157,980 records.</center>_


## Aggregation with dplyr::dcast: All Variables - Two Practices Example
```R
# Aggregation with dplyr::dcast: All Variables - Two Practices Example
reshape2::dcast(data = results, formula = variable ~ Practice_Name[c(1,4)], 
                value.var = "value", fun.aggregate = sum)
```
Result: Data Table - Recap of all variables - Two Practices example
![20 Example Line Chart](/images/DataProcess/20_Aggregation_Two_Practices.PNG){: width=100% }
_<center>Aggregation All Variables - Two Practices Example</center>_


## Aggregation: All variables - All Months
```R
# Aggregation: All variables - All Months
reshape2::dcast(data = results, formula = variable ~ Month, 
                value.var = "value", fun.aggregate = sum)
```

Result: Data Table - Recap of Patients per Month
![20 Example Line Chart](/images/DataProcess/20_Aggregation_Variables_Months.PNG){: width=100% }
_<center>All Variables - All variables ~ Months</center>_


## Line Plot Patients with Hypertension
This code shows a ggplot geom_line as an example of patients with hypertension by month.

```R
# LinePlot (ggplot2) for one Condition (Hypertension)
dfHypertension <- results %>% filter(variable=="Hypertension") %>% 
  group_by(Month, variable) %>% 
   summarize(ptsHTN = sum(value), .groups = 'drop')

# Basic Line
ln <- ggplot2::ggplot(data=dfHypertension, 
             aes(x=Month, y=ptsHTN, group=1, label=ptsHTN)) +
  geom_line(color="blue",size=1) + 
  geom_point()+ geom_text(nudge_y = 5) + 
  ggtitle("Patients with Hypertesion - All Practices")

# y-axis limits
ln +  ylim(0,110)+
theme_light() + labs(x="Enrollment Month",y="Patients")
```
## Line chart: Patients with Hypertension
![20 Example Line Chart](/images/DataProcess/20_Line_Chart_Patients_with_Hypertension.PNG){: width=100% }
_<center>Number of Patients with Hypertension - All Practices.</center>_


__

End of Post

