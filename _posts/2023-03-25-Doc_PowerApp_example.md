---
title: "PowerApps - Example RECRUITMENT App  1. Documentation"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2023-03-25 10:00:00 -0500
categories: [PowerApps, Documentation]
tags: [PowerApps Documentation]
# background: '/img/posts/01.jpg'
#pin: true check
---

Contents
========

* [Introduction](#Introduction)
* [Process Daily Update of Patients Occupying Rooms](#process_Daily_Update_of_Patients_Occupying_Rooms)
* [Process Contact and Recruitment](#Process_Contact_and_Recruitment)
* [Feature_Access_Restriction_to_One_User_at_a_Time](#Feature_Access_Restriction_to_One_User_at_a_Time)
* [Tabulations and Recaps](#Tabulations_and_Recaps)
* [Quality Control](#Quality_Control)
* [Conclusion](#Conclusion)
* [Screens Diagram](#Screens_Diagram)


# Introduction

The PowerApps Recruitment application is a tool designed to collect information on new births for the purpose of scheduling a home visit by a certified nurse around three weeks after birth. This application was developed to streamline the process of scheduling home visits and to ensure that all information related to mothers of birth (MOB) is safely stored in a central location and properly updated.

# Process_Daily_Update_of_Patients_Occupying_Rooms

The app has an entry screen with a list of hospital rooms and the supervisors of the program use that screen to enter the screen with the case details with a form for each patient. At the beginning of the working day, the supervisor inputs the name, date of birth, and contact information of MOBs currently occupying rooms, taking that information from the hospital system. For each patient, a new record is created in the database which is a SharePoint list. The previous records on those rooms are automatically flagged as “no occupied”, but remain available to be edited using the screen “Cases List” to come into those cases.

## Image: Home Screen
![Home Screen](/images/PowerApp_Recruitment/HomeScreen.PNG){: width=100% }
_Home Screen_

## Image: Rooms Today
![Rooms Today](/images/PowerApp_Recruitment/Rooms_Today.PNG){: width=100% }
_Rooms Today_


# Process_Contact_and_Recruitment

Once that first process is completed by the supervisors, a team of recruiters contacts all MOBs during the day and registers the outcomes. Additional information is filled up: insurance, address, mom’s and baby’s health, and other fields about the birth process. The recruiters then schedule a home visit date and time or indicate if the MOB did not agree to a visit or if they were not contacted.

## Image: Form Details

![Form Details](/images/PowerApp_Recruitment/Form_Details.PNG){: width=100% }
_Form Details_


# Feature_Access_Restriction_to_One_User_at_a_Time

In order to prevent loss of information, the application restricts the case details screen use to one patient at the same time. To do so, whenever a user enters the case detail screen, a new record is registered in another SharePoint list named “In_Out_Data”. The user name on this record is listed in the rooms list screen so the other users can see that the specific room is currently underwork of that first user, and the entry to that room is meanwhile locked. When the user leaves the room, the record in the list “In_Out_Data” is deleted and the entry to that room is unlocked for other users.

## Image: Cases List

![Cases List](/images/PowerApp_Recruitment/CasesList.PNG){: width=100% }
_Cases List: Third record from the Top is locked by a user currently working on record._


# Tabulations_and_Recaps

The application has also tabulations screens that provide needed information about monthly recaps on the number of MOBs contacted, eligibility, how many agreed to a home visit, and other summary information, such as the number of MOBs per county or under Medicaid.

## Image: Tabulations

![Tabulations Main](/images/PowerApp_Recruitment/TabulationsMain.PNG){: width=100% }
_Tabulations: Recap of Patients by Outcomes._


# Quality_Control

Additionally, there are also screens that help to keep the quality of the information. Whenever the mothers change rooms, because they began to labor, or for other reasons, a special screen “Switch Rooms” is available for that purpose, with access restricted to supervisors only. That way, an already filled form can be reused again within the new room. There are other quality control screens that list two cases for the same patient, mothers without delivery, differences in days between scheduled home visit dates and delivery dates, etc.

## Image: Switch Rooms

![Switch Rooms Screen](/images/PowerApp_Recruitment/Switch_Room_0310.PNG){: width=100% }
_Switch Rooms Screen._


# Conclusion:

The Recruitment application in PowerApps is an effective tool for collecting information on new births and scheduling home visits by certified nurses. It provides a streamlined process for registering information, reducing the need for manual data entry as it was previously using a spreadsheet. With its user-thought interface and centralized database, it provides a streamlined process for tracking MOBs and their home visits.  Finally, the summary reports in real-time provide valuable insights about the operations that can be used to optimize them and measure the impact of this service on the community.


# Screens_Diagram

![Screen_Flow](/images/PowerApp_Recruitment/Recruitment_Screen_Flow.png){: width=100% }
_Screen_Flow_




