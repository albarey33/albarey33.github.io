---
title: "PowerApps - RECRUITMENT App 2. Code Part III"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2023-03-15 10:00:00 -0500
categories: [PowerApps, Recruitment App]
tags: [PowerApps Code]
# background: '/img/posts/01.jpg'
#pin: true
---

Contents
========

* [Screens_Diagram](#Screens_Diagram)
* [Screen_0_Initial](#screen_0_initial)
* [Screen_1_Rooms](#screen_1_rooms)
* [Screen_2_SupplemRooms](#screen_2_supplemrooms)
* [Screen_3_CaseDetails](#screen_3_casedetails)
* [Screen_4_CasesList](#screen_4_caseslist)
* [Screen_5_SwitchRooms](#screen_5_switchrooms)
* [Screen_6_CasesTable](#screen_6_casestable)
* [Screen_7_CasesNotes](#screen_7_casesnotes)
* [Screen_8_RecapsIntro](#screen_8_recapsintro)
    * [Screen_8.1_Tabulations](#screen_81_tabulations)
        * [Screen_8.1.1_Detail_Eligibility](#screen_811_detail_eligibility)
        * [Screen_8.1.2_Detail_Agreement](#screen_812_detail_agreement)
        * [Screen_8.1.3_Detail_HV_Scheduled](#screen_813_detail_hv_scheduled)
        * [Screen_8.1.4_Detail_TotalBirths](#screen_814_detail_totalbirths)
    * [Screen_8.2_BirthsbyCounties](#screen_82_birthsbycounties)
    * [Screen_8.3_Medicaid_Months](#screen_83_medicaid_months)
    * [Screen_8.4_Medicaid_Days](#screen_84_medicaid_days)
* [Screen_9_NotDelivered](#screen_9_notdelivered)
* [Screen_10_Verification](#screen_10_verification)
    * [Screen_10.1_NotEligible](#screen_101_noteligible)
    * [Screen_10.2_DeliveryDate_ScheduledHV](#screen_102_deliverydate_scheduledhv)
    * [Screen_10.3_NotAgreedNotEligible](#screen_103_notagreednoteligible)
    * [Screen_10.4_Names_Occupying_Rooms](#screen_104_names_occupying_rooms)
    * [Screen_10.5_DuplicatedMRNs](#screen_105_duplicatedmrns)




# SCREENS DIAGRAM

## Image: Screens Diagram
![Screen_Flow](/images/PowerApp_Recruitment/Recruitment_Screen_Flow.png){: width=100% }
_Screen_Flow_

# CODE_BY_SCREENS

# Screen_0_Initial

This home screen serves as the main landing page of the app, providing users with access to other screens.

---
## Global variables
  
Following variables have been created / or updated on this screen
- _undecidefillcolor
- _supervisoraccess
- _declinedfillcolor
- tsort
- ValidCounties
- _notreachfillcolor
- _acceptedfillcolor
- tCatchmentArea
- tsort3
- tRoomsIndex
- tEligibType
- _undetermfillcolor
- _troomindexSUPP
- _troomindexSUPP
- _itemselected
- _countrowsmaindata
- tFamAgreed
- _tTimeHourMinute
- Open_Closed_Cases
- ButtonCSS
- _countrowsnodelivered
- varAppVersion
- tsort2
- _countrowsEligible
- SortDescending1
- _sortcases
- tRoomOccupied
- myStyle
- tnodelivered
- _countrowsIneligible
- _casestatusfilter

## Screen_8.3_Medicaid_Months
  
---
### Global variables
  
Following variables have been created / or updated on this screen
- Array

### 'Screen_8.3_Medicaid_Months' As screen

#### OnVisible


```
=// Screen: "Screen_8.3_Medicaid_Months".
// Screen: Count of cases with medicaid as patient medical insurance. Recap by Months. The Mother of births are eligible and agree to home visit.

Refresh(PrincipalDataBase);
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

// FILTER MAIN DATA
ClearCollect(datamedicaid, Filter(colFCData, 
                                ReasonIfnoteligible <> "not delivered",
                                !IsBlank(DeliveryDate_D)));

ClearCollect(datamedicaid2,ShowColumns(datamedicaid, "CaseOpen_DD", "Primary_Insurance", "Secondary_Insurance","MRN","MotherName","MomFirstName","FamilyagreedFCvisit","Eligibility"));
Clear(datamedicaid);
ClearCollect(datamedicaid3,AddColumns(datamedicaid2,"Total_w_Medicaid",If(Find("Medicaid",Primary_Insurance)>0||Find("Medicaid",Secondary_Insurance)>0, 1, 0),
                                                      "YearMonth",Text(DateTimeValue(CaseOpen_DD),"yyyy")&"/"&Text(DateTimeValue(CaseOpen_DD),"mm")));
ClearCollect(datamedicaid5,AddColumns(datamedicaid3,"Eligible",If(Total_w_Medicaid = 1 && Eligibility = "Eligible",1,0)));
ClearCollect(datamedicaid6,AddColumns(datamedicaid5,"Agreed",If(Eligible = 1 && FamilyagreedFCvisit = "agreed",1,0)));
ClearCollect(datamedicaid7,AddColumns(GroupBy(datamedicaid6,"YearMonth","Records"), //"Total_Eligible",Sum(Records,Eligible),
                "Total_Cases",CountRows(Records.MotherName),
                "Sum_Total_w_Medicaid",Sum(Records,Total_w_Medicaid),
                "Total_Eligible",Sum(Records,Eligible),
                "Total_Agreed",Sum(Records,Agreed)));
Clear(datamedicaid2;datamedicaid3;datamedicaid5);
ClearCollect(datacounties8,AddColumns(datamedicaid7,"% Eligible of Total w/Medicaid",Round(Total_Eligible/Sum_Total_w_Medicaid*100,1),
                                                    "% Agreed of Eligibles",Round(Total_Agreed/Total_Eligible*100,1)));




/*

ClearCollect(datamedicaid2,ShowColumns(datamedicaid, "DeliveryDate_D","Primary_Insurance", "Secondary_Insurance","MRN","FamilyagreedFCvisit","Eligibility"));
ClearCollect(datamedicaid3,AddColumns(datamedicaid2,"Total_w_Medicaid",If(Find("Medicaid",Primary_Insurance)>0||Find("Medicaid",Secondary_Insurance)>0, 1, 0)));
ClearCollect(datamedicaid4,AddColumns(datamedicaid3,"YearMonth",Text(DateValue(DeliveryDate_D),"yyyy")&"/"&Text(DateValue(DeliveryDate_D),"mm")));
ClearCollect(datamedicaid5,AddColumns(datamedicaid4,"Eligible",If(Total_w_Medicaid = 1 && Eligibility = "Eligible",1,0)));
ClearCollect(datamedicaid6,AddColumns(datamedicaid5,"Agreed",If(Eligible = 1 && FamilyagreedFCvisit = "agreed",1,0)));
ClearCollect(datamedicaid7,AddColumns(GroupBy(datamedicaid6,"YearMonth","Records"), //"Total_Eligible",Sum(Records,Eligible),
                "Sum_Total_w_Medicaid",Sum(Records,Total_w_Medicaid),
                "Total_Eligible",Sum(Records,Eligible),
                "Total_Agreed",Sum(Records,Agreed)));

ClearCollect(datacounties8,AddColumns(datamedicaid7,"% Eligible of Total w/Medicaid",Round(Total_Eligible/Sum_Total_w_Medicaid*100,1),
                                                    "% Agreed of Eligibles",Round(Total_Agreed/Total_Eligible*100,1)));

//ClearCollect(datamedicaid4,AddColumns(datamedicaid3,"CO2",Text(DateValue(CaseOpen_DD),"yyyy")&"/"&Text(DateTimeValue(CaseOpen_DD),"mm")&"-"&Text(DateTimeValue(CaseOpen_DD),"mmmm")));

/*
ClearCollect(datamedicaid2,ShowColumns(datamedicaid, "CaseOpen_DD");
/*
ClearCollect(datamedicaid3,AddColumns(datamedicaid2, "Open_Month",DateTimeValue(CaseOpen_DD)+1));


/*


/*
//ClearCollect(datamedicaid2,AddColumns(ShowColumns(datamedicaid, "Primary_Insurance"),"wMD",If(Primary_Insurance="Medicaid", 1, 0)));
//ClearCollect(datamedicaid2,AddColumns(ShowColumns(datamedicaid, "Primary_Insurance"),"wMD",If(Find("Medicaid",Primary_Insurance)>0, 1, 0)));


ClearCollect(datamedicaid2,AddColumns(ShowColumns(datamedicaid2, "CountyofResidence","MotherName"),
                    "TotalBirths",1));

ClearCollect(datacounties3,AddColumns(GroupBy(datacounties2,"CountyofResidence","Records"),
                "Sum_Births",Sum(Records,TotalBirths)));

// CATCHMENT AREA
//ClearCollect(datacounties3CA,AddColumns(GroupBy(Filter(datacounties2,CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence = "Robeson"),"CountyofResidence","Records"), "Sum_Births",Sum(Records,TotalBirths)));

// OUT OF CATCHMENT AREA (OOC)
//ClearCollect(datacounties3OOC,AddColumns(GroupBy(Filter(datacounties2,Not(CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence = "Robeson")),"CountyofResidence","Records"), "Sum_Births",Sum(Records,TotalBirths)));

ClearCollect(datacounties4,Sort(datacounties3,Sum_Births,Descending));

ClearCollect(datacounties5,AddColumns(datacounties4,"Percent",Round(Sum_Births/Count(datacounties2.TotalBirths)*100,1)));

UpdateContext({_countyfilter: Blank()})


//ClearCollect(ThreeCounties,Set(Array,["Cumberland","Hoke","Robeson"]));
//ClearCollect(ThreeCounties,Set(Array,["Cumberland","Hoke","Robeson"]));

// ClearCollect(datacountiesA, Filter(datacounties, BabyGender = "M")); //DatePicker1.SelectedDate));
// ClearCollect(datacountiesA, Filter(datacounties, AgreementDT = "12/17/2021 12:26 PM")); //DatePicker1.SelectedDate));
// ClearCollect(datacountiesC, Filter(PrincipalDataBase,RoomOccupied="Y"));

//ClearCollect(datacounties4A,
//            If(_catchmentarea = "In_Area",Filter(datacounties4,CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence ="Robeson"),
//            If(_catchmentarea = "OOC",Filter(datacounties4,Not(CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence ="Robeson")),
//            datacounties4)));
//If(Filter(datacounties4,Not(CountyofResidence ="Cumberland")));
//  If(_catchmentarea = "In_Area",CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence ="Robeson"),

//ClearCollect(tFINAL,Ungroup(Table({MyTables: tEligibility},{MyTables: tAgreement},{MyTables: tHVScheduled},{MyTables: tTotalBirths}), "MyTables"));

// _countyfilter
*/
```
### Button_Goto_Details_MedicaidPts As button

#### OnSelect


```
=Navigate('Screen_8.4_Medicaid_Days', ScreenTransition.Fade)
```
#### Text


```
="Detail Medicaid Cases"
```
## Screen_8.4_Medicaid_Days
  
---
### Screen variables
  
Following variables have been created / or updated on this screen
- OptionsYearMonth
- tYearMonthBlank

### 'Screen_8.4_Medicaid_Days' As screen

#### OnVisible


```
=// Screen: "Screen_8.4_Medicaid_Days".
// Screen: Detail by days of the information showed in the screen Medicaid_Months: cases with medicaid as patient medical insurance, by dates.

Set(tYearMonthBlank,Table({Result:Blank()}));
Set(OptionsYearMonth,Ungroup(Table({Result: tYearMonthBlank.Result},{Result: ForAll(Distinct(Sort(datamedicaid6,DateTimeValue(CaseOpen_DD),SortOrder.Ascending).YearMonth,YearMonth), {Result: ThisRecord.Value})}),"Result"));
ClearCollect(datamedicaid6bydays,If(IsBlank(filterYearMonth.Selected.Result),datamedicaid6,Filter(datamedicaid6,YearMonth=filterYearMonth.Selected.Result)))
```
# Screen_9_NotDelivered
  
---

## Screen_9_NotDelivered As screen

### OnVisible


```
=// Screen: "Screen_9_NotDelivered".
// Lists records which don't have a delivery date recorded.
Reset(TextInputSearchRoom3_2);

UpdateContext({_nodelivSinceDate: DatePicker1_7.SelectedDate  }); 
UpdateContext({_nodelivUntilDate: DatePicker2_7.SelectedDate  }); 

Refresh(PrincipalDataBase);
ClearCollect(colNotDeliv,  Filter(PrincipalDataBase, 
                                MotherName <> "empty", 
                                ReasonIfnoteligible="not delivered" || IsBlank(DeliveryDate_D) || DeliveryDate_D > Today(),
                                DateValue(DateTimeValue(CaseOpen_DD)) >= _nodelivSinceDate,
                                DateValue(DateTimeValue(CaseOpen_DD)) <= _nodelivUntilDate 
                                )); 


```
## Button5_3 As button

### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})

// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false
```
### Text


```
=""
```
## TextInputSearchRoom3_2 As text

# Screen_10_Verification
  
---

## Screen_10_Verification As screen

### OnVisible

```
=// Screen: "Screen_10_Verification".
// Screen with options to check verify information, or fix if necessary.

UpdateContext({tverification:
    Table(
        {ScreenRec: "Ineligibles",screenname: 'Screen_10.1_NotEligible'},
        {ScreenRec: "Days HV after Delivery /        Eligible & Agreed",screenname: 'Screen_10.2_DeliveryDate_ScheduledHV'},
        {ScreenRec: "Other Not Eligible or no Agreed vs HV",screenname: 'Screen_10.3_NotAgreedNotEligible'},
        {ScreenRec: "Delete Names Occupying Rooms",screenname: 'Screen_10.4_Names_Occupying_Rooms'},
       // {ScreenRec: "Log of Updates",screenname: 'Screen_7.4_LogRecords'}
        {ScreenRec: "Duplicated MRN",screenname: 'Screen_10.5_DuplicatedMRNs'} //screenname: 'Screen_7.4_DuplicatedOpenRooms '},
       
        )}
    )
```
## Screen_10.1_NotEligible
  
---

### 'Screen_10.1_NotEligible' As screen

#### OnVisible


```
=// Screen: "Screen_10.1_NotEligible".
// From 7_Verification. List of all cases, either Not Delivered or Out of Catchment area.

UpdateContext({_inelgSinceDate: DatePicker1_1.SelectedDate  }); 
UpdateContext({_inelgUntilDate: DatePicker2_1.SelectedDate  }); 

Refresh(PrincipalDataBase);
ClearCollect(colInelig,  Filter(PrincipalDataBase, 
                                MotherName <> "empty", 
                                Eligibility="Ineligible",
                                DateValue(DateTimeValue(CaseOpen_DD)) >= _inelgSinceDate,
                                DateValue(DateTimeValue(CaseOpen_DD)) <= _inelgUntilDate 
                                )); 

```
### Button_MoveNext_Ineligibles As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})

// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false
```
#### Text


```
=// Upper(Left(ThisItem.User_Email,4))
```
## Screen_10.2_DeliveryDate_ScheduledHV
  
---

### 'Screen_10.2_DeliveryDate_ScheduledHV' As screen

#### OnVisible


```
=// Screen: "Screen_10.2_DeliveryDate_ScheduledHV".
// From 7_Verification. List of cases with a field of days between the scheduled Home Visit and the delivery date.

UpdateContext({_agreedSinceDate: DatePicker1_5.SelectedDate  }); 
UpdateContext({_agreedUntilDate: DatePicker2_5.SelectedDate  }); 

Refresh(PrincipalDataBase);
ClearCollect(colAgreedDays,  Filter(PrincipalDataBase, 
                                MotherName <> "empty",
                                ReasonIfnoteligible<>"not delivered",
                                Eligibility="Eligible",
                                DateValue(DateTimeValue(CaseOpen_DD)) >= _agreedSinceDate,
                                DateValue(DateTimeValue(CaseOpen_DD)) <= _agreedUntilDate, 
                                FamilyagreedFCvisit="agreed" 
                                                                ));
        
            

```
### ButtonMoveNext_DaysAfterDeliv As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})



// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false
```
#### Text


```
=""
```
## Screen_10.3_NotAgreedNotEligible
  
---

### 'Screen_10.3_NotAgreedNotEligible' As screen

#### OnVisible


```
=// Screen: NotAgreedNotEligible
// Screen: "Screen_10.3_NotAgreedNotEligible".
// From 7_Verification. Patients Ineligible or no Agreed.

UpdateContext({_notagreedSinceDate: DatePicker1_6.SelectedDate  }); 
UpdateContext({_notagreedUntilDate: DatePicker2_6.SelectedDate  }); 

Refresh(PrincipalDataBase);
ClearCollect(colNotAgreed,  Filter(PrincipalDataBase, 
                                MotherName <> "empty",
                                ReasonIfnoteligible<>"not delivered",
                                DateValue(DateTimeValue(CaseOpen_DD)) >= _notagreedSinceDate,
                                DateValue(DateTimeValue(CaseOpen_DD)) <= _notagreedUntilDate, 
                                Not(Eligibility="Eligible" && FamilyagreedFCvisit="agreed")
                                    )); 

```
### ButtonMoveNext_DaysNotAgree As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})

// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false
```
#### Text


```
=""
```
## Screen_10.4_Names_Occupying_Rooms
  
---

### 'Screen_10.4_Names_Occupying_Rooms' As screen

#### OnVisible


```
=// Screen: "Screen_10.4_Delete_Names_Occupying_Rooms".
// From 7_Verification. For the situation when a user leaves the computer while in a room, instead of saving the info, or using the icons “back” or “home”. This screen lets the supervisors delete the name linked to the room to open the access.

Refresh(PrincipalDataBase);
Refresh(In_Out_Data);
Refresh(FC_Recruitment_Notes_Log);

ClearCollect(collboth,Filter(PrincipalDataBase, ID in In_Out_Data.ID_FCDB));

//ClearCollect(collNotes1, FirstN(FC_Recruitment_Notes_Log,5) );

ClearCollect(collNotes1, Sort(DropColumns(AddColumns(ShowColumns(FC_Recruitment_Notes_Log, "Since", "UserEmail", "ID_FCDB", "ID", "Room", "RoomOccupied"),"cSinceDT", DateTimeValue(Since)), "Since"), ID, SortOrder.Descending));

// Keep only the latest record per ID_FCDB to be called by In_Out_Data
// Group collection by "ID_FCDB" and then take the latest since datetime (First cSinceDT)
ClearCollect(collNotes2, 
ForAll(
    GroupBy(collNotes1, "ID_FCDB", "_recs" ),
    Patch(First(SortByColumns(_recs, "cSinceDT", SortOrder.Descending)), {ID_FCDB: ID_FCDB})
));

//Reset(TextInputSearchRoom3_2);
```
### Button1Mover_1 As button

#### OnSelect


```
=false
/*
Refresh(PrincipalDataBase);
Refresh(FC_Recruitment_Notes_Log);
ClearCollect(collInRooms, ShowColumns(Filter(FC_Recruitment_Notes_Log,In_Out=1),"UserEmail","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).UserEmail,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord2:ThisItem})
        )
    )
*/
```
#### Text


```
="" 
//LookUp(collInRooms,ID_FCDB=ThisItem.ID_FCDB, In_Out=1).ID_FCDB
//Upper(Left(LookUp(Users_on_Cases,ID_FCDB=ThisItem.ID).Title,4))
// Upper(Left(ThisItem.User_Email,4))  // CHANGED 2023-02-15
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Screen_10.5_DuplicatedMRNs
  
---

### 'Screen_10.5_DuplicatedMRNs' As screen

#### OnVisible


```
=// Screen:  "Screen_10.5_DuplicatedMRNs"
// To check if two cases were opened for one patient.

ClearCollect(DuplicMRN,Filter(DropColumns(AddColumns(GroupBy(Filter(PrincipalDataBase, MotherName <> "empty"),"MRN", "ByMRN"),"CountMRNs",CountRows(ThisRecord.ByMRN)),"ByMRN"),CountMRNs>1));
//ClearCollect(DuplicMRN,Filter(DropColumns(AddColumns(GroupBy(Filter(PrincipalDataBase,RoomOccupied="Y"),"Room", "ByRoom"),"CountRooms",CountRows(ThisRecord.ByRoom)),//"ByRoom"),CountRooms>1));

```
### ButtonMoveNext_DaysNotAgree_1 As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})
```
#### Text


```
=""
```

