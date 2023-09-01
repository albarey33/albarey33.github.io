---
title: "DP20 Getting Results from Sparse Matrix"      # subtitle: "Description of R Scripts for data processing."
# author: BABR #author: AlBaRey          #layout: post
date: 2022-12-20 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Melt - Dcast, ggplot, Dplyr]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

This script is designed to process a large matrix of periods and medical conditions, allowing for the export of summarized values to Power BI for dynamic visualization.

The script includes a function to convert the matrix of periods and conditions into a list format using reshape2::melt. Lastly, it utilizes Dcast or group_by operations to produce the final results.


## Link to the Complete Script in Github
[R Script - Recap of Sparse Matrix](https://github.com/albarey33/Data_Analysis_R/blob/main/20%20Recap%20of%20Sparse%20Matrix%20Melt%20Dcast%20Conditions%20vs%20Periods.R)


## Initial Source. View of the CSV file in Excel.

The following is a preview of the initial CSV data presented as an Excel spreadsheet:

![20 Data Source](/images/DataProcess/20_Enrollees_with_Medical_Conditions_Six_Months2.PNG){: width=100% }
_Matrix of Patients, Demographic info, Medical Conditions and Last Date of Visit._


## Function to Convert the matrix into a list

Use of data.table::melt reshaping tool.

All the medical conditions fields will be listed in a single column "variable". The variable "patients" is the total number of enrollees that will serve as the denominator when creating percentage parameters.

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

### Applying the fx and summarizing the results to data 
### "dtRECAP" and a group of medical conditions in vectorcond. 
results <- fx_melttables(vectorcond, dtRECAP)
```

### Melted Data: Head

The resulting data frame has 157,980 rows resulting from 7899 rows from data multiplied by 20 variables.

![20 Result of Melt Data](/images/DataProcess/20_Melted_Data_20_variables.PNG){: width=100% }
_<center>View of some of the 157,980 records.</center>_

## Melted Data: View of a random sample of 30 results. 

Most of the results have a value equal to zero representing the blank cells in the Excel view. 

![20 Melted Sample 30results](/images/DataProcess/20_Melted_Sample_30results.PNG){: width=100% }
_<center>View of some random sample 30 results.</center>_


## Aggregation of All Variables for Two Practices Example

Using reshape2::dcast

```R
# Aggregation with dplyr::dcast
reshape2::dcast(data = results, formula = variable ~ Practice_Name[c(1,4)], 
                value.var = "value", fun.aggregate = sum)
```

Result: Data Table - Recap of all variables - Two Practices example

![20 Example Line Chart](/images/DataProcess/20_Aggregation_Two_Practices.PNG){: width=100% }
_<center>Aggregation All Variables - Two Practices Example</center>_


## Aggregation for All variables - All Months

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
  geom_line(color="blue",linewidth=1) + 
  geom_point()+ geom_text(nudge_y = 5) + 
  ggtitle("Patients with Hypertesion - All Practices")

# y-axis limits
ln +  ylim(0,110)+
theme_light() + labs(x="Enrollment Month",y="Patients")
```
### Line chart: Patients with Hypertension
![20 Example Line Chart](/images/DataProcess/20_Line_Chart_Patients_with_Hypertension.PNG){: width=100% }
_<center>Number of Patients with Hypertension - All Practices.</center>_


## Aggregation All Variables - Months - Practices

This is the table to export that will serve as a source for a PowerBI dashboard:

```R
totaltable <- results %>% group_by(variable, Month, Practice_Name) %>%
  summarize(Enrollees = sum(value), .groups = 'drop')
print(totaltable[sample(3840,30),],n=30)
```

Results to export to PowerBI
![20 Results PowerBI](/images/DataProcess/20_Results_PowerBI.PNG){: width=100% }
_<center>All Variables - All Months - All Practices</center>_


## Data Mining Patients with specific conditions

The next shows the results looking for patients filtered by medical conditions, months, and practice. 

```R
# * 6.1 Data Mining List of Patients with Hypertension
results %>% filter(variable== "Hypertension", 
                   Month== "202105",
                   value == 1, 
                   Practice_Name == "HOKE PRIMARY CARE")
```

Results: ID of 8 patients 
![20 Results PowerBI](/images/DataProcess/20_Data_Mining_8_patients.PNG){: width=100% }
_<center>Detail of 8 patients</center>_



__

End of Post

