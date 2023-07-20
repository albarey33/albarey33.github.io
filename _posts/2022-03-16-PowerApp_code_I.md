---
title: "PowerApps - RECRUITMENT App 2. Code Part I"      # subtitle: "Description of R Scripts for data processing."
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

## Screen_0_Initial As screen

### OnVisible

```
=// Screen: "Screen_0_Initial". 


Set(varAppVersion, "03/13/2023 06:30 PM");
UpdateContext({_visiblemsgversion:false});
UpdateContext({tscreensgallery1:
    Table(
        {Screen: "ROOMS SUPP TODAY",screenname: Screen_1_Rooms},
        {Screen: "ROOMS SUPPLEMENTAL BIRTHS",screenname: Screen_2_SupplemRooms},
        {Screen: "CASES LIST",screenname: Screen_4_CasesList},
        {Screen: "SWITCH ROOMS",screenname: Screen_5_SwitchRooms}  ///    Screen_2.5_SwitchRooms}
        )});

UpdateContext({tscreensgallery2:
    Table(
        {Screen: "CASES - TABLE",screenname: Screen_6_CasesTable},
        {Screen: "CASES - NOTES",screenname: Screen_7_CasesNotes},
        {Screen: "RECAPS",screenname: Screen_8_RecapsIntro},
        {Screen: "NOT DELIVERED",screenname: Screen_9_NotDelivered},
        {Screen: "VERIFICATION",screenname: Screen_10_Verification},
        {Screen: "DOCUMENTATION",screenname: Screen_11_Documentation}
        )});

Set(tsort,Table({sortvar: "Room"},{sortvar: "Case Open"}, {sortvar: "Case Review"}, {sortvar: "HV Scheduled"} )); //, {sortvar: "DC to Home"}, {sortvar: "Delivery Date"}{sortvar: "Next Contact"}, 
                
// Supervisor access
Set(_supervisoraccess,
If(
//User().Email="user1@cnaccc.com" || 
Lower(User().Email)="user2@cnaccc.com" || 
Lower(User().Email)="user3@cnaccc.com" || 
Lower(User().Email)="user4@cnaccc.com",true, false));


// Color Global Variables
Set(_acceptedfillcolor, RGBA(129, 199, 156, 1));
Set(_undecidefillcolor, RGBA(255, 255, 196, 1));
Set(_undetermfillcolor, RGBA(255, 235, 161, 1));
Set(_declinedfillcolor, RGBA(255, 230, 230, 1));
Set(_notreachfillcolor, RGBA(210, 210, 210, 1));

//Set(SortDescending1, true);

If(varAppVersion <> LookUp(
         'Power Apps Versions',
         Title = "Recruitment_App",
         VersionNumber
     ),
     UpdateContext({_visiblemsgversion:true}),
     UpdateContext({_visiblemsgversion:false})
 );

Set(_troomindexSUPP, Table(             // previous colSUPProoms
    {room:"8414", index:1}, {room:"8415", index:2}, {room:"8416", index:3}, {room:"8417", index:4},
    {room:"8418", index:5}, {room:"8419", index:6}, {room:"8420", index:7}, {room:"8421", index:8},
    {room:"8422", index:9}, {room:"8423", index:10},{room:"8424", index:11},{room:"8425", index:12},
    {room:"8426", index:13},{room:"8427", index:14},{room:"8428", index:15},{room:"8429", index:16},
    {room:"8430", index:17},{room:"8431", index:18},{room:"8432", index:19},{room:"8433", index:20},
    {room:"8434", index:21},{room:"8435", index:22},{room:"8436", index:23},{room:"8437", index:24},
    {room:"8438", index:25},{room:"8439", index:26},{room:"8440", index:27},{room:"8441", index:28},
    {room:"8442", index:29},{room:"8443", index:30},{room:"8444", index:31},{room:"8445", index:32},
    {room:"8446", index:33},{room:"8447", index:34},{room:"8448", index:35},{room:"8413", index:36}));

Set(_troomindexSUPP, Table(                 // previous colSUPPBrooms
    {room:"L&D1", index:37}, {room:"L&D2", index:38}, {room:"L&D3", index:39}, {room:"L&D4", index:40},
    {room:"L&D5", index:41}, {room:"L&D6", index:42}, {room:"L&D7", index:43}, {room:"L&D8", index:44},
    {room:"w401", index:45}, {room:"w402", index:46}, {room:"w403", index:47}, {room:"w404", index:48},
    {room:"w405", index:49}, {room:"w406", index:50}, {room:"w407", index:51}, {room:"w408", index:52},
    {room:"w409", index:53}, {room:"w410", index:54}, {room:"w411", index:55}, {room:"w412", index:56},
    {room:"oth1", index:57}, {room:"oth2", index:58}, {room:"oth3", index:59}, {room:"oth4", index:60}));

Set(_tTimeHourMinute, Table(
{ time: Blank()   , hour:  Blank(), minute: Blank()}, 
{ time: "08:00 AM", hour:  8, minute:  0 }, 
{ time: "08:15 AM", hour:  8, minute: 15 }, 
{ time: "08:30 AM", hour:  8, minute: 30 }, 
{ time: "08:45 AM", hour:  8, minute: 45 }, 
{ time: "09:00 AM", hour:  9, minute:  0 }, 
{ time: "09:15 AM", hour:  9, minute: 15 }, 
{ time: "09:30 AM", hour:  9, minute: 30 }, 
{ time: "09:45 AM", hour:  9, minute: 45 }, 
{ time: "10:00 AM", hour: 10, minute:  0 }, 
{ time: "10:15 AM", hour: 10, minute: 15 }, 
{ time: "10:30 AM", hour: 10, minute: 30 }, 
{ time: "10:45 AM", hour: 10, minute: 45 }, 
{ time: "11:00 AM", hour: 11, minute:  0 }, 
{ time: "11:15 AM", hour: 11, minute: 15 }, 
{ time: "11:30 AM", hour: 11, minute: 30 }, 
{ time: "11:45 AM", hour: 11, minute: 45 }, 
{ time: "12:00 PM", hour: 12, minute:  0 }, 
{ time: "12:15 PM", hour: 12, minute: 15 }, 
{ time: "12:30 PM", hour: 12, minute: 30 }, 
{ time: "12:45 PM", hour: 12, minute: 45 }, 
{ time: "01:00 PM", hour: 13, minute:  0 }, 
{ time: "01:15 PM", hour: 13, minute: 15 }, 
{ time: "01:30 PM", hour: 13, minute: 30 }, 
{ time: "01:45 PM", hour: 13, minute: 45 }, 
{ time: "02:00 PM", hour: 14, minute:  0 }, 
{ time: "02:15 PM", hour: 14, minute: 15 }, 
{ time: "02:30 PM", hour: 14, minute: 30 }, 
{ time: "02:45 PM", hour: 14, minute: 45 }, 
{ time: "03:00 PM", hour: 15, minute:  0 }, 
{ time: "03:15 PM", hour: 15, minute: 15 }, 
{ time: "03:30 PM", hour: 15, minute: 30 }, 
{ time: "03:45 PM", hour: 15, minute: 45 }, 
{ time: "04:00 PM", hour: 16, minute:  0 }, 
{ time: "04:15 PM", hour: 16, minute: 15 }, 
{ time: "04:30 PM", hour: 16, minute: 30 }, 
{ time: "04:45 PM", hour: 16, minute: 45 }, 
{ time: "05:00 PM", hour: 17, minute:  0 }, 
{ time: "05:15 PM", hour: 17, minute: 15 }, 
{ time: "05:30 PM", hour: 17, minute: 30 }, 
{ time: "05:45 PM", hour: 17, minute: 45 })) ;


// END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END
/*
//Set(ValidCounties,Table({Result:"Cumberland"},{Result:"Hoke"},{Result:"Robeson"}));
//Set(tFamAgreed,Table({Answer:"All"},{Answer:"agreed"},{Answer:"declined"},{Answer:"undecided"},{Answer:"undetermined"},{Answer:Blank()}));
//Set(tRoomOccupied,Table({RoomOccupied:"All"},{RoomOccupied:"Y"},{RoomOccupied:"N"}));
//Set(tEligibType,Table({Type:"All"},{Type:"Eligible"},{Type:"Ineligible"}));
//Set(tCatchmentArea,Table({Area:"All"},{Area:"In_Area"},{Area:"OOC"}));

//Set(tsort2,Ungroup(Table({sortvar1: "Room"})));
//Set(tsort3,Ungroup(Table({sortvar: [Blank(), "DC to Home", "Due Date", "Delivery Date", "HV Scheduled", "Next Contact", "Case Open", "Case Review"]})));
//Set(tsort,Table({sortvar: [Blank(), "SortCaseReview", "SortCaseOpen", "SortNextContact", "SortHVSheduled"]}));
//Set(_itemselected, Blank());
//Set(_casestatusfilter,"accepted");
//UpdateContext({_casestatusfilter:"accepted"});
//Set(_casestatusfilter,"OPEN Cases");
//Set(_sortcases,"field1");
//Set(Open_Closed_Cases,Table({OpClo:"All"},{OpClo:"Open"},{OpClo:"Closed"}));
      
//Set(ButtonCSS, {
//FontFamily: Font.'Segoe UI',
//BackgroundColor: RGBA(127, 178, 57, 1),
//BorderThickness: 1,
//Color: RGBA(255,155,195,1),
//Padding: 8,
//TextAlign: Align.Center
//});

//Set(myStyle, {style:'position:relative'}) //Color: Blue,Size:46})
//"<div style='position:relative'>" & varPageContent & "</div>"
//Set(_countrowsIneligible,CountRows(Filter(PrincipalDataBase,PatientStatus="Ineligible")));
//Set(_countrowsEligible,CountRows(Filter(PrincipalDataBase,PatientStatus="Eligible")));
    // {OpClo:"Open",Type:""},
    // {OpClo:"Open",Type:""},
    // {OpClo:"Open",Type:"-"},
    // {OpClo:"Open",Type:"Eligible"},
    // {OpClo:"Open",Type:"Ineligible"},
    // {OpClo:"Open",Type:"Open/Ineligible"},
    // {OpClo:"Closed",Type:"UTR",agree="agree"},
    // {OpClo:"Closed",Type:"Managed"},
    // {OpClo:"Closed",Type:"Other/Closed"}
// COUNT ROWS IN MAIN DATA TABLE
//Set(_countrowsmaindata,CountRows(Filter(PrincipalDataBase.MotherName,MotherName<>"empty")));
//Set(tnodelivered,Filter(PrincipalDataBase,
//                            MotherName<>"empty",
//                            (ReasonIfnoteligible = "not delivered" || IsBlank(DeliveryDate_D))));

//Set(_countrowsnodelivered,CountRows(tnodelivered.MotherName));
*/
```
# Screen_1_Rooms

List of Patients that are currently occupying rooms. It lists a total of 36 rooms, either 'Occupied' by a patient or 'empty'. The initials in red: Users currently working on the MOB form.

---

## Screen_1_Rooms As screen

### OnVisible


```
=// Screen: "Screen_1_Rooms". 

Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB", "In_Out", "Since"));

Refresh(PrincipalDataBase);
ClearCollect(tOCCUPIED,  Filter(PrincipalDataBase, RoomOccupied="Y"));

// FOR SUPP ROOMS - FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS

//create an empty array to hold the room values:
ClearCollect(SUPProomList, []);
//Then, loop through each row of the table and append the room value to the roomList array:
ForAll(_troomindexSUPP, Collect(SUPProomList, room));    // _troomindexSUPP global variable

// Create empty data collection to append matching values
ClearCollect(tFilteredSUPP, []);  
//Then, loop through each row of the tOCCUPIED table and append the matching values, resulting in a filtered data tFilteredSUPP w Supplemental rooms:
ForAll(SUPProomList, Collect( tFilteredSUPP, Filter( tOCCUPIED, Room = SUPProomList[@Value] ) ) );

// FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS ======================
//ClearCollect(tOCCUPIED2,  Filter(PrincipalDataBase, RoomOccupied="Y")); // NOTE: Already created on this screen

//create an empty array to hold the SUPP room values:
ClearCollect(SUPProomList, []);
//Then, loop through each row of the table and append the room value to the roomList array:
ForAll(_troomindexSUPP, Collect(SUPProomList, room));      // _troomindexSUPP global variable

// Create empty data table collection to append matching values
ClearCollect(tFilteredSUPP, []);  
//Then, loop through each row of the tOCCUPIED table and append the matching values, resulting in a filtered data tFilteredSUPP w only Supplemental rooms:
ForAll(SUPProomList, Collect(tFilteredSUPP, Filter(tOCCUPIED, Room = SUPProomList[@Value] ) ) );

/*
//======================================================================
// END - END - END - END - END - END - END - END - END - END - END - END 
//ClearCollect(tONEDATA,  Filter(tONEDATA1, !IsBlank(Room)); 
//ForAll(colfirst4rooms, Collect(roomList, room);
//ForAll(roomList, Collect(roomList2, "[" & Concat(roomList, value & ", ", value) & "]"));
//ForAll(colfirst4rooms, Collect(roomList, room));
//Concat( colfirst4rooms.room,  "\", "\""  )
//"[" & Replace(Text(roomList), "\"", "") & "]";
//Replace(Text(roomList), "\"", "");
ClearCollect(roomList, {});
//Then, loop through each row of the table and append the room value to the roomList array:
ForAll(colSUPProoms, Collect(roomList, room));
//ForAll(roomList, Collect(roomList2, "[" & Concat(roomList, value & ", ", value) & "]"));
//ForAll(colfirst4rooms, Collect(roomList, room));
//"[" & Replace(Text(roomList), "\"", "") & "]";
//Replace(Text(roomList), "\"", "");
//Clear(colList1);
ClearCollect(roomList, {});
ClearCollect(tONEDATA, 
;

//Refresh(PrincipalDataBaseUR);
//ClearCollect(colFCD,  PrincipalDataBase); 
//ClearCollect(colFCDUR,PrincipalDataBaseUR); 
//ClearCollect(tONEDATA,Ungroup(Table({MyTables: colFCD}), "MyTables"));
*/
```
## ButtonMovetoCaseDetails As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 
// NOTE 01 REFRESH IN ONVISIBLE SO THIS WILL BE TEMPORARILY DISABLED IS ???? CHECK IF NEEDED. DOES IT WORTH  ??? 
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _fromscreen: "SUPP_Rooms"})
        )
    );

/*
//Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase}),
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR})
            )
        )
    )
*/

//  DisplayMode.Edit,DisplayMode.View)//If(x="Yes","> "&x,x))
//    _msg_visible_turn_room_to_Off:false,
//    _msg_visible_status_to_declined:false,
//    _msg_visible_outofcatchment:false,
//    _msg_visible_status_to_Ineligible:false
// Text: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).UserEmail,4))
//DM: If(Self.Text=""||Self.Text=Upper(Left(User().Email,4)),DisplayMode.Edit,DisplayMode.View)
//Fill: If(Self.Text=""||Self.Text=Upper(Left(User().Email,4)),RGBA(0, 0, 0, 0),RGBA(100, 100, 100, 0.2)) // 

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
# Screen_2_SupplemRooms
  
---

## Screen_2_SupplemRooms As screen

### OnVisible


```
=// Screen: "Screen_2_Supplemental Births Rooms". 
// List of Supplemental Births rooms. Labor & Delivery (L&D), Woman's Unit (w###) and other when is not known the room number. It lists a total of 24 rooms.

Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB", "In_Out", "Since"));

Refresh(PrincipalDataBase);
ClearCollect(tOCCUPIED,  Filter(PrincipalDataBase, RoomOccupied="Y"));

// FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS - FOR SUPLEMENTAL ROOMS

//create an empty array to hold the room values:
ClearCollect(SUPProomList, []);
//Then, loop through each row of the table and append the room value to the roomList array:
ForAll(_troomindexSUPP, Collect(SUPProomList, room));      // _troomindexSUPP global variable

// Create empty data table collection to append matching values
ClearCollect(tFilteredSUPP, []);  
//Then, loop through each row of the tOCCUPIED table and append the matching values, resulting in a filtered data tFilteredSUPP w only Supplemental rooms:
ForAll(SUPProomList, Collect(tFilteredSUPP, Filter(tOCCUPIED, Room = SUPProomList[@Value] ) ) );

```
## ButtonMovetoCaseDetails_1 As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 
// NOTE 01 REFRESH IN ONVISIBLE SO THIS WILL BE TEMPORARILY DISABLED IS ???????????????? CHECK IF NEEDED. DOES IT WORTH  ??????????? 
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));

With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
            Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _fromscreen: "SUPB_Rooms"}))
        );

/*
// When TWO data tables   // REVERTED 3/10/2023
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,
            Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase,   _databoth: "DB_1"})
            Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR, _databoth: "DB_2"})
        )))
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
        If(ThisItem.MotherName in PrincipalDataBase.MotherName,
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBase}),
            Navigate(Screen_X_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _datatouse: PrincipalDataBaseUR})
            )
        )
    )
*/
//  DisplayMode.Edit,DisplayMode.View)//If(x="Yes","> "&x,x))
//    _msg_visible_turn_room_to_Off:false,
//    _msg_visible_status_to_declined:false,
//    _msg_visible_outofcatchment:false,
//    _msg_visible_status_to_Ineligible:false
// Text: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).UserEmail,4))
//DM: If(Self.Text=""||Self.Text=Upper(Left(User().Email,4)),DisplayMode.Edit,DisplayMode.View)
//Fill: If(Self.Text=""||Self.Text=Upper(Left(User().Email,4)),RGBA(0, 0, 0, 0),RGBA(100, 100, 100, 0.2)) // 

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
# Screen_3_CaseDetails

This screen contains the case form that serves as the only way to input data, including notes. In order to switch the room from 'Occupied' to 'empty', in the details case form, change the 'Room Occupied' toggle from Y to N and then, after applying all the necessary changes in the form, save the changes. This step automatically creates a new 'empty' room with the same Room number to be filled with the next case info.


---

## Screen_3_CaseDetails As screen

### OnVisible

```
=// Screen: "Screen_3_CaseDetails". 

UpdateContext({
    _change_eligibility_time:false,
    _change_agreement_time:false,
    _change_scheduled_time:false,
    _change_hv:false,
    _dt_MTOUvisible:false,   // DataTable MoreThanOneUser visible
    _UserEmail4: Upper(Left(User().Email,4)),
    _ID_EditRecord: EditRecord.ID,
    _Confirmation_msg: false
    });

// IN_OUT_DATA - IN_OUT_DATA - IN_OUT_DATA - IN_OUT_DATA - IN_OUT_DATA - IN_OUT_DATA - IN_OUT_DATA

// FOR CASES WHEN THE USER LEAVES THE APP WITHOUT USING ICONS SAVE, BACK, HOME: NEED TO DELETE THE ONES IN FIELD IN_OUT
// Check previous same ID_FCDB sameUser Take ID(In_Out)
    ClearCollect(recwones, ShowColumns(Filter(In_Out_Data, In_Out=1, Title = _UserEmail4, ID_FCDB = _ID_EditRecord),"ID"));
// Change 1 to 0 for all previous ID(In_Out)
    ForAll( recwones, Patch(In_Out_Data, LookUp( In_Out_Data, ID = recwones[@ID] ), { In_Out: 0 } ));
// Why is the next collection for?
    ClearCollect(recwones2, ShowColumns(Filter(In_Out_Data, In_Out=1, Title = _UserEmail4, ID_FCDB = _ID_EditRecord),"ID"));

// CREATE RECORD IN_OUT: _itemInOut with temporal record created  // Next Path to Control users editing the same form at the same time / ADD INFO
    Patch(In_Out_Data,Defaults(In_Out_Data),{ID_FCDB: _ID_EditRecord, Since: Text( Now(), "mm/dd/yyyy HH:mm:ss AM/PM" ), Title: _UserEmail4, In_Out: 1});      
//UpdateContext({_itemInOut: Patch(In_Out_Data,Defaults(In_Out_Data),{ID_FCDB: _ID_EditRecord, Since: _Sincevar, Title: _UserEmail4, In_Out: 1})});      
//ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data, ID_FCDB = _ID_EditRecord && In_Out=1),"Title","ID_FCDB", "Since", "In_Out"));

// NOTES_LOG_DATA - NOTES_LOG_DATA - NOTES_LOG_DATA - NOTES_LOG_DATA - NOTES_LOG_DATA - NOTES_LOG_DATA
// CREATE RECORD LOG: _itemLogRecs with temporal record created  // Next Path to Control users editing the same form at the same time / ADD INFO
UpdateContext({_itemLogRecs: Patch(FC_Recruitment_Notes_Log,Defaults(FC_Recruitment_Notes_Log),
{ID_FCDB: EditRecord.ID, Since: Text( Now(), "mm/dd/yyyy HH:mm:ss AM/PM" ), How_In: _fromscreen, RoomOccupied:EditRecord.RoomOccupied, Room:EditRecord.Room, UserEmail: _UserEmail4, VersionApp:Text(varAppVersion)})});       
// MotherName: EditRecord.MotherName, MomFirstName: EditRecord.MomFirstName, MRN: EditRecord.MRN, CaseOpen_DT: EditRecord.CaseOpen_DD,

UpdateContext({
    tInsurBlank:Table({Result:""}),
    tOther:Table({Result:"Add Insurance ->"}),
    tCountyBlank:Table({Result:""}),
    tCountyOther:Table({Result:"Add County ->"})
    });

// Visible DataTable with 1+ users in one case
If(CountIf(In_Out_Data, ID_FCDB = _ID_EditRecord && In_Out=1)>1,UpdateContext({_dt_MTOUvisible:true}), UpdateContext({_dt_MTOUvisible:false}));

/*
// - END - END - END - END - END - END - END - 
    //_change_visittype:false,
    //_change_address:false,
    //_change_phone:false,
    //_change_comments:false,
//UpdateContext({dt_MTOUvisible:false});
    //_newcommentsform:false,
// Collect(Current_Users, {UserEmail: User().Email, Recruitment_ID: EditRecord.ID, Since:LabelClock.Text}); COLLECTION DOESN'T WORK FOR MULTIPLE USERS
// Collect(Current_Users, {UserEmail: User().Email, Recruitment_ID: EditRecord.ID, Since:LabelClock.Text}); COLLECTION DOESN'T WORK FOR MULTIPLE USERS
// Next Path to Control users editing the same form at the same time / ADD INFO
//Patch(PrincipalDataBase,LookUp(PrincipalDataBase, ID=EditRecord.ID),{User_Email: User().Email, Since:LabelClock.Text});
//UpdateContext({_recoloco: Patch(PrincipalDataBase,LookUp(PrincipalDataBase, ID=EditRecord.ID),{User_Email: User().Email, Since:LabelClock.Text})});

//, Since:LabelClock.Text

    // DELETE  _msg_visible_turn_room_to_Off:true, 
    // DELETE  _msg_visible_status_to_declined:true, 
    // DELETE _msg_visible_outofcatchment:true, 
    // DELETE _msg_visible_status_to_Ineligible:true, 
//  _roomonoffBefore:DCV_RoomOnOff.SelectedText.Value,
//  _insurancetype:ThisItem.InsuranceType,
//  _room:EditRecord.Room,
//  _room2:DataCardValue1.Text,
//  _mothername:DCV_MotherName.Text,
//  _openclosedcase:ToggleCase.Value  disabled 20220715
//    _roomonoffAfter:DataCardValue44.SelectedText.Value,
*/
```
## Button_Background As button

### Text


```
="Save"
```
## Button_SEND_Forms_1 As button

### OnSelect


```
=// BUTTON SAVE: Save Forms in Case Detail screen. Only way in the app to save patient information.

// SAVE DATA FROM FORMS !!!
SubmitForm(CaseDetailsForm);SubmitForm(FormComments);         // using record EditRecord

// FC_Recruitment_Notes_Log - FC_Recruitment_Notes_Log - FC_Recruitment_Notes_Log - FC_Recruitment_Notes_Log //
Patch(FC_Recruitment_Notes_Log,LookUp(FC_Recruitment_Notes_Log, ID=_itemLogRecs.ID),
{CaseReview_DD: Text(Now(), "mm/dd/yyyy HH:mm:ss AM/PM" ), RoomOccupied: If(ToggleRoomYN.Value=true,"Y","N"), How_Out: "Save"});
// MotherName: DCV_MotherName.Text, MomFirstName: DataCardValue16.Text, MRN: DataCardValueMRN.Text, CaseOpen_DT: lbl_CaseOpen_DD.Text, 

UpdateContext({_now:Text(Now(), "mm/dd/yyyy HH:mm AM/PM" )});
// If Closing the Room, create a new case with new form
If(EditRecord.RoomOccupied = "Y" && ToggleRoomYN.Value = false, // && CountIf(PrincipalDataBase.RoomOccupied, RoomOccupied="Y")<38,
    // TRUE
    //NewForm(formNewFormBlank); SubmitForm(formNewFormBlank);  // Refresh(PrincipalDataBase)
    Patch(PrincipalDataBase,Defaults(PrincipalDataBase),
    {MotherName: "empty", RoomOccupied: "Y", Room: DCV_Room.Text, CaseReview_DD: _now});   //Changed 02/25/23    
    // ELSE
    //SubmitForm(CaseDetailsForm);SubmitForm(FormComments)     //;UpdateContext({_msg_visible_turn_room_to_Off: false})
        );

// If reason not delivered changes to delivered (Eligible) -> Open the Case
If(
    EditRecord.ReasonIfnoteligible = "not delivered" && RadioEligibility.Selected.Value="Eligible",
    // TRUE // Patch - Record Update Open Case DT 
    Patch(PrincipalDataBase,LookUp(PrincipalDataBase,ID=EditRecord.ID),{CaseOpen_DD:_now}) //Changed 02/25/23
  );

// CHANGES ON
If(_change_hv,        Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "HV_DateTime", ID_FCDB: _ID_EditRecord}));
//If(_change_visittype, Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "VisitType",   ID_FCDB: _ID_EditRecord}));
//If(_change_comments,  Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "Comments",    ID_FCDB: _ID_EditRecord}));
//If(_change_phone,     Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "Phone",       ID_FCDB: _ID_EditRecord}));
//If(_change_address,   Patch(Change_On_Data,Defaults(Change_On_Data),{Title: "Address",     ID_FCDB: _ID_EditRecord}));

//UpdateContext({_timervaluestart: Timer_X_details.Value});
UpdateContext({_Confirmation_msg: true}) // Back()

/*
// END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END - END
// Refresh(FC_Recruitment_Notes_Log);   // NOTE 2023-02-21: DISABLED BECAUSE THE RECORD WAS ALREADY CREATE IN ONVISIBLE ENTERING THIS SCREEN
//EditForm(Form_Log);SubmitForm(Form_Log); // REPLACED BY PATCH 02/25/23 // PUT BEFORE SUBMIT FORMS CaseDetails and Comments using record _itemLogRecs
// Refresh(PrincipalDataBase);   // NOTE 2023-02-21: DISABLED BECAUSE USING ONLY EDITRECORD WHICH DOESN'T DEPEND ON FCDATA
// CHECK WHILE - LOOP TO PREVENT DUPLICATED ROOMS
//Patch(Current_Users,LookUp(Current_Users,Title=User().Email),{Recruitment_ID:0},{Since:"-/-"});
//Remove(Current_Users,LookUp(Current_Users,Recruitment_ID=EditRecord.ID),All);  // PUT BEFORE SUBMIT FORMS CaseDetails and Comments
https://www.dynamicpeople.nl/en/news/while-loop-powerapps/#:~:text=The%20while%20loop%20can%20be%20thought%20of%20as,are%20still%20many%20scenario%20in%20which%20it%20doesn%E2%80%99t.
// Patch - Update Case_Review_DT - Everytime Save button is Selected (OnSelect) Patch - Update Comments
// Patch(PrincipalDataBase,LookUp(PrincipalDataBase,ID=EditRecord.ID),{CaseReview_DD:LabelClock.Text});
// Patch - Update Comments
//Patch(PrincipalDataBase,LookUp(PrincipalDataBase,CaseID=EditRecord.CaseID),{Comments:TextInputComments.Text});
//Patch(PrincipalDataBase,LookUp(PrincipalDataBase,CaseID=EditRecord.CaseID),{SupervisorNotes:TextInputSupervisor.Text});
If(EditRecord.RoomOccupied = "Y" && ToggleRoomYN.Value = false,
        // TRUE
    SubmitForm(CaseDetailsForm);SubmitForm(FormComments);NewForm(formNewFormBlank);SubmitForm(formNewFormBlank),  //UpdateContext({_msg_visible_turn_room_to_Off: false}),
        // ELSE
    SubmitForm(CaseDetailsForm);SubmitForm(FormComments)     //;UpdateContext({_msg_visible_turn_room_to_Off: false})
        );

// ELSE If County is Not Cumberland Hoke Robeson and Eligibility is option "Eligible" // Delete >>>  and case status is not Ineligible
If(
        Not(Counties3.SelectedText.Value = "Cumberland" || Counties3.SelectedText.Value = "Hoke" || Counties3.SelectedText.Value = "Robeson") && RadioEligibility.SelectedText.Value = "Eligible",                      // Delete?? - Situation not possible anymore
    // TRUE Show Message "Ineligible: county out of catchment area"
    UpdateContext({_msg_visible_outofcatchment: false});UpdateContext({_msg_visible_outofcatchment: true}),
*/
```
### Text


```
=" "
```
## ImageSpinnerWheel As image

### Image


```
='1487_spinner'
```

# Screen_4_CasesList
  
---
## Screen variables
  
Following variables have been created / or updated on this screen
- _casestatusfilter
- _eligibtype
- _roomoccupiedfilter

## Screen_4_CasesList As screen

### OnVisible


```
=// Screen: "Screen_4_CasesList". 
// Powerapps gallery with all the cases, with a filters of Eligibility, Agreement, Occupied Rooms, and sort options. This list can help to identify cases with missing information (highlighted).

Reset(TextInputSearchRoom3);

Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));  //DateValue(DateTimeValue(CaseOpen_DT))>=Today()-5)
Refresh(PrincipalDataBase);
ClearCollect(colFCData,  Filter(PrincipalDataBase, MotherName <> "empty")); 

UpdateContext({_startgalldate: DatePicker1_2.SelectedDate, _endgalldate: DatePicker2_2.SelectedDate}); 
If(IsBlank(_eligibtype),UpdateContext({_eligibtype:"All"}));
If(IsBlank(_sortcolumn),UpdateContext({_sortcolumn:"SortCaseReview"}));      // {_sortcolumn:"SortCaseReview"}
If(IsBlank(SortDescending1),UpdateContext({SortDescending1:false}));

// - END - END - END - END - END - END - END - END - END - END - END - END - END
//Refresh(PrincipalDataBaseUR);  // NULLED 3/10/2023
// It will be Eliminated. NO TWO data tables. The reason is because if switching room to a record in FCDBUR, it will be occupied but not listed in the "Rooms Today screen"
//If(IsBlank(_roomoccupiedfilter),UpdateContext({_roomoccupiedfilter:"All"}));
//If(IsBlank(_casestatusfilter),UpdateContext({_casestatusfilter:"All Status"}));
//    _now: Text(Now(),DateTimeFormat.ShortDateTime),
//  _roomonoffBefore:DCV_RoomOnOff.SelectedText.Value,
//    _msg_visible_turn_room_to_Off:false,
//   _change_eligibility_time:false,
//  UpdateContext({_casestatusfilter:ThisItem.Status})

```
## Button1Mover As button

### OnSelect


```
=//Refresh(PrincipalDataBase);    // CHECK NOTE 01 
// NOTE 01 REFRESH IN ONVISIBLE SO THIS WILL BE TEMPORARILY DISABLED IS ???????????????? CHECK IF NEEDED. DOES IT WORTH  ??????????? 
Refresh(In_Out_Data);
ClearCollect(collInRooms, ShowColumns(Filter(In_Out_Data,In_Out=1),"Title","ID_FCDB"));
With({x: Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))}, 
    If(x=""||x=Upper(Left(User().Email,4)),           
        EditForm(CaseDetailsForm);
        EditForm(FormComments);
            Navigate(Screen_3_CaseDetails,ScreenTransition.Fade,{EditRecord:ThisItem, _fromscreen: "Cases_List"})   // , _databoth: "DB_1"
            )
        )


```
### Text


```
=Upper(Left(LookUp(collInRooms,ID_FCDB=ThisItem.ID).Title,4))

//Upper(Left(LookUp(Users_on_Cases,ID_FCDB=ThisItem.ID).Title,4))
// Upper(Left(ThisItem.User_Email,4))  // CHANGED 2023-02-15
//Upper(Left(LookUp(Current_Users,Recruitment_ID=ThisItem.ID).UserEmail,4))
```
## btn_CaseOpenSort As button

### OnSelect


```
=UpdateContext({
    _sortcolumn: "SortCaseOpen",
    SortDescending1: !SortDescending1})
```
### Text


```
=
```
## btn_ReviewSort_3 As button

### OnSelect


```
=UpdateContext({
    _sortcolumn: "SortCaseReview",
    SortDescending1: !SortDescending1})
```
### Text


```
=
```
## btn_RoomSort As button

### OnSelect


```
=UpdateContext({
    _sortcolumn: "BySortRoom",
    SortDescending1: !SortDescending1})
```
### Text


```
=
```
## TextInputSearchRoom3 As text

## btn_HVSchedSort As button

### OnSelect


```
=UpdateContext({
    _sortcolumn: "SortHVScheduled",
    SortDescending1: !SortDescending1})
```
### Text


```
=
```
