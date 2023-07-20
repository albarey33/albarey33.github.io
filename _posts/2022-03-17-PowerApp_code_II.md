---
title: "PowerApps - RECRUITMENT App 2. Code Part II"      # subtitle: "Description of R Scripts for data processing."
#author: Alejando BaRey          #layout: post
date: 2023-03-15 10:00:00 -0500
categories: [PowerApps, Code]
tags: [PowerApps, Code]
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


# Screen_5_SwitchRooms
  
---
## Global variables
  
Following variables have been created / or updated on this screen

## Screen_5_SwitchRooms As screen

### OnVisible


```
=// Screen: "Screen_5_SwitchRooms". 
// Powerapps gallery to Switch Rooms. Change room numbers. This screen is available only for supervisors.

Reset(TextInputSearchRoom3_1);
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));  //DateValue(DateTimeValue(CaseOpen_DT))>=Today()-5)
Refresh(PrincipalDataBase);
UpdateContext({_startgalldate: DatePicker1_4.SelectedDate, _endgalldate: DatePicker2_4.SelectedDate});
ClearCollect(colNoEmpty,  Filter(PrincipalDataBase, MotherName <> "empty", RoomOccupied = "N")); 
ClearCollect(col_Empty,  Filter(PrincipalDataBase,  MotherName = "empty"  && RoomOccupied = "Y")); 
UpdateContext({_selected_fromroom_gallery: false});    // To visualize selected icon Added 2023-03-08
UpdateContext({_selected_toroom_gallery:   false});    // To visualize selected icon Added 2023-03-08
UpdateContext({switchfromroomrecord: LookUp(PrincipalDataBase,ID=-1)});    //Clean Previously Saved Records
UpdateContext({switchtoroomrecord: LookUp(PrincipalDataBase,ID=-1)});      //Clean Previously Saved Records


// - END - END - END - END - END - END - END - END - END - END - END - END - END
//If(IsBlank(_sortcolumn),UpdateContext({_sortcolumn:"SortCaseReview"}));      // {_sortcolumn:"SortCaseReview"}
//UpdateContext({_newrequest: true  , requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=-1)}),
//ClearCollect(colFCDUR,PrincipalDataBaseUR); 
//ClearCollect(tBOTHDATA,Ungroup(Table({MyTables: colFCDALL},{MyTables: colFCDUR}), "MyTables"));
//Clear(colFCDALL); Clear(colFCDUR);
//If(IsBlank(_roomoccupiedfilter),UpdateContext({_roomoccupiedfilter:"All"}));
//If(IsBlank(_casestatusfilter),UpdateContext({_casestatusfilter:"All Status"}));
//    _now: Text(Now(),DateTimeFormat.ShortDateTime),
//  _roomonoffBefore:DCV_RoomOnOff.SelectedText.Value,
//    _msg_visible_turn_room_to_Off:false,
//   _change_eligibility_time:false,
//  UpdateContext({_casestatusfilter:ThisItem.Status})
//Refresh(PrincipalDataBaseUR);
//If(IsBlank(_eligibtype),UpdateContext({_eligibtype:"All"}));
//If(IsBlank(SortDescending1),UpdateContext({SortDescending1:false}));

```
## Button1Mover_2 As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 
// NOTE 01 REFRESH IN ONVISIBLE SO THIS WILL BE TEMPORARILY DISABLED IS ???????????????? CHECK IF NEEDED. DOES IT WORTH  ??????????? 

UpdateContext({switchfromroomrecord: LookUp(PrincipalDataBase,ID=ThisItem.ID)});
UpdateContext({_selected_fromroom_gallery: true});    // Added 2023-03-08

/*
//ResetForm(SelectedtoSwitchRoom_Form); NewForm(SelectedtoSwitchRoom_Form);    // NULLED 3/11/2023
//UpdateContext({_fromrecruitmentlist: true});
//UpdateContext({_newrequest: true});
//UpdateContext({_newformvisible:true});
//Reset([@tgl_CaseNotList]);
//If(IsBlankOrError(LookUp(FC_Mailings_Data, ID_FCDB=ThisItem.ID).ID),
    //NewForm(NewRequestForm); UpdateContext({_newrequest: true  , requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=-1)}),
    //EditForm(NewRequestForm); UpdateContext({_newrequest: false, requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=ThisItem.ID)})
    //); 
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase,   _databoth: "DB_1"}),
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
            )
        )
    )
*/
```
### Text


```
=Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))

//Upper(Left(LookUp(Users_on_Cases,ID_FCDB=ThisItem.ID).Title,4))
// Upper(Left(ThisItem.User_Email,4))  // CHANGED 2023-02-15
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Button1Mover_3DELETE As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 

UpdateContext({switchtoroomrecord: LookUp(PrincipalDataBase,ID=ThisItem.ID)});
UpdateContext({_selected_toroom_gallery: true});    // Added 2023-03-08
//ResetForm(SelectedtoSwitchRoom_Form); NewForm(SelectedtoSwitchRoom_Form);   // nulled 3/11/2023
//UpdateContext({_fromrecruitmentlist: true});
//UpdateContext({_newrequest: true});
//UpdateContext({_newformvisible:true});
//Reset([@tgl_CaseNotList]);
//If(IsBlankOrError(LookUp(FC_Mailings_Data, ID_FCDB=ThisItem.ID).ID),
    //NewForm(NewRequestForm); UpdateContext({_newrequest: true  , requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=-1)}),
    //EditForm(NewRequestForm); UpdateContext({_newrequest: false, requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=ThisItem.ID)})
    //); 

/*
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,/
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase,   _databoth: "DB_1"}),
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
            )
        )
    )
*/
```
### Text


```
=Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))

//Upper(Left(LookUp(Users_on_Cases,ID_FCDB=ThisItem.ID).Title,4))
// Upper(Left(ThisItem.User_Email,4))  // CHANGED 2023-02-15
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## ButtonMovetoCaseDetails_2 As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 
// NOTE 01 REFRESH IN ONVISIBLE SO THIS WILL BE TEMPORARILY DISABLED IS ???????????????? CHECK IF NEEDED. DOES IT WORTH  ??????????? 

UpdateContext({switchtoroomrecord: LookUp(PrincipalDataBase,ID=ThisItem.ID)});
UpdateContext({_selected_toroom_gallery: true});    // Added 2023-03-08

/*
// ResetForm(SelectedtoSwitchRoom_Form); NewForm(SelectedtoSwitchRoom_Form);   // nulled 3/11/2023
//UpdateContext({_fromrecruitmentlist: true});
//UpdateContext({_newrequest: true});
//UpdateContext({_newformvisible:true});
//Reset([@tgl_CaseNotList]);
//If(IsBlankOrError(LookUp(FC_Mailings_Data, ID_FCDB=ThisItem.ID).ID),
    //NewForm(NewRequestForm); UpdateContext({_newrequest: true  , requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=-1)}),
    //EditForm(NewRequestForm); UpdateContext({_newrequest: false, requestrecord: LookUp(FC_Mailings_Data,ID_FCDB=ThisItem.ID)})
    //); 
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase,   _databoth: "DB_1"}),
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
            )
        )
    )
*/
```
### Text


```
=Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))
//Upper(Left(LookUp(Filter(FirstN(FC_Recruitment_Notes_Log,100), In_Out=1),ID_FCDB=ThisItem.ID).UserEmail,4))
//Upper(Left(ThisItem.User_Email,4))   CHANGE 2023-02-15
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
//Upper(Left(ThisItem.UserEmail,4))
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).Title,4))&Text(DateTimeValue(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).Since),"- HH:mm")

```
## btn_SaveChanges As button

### OnSelect


```
=// BUTTON SAVE CHANGES

// CHANGE THE RECORD TO KEEP
Patch(PrincipalDataBase,LookUp(PrincipalDataBase,ID=switchfromroomrecord.ID),{Room: switchtoroomrecord.Room, RoomOccupied: "Y"}); //Changed 2023-03-08

// CHANGE THE RECORD TO CLOSE
Patch(PrincipalDataBase,LookUp(PrincipalDataBase,ID=switchtoroomrecord.ID),{RoomOccupied: "N"});                                  //Changed 2023-03-08

// CHANGES ON
Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "Switch_Rooms"&switchfromroomrecord.Room&switchfromroomrecord.RoomOccupied&"->"&switchtoroomrecord.Room&switchtoroomrecord.RoomOccupied, ID_FCDB: switchfromroomrecord.ID});

UpdateContext({switchfromroomrecord: LookUp(PrincipalDataBase,ID=-1)});    //Clean Previously Saved Records
UpdateContext({switchtoroomrecord: LookUp(PrincipalDataBase,ID=-1)});      //Clean Previously Saved Records

Navigate(Screen_0_Initial)

/* END - END - END - END - END - END - END - END - END
//switchfromroomrecord.Room&switchfromroomrecord.RoomOccupied    //switchtoroomrecord.Room&"Y"
//switchtoroomrecord.Room&switchtoroomrecord.RoomOccupied        //switchtoroomrecord.Room&"N"
If(switchfromroomrecord.ID in PrincipalDataBase.ID,
    Patch(PrincipalDataBase  ,LookUp(PrincipalDataBase,  ID=switchfromroomrecord.ID),{Room: switchtoroomrecord.Room, RoomOccupied: "Y"}), //Changed 2023-03-08
    Patch(PrincipalDataBaseUR,LookUp(PrincipalDataBaseUR,ID=switchfromroomrecord.ID),{Room: switchtoroomrecord.Room, RoomOccupied: "Y"}), //Changed 2023-03-08
);
    Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase,   _databoth: "DB_1"}),
    Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
*/

```
### Text


```
="Save"&Char(10)&"Changes"
```
## TextInputSearchRoom3_1 As text

# Screen_6_CasesTable
  
---
## Global variables
  
Following variables have been created / or updated on this screen
- \n    varCSVFile
- \n     varJSON_NewPts

## Screen_6_CasesTable As screen

### OnVisible


```
=// Screen: "Screen_6_CasesTable". 
// Lists all cases in a Spreadsheet style with the feature of sorting by any of the date fields. It contains a data table.

ClearCollect(tRoomOccupiedCT, Table({RoomOccupied:"All"},{RoomOccupied:"Y"},{RoomOccupied:"N"})); //tRoomOccupied
Refresh(PrincipalDataBase);
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

Reset(TextInputSearchNameMRN_1);
UpdateContext({_roomoccupiedD2: "Y"});
If(IsBlank(SortDescending2),UpdateContext({SortDescending2:true}));

// Code To Export JSON / CSV file
ClearCollect(TrackA, AddColumns(Filter(colFCData, RoomOccupied = "Y"),
            "tDOB_D", Text(DOB_D),
            "tDeliveryDate_D", Text(DeliveryDate_D),
            "tDCtoHomeDate_D", Text(DCtoHomeDate_D)
            ));

ClearCollect(TrackB,ShowColumns(TrackA,"MotherName","MomFirstName","MomMiddleName","tDOB_D","MRN","tDCtoHomeDate_D",
                                       "tDeliveryDate_D","BabysName","BabyLastName","BabyGender", "MethodofDelivery","BirthOutcome",
                                       "PreferredTelephoneContact","Address","CountyofResidence"
                                    ));

UpdateIf(TrackB, IsBlank(Address),                      { Address: "-" } );
UpdateIf(TrackB, IsBlank(PreferredTelephoneContact),    { PreferredTelephoneContact: "-" } );
UpdateIf(TrackB, IsBlank(CountyofResidence),            { CountyofResidence: "-" } );
UpdateIf(TrackB, IsBlank(tDOB_D),                       { tDOB_D: "-" } );
UpdateIf(TrackB, IsBlank(BirthOutcome),                 { BirthOutcome: "-" } );
UpdateIf(TrackB, IsBlank(MethodofDelivery),             { MethodofDelivery: "-" } );
UpdateIf(TrackB, IsBlank(BabyGender),                   { BabyGender: "-" } );
UpdateIf(TrackB, IsBlank(BabysName),                    { BabysName: "-" } );
UpdateIf(TrackB, IsBlank(BabyLastName),                 { BabyLastName: "-" } );
UpdateIf(TrackB, IsBlank(tDeliveryDate_D),              { tDeliveryDate_D: "-" } );
UpdateIf(TrackB, IsBlank(tDCtoHomeDate_D),              { tDCtoHomeDate_D: "-" } );
UpdateIf(TrackB, IsBlank(MRN),                          { MRN: "-" } );
UpdateIf(TrackB, IsBlank("tDOB_D"),                     { tDOB_D: "-" } );
UpdateIf(TrackB, IsBlank(MomMiddleName),                { MomMiddleName: "-" } );
UpdateIf(TrackB, IsBlank(MomFirstName),                 { MomFirstName: "-" } );


//ClearCollect(tBOTHDATA,Ungroup(Table({MyTables: colFCD}), "MyTables"));      //  ,{MyTables: colFCDUR}
```
## Gallery_SortCasesOptions As gallery.'BrowseLayout_Horizontal_TwoTextOneImageVariant_ver4.0'

### Items


```
=tsort
```
## Gallery_RoomOccupied As gallery.'BrowseLayout_Horizontal_TwoTextOneImageVariant_ver4.0'

### Items


```
=tRoomOccupiedCT
```
### OnSelect


```
=
```
## TextInputSearchNameMRN_1 As text

# Screen_7_CasesNotes
  
---
## Global variables
  
Following variables have been created / or updated on this screen

## Screen_7_CasesNotes As screen

### OnVisible


```
=// Screen: "Screen_7_CasesNotes". 
// Excel style list of cases with Recruitment and Supervisor Notes fields. Powerapps datatable.

ClearCollect(tRoomOccupiedCN, Table({RoomOccupied:"All"},{RoomOccupied:"Y"},{RoomOccupied:"N"})); //tRoomOccupied
Refresh(PrincipalDataBase);
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

Reset(TextInputSearchNameNotes);
UpdateContext({_roomoccupiedD: "Y"});
If(IsBlank(SortDescending3),UpdateContext({SortDescending3:true}));

// ClearCollect(tBOTHDATA,Ungroup(Table({MyTables: colFCD}), "MyTables"));    //  ,{MyTables: colFCDUR}
```
## GalleryCasesNotes As gallery.'BrowseLayout_Horizontal_TwoTextOneImageVariant_ver4.0'

### Items


```
=tRoomOccupiedCN
```
### OnSelect


```
=
```
## TextInputSearchNameNotes As text

## GallerySortOptionsNotes As gallery.'BrowseLayout_Horizontal_TwoTextOneImageVariant_ver4.0'

### Items


```
=tsort
```
# Screen_8_RecapsIntro
  
---
## Global variables
  
Following variables have been created / or updated on this screen
- trecaps

## Screen_8_RecapsIntro As screen

### OnVisible


```
=// Screen: "Screen_8_RecapsIntro". 
// Screen with Options to three screens with collected information. 

Refresh(PrincipalDataBase);
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

//UpdateContext({_countrowsFCD: CountIf(colFCD.MotherName,MotherName<>"empty")});
//UpdateContext({_countrowsFCDUR: CountIf(colFCDUR.MotherName,MotherName<>"empty")});
UpdateContext({_countrowsmaindata: CountRows(colFCData)});

UpdateContext({tnodelivered: Filter(colFCData,
                                   (ReasonIfnoteligible = "not delivered" || IsBlank(DeliveryDate_D)))});

UpdateContext({_countrowsnodelivered: CountRows(tnodelivered.MotherName)});

//CountIf(PrincipalDataBase,MotherName<>"empty",(ReasonIfnoteligible = "not delivered" || IsBlank(DeliveryDate_D)).MotherName)});

UpdateContext({trecaps:
	    Table(
        	{ScreenRec: "Tabulations",screenname: 'Screen_8.1_Tabulations'},
	        {ScreenRec: "Births by Counties",screenname: 'Screen_8.2_BirthsbyCounties'},
	        {ScreenRec: "Cases with Medicaid insurance",screenname: 'Screen_8.3_Medicaid_Months'}
	        )
	});

/*
Set(trecaps,
    Table(
        {ScreenRec: "Tabulations",screenname: 'Screen_5.1_Tabulations'},
        {ScreenRec: "Births by Counties",screenname: 'Screen_5.2_BirthsbyCounties'},
        {ScreenRec: "Cases with Medicaid insurance",screenname: 'Screen_5.3_Medicaid_Months'}
        )
    )
*/
```
## Screen_8.1_Tabulations
  
---
### Global variables
  
Following variables have been created / or updated on this screen

### 'Screen_8.1_Tabulations' As screen

#### OnVisible


```
=// Screen: "Screen_8.1_Tabulations". 
// Count of the number of cases by Date, grouped by four different criteria as in the next screens. Clicking on each number leads to a detailed list for either the specific date or the overall totals.  
// Cases without a delivery date are excluded of these tabulations.
//UpdateContext({_daterange: "By Case Open Dates"});   // added 2023-03-11
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

// FILTER MAIN DATA
ClearCollect(datanoemptynodeliv, Filter(colFCData, 
                                ReasonIfnoteligible <> "not delivered",
                                !IsBlank(DeliveryDate_D),
                                If(Options_Period_Filter.SelectedText.Value = "By Delivery Dates",
                (DeliveryDate_D >= DatePicker1_3.SelectedDate && DeliveryDate_D <= DatePicker2_3.SelectedDate),
                (DateValue(DateTimeValue(CaseOpen_DD)) >= DatePicker1_3.SelectedDate && DateValue(DateTimeValue(CaseOpen_DD)) <= DatePicker2_3.SelectedDate))
            ));

ClearCollect(collTabulations,RenameColumns(
    ShowColumns(datanoemptynodeliv, "ScheduledDT","Eligibility", "EligibilityDT", "FamilyagreedFCvisit", "AgreementDT", "HVScheduled", "MotherName","CaseOpen_DD","DeliveryDate_D"), // "Status", 
    "Eligibility", "EligInelig",
    "MotherName", "Name",
    "FamilyagreedFCvisit", "AgreementAnswer"));

//"EligibilityDT", "EligibilityDT","HVScheduled", "HVScheduled",,"AgreementDT", "AgreementDT"

// COLLECTION ELIGIBILITY
ClearCollect(tEligibility,DropColumns(AddColumns(
    ShowColumns(collTabulations,"EligibilityDT","EligInelig","Name"),   //,"Status"
                "Date", DateValue(DateTimeValue(EligibilityDT)),
                "EligibilityBlank",If(IsBlank(EligInelig),1,0),
                "Eligible",If(EligInelig="Eligible",1,0),
                "Ineligible",If(EligInelig="Ineligible",1,0)), "EligibilityDT", "EligInelig"));

// COLLECTION AGREEMENT
ClearCollect(tAgreement,DropColumns(AddColumns(ShowColumns(collTabulations,"AgreementDT","AgreementAnswer","Name"),      //,"Status"
                "Date", DateValue(DateTimeValue(AgreementDT)),
                "agreed",If(AgreementAnswer="agreed",1,0),
                "declined",If(AgreementAnswer="declined",1,0),
                "undecided",If(AgreementAnswer="undecided",1,0),
                "undetermined",If(AgreementAnswer="undetermined",1,0),
                "AgreementAnswerBlank",If(IsBlank(AgreementAnswer),1,0)  // Moving down
                ), "AgreementDT", "AgreementAnswer"));

// COLLECTION HVSCHEDULED
ClearCollect(tHVScheduled,DropColumns(AddColumns(ShowColumns(collTabulations,"ScheduledDT","HVScheduled","Name"),       //,"Status"
                "Date", DateValue(DateTimeValue(ScheduledDT)),
                "HVScheduledBlank",If(IsBlank(HVScheduled),1,0),
                "HVScheduledY",If(HVScheduled="Y",1,0),
                "HVScheduledN",If(HVScheduled="N",1,0)), "ScheduledDT","HVScheduled"));

// COLLECTION TOTALBIRTHS_BY_DELIVERYDATE
ClearCollect(tBirthsDD,DropColumns(AddColumns(ShowColumns(collTabulations, "DeliveryDate_D","Name"),      // "DeliveryDate_D","Name"),                   //,"Status"
                "Date", DeliveryDate_D,         //  If(IsBlank(DeliveryDate_D),"",DateValue(CaseOpen_DD)),
                "TotalBirthsDD",1
                ), "DeliveryDate_D"));

// COLLECTION TOTALBIRTHS_BY_CASEOPEN
ClearCollect(tTotalBirths,DropColumns(AddColumns(ShowColumns(collTabulations,"CaseOpen_DD","Name"),  
                "Date", DateValue(DateTimeValue(CaseOpen_DD)),
                "TotalBirths",1
                ), "CaseOpen_DD"));

ClearCollect(tFINAL,Ungroup(Table({MyTables: tEligibility},{MyTables: tAgreement},{MyTables: tHVScheduled},{MyTables: tBirthsDD},{MyTables: tTotalBirths}), "MyTables"));

ClearCollect(tTablebyDate,AddColumns(GroupBy(tFINAL,"Date","Records"),
                "Sum_Eligible",Sum(Records,Eligible),
                "Sum_Ineligible",Sum(Records,Ineligible),
                "Sum_EligibilityBlank",Sum(Records,EligibilityBlank),
                "Sum_agreed",Sum(Records,agreed),
                "Sum_declined",Sum(Records,declined),
                "Sum_undecided",Sum(Records,undecided),
                "Sum_undetermined",Sum(Records,undetermined),
                "Sum_AgreementAnswerBlank",Sum(Records,AgreementAnswerBlank),
                "Sum_HVScheduledY",Sum(Records,HVScheduledY),
                "Sum_HVScheduledN",Sum(Records,HVScheduledN),
                "Sum_HVScheduledBlank",Sum(Records,HVScheduledBlank),
                "Sum_TotalBirthsDD",Sum(Records,TotalBirthsDD),
                "Sum_TotalBirths",Sum(Records,TotalBirths)
                ))

```
## Screen_8.1.1_Detail_Eligibility
  
---
### Global variables
  
Following variables have been created / or updated on this screen

### 'Screen_8.1.1_Detail_Eligibility' As screen

#### OnVisible


```
=// Screen: "Screen_8.1.1_Detail_Eligibility".
// From 5.1_Tabulations. Detail by date of cases Eligible / Not Eligible

Refresh(PrincipalDataBase);
ClearCollect(colTabElig,  
        Filter(PrincipalDataBase, 
                MotherName <> "empty", 
                ReasonIfnoteligible<>"not delivered",
                //EligibilityDT <> "Null", 
                !IsBlank(DeliveryDate_D),
            If(_daterange = "By Delivery Dates",
            (DeliveryDate_D >= _tabulsincedate && DeliveryDate_D <= _tabuluntildate),
            (DateValue(DateTimeValue(CaseOpen_DD)) >= _tabulsincedate && DateValue(DateTimeValue(CaseOpen_DD)) <= _tabuluntildate)),
            If(_allrows, !IsBlank(DateValue(DateTimeValue(EligibilityDT))), DateValue(DateTimeValue(EligibilityDT))=_filterdate),
            Eligibility=_filtercolumn
                )); 


```
### btnMoveNextScrEligibility As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
ViewForm(FormComments);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})
// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false  

```
#### Text


```
=//Upper(Left(ThisItem.User_Email,4))
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Screen_8.1.2_Detail_Agreement
  
---
### Global variables
  
Following variables have been created / or updated on this screen

### 'Screen_8.1.2_Detail_Agreement' As screen

#### OnVisible


```
=// Screen: "Screen_8.1.2_Detail_Agreement".
// From 5.1_Tabulations. Detail by Agreement

Refresh(PrincipalDataBase);
ClearCollect(colAgree,  
        Filter(PrincipalDataBase, 
                MotherName <> "empty", 
                ReasonIfnoteligible<>"not delivered",
                //EligibilityDT <> "Null", 
                !IsBlank(DeliveryDate_D),
            If(_daterange = "By Delivery Dates",
            (DeliveryDate_D >= _tabulsincedate && DeliveryDate_D <= _tabuluntildate),
            (DateValue(DateTimeValue(CaseOpen_DD)) >= _tabulsincedate && DateValue(DateTimeValue(CaseOpen_DD)) <= _tabuluntildate)),
            If(_allrows, !IsBlank(DateValue(DateTimeValue(AgreementDT))), DateValue(DateTimeValue(AgreementDT))=_filterdate),
            FamilyagreedFCvisit=_filtercolumn
                )); 

```
### ButtonMoveNext As button

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
=//Upper(Left(ThisItem.User_Email,4))
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Screen_8.1.3_Detail_HV_Scheduled
  
---

### 'Screen_8.1.3_Detail_HV_Scheduled' As screen

#### OnVisible


```
=// Screen: "Screen_8.1.3_Detail_HV_Scheduled".
// From 5.1_Tabulations. Detail by HV Scheduled: Yes / No.

Refresh(PrincipalDataBase);
ClearCollect(colHVSched,  
        Filter(PrincipalDataBase, 
                MotherName <> "empty", 
                ReasonIfnoteligible<>"not delivered",
                //EligibilityDT <> "Null", 
                !IsBlank(DeliveryDate_D), 
            If(_daterange = "By Delivery Dates",
            (DeliveryDate_D >= _tabulsincedate && DeliveryDate_D <= _tabuluntildate),
            (DateValue(DateTimeValue(CaseOpen_DD)) >= _tabulsincedate && DateValue(DateTimeValue(CaseOpen_DD)) <= _tabuluntildate)),
            If(_allrows, !IsBlank(DateValue(DateTimeValue(ScheduledDT))), DateValue(DateTimeValue(ScheduledDT))=_filterdate),
            HVScheduled=_filtercolumn
                )); 

```
### Button_MoveNext_HVSched As button

#### OnSelect


```
=ViewForm(CaseDetailsForm_1);
EditForm(FormComments_1);
Navigate(Screen_3_CaseDetails_View,ScreenTransition.Fade,
{EditRecord:ThisItem})
// _msg_visible_turn_room_to_Off:false,
// _msg_visible_status_to_declined:false,
// _msg_visible_outofcatchment:false,
// _msg_visible_status_to_Ineligible:false

```
#### Text


```
=//Upper(Left(ThisItem.User_Email,4))
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Screen_8.1.4_Detail_TotalBirths
  
---

### 'Screen_8.1.4_Detail_TotalBirths' As screen

#### OnVisible


```
=// Screen: "Screen_8.1.4_Detail_TotalBirths".
// From 5.1_Tabulations. Detail Total Births by date.

Refresh(PrincipalDataBase);
ClearCollect(colTabulTotalBirths,  
        Filter(PrincipalDataBase, 
                MotherName <> "empty", 
                ReasonIfnoteligible<>"not delivered",
                //EligibilityDT <> "Null", 
                !IsBlank(DeliveryDate_D), 
            If(_daterange = "By Delivery Dates",
            (DeliveryDate_D >= _tabulsincedate && DeliveryDate_D <= _tabuluntildate),
            (DateValue(DateTimeValue(CaseOpen_DD)) >= _tabulsincedate && DateValue(DateTimeValue(CaseOpen_DD)) <= _tabuluntildate)),
            If(_allrows, 
                If(_filtercolumn = "OpenCases", 
                    !IsBlank(DateValue(DateTimeValue(CaseOpen_DD   ))), 
                    !IsBlank(DateValue(              DeliveryDate_D)) 
                    ), 
                    //DateValue(DateTimeValue(CaseOpen_DD   ))>=_tabulsincedate, 
                    //DateValue(              DeliveryDate_D) >=_tabulsincedate), 
                If(_filtercolumn = "OpenCases", 
                    DateValue(DateTimeValue(CaseOpen_DD   )) =_filterdate, 
                    DateValue(              DeliveryDate_D)  =_filterdate))
                    )
                ); 
```
### btnMoveNextScreenTotalBirths As button

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
=//Upper(Left(ThisItem.User_Email,4))
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## Screen_8.2_BirthsbyCounties
  
---
## Screen variables
  
Following variables have been created / or updated on this screen
- Array

### 'Screen_8.2_BirthsbyCounties' As screen

#### OnVisible


```
=// Screen: "Screen_8.2_BirthsbyCounties". 
// Count of births by Mother's county of residence.

Refresh(PrincipalDataBase);
ClearCollect(colCounties,  Filter(PrincipalDataBase, 
                                MotherName <> "empty",
                                ReasonIfnoteligible <> "not delivered"));

// FILTER MAIN DATA
ClearCollect(datacounties, Filter(colCounties,
                                DeliveryDate_D >= DatePicker1.SelectedDate,
                                DeliveryDate_D <= DatePicker2.SelectedDate
                                        ));

ClearCollect(datacounties2,AddColumns(ShowColumns(datacounties, "CountyofResidence","MotherName"),
                    "TotalBirths",1));

ClearCollect(datacounties3,AddColumns(GroupBy(datacounties2,"CountyofResidence","Records"),
                "Sum_Births",Sum(Records,TotalBirths)));

// CATCHMENT AREA
//ClearCollect(datacounties3CA,AddColumns(GroupBy(Filter(datacounties2,CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence = "Robeson"),"CountyofResidence","Records"), "Sum_Births",Sum(Records,TotalBirths)));

// OUT OF CATCHMENT AREA (OOC)
//ClearCollect(datacounties3OOC,AddColumns(GroupBy(Filter(datacounties2,Not(CountyofResidence ="Cumberland" || CountyofResidence ="Hoke" || CountyofResidence = "Robeson")),"CountyofResidence","Records"), "Sum_Births",Sum(Records,TotalBirths)));

ClearCollect(datacounties4,Sort(datacounties3,Sum_Births,SortOrder.Descending));

ClearCollect(datacounties5,AddColumns(datacounties4,"Percent",Round(Sum_Births/Count(datacounties2.TotalBirths)*100,1)));

UpdateContext({_countyfilter: Blank()})

// - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - 
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
```
### Button2 As button

#### OnSelect


```
=UpdateContext({_countyfilter:Blank()})

```
#### Text


```
="clear filter"
```

