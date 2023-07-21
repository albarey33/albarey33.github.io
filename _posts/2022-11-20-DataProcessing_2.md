---
title: "Data Processing with R - Part 2"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-11-20 10:00:00 -0500
categories: [Data Processing, R]
tags: [R, Dplyr, Data Processing, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

Procedures to process data using R, necessary steps required before generating reports, recaps or to use as input for machine learning models. By the next cases, sometimes the data is taken from different sources, folders and files, sometimes the data is processed using a pattern on the name to include it as a field  #<-- necessary for recap of info.-->

___

# 11 Split One Column into Two Fields

Split one field with compounded data ('Practice...NPI') into two by using the function str_split_fixed (stringr) and a separator. In this example, per each practice it was required to separate the National Provider Identifier (NPI) from the name.

## Initial Data
![11 Input](/images/DataProcess/11_Initial_Data_with_the_field_required_to_split.PNG){: width=100% }
_Initial Data with the field required to split_

## Code to split 
Split into two fields using the separator ": "

```R
stringr::str_split_fixed(PPL_df$Practice...NPI, ": ", n = 2) 
```

The new data frame is then appended to the original data frame

## Final Result
![11 Result](/images/DataProcess/11_Final_Result.PNG){: width=100% }
_Result with only the practice name_

### Code: [R Script - Split One Column into Two Fields](https://github.com/albarey33/Data_Analysis_R/blob/main/11%20Split%20One%20Column%20into%20Two%20Fields.R)

___

# 12 Inputation using LeftJoin and Coalesce.R

Another example of imputation of missing values but in this case using the information located in the same data table, for the same MID but in other periods. This was used on demographic values as for example race, DOB, that we can assume don't change for the same individual. A table without missing values is created using dplyr::group_by. Then the original data is merged with dplyr::left_join(). Finally the replacing value is taken with coalesce.

## Initial data with missing values
![12 Result](/images/DataProcess/12_Initial_Data_with_Missing_info.PNG){: width=100% }
_Initial data with missing values_

## Code to create a new data frame by excluding the rows with missing (NA) values:
```R
df %>% dplyr::arrange(Period) %>% 
  filter(!is.na(Type)) %>% select(MID, Type) %>% 
  group_by(MID, Type) %>% tally() %>% 
  arrange(MID, desc(n)) %>% rename(Type_wo_NA = Type)
```

## Final result with the option to take value from the new table using coalesce
![12 Result](/images/DataProcess/12_Final_Result_Replacing_NA_values.PNG){: width=100% }
_Result of the Script_

## Code: [R Script - Inputation using LeftJoin and Coalesce](https://github.com/albarey33/Data_Analysis_R/blob/main/12%20Inputation%20using%20LeftJoin%20and%20Coalesce.R)

___

# 13 Match Replace Pattern ReGex.R

One example of application of regular expressions to change the date format.

## Initial data Date + Time
![13 Initial Date + Time](/images/DataProcess/13_Initial_Data_Regex.PNG){: width=100% }
_Initial data: Date + Time_

## Code with Regular expression. Take the characters before the first space
```R
gsub('([0-9]+) .*', '\\1', df$Int_Date)
```
## Final result: Only date
![13 Result](/images/DataProcess/13_Resulting_Data_Regex.PNG){: width=100% }
_Final results: Only date_

## Code: [R Script - Match Replace Pattern ReGex](https://github.com/albarey33/Data_Analysis_R/blob/main/13%20Match%20Replace%20Pattern%20ReGex.R)

___

# 14 Add Index column to data frame.R

An index column with unique values was created to use insted of the patients identifier MID or the name. It can be created either by using stringr::str_pad() or stringf (base). An index is necessary in some situations to replace and don't reveal the patient personal information.  

## Generated Index column
Forcing the number of digits to 7. The array length is calculated by the number of rows of the data frame this array will be attached to. 

```R
PPL_df$Index <- paste0('E',stringr::str_pad(1:10, 7, side="left", pad="0"))
```

## Final result
![14 Result](/images/DataProcess/14_Generated_Index_field_attached_to_dataframe.PNG){: width=100% }
_Final result: Attached generated Index field_

## Code: [R Script - Add Index column to data frame](https://github.com/albarey33/Data_Analysis_R/blob/main/14%20Add%20Index%20column%20to%20data%20frame.R)

___

# 15 Examples BarPlots ggplot.R

This charts show the changes in the population enrollment, the ED, IP Visits and Readmissions in the previous 12 months. There were used either barplots or ggplot, geom_bar to compare both.  

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


## Code: [R Script - Examples BarPlots ggplot](https://github.com/albarey33/Data_Analysis_R/blob/main/15%20Examples%20BarPlots%20ggplot.R)

___

# 16 Date type recap by time periods Week Day Lubridate.R

This script shows some applications of the lubridate package, to change the date formats, or to extract the year and months of the date values to summarize interactions. Also to add the weekdays or number of week on the year. 

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


## Code: [R Script - Date type recap by time periods](https://github.com/albarey33/Data_Analysis_R/blob/main/16%20Date%20type%20recap%20by%20time%20periods%20Week%20Day%20Lubridate%20.R)

___

# 17 Split string of values into multiple fields and summarize.R

In the example there is a field with the reasons for service each interaction had. The most cases have grouped by commas two or more reasons. In order to split this values so that we can tally the total number of reasons for all the patients in a period, by using: with(dfRecapNoUTR, strsplit(Reason.s..for.Service, ',')). It splits the field into a list of lists. These lists are then converted into a sparse matrix to summarize it.

## Split a String of Values into Multiple Columns per Value
Using comma as separator

## Initial Data
![17 format Date](/images/DataProcess/17_Data_Reasons_for_Service.PNG){: width=100% }
_Initial Data: Reasons for Service merged with commas_

```R
dat <- with(dfRecapNoUTR, strsplit(Reason.s..for.Service, ','))  # Result: Lists within lists
df2 <- data.frame(ID = factor(rep(dfRecapNoUTR$ID,               # ID ~ Reasons   
                                  times = lengths(dat)),
                              levels = dfRecapNoUTR$ID),
                  Reason.s..for.Service = unlist(dat))
# Sparse Matrix. Convert table to data frame
df3 <- as.data.frame(cbind(ID = dfRecapNoUTR$ID,
                   table(df2$ID, df2$Reason.s..for.Service)))  
head(df3)
```
## Graph
![17 Sparse Matrix](/images/DataProcess/17_Result_Sparse_Matrix.PNG){: width=100% }
_Result: Sparse Matrix_

## Graph
![17 Summarized Values](/images/DataProcess/17_Result_Summarized.PNG){: width=100% }
_Result: Summarized values_

## Code: [R Script - Split string of values](https://github.com/albarey33/Data_Analysis_R/blob/main/17%20Split%20string%20of%20values%20into%20multiple%20fields%20and%20summarize.R)

___

# 18 Merge multiple fields of labels into one.R

The purpose of this script is the inverse of the previous one. With multiple medical conditions in the data, with one field per condition, the script takes per each MID the conditions on which each patient is flagged as "Yes". The script take the names of those flags and merge all those fields using apply-collapse. Finally all the resulting consecutive commas are removed with a regular expression.

## Graph
![18 Initial Data](/images/DataProcess/18_SomeMedConditions_InitialData.PNG){: width=100% }
_Some Medical Conditions Initial Data_

## Defined function to convert the values "Yes" into the name of the condition
```R
# Defined function to convert the values "Yes" into the name of the condition
fxonlyfieldnamestochangevalues <- function(df){
  for(ii in seq_along(df)){
    df[,ii] <- ifelse(df[,ii]  == "Yes", names(df)[ii],"")
  }
  df
}
```

```R
# Code to merge the labels and remove commas
allconditions <- apply( dfclinicalconditions,1,paste,collapse = ",")
allconditions <- gsub("^,*|(?<=,),|,*$","", allconditions, perl=T)
allconditions <- gsub(",", ", ", allconditions)
# Add to the data frame as one column
dfclinicalconditions$onecolumn <- allconditions
```

## Graph
![18 Final Result](/images/DataProcess/18_SomeMedConditions_FinalResult.PNG){: width=100% }
_Some Medical Conditions Merged into one Field. Final Result_


## Code: [R Script - Merge multiple fields of labels](https://github.com/albarey33/Data_Analysis_R/blob/main/18%20Merge%20multiple%20fields%20of%20labels%20into%20one.R)

___

# 19 Classification Categories using mutate.R

Use mutate to add helper columns, then group_by to get a recap of interaction classification by categories. This method is similar to the Excel pivot tables. 

## Code: Add Column Interaction Classification based on other fields using dplyr::mutate and if_else.
```R
# Int_Classification
dfINT <- dfINT %>% 
  mutate(Interaction_Classification = 
     if_else(Outgoing.Contact.Result == "Unable to Reach (UTR)", 
            "Unable to Reach (UTR)",
     if_else(Prim.Part.PCI == "Yes" & Mode.PCI == "Yes",
            "Completed Pt. Centered Interaction",
     if_else(Prim.Part.PCI == "Yes" & !Mode %in% c('Fax', 'Mail'),
            "Other Completed Interactions w/Pt.",
     "Other Interactions"))))
```

## Result of Classification using Helper column
![19 Result of Data Mining](/images/DataProcess/19_Helper_Column_with_Dplyr_Mutate.PNG){: width=100% }
_All Interactions classified by the defined helper column Interaction_Classification._



## Data Mining: Patients that received more that 6 interactions.

```R
dfINT %>% filter(Interaction_Classification == "Completed Pt. Centered Interaction") %>% 
  group_by(Client) %>% tally() %>% arrange(desc(n)) %>% filter(n >= 6)
```

## Graph
![19 Result of Data Mining](/images/DataProcess/19_Data_Mining_Patients_who_Received_Most_Interactions.PNG){: width=100% }
_Code of Patients that received more that 6 interacions._


## Code: [R Script - Classification Categories using mutate](https://github.com/albarey33/Data_Analysis_R/blob/main/19%20Classification%20Categories%20using%20mutate.R)

___


# 20 Recap of Sparse Matrix Melt Dcast Conditions vs Periods.R

To process a big matrix of periods and medical conditions so to export the summarized values to Power BI and visualize them dinamically. The script also calculates the number of visits performed more than a year before (> 365 days), by reading the date of the last visit per patient. It contains a function to convert to a list the matrix periods - conditions using reshape2::melt. It uses dcast or group_by to get the final results. 

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
_View of some of the 157,980 records._


## Summarize Results

### Aggregation with dplyr::dcast: All Variables - Two Practices Example
```R
# Aggregation with dplyr::dcast: All Variables - Two Practices Example
reshape2::dcast(data = results, formula = variable ~ Practice_Name[c(1,4)], 
                value.var = "value", fun.aggregate = sum)
```
Result: Data Table - Recap of all variables - Two Practices example
![20 Example Line Chart](/images/DataProcess/20_Aggregation_Two_Practices.PNG){: width=100% }
_Aggregation All Variables - Two Practices Example_


### Aggregation: All variables - All Months
```R
# Aggregation: All variables - All Months
reshape2::dcast(data = results, formula = variable ~ Month, 
                value.var = "value", fun.aggregate = sum)
```

Result: Data Table - Recap of Patients per Month
![20 Example Line Chart](/images/DataProcess/20_Aggregation_Variables_Months.PNG){: width=100% }
_All Variables - All variables ~ Months_


### Line Plot Patients with Hypertension
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
### Line chart: Patients with Hypertension
![20 Example Line Chart](/images/DataProcess/20_Line_Chart_Patients_with_Hypertension.PNG){: width=100% }
_Number of Patients with Hypertension - All Practices._




### Code: [R Script - Recap of Sparse Matrix](https://github.com/albarey33/Data_Analysis_R/blob/main/20%20Recap%20of%20Sparse%20Matrix%20Melt%20Dcast%20Conditions%20vs%20Periods.R)


___

END OF POST
