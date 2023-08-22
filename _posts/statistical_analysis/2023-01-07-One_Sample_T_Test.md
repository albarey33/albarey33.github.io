---
title: "SA01 One Sample T-Test"  # subtitle: "Methods to Verify Hipothesis about samples vs populationsDescription of R Scripts for data processing."
# author: BABR
date: 2023-01-07 10:34:00 -0500
categories: [Statistical Analysis]            # , R
tags: [R, T-Test]          # layout: post
# background: '/img/posts/01.jpg'
---

A T-Test is a statistical hypothesis test used to determine if the mean of a sample is significantly different from a hypothesized population mean. The test assesses whether there is enough evidence in the sample data to support the claim that the true population mean differs from the hypothesized value.

This post demonstrates the application of a T-Test statistical analysis to evaluate the results of three recruiters who contact mothers of newborn babies to obtain their agreement for a future follow-up. The evaluation focuses on the proportion of patients agreeing to the follow-up (home visit) for each recruiter. This evaluation specifically targets eligible patients living within the coverage area.

## SA01 Statistical Analysis Small Samples T-Test.R

[R Script - One Sample T-Test](https://github.com/albarey33/Statistical_Analysis_R/blob/main/SA01%20One_Sample_T-Test%20FC_Recruitment.R)




<!--- #### Brief Description: --->



## Image: Last 20 cases of Eligibles with Contacts

The next is a view of the last 20 registered records from a 5114 records data:

![Screen_Flow](/images/Statistical/Tail_20cases.PNG){: width=100% }
_Last 20 cases Eligibles with Contacts_

## Transformation of CaseOpen_DD date field

This particular data has a particular issue that is not consistent in the time registered. Part of the data includes seconds, and the older part is registered only until minutes. So in order to create a mirror field of that without mistakes, the next code is applied:

```R
# In some records the format includes seconds 
df$CaseOpen_DDCHECK <- coalesce(
  as.POSIXct(df$CaseOpen, format = "%m/%d/%Y %I:%M:%S %p", tz = "EST"),
  as.POSIXct(df$CaseOpen, format = "%m/%d/%Y %I:%M %p", tz = "EST")
)
df

# Create field year - week
df$yearweek <- paste0(format.Date(df$CaseOpen_DDCHECK, "%Y"), "/", 
                  sprintf("%02d", week(df$CaseOpen_DDCHECK)))
df$yearweek[is.na(df$yearweek)]

```

The next is the obtained recap for all the records: 

![Total_Recap](/images/Statistical/Total_Recap.PNG){: width=100% }
_Total_Recap_

From a total of 5103 records, 4244 have an answer "agreed", which represents the 83.16%. This will be the population average.

___

## Calculation of Agreement per Recruiter

First, the percentages per week are calculated per recruiter with the next defined function: 

```R
fx_convertdfCM <- function(data, CM){
  # Filter CM
  data <- data %>% filter(UserEmail == CM)
  resultCM <- dcast(data, yearweek + UserEmail ~ FamilyagreedFCvisit, value.var = "n", fill = 0)
  resultCM <- resultCM %>% mutate(Total = agreed + declined + undecided + undetermined  )
  resultCM <- resultCM %>% mutate(PercentAgreed = agreed / Total )
  resultCM <- resultCM[resultCM$Total>4,] # eliminating outliers
  resultCM
}
```

Which returns the next data: 

### Results Recruiter 1: 

![Total_Recap](/images/Statistical/ResultsCM01.PNG){: width=100% }
_Results Recruiter 1_

### Results Recruiter 2: 

![Total_Recap](/images/Statistical/ResultsCM02.PNG){: width=100% }
_Results Recruiter 2_

### Results Recruiter 3: 

![Total_Recap](/images/Statistical/ResultsCM03.PNG){: width=100% }
_Results Recruiter 3_


## HYPOTHESIS TESTING - NULL HYPOTHESES: INDIVIDUAL RESULTS ARE NOT LESS THAN THE HISTORICAL AVERAGE

The Null hypothesis stated in this case is that the results showed by the recruiters aren't significantly lower than the historical average previously calculated. Confidence Level: 95%. Significance Level: 0.05

In order to evaluate that the t-test function in R is used: 

```R
t.test(dfCM01$PercentAgreed, mu = mu_value, 
       alternative = "less", conf.level = 0.95, alpha = 0.05)
t.test(dfCM02$PercentAgreed, mu = mu_value, 
       alternative = "less", conf.level = 0.95, alpha = 0.05)
t.test(dfCM03$PercentAgreed, mu = mu_value, 
       alternative = "less", conf.level = 0.95, alpha = 0.05)

```

## T-Test Results and Conclusions

![TTest_Results](/images/Statistical/TTest_Results.PNG){: width=100% }
TTest_Results

### Recruitment 1: 
The p-value (0.5268) is greater than the significance level (α = 0.05), suggesting that <b>there is not enough evidence to reject the null hypothesis.</b> The null hypothesis is that the true mean of the population is greater than or equal to 0.8316676. The confidence interval ( -Inf, 0.8652297 ) also includes values greater than 0.8316676. Therefore, we don't have evidence to conclude that the true mean is less than 0.8316676.

### Recruitment 2:
The p-value (0.1428) is greater than the significance level (α = 0.05), indicating that <b>there is not enough evidence to reject the null hypothesis.</b> The null hypothesis is that the true mean of the population is greater than or equal to 0.8316676. The confidence interval ( -Inf, 0.8527055 ) includes values greater than 0.8316676. Therefore, we don't have evidence to conclude that the true mean is less than 0.8316676.

### Recruitment 3:
The p-value (0.01543) is less than the significance level (α = 0.05), indicating that <b>we have evidence to reject the null hypothesis.</b> The null hypothesis is that the true mean of the population is greater than or equal to 0.8316676. The confidence interval ( -Inf, 0.8142884 ) also includes values less than 0.8316676. Therefore, we have evidence to conclude that the true mean is less than 0.8316676.



___

End of Post

