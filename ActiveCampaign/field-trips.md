# Field Trips
Bookeo ID: 35  
Bookeo Product ID: 2147RMYUM147550F88EC  
Bookeo Custom Field (Details): 2147RMYUM147550F88EC_N37AFAXW  

TRFA visits schools and organizations that want an onsite cooking demonstration.

# Requirements
Customer starts at the [Site Booking form](https://therealfoodacademy.com/field-trip-details/) to "Plan" the event and provide event details. The form submission will redirect them (_unless this is an updated form_) to the [Bookeo "BOOK" page](https://bookeo.com/cookingwithkidsmiami?type=2147RMYUM147550F88EC) for field trips. If the form submission is on an update, redirect them to the [FAQ page](https://therealfoodacademy.com/frequently-asked-questions/#field-trips).

Both the inital and the updated form in Gravity Forms will push the data to ActiveCampaign custom fields via the Feed addon. 

# Custom Fields
The booking is sent to Google calendar via Bookeo, so we need to insert the recap of the trip details into Bookeo, which will send to the calendar. The Push To Bookeo task (webhook) will be called whenever the Info field is updated.  

_QUESTION: There are four possible dates available in Field Trips. It is possible that two dates are within less than 7 days of each other. If that is true, then the automations might overlap enough to cause problems for the reminders and headcount requests. Is there anything within the Push To Bookeo process, record contents, etc, that will allow us to deal with this issue?_

# Push To Bookeo: (Field Trip Info 1)
Webhook endpoint: https://trfaapi.com/v3/class/fieldtrip  

Gravity Forms creates the Temp Event data. The TempProcessing part of API (called via webhook in AC) will parse and place in the correct set of fields. There are 4 Field Trip "sets" to work with. A daily call to the "Cleanup" function will "bubble up" the dates from a higher set (ie: 3) to a lower set (ie: 2) when the date in the lower is in the past. 

# ActiveCampaign Custom Field Info
Field Trips	related custom fields in ActiveCampaign for storing the booking details provided by the contact. Please note that at the time of the details form, no date or time has been selected. 

Field Trips is one of two events that use the Event Address fields in ActiveCampaign (_the other is Kids Party_). Since a field trip is probably NOT going to be booked by a Mom who will also book a kids party at her location, we felt a single set of data points was sufficient.  

**Event Address**  

| ID | AC Field Name | Personalization | GF param   |
|----|---------------|-----------------|------------|
| 34 | Address 1     | %ADDRESS_1%     | address    |
| 35 | Address 2     | %ADDRESS_2%     | (not used) |
| 36 | City          | %CITY%          | city       |
| 37 | State         | %STATE%         | state      |
| 38 | Zip           | %ZIP%           | zip        |

**Field Trip Specifics**  

| ID  | AC Field Name             | Personalization             | GF Param     |
|-----|---------------------------|-----------------------------|--------------|
| 25  | School or Organization    | %SCHOOL_NAME%               | school       |
| 102 | Field Trip Count          | %FIELD_TRIP_COUNT%          | count        |
| 26  | Field Trip Date 1         | %FIELD_TRIP_DATE_1%         | date         |
| 27  | Field Trip Time 1         | %FIELD_TRIP_TIME_1%         | time         |
| 40  | Field Trip Participants 1 | %FIELD_TRIP_PARTICIPANT_1%  | participants |
| 30  | Field Trip Age Range 1    | %FIELD_TRIP_AGE_RANGE_1%    | ageRange     |
| 72  | Field Trip Info 1         | %FIELD_TRIP_INFO_1%         | info         |
| 92  | Field Trip Final 1        | %FIELD_TRIP_FINAL_1%        | final        |
| 28  | Field Trip Date 2         | %FIELD_TRIP_DATE_2%         | date         |
| 29  | Field Trip Time 2         | %FIELD_TRIP_TIME_2%         | time         |
| 41  | Field Trip Participants 2 | %FIELD_TRIP_PARTICIPANTS_2% | participants |
| 31  | Field Trip Age Range 2    | %FIELD_TRIP_AGE_RANGE       | ageRange     |
| 73  | Field Trip Info 2         | %FIELD_TRIP_INFO_2%         | info         |
| 93  | Field Trip Final 2        | %FIELD_TRIP_FINAL_2%        | final        |
| 74  | Field Trip Date 3         | %FIELD_TRIP_DATE_3%         | date         |
| 75  | Field Trip Time 3         | %FIELD_TRIP_TIME_3%         | time         |
| 76  | Field Trip Participants 3 | %FIELD_TRIP_PARTICIPANT_3%  | participants |
| 77  | Field Trip Age Range 3    | %FIELD_TRIP_AGE_RANGE_3%    | ageRange     |
| 78  | Field Trip Info 3         | %FIELD_TRIP_INFO_3%         | info         |
| 94  | Field Trip Final 3        | %FIELD_TRIP_FINAL_3%        | final        |
| 79  | Field Trip Date 4         | %FIELD_TRIP_DATE_4%         | date         |
| 80  | Field Trip Time 4         | %FIELD_TRIP_TIME_4%         | time         |
| 81  | Field Trip Participants 4 | %FIELD_TRIP_PARTICIPANT_4%  | participants |
| 82  | Field Trip Age Range 4    | %FIELD_TRIP_AGE_RANGE_4%    | ageRange     |
| 83  | Field Trip Info 4         | %FIELD_TRIP_INFO_4%         | info         |
| 95  | Field Trip Final 4        | %FIELD_TRIP_FINAL_4%        | final        |  

# Gravity Forms: Field Trips (id: 15)

| ID  | GF Field Name        | Personalization                         | GF Param     |  
|-----|----------------------|-----------------------------------------|--------------|  
| 2   | Name                 |                                         |              |  
| 2.3 | First                | {Name (First):2.3}                      | first        |  
| 2.6 | Last                 | {Name (Last):2.6}                       | last         |  
| 3   | Email                | {Email:3}                               | email        |  
| 5   | Position             | {Your Position:5}                       | position     |  
| 6   | Phone                | {Phone:6}                               | phone        |  
| 8   | Name of School       | {Name of School or Organization:8}      | school       |  
| 9   | Address              |                                         |              |  
| 9.1 | Address              | {Address (Street Address):9.1}          | address      |  
| 9.3 | City                 | {Address (City):9.3}                    | city         |  
| 9.4 | State                | {Address (State / Province):9.4}        | state        |  
| 9.5 | Zip                  | {Address (ZIP / Postal Code):9.5}       | zip          |  
| 10  | # of Particiapants   | {# of Participants:10}                  | participants |  
| 11  | Age Range            | {Age Range:11}                          | ageRange     |  
| 12  | Consent              | {Consent: 12}                           |              |  
| 13  | Special Instructions | {Special Instructions:13}               |              |  
| 14  | Info                 | {Info:14}                               |              |  
| 15  | Info Type (Field Trip)| {Info Type:15} (default = Field Trips)  | type         |  
| 16  | GF Update Status     | {GF Update Status:16}                   | 

FIELD TRIP Details  
ActiveCampaign Feed settings. Add a Contact Note (in the format shown below), insert into the Field Trips list, add the tag (Field Trip . Details . Received)  

Contact Info: 

-----------------------
Name: {Name (First):2.3} {Name (Last):2.6}  
Email: {Email:3}  
Phone: {Phone:6}  
Position: {Your Position:5}  

Event Info:  

-----------------------  
School/Org: {Name of School or Organization:8}  
Address:  
{Event Address (Street Address):9.1}  
{Event Address (City):9.3}, {Event Address (State / Province):9.4}  {Event Address (ZIP / Postal Code):9.5}  
Participants: {# of Participants:10}  
Age Range: {Age Range:11}  
Special Instructions: {Special Instructions:13}  

# Links For Testing 
>_Links to test the form:_  
**Test Data Embedded Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&position=Administrator&school=School+Of+Hard+Knocks&address=7833+E+Main&city=Miami&state=FL&zip=33138&participants=52&ageRange=7-15

>**ActiveCampaign Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%FIELD_TRIP_PARTICIPANT_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%  

>**Gravity Forms Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Phone:6}&position={Your Position:5}&school={Name of School or Organization:8}&address={Address (Street Address):9.1}&city={Address (City):9.3}&state={Address (State / Province):9.4}&zip={Address (ZIP / Postal Code):9.5}&participants={# of Participants:10}&ageRange={Age Range:11}&update=true  

# ActiveCampaign Push To Custom Fields  
The ActiveCampaign Addon for Gravity Forms allows us to push data directly out of the form and into fields in ActiveCampaign. We can also set tags, and apply the customer updates to a specific list. If the customer already exists, the addon will just update the existing record.  

| Form Field                     | > | ActiveCampaign Custom Field |
| -------------------------------|---|-----------------------------| 
| Email                          | > | Email                       |
| Name (First)                   | > | First Name                  |
| Name (Last)                    | > | Last Name                   |
| Phone                          | > | Phone Number                |
| Name of School or Organization | > | School or Organization      |
| Your Position                  | > | Position                    |
| Address (Address)              | > | Address 1                   | 
| Address (City)                 | > | City                        |
| Address (State)                | > | State                       |
| Address (Zip)                  | > | Zip                         |
| Info Type                      | > | Temp Name                   |
| Info                           | > | Temp Event Info             |
| Into Type                      | > | Temp Event Type             |
| GF Update Status               | > | Temp Event Update           |

Insert into Field Trip List (id: 13)  
Add Tag: (Field Trip . Details . Received)  

# Form Submission **Redirect**
When the customer submits the Form, it should redirect in this way:  
First time:  
https://bookeo.com/cookingwithkidsmiami?type=2147RMYUM147550F88EC  
Updated=true:  
Insert a text element on the field trip details page in this format:  
><br/><p>Thanks {Name (First):2.3}!</p><p>We got your updated field trip details, and will adjust your order accordingly.</p><p>Please <strong>carefully</strong> review your details below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/field-trips/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Phone:6}&position={Your Position:5}&school={Name of School or Organization:8}&address={Address (Street Address):9.1}&city={Address (City):9.3}&state={Address (State / Province):9.4}&zip={Address (ZIP / Postal Code):9.5}&participants={# of Participants:10}&ageRange={Age Range:11}&update=true">update field trip link</a> again to make changes.</p><p>Feel free to visit our <a href="https://therealfoodacademy.com/frequently-asked-questions/#field-trips">Field Trip FAQ Section</a> if you have any questions.</p><p>FIELD TRIP INFO:<br/>======================================<br/>{Info:14}</p><br/>  

## Gravity Forms should also:  
 1. Send Confirmation email to TRFA staff with initial (or updated) details  
 2. Send form data to ActiveCampaign via the AC Feed 
 3. Insert the customer to the Field Trip list (id: 13)
 4. Tag the customer with the initial "automation start" tag
  
# Bookeo: 90 MINUTE FIELD TRIP TO YOUR LOCATION (id: 35)  
>Bookeo Product ID: 2147RMYUM147550F88EC  
Bookeo Custom Field (Details): 2147RMYUM147550F88EC_N37AFAXW  

API should insert these tags into the contact record:  
_Tags: 16, 222, 4 (confirmed and placed in the bookeo_ac_tag table)_  

| ID  | Tag Name                      |
|:---:|-------------------------------|
| 16  | Client Type . Field Trips     |
| 222 | Field Trip . Purchased        |
| 4   | Field Trip . Booking Complete |  

# API 
The Bookeo to ActiveCampaign (v3) API should send the customer record (or update existing) with Field Trip date and time to appropriate field set. There are four Field Trip fields sets that have the same fields in each. During the insertion, the API should move field set 2 data into the field set 1 data if field set 1's date has already passed, otherwise place the incoming "booking" into the next available field set.  

Link to Field Trip booking: https://bookeo.com/cookingwithkidsmiami?type=2147RMYUM147550F88EC
Update field trip details -> https://trfaapi.com/v3/util/url/field-trips/%SUBSCRIBERID%

# LINKS
These are the various pages that are a part of the automation and flow of the Field Trip Process.  

[Oxygen Landing Page](https://therealfoodacademy.com/field-trips/)  
[Oxygen Details Form](https://therealfoodacademy.com/field-trips-oxygen/)  <- Will be inserted into the live page at https://therealfoodacademy.com/field-trips/  
[Bookeo Booking Form](https://bookeo.com/cookingwithkidsmiami?type=2147RMYUM147550F88EC)  

# Automation Notes

## Field Trip / Init

## Field Trip / Headcount Reminder

## Field Trip / Nag To Book 

## Field Trip / Nag For Details 

## Field Trip / Headcount Received 

## Field Trip / Request Feedback

## Field Trip / FU Next Year