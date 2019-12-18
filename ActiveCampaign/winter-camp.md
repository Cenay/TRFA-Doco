# Winter Camp
Bookeo ID: 40  
Product ID: 214PLUHFY157CE44EA78  
Custom Field (More Info): 214PLUHFY157CE44EA78_L9WNXY3W 

### Requirements
The winter camp event is held each winter, and lasts about 2 weeks. Our automation's goal is to capture details about their kids (for future followup and offers), notify them of the FAQs and other info they need to have a good winter camp experience. We also will follow up after the camp to solicit feedback.  

## PROCESS
### Customer
 1) The customer will start at the [TRFA site](https://therealfoodacademy.com/), on the [Winter Camp](https://therealfoodacademy.com/the-best-winter-camp-in-miami/) page.
 2) Once they click the **Bookeo Book** button and "book" a week of camp, they will complete payment on the Bookeo interface (embedded into site).
 3) They will receive the email asking (from AC, see below) for more information and will complete that as well. (This is a Gravity Form which will push/update data to the ActiveCampaign contact record that matches the email address).  Note: These insertions update the Kids Info sets which include: Name, Birth Date, Allergies and More Info. There are 3 kid 'sets'. 
 4) After camp session ends, customer gets a new email from AC requesting feedback. Email they receive sends them to the [feedback](https://therealfoodacademy.com/feedback-on-kids-winter-camp/) page.

### API
Bookeo will package up the booking data and call our API (via webhook). The API will fire off and insert them into ActiveCampaign with new tag(s).  
     
*Note: Since Bookeo will only call one webhook on a booking, this is an all or nothing type of thing. We can't flip the version until ALL the classes and events are ready.* 

   Tags set in ActiveCampaign (from **bookeo_ac_tag** for Winter Camp [**89**])
   * 212: Winter Camp . Purchase . 2019
   * 20: Client Type . Kids
   * 213: Winter Camp . Purchased . Need Details

Fields sent via API for Winter Camp 
   * First Name
   * Last Name
   * Email
   * Person Type -> Customer  (default, every booking)
   * Phone
   * 1st Week (unless it has a value, then 2nd week, unless it has a value, etc)
 
1) ActiveCampaign will respond to the "tag" event **Winter Camp . Purchased . Need Details** which the API adds 

## GRAVITY FORMS (id: 11)
Form located here:  
https://therealfoodacademy.com/winter-camp-details/  

Form submission should redirect to the [FAQ page](https://therealfoodacademy.com/frequently-asked-questions/#summer-camp) for the winter camp program  

We want to ask them for more info, but not require them to complete the name, email, phone fields again. If we have info on the kids, we want to provide that as well. Create a link in ActiveCampaign that pushes that data via query strings that looks like this: 

>Winter Camp "Details" Gravity Link  
https://therealfoodacademy.com/winter-camp-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%

Where this is appended to the default of name, email and phone  
```
&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%
&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%
&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%
```
Test Link: 
https://therealfoodacademy.com/winter-camp-details/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=928-978-1919&name1=Kinsley&dob1=8-29-2012&allergies1=None&more1=Loves+Cooking&name2=Tallen&dob2=7-20-2006&allergies2=None&more2=&name3=&dob3=&allergies3=&more3=


numKids (GF field 11) = number of kid entries to display

In order to "select" the number of kids on the details form, insert this code to the functions file to preselect based on data incoming. 
```
// Eval numKids and "select" or "check"
add_filter( 'gform_field_value_numKids', function ( $value ) {
	if ( rgget( 'name3' ) ) {
		$value = '3 Kids';
	} elseif ( rgget( 'name2' ) ) {
		$value = '2 Kids';
	} elseif ( rgget( 'name1' ) ) {
		$value = '1 Kid';
	}
	return $value;
} );
```

# Bookeo: Winter Camp

>WINTER CAMP WEEKLY $350 (id: 41 )
WINTER CAMP DAILY $85 (id: 89)
---

**Bookeo Winter Camp Weekly**  
Product ID: 214PLUHFY157CE44EA78  
Custom Field (More Info): 214PLUHFY157CE44EA78_L9WNXY3W  

**Bookeo Winter Camp Daily**  
Product ID: 2149YFN6E16E5BB74EF8  
Custom Field (Experience): 2149YFN6E16E5BB74EF8_KNCMLPJX  
Custom Field (Details For Camp): 2149YFN6E16E5BB74EF8_JLMEML6N  

---

## API
The API should perform three things on each new (or updated) booking for the Summer Camp / Winter Camp programs.  
 * Create (or update) contact record in ActiveCampaign
 * Add Tags to contact based on tags found in pivot table 
 * Sort dates into correct fields (even if there are existing dates) 

### Create/Update Contact

---
Like pretty much every class or event sent from Bookeo, the ActiveCampaign record should update the person type field with "Customer".  Should insert to the List: All Contacts in addition to the defaut fields of name, email and phone.    

### Add Tags To Contact 

---
Tags for the Winter Camp program are in the bookeo_ac_tags table, and should be inserted from there and not hardcoded.  


**WINTER WEEKLY : API should insert these tags into the contact record for Weekly:**  
Tags:  
20: Client Type . Kids  
212: Winter Camp . Purchase . 2019  
213: Winter Camp . Purchased . Need Details  

**WINTER DAILY : API should insert these tags into the contact record for Daily:**  
Tags:  
20: Client Type . Kids  
212: Winter Camp . Purchase . 2019  
213: Winter Camp . Purchased . Need Details    
**209: Winter Camp . Day Pass**  

---

## Sort Program Dates

---
The Bookeo to API integration should send the customer records (or update existing) with camp start date (in one of 5 fields if weekly, and one of 3 fields if daily). 

API should also "sort" the incoming dates into 1st week, 2nd week, 3rd week, 4th week and 5th week. Recap: Should move 2 up to 1 if the new date is BEFORE the original date. In other words, should not insert into date 2 if date 1 has already passed. (Use date 1 instead after bubbling up the info).  

**Daily Camp processing:**  
Only additional thing needed from API for Daily Camp processing, is the insertion of the extra tag (209: Winter Camp . Day Pass) which  is a part of the bookeo_ac_tag table. 

# Automation Notes
This is the text representation of the automation steps and logic for the Winter Camp sequences. The process is **exactly** the same for Summer Camp. 

## Winter Camp / Request Info

---
 * TRIGGER: TAG+: (Winter Camp . Purchased . Need Details) added by API
 * LIST+: All Contacts
 * LIST+: Winter Camp (id: 12)
 * WAIT: 10 minutes
 * SEND: Email requesting more info on kids
   > Link: (provide their details)  
   https://therealfoodacademy.com/winter-camp-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%
 * WAIT: 1 day
 * SEND: Email (2nd) requesting more info on kids
   > Link: (Winter Camp details)  
   https://therealfoodacademy.com/winter-camp-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%

   > Link: (Frequently Asked Questions)  
   https://therealfoodacademy.com/frequently-asked-questions/#summer-camp
 * WAIT: 1 day
 * SEND: Email (3rd) requesting more info on kids
   >Link: (Winter Camp details)  
   https://therealfoodacademy.com/winter-camp-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%

   >Link: (Frequently Asked Questions)  
   https://therealfoodacademy.com/frequently-asked-questions/#summer-camp
 * GOAL: Tag applied (Winter Camp 2019 . Details . Received)
 * IF/ELSE: Has tag (Winter Camp 2019 . Details . Received)
   * YES Path: 
     * FIRE: _Winter Camp / Init_
     * END: -> Exit
   * NO Path:
     * NOTIFICATION: Send email to TRFA that we are still missing kid info 
     * END: -> Exit

## Winter Camp / Init

---
 * TRIGGER: None (must be placed in this automation)
 * TAG+: (Winter Camp 2019) <- This will need to be updated each year
 * IF/ELSE [1]: Has tag (Winter Camp 2019 . Details . Received)
   * YES PATH:
     * TAG-: (Winter Camp . Purchased . Need Details)
     * GOAL: Ready for Winter Camp Reminders
     > Goal condition:  
    Contact has tag (Winter Camp 2019 . Details . Received) and Contact does not have tag (Winter Camp . Purchased . Need Details)
     * TAG+: (Winter Camp . Start Reminder Sequence)
     * FIRE: _Winter Camp / Push To Bookeo_  (new endpoint https://trfaapi.com/v3/class/weekly-camp/winter)
     * IF/ELSE [2]: 1st Week is before current date
     * YES Path: 
        * WAIT: Until 1st Week is current date
        * FIRE: _Winter Camp / Request Feedback_
        * END: -> Exit
     * NO Path:  
        * NOTIFICATION: Send email to Cenay that we hit a situation where the 1st Week date is NOT before the current date at the time of INIT, and this should NOT be possible. 
        * WAIT: Until 1st Week is current date (to gather the contacts)
        FIRE: _Winter Camp / Request Feedback_ 
        * END: -> Exit
   * NO Path:
     * NOTIFICATION: Send email to Cenay, this shouldn't be possible
     * WAIT: Until contact has tag (Winter Camp 2019 . Details . Received)
     * GOTO: Yes Path of IF/ELSE [1]

## Winter Camp / Push To Bookeo
This automation is setup so that whenever 1st Week value changes, this automation is called. Since this "pushed" the kid info to Bookeo for the roster, we can't let it continue if we don't have at least NAME_1  
TODO: Update the automations to reflect the correct name once Khurram gets back to me. 

---
 * TRIGGER: Contact's Field 1st Week changes
 * IF/ELSE Name 1 is not blank
   * YES Path: 
     * WEBHOOK: POST https://trfaapi.com/v3/class/wekkly-camps/winter-camps (Asked Khurram to change this to ./weekly-camps/winter)
     * END: -> Exit
   * NO Path:
     * END: -> Exit

## Winter Camp / Reminders

---
 * TRIGGER: Tag added (Winter Camp . Start Reminder Sequence) <- comes from init
 * WAIT: Until 8 days before 1st Week (at 10:00am)
 * SEND: Email to remind them the session is starting soon
   >Link: (Frequently Asked Questions)  
   https://therealfoodacademy.com/frequently-asked-questions/#summer-camp

   >Link: (Winter Camp details)  
   https://therealfoodacademy.com/winter-camp-details/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%&phone=%PHONE%&name1=%NAME_1%&dob1=%BIRTH_DATE_1%&allergies1=%ALLERGIES_1%&more1=%MORE_ABOUT_1%&name2=%NAME_2%&dob2=%BIRTH_DATE_2%&allergies2=%ALLERGIES_2%&more2=%MORE_ABOUT_2%&name3=%NAME_3%&dob3=%BIRTH_DATE_3%&allergies3=%ALLERGIES_3%&more3=%MORE_ABOUT_3%
 * TAG-: (Winter Camp . Start Reminder Sequence)  
 * END: -> Exit

## Winter Camp / Request Feedback

---
 * TRIGGER: None (called in Winter Camp / Init on the day the session starts)
 * WAIT: 5 days (added to sequence on start date of session)
 * SEND: Email requesting feedback on the session
   >Link: (leave a review)  
   https://therealfoodacademy.com/feedback-on-kids-winter-camp/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%
 * WAIT: 2 days
 * SEND: Email requesting feedback one more time
   >Link: (leave a review)  
   https://therealfoodacademy.com/feedback-on-kids-winter-camp/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%
 * GOAL: Visited Winter Camp Feedback Page
 * END: -> Exit

## Winter Camp / Pre-Winter FU  

---
 * TRIGGER: Wait until current month is September
 * IF/ELSE: Has any of these tags: Winter Camp 2018, .. 2019 
   * SEND: Email reminding them winter break is coming  




# DEV NOTES

## Support Notes (ActiveCampaign)
Tim Hole requested our notes and API calls (and responses) so they could reproduce the issue. Documented below: 

Call out via Postman:
```
PUT https://trfa.api-us1.com/api/3/fieldValues/21
```

Body:
```
{
    "fieldValue": {
        "contact": 1,
        "field": 21,
        "value": "2019-06-24"
    }
}
```
Full response received back: 
```
{
    "contacts": [
        {
            "cdate": "2019-05-29T14:20:20-05:00",
            "email": "cenay@cenaynailor.com",
            "phone": "(928) 978-1919",
            "firstName": "Cenay",
            "lastName": "Nailor",
            "orgid": "0",
            "orgname": "",
            "segmentio_id": "",
            "bounced_hard": "0",
            "bounced_soft": "0",
            "bounced_date": "0000-00-00",
            "ip": "167773991",
            "ua": "okhttp/3.14.1",
            "hash": "a1b19ba8052d9daf5a779b65d49a8c75",
            "socialdata_lastcheck": "0000-00-00 00:00:00",
            "email_local": "",
            "email_domain": "",
            "sentcnt": "7",
            "rating_tstamp": "2019-06-14",
            "gravatar": "3",
            "deleted": "0",
            "anonymized": "0",
            "adate": "2019-06-21T14:16:13-05:00",
            "udate": "2019-06-20T14:47:20-05:00",
            "edate": "2019-06-11T18:48:45-05:00",
            "deleted_at": "0000-00-00 00:00:00",
            "created_utc_timestamp": "2019-05-29 14:20:20",
            "updated_utc_timestamp": "2019-06-20 14:47:20",
            "links": {
                "bounceLogs": "https://trfa.api-us1.com/api/3/contacts/1/bounceLogs",
                "contactAutomations": "https://trfa.api-us1.com/api/3/contacts/1/contactAutomations",
                "contactData": "https://trfa.api-us1.com/api/3/contacts/1/contactData",
                "contactGoals": "https://trfa.api-us1.com/api/3/contacts/1/contactGoals",
                "contactLists": "https://trfa.api-us1.com/api/3/contacts/1/contactLists",
                "contactLogs": "https://trfa.api-us1.com/api/3/contacts/1/contactLogs",
                "contactTags": "https://trfa.api-us1.com/api/3/contacts/1/contactTags",
                "contactDeals": "https://trfa.api-us1.com/api/3/contacts/1/contactDeals",
                "deals": "https://trfa.api-us1.com/api/3/contacts/1/deals",
                "fieldValues": "https://trfa.api-us1.com/api/3/contacts/1/fieldValues",
                "geoIps": "https://trfa.api-us1.com/api/3/contacts/1/geoIps",
                "notes": "https://trfa.api-us1.com/api/3/contacts/1/notes",
                "account": "https://trfa.api-us1.com/api/3/contacts/1/account",
                "customerAccount": "https://trfa.api-us1.com/api/3/contacts/1/customerAccount",
                "organization": "https://trfa.api-us1.com/api/3/contacts/1/organization",
                "plusAppend": "https://trfa.api-us1.com/api/3/contacts/1/plusAppend",
                "trackingLogs": "https://trfa.api-us1.com/api/3/contacts/1/trackingLogs",
                "scoreValues": "https://trfa.api-us1.com/api/3/contacts/1/scoreValues"
            },
            "id": "1",
            "account": null,
            "customerAccount": null,
            "organization": null
        }
    ],
    "fieldValue": {
        "contact": 1,
        "field": 21,
        "value": "2019-06-24",
        "cdate": "2019-05-30T13:34:54-05:00",
        "udate": "2019-06-21T14:17:04-05:00",
        "links": {
            "owner": "https://trfa.api-us1.com/api/3/fieldValues/24/owner",
            "field": "https://trfa.api-us1.com/api/3/fieldValues/24/field"
        },
        "owner": 1,
        "id": "24"
    }
}
```

## BUBBLE UP
This will process all dates at the same time. Summer Camp, then Kids Class, then Adult Day, then Adult Evening
/v3/utils/bubbleUpDates <- Khurran renamed this to /v3/utils/cleanup

## ASK FOR DOB
Follow up question for the Summer Camp session for %NAME_1%

Hey  Mary Jane , 

Yeah, this email is late...we're in the middle of switching out our email marketing tool and a few small glitches have plagued us, but we are getting a handle on it, finally. 

Our booking program asked for %NAME_1%  and %NAME_2%'s ages, and not their birth dates. Like I said, glitches. If you are willing, we would like to know their birth dates so we can send over some promotions and discounts on parties and things like that. You can reply to this email, or just complete the Summer Camp Detail form (again)... either way works. 
https://therealfoodacademy.com/summer-camp-details/

Thanks in advance! I hope that  Nika  and  Jana  are enjoying their Summer Camp session! 

Sincerely, 

Cenay 
(on behalf of Art and Maria at The Real Food Academy)


# Automation Notes  

[Q] **Can we:** Can the feedback form be gravity? If they submit, we could update tags about feedback. Use Javascript to update a hidden field (with number of stars). What sets the feedback "out" trigger? Tag? Date? Loop counter?

[Q] Can I CSS the "stars" inside Gravity to match something like this? 


## First email: 
```
I'm so glad you signed-up for The Real Food Academy Summer Camp program. It's something we know your kid(s) will love.

Between now and the first day of camp, you will probably think of some question for us. We love talking to our customers and you are always welcome to call us, however, before you do, you may want read our [Frequently Asked Questions](https://therealfoodacademy.com/frequently-asked-questions/#summer-camp) to review some of the questions we receive most.

Since we want all our camp participants to have the best week of their life, we would love to find out a little bit more about your child (or children), and any allergies or special needs they have.

IMPORTANT: Please click the [Summer Camp Details](https://therealfoodacademy.com/summer-camp-details/) link to give us more information about your kid(s), this is a REQUIREMENT to complete your booking!

I look forward to seeing you at The Real Food Academy soon!

Warm regards, 
```

NOTES on Winter Camp differences
Winter Camp pivot
41 20
41 212
41 213
