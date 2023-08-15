---
title: "DP17 Splitting and Summarizing String Values"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-17 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, strsplit]
# background: '/img/posts/01.jpg'
#pin: true
---


## Description

<!--Split strings of values into multiple fields and summarize.R-->

In the example, there is a "Reasons for Service" field that comprises the checked reasons for service each interaction had. Some are single reasons but most cases have grouped two or more reasons separated by commas. To measure the monthly impact of each reason individually, it is necessary to apply to split and summarize this data.

In order to split these values we can tally the total number of reasons for all the patients in a period, by using the base::strsplit() function. It splits the field into a list of lists. These lists are then converted into a sparse matrix to summarize them.


## Link to the Complete Script in Github
[R Script - Split string of values](https://github.com/albarey33/Data_Analysis_R/blob/main/17%20Split%20string%20of%20values%20into%20multiple%20fields%20and%20summarize.R)


## Initial Data for the Field: Reasons for Service

For a data frame with 9355 records, let's check the most frequent combination of "Reasons for Service", with the most frequent values at the top and listing the top 20. For that purpose, the next code is applied: 

```R
head(dfINT %>% group_by(Reason.s..for.Service) %>% tally %>% arrange(desc(n)),20)

```

### Table: Combination of Reasons. Top 20 most frequent.

| Reason.s..for.Service                                       |   n |
|------------------------------------------------------------|-----|
| Assessment,Education,Engagement,Other                      | 2505|
| Post Discharge Call / Visit                                | 1155|
| Care Coordination                                          | 1048|
| Engagement                                                |  505|
| Assessment,Education,Engagement                             |  320|
| Assessment                                                |  310|
| Assessment,Engagement                                      |  304|
| Assessment,COVID-19 Education,Education,Engagement          |  215|
| Other                                                     |  205|
| Assessment,Engagement,Post Discharge Call / Visit           |  197|
| Medication Management                                     |  171|
| Monitoring                                                |  150|
| Care Coordination,Education                                |  148|
| Assessment,Other,Post Discharge Call / Visit                |  110|
| Education,Engagement                                       |  101|
| Education,Engagement,Monitoring                             |   93|
| Assessment,COVID-19 Education,Education,Engagement,Other    |   77|
| Assessment,COVID-19 Education,Other,Post Discharge Call...  |   75|
| Engagement,Post Discharge Call / Visit                     |   70|
| Education                                                  |   51|


### Combination of Reasons in the R Console 

![17 format Date](/images/DataProcess/17_Data_Reasons_for_Service.PNG){: width=100% }
_Initial Data: Reasons for Service merged with commas_


## Add an ID Index column
dfRecapNoUTR$ID <- seq.int(nrow(dfRecapNoUTR))   

## Reasons for Service for the first five records

|   ID | Reasons                                |
|------|----------------------------------------|
|   1  | Assessment,Education,Engagement,Other   |
|   2  | Assessment,Other,Post Discharge Call / Visit |
|   3  | Post Discharge Call / Visit             |
|   4  | Assessment,Education,Engagement,Other   |
|   5  | Assessment,Education,Engagement,Other   |


## Getting List of Lists 

The next step involves splitting the string values into multiple columns using the strsplit function, using comma as separator.

```R
dat <- with(dfRecapNoUTR, strsplit(Reason.s..for.Service, ','))  # Result: List of Lists
```

## List of Lists for first five records

![17 format Date](/images/DataProcess/17_List_within_List_Five.PNG){: width=100% }
_Partial Results: List within List for first 5 records_


## Unlisting the values and relating them with the corresponding ID they belong to

The index ID is repeated for each reason in the record as many times as the number of reasons within the record:

```R
df2 <- data.frame(ID = rep(dfRecapNoUTR$ID, 
                           times = lengths(dat)),
                  Reason.s..for.Service = unlist(dat))
```

## Resulting Unlisted Reasons by ID

|  ID  | Reason.s..for.Service         |
|------|------------------------------|
|   1  | Assessment                   |
|   1  | Education                    |
|   1  | Engagement                   |
|   1  | Other                        |
|   2  | Assessment                   |
|   2  | Other                        |
|   2  | Post Discharge Call / Visit  |
|   3  | Post Discharge Call / Visit  |
|   4  | Assessment                   |
|   4  | Education                    |
|   4  | Engagement                   |
|   4  | Other                        |
|   5  | Assessment                   |
|   5  | Education                    |
|   5  | Engagement                   |
|   5  | Other                        |


## Create a table ID ~ Reasons

```R
# Sparse Matrix. Convert table to data frame
df3 <- as.data.frame(cbind(ID = dfRecapNoUTR$ID,
                   table(df2$ID, df2$Reason.s..for.Service)))  
head(df3)
```

## Resulting Sparse Matrix

![17 Sparse Matrix](/images/DataProcess/17_Result_Sparse_Matrix.PNG){: width=100% }
_Result: Sparse Matrix_

## Joining the sparse matrix to the main data and Recap per Year ~ Month


```R
dfRecapReasons <- dfRecapNoUTR %>% select("YearMonth", "ID") %>% 
  inner_join(df3, by = 'ID')
dfRecapReasons <- data.table(dfRecapReasons %>% 
                               group_by(YearMonth) %>% 
                               select(-c("ID")) %>% 
                               summarise_all(list(sum)))

```


![17 Summarized Values](/images/DataProcess/17_Result_Summarized.PNG){: width=100% }
_Result: Summarized values_


__

End of Post
