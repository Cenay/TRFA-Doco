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

## Format of Field Trip Info Pushed to Bookeo
```
Contact Info: 
-----------------------
Name: Cenay Nailor  
Email: cenay@cenaynailor.com
Phone: (928) 978-1919
Position: Administrator

Event Info:  
-----------------------  
School/Org: School of Hard Knocks
Address:  
7833 E Supai Dr, Prescott Valley, AZ 86314
Participants: 50
Age Range: 7-12

Special Instructions: 
----------------------
Ignore me, these are the notes added by the customer. 

```

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
Customer provides details about their upcoming field trip event on this form.  
>CHANGE Log:  
{1/05/2020} Info field changed from 14 (hidden field) to 18 (paragraph field, hidden)  
{1/10/2020} Added: Location dropbox so they can select where to hold the event.  

| ID  | GF Field Name        | Personalization                        | GF Param     |
|-----|----------------------|----------------------------------------|--------------|
| 2.3 | First                | {Name (First):2.3}                     | first        |
| 2.6 | Last                 | {Name (Last):2.6}                      | last         |
| 3   | Email                | {Email:3}                              | email        |
| 5   | Position             | {Your Position:5}                      | position     |
| 6   | Phone                | {Phone:6}                              | phone        |
| 8   | Name of School       | {Name of School or Organization:8}     | school       |
| 19  | Location             | {Location:19}                          | location     |
| 9.1 | Address              | {Address (Street Address):9.1}         | address      |
| 9.3 | City                 | {Address (City):9.3}                   | city         |
| 9.4 | State                | {Address (State / Province):9.4}       | state        |
| 9.5 | Zip                  | {Address (ZIP / Postal Code):9.5}      | zip          |
| 10  | # of Particiapants   | {# of Participants:10}                 | participants |
| 11  | Age Range            | {Age Range:11}                         | ageRange     |
| 12  | Consent              | {Consent: 12}                          |              |
| 17  | Experience           | {Experience:17}                        | experience   |
| 13  | Special Instructions | {Special Instructions:13}              |              |
| 18  | Info                 | {Info:18} (changed from 14)            |              |
| 15  | Info Type            | {Info Type:15} (default = Field Trips) | type         |
| 16  | GF Update Status     | {GF Update Status:16}                  | update       |

# Active Campaign Field Trip Feed Requirements
ActiveCampaign Feed settings. Add a Contact Note (in the format shown below), insert into the Field Trips list, add the tag (Field Trip . Details . Received)  

**Field Mapping**  
The "Feed" sends data from the form entries to the ActiveCampaign application. Since we are tracking multiple "sets" of trips, we can't send directly to the fields in ActiveCampaing or we risk overwriting an existing trip that isn't yet complete. We've created a set of temporary fields that we do push to, and let the API pick the data up, and perform conditional logic on where they will be "moved to".   

| AC Field Name          | GF Form Field Name                 |
|------------------------|------------------------------------|
| Email                  | {Email:3}                          |
| First Name             | {Name (First):2.3}                 |
| Last Name              | {Name (Last):2.6}                  |
| Phone                  | {Phone:6}                          | 
| School or Organization | {Name of School or Organization:8} |
| Position               | {Your Position:5}                  | 
| Address 1              | {Address (Street Address):9.1}     |
| City                   | {Address (City):9.3}               |
| State                  | {Address (State / Province):9.4}   |
| Zip	                 | {Address (ZIP / Postal Code):9.5}  | 
| Field Trip Count       | {Experience:17}                    | 
| Temp Name              | {Info Type:15}                     |
| Temp Event Info        | {Info:18}                          | 
| Temp Event Type        | {Info Type:15}                     |
| Temp Event Update      | {GF Update Status:16}              | 

**Contact Note Construction**  
In addition to pushing the data into the fields shown above, the "Feed" also adds a "Note" which translate into a contact level note. We use this to push in the complete "constructed" info data, an update status (true/false) and a created date. 

```
FIELD TRIP Details
=======================
{Info:18}

Updated: {GF Update Status:16}
```
**Contact Tags Insertion**  
In additional to pushing data, and building a contact note, the "feed" also adds a tag to the contact record. 
```
Field Trip . Details . Provided  
```

# Gravity Forms: Field Trips Final Headcount (id: 16)
Customer provides the final headcount for their upcoming field trip event on this form.  
>CHANGE Log:  
{1/05/2020} Info field changed from 14 (hidden field) to 18 (paragraph field, hidden)  
{1/10/2020} Added: Location dropbox so they can select where to hold the event. 
{1/27/2020} Added: HTML code block (GF: 20) and Hidden field expired (GF:21)

| ID  | GF Field Name        | Personalization                        | GF Param     |
|-----|----------------------|----------------------------------------|--------------|
| 2.3 | First                | {Name (First):2.3}                     | first        |
| 2.6 | Last                 | {Name (Last):2.6}                      | last         |
| 3   | Email                | {Email:3}                              | email        |
| 5   | Position             | {Your Position:5}                      | position     |
| 6   | Phone                | {Phone:6}                              | phone        |
| 8   | Name of School       | {Name of School or Organization:8}     | school       |
| 21  | Location             | {Location:21}                          | location     |
| 9.1 | Address              | {Address (Street Address):9.1}         | address      |
| 9.3 | City                 | {Address (City):9.3}                   | city         |
| 9.4 | State                | {Address (State / Province):9.4}       | state        |
| 9.5 | Zip                  | {Address (ZIP / Postal Code):9.5}      | zip          |
| 10  | Final Headcount      | {Final Headcount:10}                   | guests       |
| 11  | Age Range            | {Age Range:11}                         | ageRange     |
| 12  | Consent              | {Consent: 12}                          |              |
| 17  | Experience           | {Experience:17}                        | experience   |
| 13  | Special Instructions | {Special Instructions:13}              |              |
| 18  | Allergy List         | {Allergy List:18}                      | FILE UPLOAD  |
| 18  | Info                 | {Info:18} (changed from 14)            |              |
| 19  | Particiapants        | {Participants:19} (hidden)             | participants |
| 15  | Info Type            | {Info Type:15} (default = Field Trips) | type         |
| 16  | GF Update Status     | {GF Update Status:16}                  | update       |
| 20  | HTML Code Block      | {NONE}                                 |              |
| 21  | Expired              | {Expired:21} (default=false)           | expired      |

# Active Campaign Field Trip Headcount Feed Requirements
ActiveCampaign Feed settings. Add a Contact Note (in the format shown below), insert into the Field Trips list, add the tag (Field Trip . Final Headcount Provided)  

**Field Mapping**  
The "Feed" sends data from the form entries to the ActiveCampaign application. This feed assumes that we are working with the Field Trip 1 set, as that should be the one the automations are running for. Therefore, these push directly into the AC fields.  

| AC Field Name          | GF Form Field Name                 |
|------------------------|------------------------------------|
| Email                  | {Email:3}                          |
| First Name             | {Name (First):2.3}                 |
| Last Name              | {Name (Last):2.6}                  |
| Phone                  | {Phone:6}                          | 
| School or Organization | {Name of School or Organization:8} |
| Position               | {Your Position:5}                  | 
| Address 1              | {Address (Street Address):9.1}     |
| City                   | {Address (City):9.3}               |
| State                  | {Address (State / Province):9.4}   |
| Zip	                 | {Address (ZIP / Postal Code):9.5}  | 
| Field Trip Count       | {Experience:17}                    | 
| Temp Name              | {Info Type:15}                     |
| Field Trip Info 1      | {Info:20}                          | 
| Field Trip Final 1     | {Final Headcount:10}               |
| Field Trip Participants 1 | {Participants:19}               | 

**Contact Note Construction**  
In addition to pushing the data into the fields shown above, the "Feed" also adds a "Note" which translate into a contact level note. We use this to push in the complete "constructed" info data, an update status (true/false) and a created date. 

```
FIELD TRIP Headcount Provided
=======================
Final Headcount:  {Final Headcount:10}

{Info:20}

Updated: {GF Update Status:16}
```
**Contact Tag Insertion**  
In additional to pushing data, and building a contact note, the "feed" also adds a tag to the contact record. 
```
Field Trip . Final Headcount Provided  
```


## Format Of Push To Bookeo Content
Below is a live sample of the content pushed to Bookeo (and therefore into the Google calendar description for the event)
```
Details: Contact Info: 
-----------------------
Name: Cristi Stalcup
Email: cstalcup02@yahoo.com
Phone: (786) 325-9358
Position: Parent

Event Info: 
-----------------------
School/Org: Arch-Angel Home School Group
Address: 
570 NE 81st Street, Miami, Florida 33138
Participants: 40 (original count)
Final Headcount: 35
Age Range: 3-18

Special Instructions: 
-----------------------
{Special Instructions:13}  

Date/Time: 01/09/2020 @ 09:00 AM

Final Headcount: 35
```

# Notifications
When the final headcount is received, send this notification email to Art and Cenay

**Subject line:**  
Field Trip Final Headcount received for %FIRSTNAME%  %LASTNAME% of %FIELD_TRIP_FINAL_1%   

**Body**  
```
%FIRSTNAME%  %LASTNAME% just submitted their final headcount of %FIELD_TRIP_FINAL_1% for the trip planned on %FIELD_TRIP_DATE_1% at %FIELD_TRIP_TIME_1%. 

The details of their trip: 

%FIELD_TRIP_INFO_1%

```

# Links For Testing 
Add &update=true to the end to see the effects of an updated link  

>_Links to test the form:_  
**Test Data Embedded Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&position=Administrator&school=School+Of+Hard+Knocks&address=7833+E+Main&city=Miami&state=FL&zip=33138&participants=52&ageRange=7-15&count=Second+Field+Trip  

>**ActiveCampaign Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%FIELD_TRIP_PARTICIPANT_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%  

>**Gravity Forms Version**  
https://therealfoodacademy.com/field-trips-oxygen/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Phone:6}&position={Your Position:5}&school={Name of School or Organization:8}&address={Address (Street Address):9.1}&city={Address (City):9.3}&state={Address (State / Province):9.4}&zip={Address (ZIP / Postal Code):9.5}&participants={# of Participants:10}&ageRange={Age Range:11}&update=true  

# ActiveCampaign Push To Custom Fields  
The ActiveCampaign Addon for Gravity Forms allows us to push data directly out of the form and into fields in ActiveCampaign. We can also set tags, and apply the customer updates to a specific list. If the customer already exists, the addon will just update the existing record.  

| ActiveCampaign Custom Field | > | Form Field                     | 
|-----------------------------|---| -------------------------------| 
| Email                       | > | Email                          | 
| First Name                  | > | Name (First)                   | 
| Last Name                   | > | Name (Last)                    | 
| Phone Number                | > | Phone                          | 
| School or Organization      | > | Name of School or Organization | 
| Position                    | > | Your Position                  | 
| Address 1                   | > | Address (Address)              | 
| City                        | > | Address (City)                 | 
| State                       | > | Address (State)                | 
| Zip                         | > | Address (Zip)                  | 
| Field Trip Count            | > | Experience                     | 
| Temp Name                   | > | Info Type                      | 
| Temp Event Info             | > | Info                           | 
| Temp Event Type             | > | Info Type                      | 
| Temp Event Update           | > | GF Update Status               | 


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
  
## Gravity Form Expiration Notice
In the event the field trip info has "expired" (it's been more than 30 days since the details were added, but no actual booking has happened), add a message about why the details are not present. New hidden field (GF: 21 | param: expired)

Display this HTML Message
```
<div class="expire-warning">
<h3>We're sorry, but the Field Trip details you are trying to edit, have expired.</h3>

<p>You will need to complete the details again. We only hold details for 30 days without a booking. You can refer to your original confirmation email for the details supplied.</p>
<p>Thanks for understanding.</p>
</div>
```


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

[Landing Page](https://therealfoodacademy.com/field-trips/)  
[Details Form](https://therealfoodacademy.com/field-trips-details/) 
[URL Generator Link](https://trfaapi.com/v3/util/url/field-trips/%SUBSCRIBERID%)
[Field Trip Final Headcount]()
[Bookeo Booking Form](https://bookeo.com/cookingwithkidsmiami?type=2147RMYUM147550F88EC)  

# Automation Notes

## Field Trip / Init
 * TRIGGER: Tag applied (Field Trip . Booking Complete)
 * GOAL: Have enough to continue
   >(condition: has tags )  
   Field Trip . Booking Complete  
   Field Trip . Details . Received  
   OR  
   Field Trip . Bypass Init Hold  
 * WAIT: 20 minutes
 * IF/ELSE: Tag (Field Trip . Details . Received)
   * **YES** Path: 
     * LIST: Add to All Contacts
     * SEND: Email with details and link to FAQ
     >Links:  
     FAQ: https://therealfoodacademy.com/frequently-asked-questions/#field-trips
     W9: https://therealfoodacademy.com/files/W92014.pdf  
     Cert of Ins: https://therealfoodacademy.com/files/CertificateofInsuranceforCookingwithKidsgeneral2017.pdf  

     * FIRE: Field Trip / Confirmation Open Nag
     * FIRE: Field Trip / Headcount Reminder
     * TAG-: (Field Trip . Booking Complete)
     * TAG-: (Field Trip . Final Headcount Provided)
     * FIRE: Field Trip / Push To Bookeo
     * END: -> Exit
   * **NO** Path:  
     * WAIT: Until contact has tag (Field Trip . Details . Received)
     * IF/ELSE: Contact does not have the tag (Field Trip . Details . Received)
       * **YES** Path: 
         * SEND: Notification to TRFA
         * END: -> Exit
       * **NO** Path: 
         * END: -> Exit

_Visual representation of the **Field Trip / Init** automation_
![Field Trip / Init](/img/field-trip-init-1.jpg)
![Field Trip / Init](/img/field-trip-init-2.jpg)


## Field Trip / Confirmation Open Nag
 * TRIGGER: Started manually by other automations or admin
 * WAIT: 1 day
 * IF/ELSE: Has not opened Field Trip Confirmation of Booking  
   * **YES** Path:
     * SEND: Field Trip Confirmation Nag
     >Links:  
     FAQ: https://therealfoodacademy.com/frequently-asked-questions/#field-trips
     W9: https://therealfoodacademy.com/files/W92014.pdf  
     Cert of Ins: https://therealfoodacademy.com/files/CertificateofInsuranceforCookingwithKidsgeneral2017.pdf  

     * WAIT: 2 days
     * SEND: Field Trip Confirmation Nag
     >Links:  
     FAQ: https://therealfoodacademy.com/frequently-asked-questions/#field-trips
     W9: https://therealfoodacademy.com/files/W92014.pdf  
     Cert of Ins: https://therealfoodacademy.com/files/CertificateofInsuranceforCookingwithKidsgeneral2017.pdf  

     * WAIT: 2 days
     * GOAL: Has opened either confirmation email
     * IF/ELSE: Has opened either confirmation email
       * YES Path: END: -> Exit
       * NO Path: 
         * SEND: Notification email to TRFA
         * END: -> Exit
   * **NO** Path: END: -> Exit  

_Visual representation of the **Field Trip / Confirmation Open Nag** automation_
![Field Trip Confirmation Open Nag](/img/field-trip-confirmation-open-nag-1.jpg)
![Field Trip Confirmation Open Nag](/img/field-trip-confirmation-open-nag-2.jpg)

## Field Trip / Push To Bookeo
 * TRIGGER: Field Trip Info 1 content changes (updated)
 * IF/ELSE: (Field Trip Info 1) is not blank
   * **YES** Path: 
     * WEBHOOK: https://trfaapi.com/v3/class/fieldtrip 
     * END: -> Exit
   * **NO** Path: END -> Exit

_Visual representation of the **Field Trip / Push To Bookeo** automation_
![Field Trip Push To Bookeo](/img/field-trip-push-to-bookeo.jpg)


## Field Trip / Headcount Reminder
 * TRIGGER: 4 days before (Field Trip Date 1)
 * IF/ELSE: Already has tag (Field trip . Start Reminder Sequence)
   * **YES** Path -> End -> Exit
   * **NO** Path:
     * TAG+: (Field Trip . Start Remidner Sequence)
     * WAIT: Until 3 days before (Field Trip Date 1)
     * SEND: Request Headcount email
      >Links:  
      https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%PARTICIPANTS_1%&guests=%FIELD_TRIP_FINAL_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%&update=true  
     * WAIT: Until 2 days before (Field Trip Date 1)
     * SEND: Request Headcount email
      >Links:  
      https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%PARTICIPANTS_1%&guests=%FIELD_TRIP_FINAL_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%&update=true  
     * WAIT: Until 1 days before (Field Trip Date 1)
     * SEND: Request Headcount email
      >Links:  
      https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%PARTICIPANTS_1%&guests=%FIELD_TRIP_FINAL_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%&update=true  
     * GOAL: We got the tag (Field Trip . Final Headcount Provided)
     * TAG-: (Field Trip . Start Remidner Sequence)
     * IF/ELSE: (Field Trip Final 1) is blank
       * **YES** Path:
         * SEND: Notification email to TRFA
         * END: -> Exit
       * **NO** Path: END -> Exit

_Visual representation of the **Field Trip / Headcount Reminder** automation_
![Field Trip / Headcount Reminder](/img/field-trip-headcount-reminder-1.jpg)
![Field Trip / Headcount Reminder](/img/field-trip-headcount-reminder-2.jpg)
![Field Trip / Headcount Reminder](/img/field-trip-headcount-reminder-3.jpg)


## Field Trip / Request Feedback
 * TRIGGER: It's the date of the Field Trip Date 1
 * IF/ELSE: Tag exists (Field Trip . Feedback . Provided)
   * **YES** Path: -> Exit
   * **NO** Path: 
     * TAG+: (Field Trip . Feedback . Requested)
     * SEND: Email requesting feedback
       >Links:  
        https://therealfoodacademy.com/feedback-on-field-trips/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%
     * WAIT: 3 days
     * SEND: 2nd email requesting feedback
       >Links:  
        https://therealfoodacademy.com/feedback-on-field-trips/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%
     * TAG-: (Field Trip . Feedback . Requested)
     * END: -> Exit

_Visual representation of the **Field Trip / Request Feedback** automation_
![Field Trip / Request Feedback](/img/field-trip-request-feedback-1.jpg)
![Field Trip / Request Feedback](/img/field-trip-request-feedback-2.jpg)

## Field Trip / Nag For Details 
In the event a field trip is booked via Bookeo or Facebook first, there are no details about the trip. We need that, so send them an email requesting they visit the field trip details form to complete that.  

>UPDATED: 1/14/2020 -> Remove this when doco is updated TODO

 * TRIGGER: Tag added (Field Trip . Purchased)
 * WAIT: 2 hours 
 * IF/ELSE Has tag (Field Trip . Details . Received)
   * **YES** Path: END -> Exit
   * **NO** Path: 
     * SEND: Field Trip Nag for Details
      >Links:  
	  Field Trip Details (https://therealfoodacademy.com/field-trip-details/)  
	 * WAIT: 2 days
     * SEND: Field Trip Nag for Details
      >Links:  
	  Field Trip Details (https://therealfoodacademy.com/field-trip-details/)  
	 * WAIT: 2 days
	 * GOAL: Tag added (Field Trip . Details . Received)
	 * IF/ELSE No Tag (Field Trip . Details . Received)
	   * **YES** Path: Send AC Notification to Art
	   * **NO** Path: End -> Exit
	   
_Visual representation of the **Field Trip / Nag For Details** automation_
![Field Trip / Nag for Details](/img/field-trip-nag-for-details-1.jpg)	   
![Field Trip / Nag for Details](/img/field-trip-nag-for-details-2.jpg)	   

## Field Trip / Headcount Received 
 * TRIGGER: Tag added (Field Trip . Final Headcount Provided)
 * SEND: (Notification only) Send details to Art
 * WAIT: Until 1 day before Field Trip
 * SEND: Reminder email to customer  
   >Link:  
   https://therealfoodacademy.com/frequently-asked-questions/#field-trips  
 * TAG-: (Field Trip . Final Headcount Provided)
 * END: -> Exit

_Visual representation of the **Field Trip / Received Headcount** automation_
![Field Trip / Received Headcount](/img/field-trip-received-headcount.jpg)

## Field Trip / FU Next Year

# Field Trip Final Headcount (id: 16)
Gravity Forms "edit" link  
https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Phone:6}&position={Your Position:5}&school={Name of School or Organization:8}&address={Address (Street Address):9.1}&city={Address (City):9.3}&state={Address (State / Province):9.4}&zip={Address (ZIP / Postal Code):9.5}&participants={Participants:19}&guests={Final Headcount:10}&ageRange={Age Range:11}&update=true

ActiveCampaign Headcount Update Link (testing)  
https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&position=%POSITION%&school=%SCHOOL_NAME%&address=%ADDRESS_1%&city=%CITY%&state=%STATE%&zip=%ZIP%&participants=%FIELD_TRIP_PARTICIPANTS_1%&guests=%FIELD_TRIP_FINAL_1%&ageRange=%FIELD_TRIP_AGE_RANGE_1%&update=true  

Testing Real Link:  
https://therealfoodacademy.com/field-trip-final-headcount-and-details/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&position=Administrator&school=Miami+Dade+Middle+School&address=8832+W+South+Beach&city=Miami&state=Florida&zip=33138&participants=55&guests=43&ageRange=7-14&update=true  

Testing location: 
https://therealfoodacademy.com/find-trip-final-headcount-ac/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&position=Administrator&school=Miami+Dade+Middle+School&address=8832+W+South+Beach&city=Miami&state=Florida&zip=33138&participants=43&guests=55&ageRange=7-14&update=true  

# Testing - Video

