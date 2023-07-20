---
title: "Data Processing with R (Part 1)"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2022-11-10 10:34:00 -0500
categories: [Data Processing, R]
tags: [R, Dplyr, Data Processing, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

Procedures to process data using R, necessary steps required before generating reports, recaps or to use as input for machine learning models. By the next cases, sometimes the data is taken from different sources, folders and files, sometimes the data is processed using a pattern on the name to include it as a field #<-- necessary for recap of info.-->

# 01 Merging Excel files with equal structure with inherits

<!--- #### Brief Description:  {: width="832" height="505" } -->

The purpose of this script is to import data on parts (whenever are limitations to get data) and integrate them into one CSV file. The script first create a list of the required Excel files names located in the source folder by using list.files and regex to define a pattern. Then a function that includes readxl::read_excel(filename) is defined to read the data inside the Excel files, creating tibbles (data tables). For convenience, the defined function also converts the logical fields to character using "inherits". Finally the tibbles are merged into one CSV using do.call bind_rows.

![Mergin Files](/images/DataProcess/01_Merging_Excel_Filespng.PNG){: width="832" height="505" }    
_Merging Excel Files_


### Code: [R Script - Merge Excel Files with Inherits](https://github.com/albarey33/Data_Analysis_R/blob/main/01%20Merging%20Excel%20files%20with%20equal%20structure%20with%20inherit.R)



___

# 02 Merging Excel files with equal structure with mutate across

Similar to the previous script but using where - accross instead of inherit to read and merge excel data files.

### Code: [R Script - Merge Excel Files with Mutate Across](https://github.com/albarey33/Data_Analysis_R/blob/main/02%20Merging%20Excel%20files%20with%20equal%20structure%20with%20mutate%20across.R)
___

# 03 Merging CSV files with equal structure

This script differs from the previous ones on the format of the source files. In this case the data source is in CSV format.

### Code: [R Script - Merge CSV files with equal structure](https://github.com/albarey33/Data_Analysis_R/blob/main/03%20Merging%20CSV%20files%20equal%20structure.R)

___

# 04 Merging CSV files different structure.R

This script is applied whenever there are two data sources in csv with different structure of fields. It contains a function that shows the fields that differs in names or is present in only one of the data sources. 

## Example: Two DataTables with different fields. Check before merge
![04 Two DataTables with different fields. Check before merge](/images/DataProcess/04_Two_DataTables_with_different_fields_check_before_merge.PNG){: width="500" height="505" }
_Two DataTables with different fields. Check before merge_

## Function: Matching two data tables with different named fields.
```R
fx_comparison_two_DFs_Fields <- function(df_x, df_y){
  LeftW <- data.frame(field=names(df_x),match=match(names(df_x), names(df_y)), "L in R")
  RightW <- data.frame(field=names(df_y),match=match(names(df_y), names(df_x)),"R in L")
  compardf <- LeftW %>% full_join(RightW, by= 'field') %>% filter(is.na(match.x) | is.na(match.x) )
  print(compardf)
}
```
_Matching two data tables with different named fields._


## Result of the comparison by using the script

![04 Comparison of Fields with Differences](/images/DataProcess/04_Comparison_Differences.PNG){: width="350" height="350" }
_Result: Comparison of Differences_

Using dplyr::rename is possible to match back the names so the fields can merge.


## Code: [R Script - Merge CSV files with different structure](https://github.com/albarey33/Data_Analysis_R/blob/main/04%20Merging%20CSV%20files%20different%20structure.R)

___

# 05 Comparison and Changes between Two Periods.R

In this example the script identifies patients present in one of the two periods and absent in the other, either because the patient was new or was unenrolled. With table and group_by is possible to tally the numbers and associate these with, for example, the practices they were linked to.

## Example Main Results by Groups of Changes.

![05 Main Results](/images/DataProcess/05_Changes_of_Practices_per_Patient.PNG){: width=100% }<!--{: width="350" height="350" }-->
_Result: Comparison of Differences_

## Result of the comparison.

![05 Results](/images/DataProcess/05_Result_Comparison.PNG){: width=100% }<!--{: width="350" height="350" }-->
_Result: Comparison of Differences_

## Detail of First Result.

![05 Detail](/images/DataProcess/05_Detail_First_Result.PNG){: width=100% }<!--{: width="350" height="350" }-->
_Result: Comparison of Differences_

## Code: [R Script - Comparison and Changes between Two Periods](https://github.com/albarey33/Data_Analysis_R/blob/main/05%20Comparison%20and%20Changes%20between%20Two%20Periods.R)

___

# 06 Change Values based on External Table - Similar to Match Index.R

Similar to Match Index in Excel, with this script data is taken from another table to apply to specific records using a common key on these two tables. In this example we update the names of practices which come with outdated names from the source. The new names are kept in a specific table, created inside the script (hardcoded). This is also known as imputation of values. In this script a specific function was coded with the function "match" (base).

## Detail of info Previous to Imputation.

![06 Pts per Practice before Inputation](/images/DataProcess/06_Pts_per_Practice_before_Imputation.PNG){: width=100% } <!--{: width="350" height="350" }-->
_Result: Comparison of Differences_


## Function for Imputation.
```R
fx_change_Practice_Names <- function(PCP_field_name, df, dfChanges){
  LocPCP <- match(PCP_field_name,names(df))
  for(i in 1:nrow(dfChanges)){
    print(paste0("Records with ",dfChanges[i,1],": ", df %>% 
                   filter(df[ ,LocPCP] == dfChanges[i,1]) %>% tally()))
    df[ ,LocPCP][df[ ,LocPCP] == dfChanges[i,1]] <- dfChanges[i,2]
    print(paste0("  -------->   CHANGED TO :", dfChanges[i,2]))
  }
  df
}
```

## Function executed. Change Practice names showing number of changed records.
![06 Function Executed](/images/DataProcess/06_Function_Executed_with_number_of_Records_changed.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Result: Function applied_

## Final Results. Practice with changed names and their records.
![06 Results](/images/DataProcess/06_Final_Results_only_Practices_w_applied_Imputation.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Result: Results After imputation_

## Code: [R Script - Change Values based on External Table](https://github.com/albarey33/Data_Analysis_R/blob/main/06%20Change%20Values%20based%20on%20External%20Table%20-%20Match%20Index.R)

___

# 07 Join two Tables for Classification by Type of Practices

Another example of relational data tables. This script shows also a method to imputate values, but in this case the table that contains the data to update, let's say new names, doesn't come from a table hardcoded in the same script, but it is imported. In this example we assign a classification of practices by type: Family, Pediatric, etc. The way to imputate the values in this script is using left_join.

## List of Patients and Practices. View of first 10 from a list of 3000.
![07 Results](/images/DataProcess/07_First_10_Pts_w_Practices_Previous.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_List of Patients and Practices_

## List of Practices and Type.
![07 Results](/images/DataProcess/07_First_10_Practices_with_Type.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_List of Practices and Type_

## Code: Left_join Two Tables. Common key: 'Practice.Name'
```R
PPL_df <- PPL_df %>% left_join(practices_type, 
              by = c('Practice_Name' = 'Practice.Name'))
```

## Final result. Recap of Patients by Type of Practice.

```R
PPL_df %>% dplyr::group_by(Practice.Type) %>% 
                        tally() %>% arrange(desc(n))
```
## Results.
![07 Results](/images/DataProcess/07_Final_Count_of_Patient_by_Type_of_Practice.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Recap of Patients by Practice Type_

## Code: [R Script - Join two Tables](https://github.com/albarey33/Data_Analysis_R/blob/main/07%20Join%20two%20Tables%20for%20Classification%20by%20Type%20of%20Practices.R)

___

# 08 Apply changes to Group of Fields using Regular Expressions

This script applies a regular expression defined in a function to convert currency values in a text with a dollar sign "$0,000" into numeric format.

![08 Results](/images/DataProcess/08_Structure_of_Imported_Data_Cost_Fields_as_Characters.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields as Characters_

## Function to Change the Format of Cost as character to numeric
Using Regular Expression to remove the $ signs and the commas. Then converting to numeric format.

```R
fx_convmoney <- function(x){as.numeric(gsub("[\\$,]", "", x))}
```
## Results
![08 Results](/images/DataProcess/08_Structure_of_Cost_Fields_after_the_Change_of_Format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Structure of Cost Fields after the Change of Format_

## Code: [R Script - Apply changes to Cost fields](https://github.com/albarey33/Data_Analysis_R/blob/main/08%20Apply%20changes%20to%20Cost%20fields.R)

___

# 09 Apply changes to Filtered Group of Records

For only the fields related to medical conditions in the data, the empty or N/A values are replaced by the value "No". The reason of this change is to use the "Yes"/"No" for a later presentation in a PowerBI chart. These changes are applied using dplyr functions mutate and case_when.

## Initial Values
![09 Input Format](/images/DataProcess/09_Medical_Conditional_original_format.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Original Structure of values for Medical Conditions_

```R
# Replace empty value "" or NA by "No"
PPL_df[,RangeConditions][PPL_df[,RangeConditions]==""] <- "No"
PPL_df[,RangeConditions][is.na(PPL_df[,RangeConditions])] <- "No"
```

Other method for multiple replacement. Example for only "Dual" field
```R
PPL_df %>% mutate(Dual = case_when(Dual == ""    ~ "No", 
                                   Dual == "YES" ~ "Yes"))
```

## Results
![09 Results](/images/DataProcess/09_Medical_Conditional_final_structure.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Final arranged values for Medical Conditions_


## Code: [R Script - Apply changes to Medical Condition fields](https://github.com/albarey33/Data_Analysis_R/blob/main/09%20Apply%20changes%20to%20Medical%20Condition%20fields.R)

___

# 10 Matrix Counter of Member Months using Melt DCast (equivalent to Pivot tables)

This code calculates the number of months each patient was enrolled in the 12 months previous to an specific months. For ilustration, this script shows a calculation for only the enrollment month and the previous one. So a patient can have one or two member-months. The data comes in just two fields: Period and MID (patient identifier). The functions melt and dcast (equivalent of pivot tables) are used to convert the data into a table MID vs Period.

## Initial Data
![10 Input Format](/images/DataProcess/10_Initial_Data_Simulated_Enrollment_in_Four_Periods.PNG){: width=100% }
_Initial Data Simulated Enrollment in Four Periods_

## Convert to Matrix Member ~ Period
```R
# 1 or 0 if the pt is enrolled in each Period
castedMIDPeriod <- data.table::dcast(melt(data.table(df), 
                           id.vars = c("MID", "Period"), 
                           measure.vars = c("Period")),
                           MID ~ Period, fun.agg = length)
```
![10 Matrix MbPeriod](/images/DataProcess/10_Matrix_Enrollment_Member_Periods.PNG){: width=100% }
_Matrix Enrollment Member Periods_

## Counter of Two Months
Fields of member-months are created per ending period. The code returns since the first period that have a previous one (202102) 
Code: Counter of Current + Previous month. 
```R
# Counter of Current + Previous month
for(i in 2:nrow(dfPeriod)){   
  #  i <- 12
  dfPeriodfincol <- dfPeriod$Period[i]
  dfPeriodfincol     # Name of Month example "201809"
  C3fincol <- match(dfPeriodfincol, names(castedMbMos))
  C3fincol        # Index of Month example "13"
  dfPeriodinicol <- dfPeriod$Period[i-1]
  dfPeriodinicol
  C3inicol <- match(dfPeriodinicol, names(castedMbMos))
  C3inicol
  castedMbMos[,C3inicol:C3fincol]
  castedMbMos <- castedMbMos %>% 
    mutate( !!paste0(dfPeriodfincol, 'MbMo') := 
              rowSums(castedMbMos[,C3inicol:C3fincol]))
}
```

## Final Result of Script
![10 Results](/images/DataProcess/10_Final_Results_Calculated_MemberMonths.PNG){: width=100% }   <!--# {: width="550" height="350" }-->
_Calculated Member Months_

## Code: [R Script - Count of Member Months Matrix Melt DCast](https://github.com/albarey33/Data_Analysis_R/blob/main/10%20Count%20of%20Member%20Months%20Matrix%20Melt%20DCast%20.R)

_____

## END OF POST
___


