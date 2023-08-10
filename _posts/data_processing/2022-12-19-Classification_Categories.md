---
title: "DP19 Classification Categories using Mutate"      # subtitle: "Description of R Scripts for data processing."
#author: AlBaRey          #layout: post
date: 2022-12-19 10:00:00 -0500
categories: [Data Processing]         # , R
tags: [R, Dplyr, Data Analysis]
# background: '/img/posts/01.jpg'
#pin: true
---

## Description

Use mutate to add helper columns, then group_by to get a recap of interaction classification by categories. This method is similar to the Excel pivot tables. 

## Link to the Complete Script in Github
[R Script - Classification Categories using mutate](https://github.com/albarey33/Data_Analysis_R/blob/main/19%20Classification%20Categories%20using%20mutate.R)


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




__

End of Post
