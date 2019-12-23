# Utilities In The API or Automation
These are the things we need (or have) to automate in some way. Some will be in the ActiveCampaign automation section, and some will be within our API. 

## API: Daily Cleanup (formerly Bubble Up)
---
We are now tracking multiple dates within a type (kids class, adult day, adult evening, summer camp). Since the automations are designed to use only 1 date in testing and reminders, we need to "bubble up" the newer dates into that position. 

Since we don't want to lose that data totally (when a date is removed), we'll create a new custom field that represents a "history" of the dates. Before removing it, append the type of date and the actual date to the history field. 

Revisions: 
 * When moving the Kids Party 2 info into Kids Party Info 1, extract the Party For: {NAME} and place in the field "Kids Party Name 1"

**NOTE: Field created in ActiveCampaign. Called "Booking History"**  
Sample: 
```
Previous Bookings:  
Summer Camp 2019 - 06/10/2019  
Summer Camp 2019 - 07/08/2019  
Kids Classes - 07/06/2019
```

Gravity Forms -> Inserts records to AC -> Temp Fields  
AC -> Automation -> Process Temp Info  
Process Temp Info -> Send to Group 1, or Group 2 -> Process the names accordingly  
ALSO -> Process birthdate -> Into the known Group 1 or Group 2  
  -> Basically calling this... /util/changekidsnames  
  /util/changekidsnames -> process both the name and the birthdate?  

### /util/changekidsnames
---
Check that the Kids Party Name 1 is present as any of Name 1, Name 2, Name 3 and if not, insert to next available name (if allowed) and if not allowed, insert a record to the transaction log to highlight the need for a 4th name. (Any part of the name) Compare (any part) Kids Party Name 1 to each of Name 1, 2, 3. (Future revisions SHOULD allow checking multiple "locations" for the name to move to the kids name fields like Saturday Classes, Gingerbread House Decorating, etc)  

Needs to deal with this possibility:
 * Jonah (10/25/13) and Jude (10/05/15) Werner
 * Henry and Hugh
 * Henry & Hugh, Eliva
 * Tallen Rogers

## Booking Updated
When items sent to the /bookings/update endpoint, we need to adjust the AC records. grab the original ('created') and the new log record ('updated'), use created to find the "date to change", update THAT date in the AC field set to match the newly updated date from 'updated' record. 

Notes from meeting (10/31/2019)
  Booking Updated in Bookeo by Admin
   * LOG: New webhook = Updated
   * Lookup the original (created), grab the date
   * Determine which "set" to udpate
   * Create contact note in the language that explains that change
     * Party date manually updated from 11/1/2019 to 11/15/2019 by admin in Bookeo
  * Add a tag to contact record (195: Utility . Manually Update)

### Automation: Set a tag when more than 1 kid
---
When we receive more than one kid's name (Name 1, Name 2 and Name 3), should auto-set a tag for Multi-Child Family. Automation will monitor the Name 2 (and Name 3) fields, and when one of them set or updated, add the tag. 

### Automation: Set a tag when more than 1 date booked for Summer Camp
---
When we receive more than one booking for a customer within a single Summer Camp program (year), should auto-set a tag for "Multi-Week Summer Camp". Automation to monitor the 2nd Week, 3rd Week, 4th Week and 5th Week fields. When any of them added or changed, insert the tag. 

# Push To Bookeo
There is an automation in ActiveCampaign for each of the Adult Party, Kids Party, Field Trips, and Summer Camp. Each is set to call the Push when it's attendent INFO field changes. (ie: Adult Party Info 1)

### Webhooks in API
> Adult Party:  
https://trfaapi.com/v3/class/adultsparty  

> Kids Party:  
https://trfaapi.com/v3/class/kidsparty

> Field Trips:  
https://trfaapi.com/v3/class/fieldtrip

> Summer Camp:  
https://trfaapi.com/v3/class/summercamps

# Automation Details
### Adult Party / Push Info To Bookeo
 * TRIGGER: Field change for either (Adult Party Info 1) or (Adult Party Final 1)
 * IF/ELSE: Adult Party Info 1 is not blank
   * Yes -> Call webhook https://trfaapi.com/v3/class/adultsparty  -> END: -> Exit
   * No -> END: -> Exit

_Visual of the automation_  
![Adult Party Automation](/img/adult-party-push-to-bookeo.jpg)

### Kids Party / Push Info To Bookeo
 * TRIGGER: Field change for either (Kids Party Info 1) or (Kids Party Final 1)
 * IF/ELSE: Kids Party Info 1 is not blank
   * Yes -> Call webhook https://trfaapi.com/v3/class/kidsparty  -> END: -> Exit
   * No -> END: -> Exit

_Visual of the automation_  
![Kids Party Automation](/img/kids-party-push-to-bookeo.jpg)

### Field Trips / Push Info To Bookeo
 * TRIGGER: Field change for either (Field Trip Info 1) or (Field Trip Final 1)
 * IF/ELSE: Field Trip Info 1 is not blank
   * Yes -> Call webhook https://trfaapi.com/v3/class/fieldtrip  -> END: -> Exit
   * No -> END: -> Exit

_Visual of the automation_  
![Field Trip Automation](/img/field-trip-push-to-bookeo.jpg)

### Summer Camp / Push Info To Bookeo
 * TRIGGER: Field change for (1st Week)
 * IF/ELSE: Name 1 is not blank
   * Yes -> Call webhook https://trfaapi.com/v3/class/summercamps -> END: -> Exit
   * No -> END: -> Exit

_Visual of the automation_  
![Summer Camp Automation](/img/summer-camp-push-to-bookeo.jpg)

### Booking History

> WEBHOOK:  
https://trfaapi.com/v3/util/generate-booking-history/%SUBSCRIBERID%
