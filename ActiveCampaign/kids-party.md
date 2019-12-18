# Kids Party 
Bookeo ID: 45  
Bookeo Product ID: 214TMEXMR1518911A3CA  

Parents can book a party event at Bookeo and pay a $200 deposit to hold the date. The "details" will need to be completed to get the headcount, entree, desserts, etc. This will be with Gravity Forms so we can insert them into the lists. The form will redirect to the Bookeo "Book" page for Kids Party. 

QUESTION: Can Gravity include a link back to the "details" info? (Yes, implemented)

## Current Process
The process (for the customer) is in this order:
 1) Visit kids party page, select "package"
 2) Complete package form with details
 3) Book date and time and pay deposit
 4) -> Automation, FAQ, reminders, final head count
 5) Provide final head count closer to the party date

 >UPDATE:  
 Art wants to add "Facebook Sales" to his arsenal, which means many of the new "bookings" will start with the deposit, and need to ask for the details in the confirmation phase. This update will require changes to several existing automations, in addition to creating new automations for the "Nag For Details" process.  

Form submission should redirect to the Bookeo page (widget) as defined here: http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA

>UPDATE: Conditionally send them to booking page if GF Update Status = false and publish "thanks" note to page when GF Update Status = true.  

---

## Bookeo: KIDS BIRTHDAY PARTY (id: 45)

---
API should insert these tags into the contact record: 
Tags: 57, 62, 70 (confirmed)

57: Kids Party . Purchased  
62: Client Type . Kids Party  
70: Kids Party . Booking Complete  

Bookeo Custom field: 
Party Info: 214TMEXMR1518911A3CA_NH6LEYJ3

## API
The Bookeo to API should send the customer record (or update existing) with Party date and time to appropriate field. Should move 2 up to 1 if the new date is BEFORE the original. Should not insert into date 2 if the date 1 has already passed. (Use Date 1 instead after bubbling up the info)

NOTE: When comparing the Set 1 data to Set 2 data, extract and compare: Birthdate, Party For info and skip looking at Add on's at all. 

## Tags
Should add these tags: 
 * Client Type . Kids Party
 * Kids Party . Purchased
 * Kids Party . Booking Complete


# Process On Final Head Count
When the automation sends out the email to the client, include a "switch" to determine estimate or final head count. Populate all the data into the fields that is known. (The data is in one single field, can we write into the API call a parser to construct the required fields into fields, construct the URL, and populate the form)  

>NOTE: This changed. They can update the party details, but there is now a second form for final headcount. The API and Push To Bookeo have been updated to append the Final Headcount to the "details" section written back to Bookeo.   

Form currently is first, last, email, final headcount, *I understand* agreements.  

To make a change, they visit the form again and submit new data.  

Ideal -> We provide a link that they click to "update" the party details, it picks up the data we already know (in Kids Party Info 1 field) and parse it back into the form. (API: Build the URL)

# Feedback Request
Emails should send them to the new https://therealfoodacademy.com/feedback/?type=Kids+Party location with the type completed based on the "type" of event we are requesting. 

This can NOT be implemented until we have a way to display the "reviews" from Gravity Forms via Easy Posts. Until then, redirect the automation to this link: 
https://therealfoodacademy.com/feedback-on-kids-birthday-parties/

## Can We..
Create an endpoint that will take the "data" from ActiveCampaign, parse out the various "data points" from the Party Info, and create then call a URL that passes that data? 

In other words:
*Customer receives an email from ActiveCampaign that says "if you need to make changes to the party, click here", and the link they click calls the API, which builds the correct URL with query strings (ie: first=customer-name&last=last-name&email=email&party-package=the_one_they_selected&all-the-other-fields) and behaves as if they just clicked the constructed link?*

>Answer: YES  
See new endpoint called /util/url/kids-party/%SUBSCRIBERID%. It construct's a new URL and parses out all the info from the Party Info field, and add's as parameters to the base URL. (Sample below)  
https://trfaapi.com/v3/util/url/kids-party/295

>UPDATE: Because Art wants the "process" to ALSO include the opposite order (Booking completed before the form is), we are adding changes to the automations and API. I will add the **UPDATE** note where it impacts the doco. 


# LINKS
These are the various pages that are a part of the automation and flow of the kids party process

Customer Starts Here:
https://therealfoodacademy.com/book-a-cooking-birthday-party/

They pick a cuisine and one of these is used:  
https://therealfoodacademy.com/birthday-party-package-details-budding-chef/  
https://therealfoodacademy.com/birthday-party-package-details-executive-chef/  
https://therealfoodacademy.com/birthday-party-package-details-top-chef/  

>Each of these inserts a record into ActiveCampaign along with the TAG **Kids Party . Details . Received**

Complete the form and get redirected to the booking page: 
http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA (embedded within the /calendar/ page)

>UPDATE: Conditionally send them to booking page if GF Update Status = false and publish "thanks" note to page when GF Update Status = true.

At this point, there is a record in AC, it has been tagged that we have the details. No date exists. 

IF the booking is completed, the API updates the record to add the new tags (see above), add the date and time, change the person type to "Customer" and start the Kids Party / Init automation. 

>UPDATE: New Automation for Kids Party / Nag To Book that will send the customer back to the party page to complete the form. The forms (all three) have to be revised to conditionally send them to either the booking page or the Thank you message, if they complete the form (first time vs again).  
# Gravity Forms (id: )

## Passed Parameter Field Names

 * address
 * city
 * state
 * zip



# Automation Notes
We now need a step _before_ our initial steps since they might be booking via Bookeo _before_ they complete the party forms. This will be true when they stumble upon the calendar items and when Art begins his "Facebook Booking" campaign.  

### Kids Party / Nag For Details

---
 * TRIGGER: Tag added (Kids Party . Purchased) <- comes from Bookeo/API
 * IF/ELSE: Contact does not have (Kids Party . Details . Received) tag
   * YES Path: 
     * WAIT: 10 minutes
     * IF/ELSE: Kids Party Info 1 is blank
       * YES Path: 
         * SEND: Email reminding them to provide the details
           >Link: (Kids Birthday Party details)  
           https://therealfoodacademy.com/book-a-cooking-birthday-party/
         * WAIT: 2 days
         * SEND: Email reminding them to produve the details
           >Link: (Kids Birthday Party details)  
           https://therealfoodacademy.com/book-a-cooking-birthday-party/
         * GOAL: (Kids Party . Details . Received) tag is present
         * WAIT: 20 minutes
         * IF/ELSE: Contact does not have (Kids Party . Details . Received) tag
           * YES Path: 
             * NOTIFICATION: Send TRFA email about lack of details
             * END: -> Exit
           * NO Path: 
             * FIRE: ProcessTempInfo automation
             * NO: END -> Exit             
       * NO Path:
         * END: -> Exit
   * NO Path: 
     * END: -> Exit

_Visual representation of the automation:_ 
![Kids Party / Nag For Details](/img/kids-party-nag-for-details.jpg)


### Kids Party / Init

---
 * TRIGGER: Tag added (Kids Party . Booking Complete) <- API adds 
 * GOAL: Hold until we have enough to start the init 
   >(condition: has tags )  
   Kids Party . Booking Complete  
   Kids Party . Details . Received  
   OR  
   Kids Party . Bypass Init Hold  
 * WAIT: 20 minutes
 * IF/ELSE: Tag exists (Kids Party . Details . Received)
   * YES Path:
     * LIST: Add to All Contacts
     * SEND: Email to confirm the booking

     >Link: (check out our FAQ page)  
     https://therealfoodacademy.com/frequently-asked-questions/#birthday-parties
    
     >Link: (update birthday party details)  
     https://trfaapi.com/v3/util/url/kids-party/%SUBSCRIBERID% 

     * FIRE: Kids Party / Confirmation Open Nag
     * FIRE: Kids Party / Headcount Reminder
     * TAG-: Kids Party . Booking Complete (to get ready for next party)
     * FIRE: Kids Party / Push To Bookeo
     * END: -> Exit
   * NO Path: 
     * WAIT UNTIL: Tag Kids Party . Details . Received (up to 10 days)
     * IF/ELSE: Still no (Kids Party . Details . Received) tag
       * YES: Notify TRFA staff -> Exit
       * NO: End -> Exit

_Visual representation of the automation:_ 
![Kids Party / Init](/img/kids-party-init.jpg)

### Kids Party / Confirmation Open Nag

---
Customers are calling when they don't receive an email from TRFA, despite the fact we did. To remove the worry from Art, we are now testing that they OPENED the email, and if they don't, resend again, and again. If they still haven't opened, notify TRFA staff and send the link to the customer in ActiveCampaign so they can call them. 

 * TRIGGER: None. Push into there from the (Kids Party / Init) automation
 * WAIT: 1 day
 * IF/ELSE: Customer has not opened (Kids Party Confirmation of Party Booking) email
   * YES Path: 
     * SEND: Email that is mirror of original confirmation email. 
     >Link: (check out our FAQ page)  
     https://therealfoodacademy.com/frequently-asked-questions/#birthday-parties
    
     >Link: (update birthday party details)  
     https://trfaapi.com/v3/util/url/kids-party/%SUBSCRIBERID% 

     * WAIT: 1 day
     * SEND: Email that is mirror of prior two attempts. 
     >Link: (check out our FAQ page)  
     https://therealfoodacademy.com/frequently-asked-questions/#birthday-parties
    
     >Link: (update birthday party details)  
     https://trfaapi.com/v3/util/url/kids-party/%SUBSCRIBERID% 
     
     * WAIT: 1 more day
     * IF/ELSE: Customer has still not opened any of the three confirmations
       * YES Path: Send notification to TRFA staff -> Exit
       * NO Path: END: -> Exit

   * NO Path: 
     * END: -> Exit

Kids Party / Nag To Book

---
 * TRIGGER: Starts with tag (Kids Party . Details Received)
    WAIT: 10 Minutes
    IF Tag Exists KIDS PARTY . PURCHASED (set by API) -> Leave
    Otherwise: 
        Send FU email to ask them to complete the booking -> http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA

Kids Party / Headcount Reminder

---
    Starts with "5 Days before The Party date"
    WAIT: Until current date is 4 days before Kids Party Date
    Send email requesting Headcount -> Link to headcount form -> https://therealfoodacademy.com/kids-party-final-headcount/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%
    WAIT: Until current date is 2 days before Kids Party Date
    Send email requesting Headcount (2)
    WAIT: Until current date is day before party
    Send email (IMPORTANT) Request Headcount
    GOAL: -> Start "Kids Party / Headcount Received"

Kids Party / Headcount Received

---
    Starts with tag KIDS PARTY . FINAL HEADCOUNT RECEIVED (or called via another automation)
    WAIT: day before Kids Party Date
    Send email "Reminder" and FAQ and recap of party details

    NOTE: Original TRFA Page with headcount
    https://therealfoodacademy.com/birthday-party-final-headcount/  

    New TRFA Page with headcount
    https://therealfoodacademy.com/kids-party-final-headcount/  

Kids Party / Request Feedback

---
    WAIT 1 Day
    Check if "Kids Party . Feedback . Provided" tag is present
    IF NO -> Add tag "Kids Party . Feedback . Requested"
    Send email: (1) Feedback request
    WAIT 3 days
    Send email: (2) Feedback request
    WAIT 2 days
    Send email: (3) Feedback request
    -> Goal (either) tag "Kids Party . Feedback . Provided" added
        (or) Third email sent with no interaction with customer
    LINK for Feedback Request (UPDATE this once the new review method implemented)
    https://therealfoodacademy.com/feedback-on-kids-birthday-parties/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%


## Links
These are the various links from the site: 
Original 
Kids Party: https://therealfoodacademy.com/book-a-cooking-birthday-party/
Booking Budding: (originals converted to new method, see below for links)
Headcount: https://therealfoodacademy.com/birthday-party-final-headcount/

New (now all current):  
Kids Party (nc): https://therealfoodacademy.com/book-a-cooking-birthday-party/
Booking Budding: https://therealfoodacademy.com/birthday-party-package-details-budding-chef/
Booking Executive: https://therealfoodacademy.com/birthday-party-package-details-executive-chef/
Booking Top: https://therealfoodacademy.com/birthday-party-package-details-top-chef/
Headcount: https://therealfoodacademy.com/kids-party-final-headcount/

# Gravity Forms
One form for each package. Each form should have two confirmations. A default one which sends them to the Bookeo Booking widget, and a conditional one that fires in the event the GF Update Status field is **true**  

# BUDDING CHEF (id: 7)
>Link To Test the form with:  
https://therealfoodacademy.com/birthday-party-package-details-budding-chef/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&birthday_name=Tallen&dob1=7/20/2006&location=The+Real+Food+Academy&guests=15&entree=Pizza&dessert1=Decorate+Cupcakes&dessert2=Chocolate+Covered+Fruit+Skewers&salmon=1&cheese=1&waldorf=1&proscuitto=1&quiche=1&caprese=1&hummus=1&guacamole=1&tzatziki=1&cookies=1&fondue=1&brownies=1&balloons=1&extra-hour=1&zumba=1&update=true

**Budding: Passed parameters names and field ID's**  
first : 2.3 {Name (First):2.3} *  
last  : 2.6 {Name (Last):2.6} *  
email : 3 *  
phone : 4 *  
type : 72 (hidden)
birthday_name 9 *  
dob1 : 11 *  
info1 : 21 * (hidden)
location (dropdown) : 7 *
 * The Real Food Academy
 * My Location
   If My Location
    * address : 71 *
guests : 12 *
entree : 13 *
dessert1 : 14 *
dessert2 : 15 *

--- 

### Budding: Add on's

---
salmon : 22  
cheese : 23  
waldorf : 25  
proscuitto : 30  
quiche  : 33  
caprese : 36  
hummus : 39  
guacamole : 42  
tzatziki : 45  
cookies : 48  
fondue : 51  
brownies : 54  
balloons : 63  
extra-hour : 67  
zumba : 69.1  
special : 70 
update : 73 (default false) 

## Budding: Confirmations:  
 * Default - Redirect -> http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA  
 * Client - Thanks - No redirect  
 Conditional (based on GF Update Status = true)  
 Type: Text on screen - See content below:  

   >Thanks {Name (First):2.3}!  
We got your updated party information, and will adjust your order accordingly.   
Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-budding-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Budding Entree:13}&dessert1={Budding Dessert 1:14}&dessert2={Budding Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69}&update=true">update party link</a> again to make those changes.    
{Party Info:21}

## Budding: Notifications: 

 * Admin Notification  -> Sends entry details to admin_email  
 * Client Email Notification of Form Details -> Sends email to client with subject line of **Party Detail Changes Confirmation - The Real Food Academy** (see content below)   
    >Thanks {Name (First):2.3}!  
We got your updated party information, and will adjust your order accordingly.   
Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-budding-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Budding Entree:13}&dessert1={Budding Dessert 1:14}&dessert2={Budding Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.    
{Party Info:21}

# EXECUTIVE CHEF (id: 8)
>Link to test the form with:   
https://therealfoodacademy.com/birthday-party-package-details-executive-chef/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&birthday_name=Tallen&dob1=7/20/2006&location=The+Real+Food+Academy&guests=15&entree=Fettuccine+Pasta&dessert1=Strawberry+Shortcake&entree2=The+Traditional+Pizza&dessert2=Chocolate+Covered+Fruit+Skewers&salmon=1&cheese=1&waldorf=1&proscuitto=1&quiche=1&caprese=1&hummus=1&guacamole=1&tzatziki=1&cookies=1&fondue=1&brownies=1&balloons=1&extra-hour=1&zumba=1&update=true

**Executive: Passed parameters names and field ID's**  
first : 2.3 {Name (First):2.3} *  
last  : 2.6 {Name (Last):2.6} *  
email : 3 *  
phone : 4 *  
type : 75 (hidden)
birthday_name 9 *  
dob1 : 11 *  
info1 : 21 * (hidden)
location (dropdown) : 7 *
 * If My Location  
 >address : 74 *  

guests : 12 *  
entree : 13 *  
dessert1 : 14 *  
entree2 : 71 *  
dessert2 : 15 *  

--- 

### Executive: Add on's

---
salmon : 22  
cheese : 23  
waldorf : 25  
proscuitto : 30  
quiche  : 33  
caprese : 36  
hummus : 39  
guacamole : 42  
tzatziki : 45  
cookies : 48  
fondue : 51  
brownies : 54  
balloons : 63  
extra-hour : 67  
zumba : 69.1  
special : 70  
update : 76

## Executive: Confirmations:  
 * Default - Redirect -> http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA  
 * Conditional - Text on screen - See content below: 

>Thanks {Name (First):2.3}!  
We got your updated party information, and will adjust your order accordingly.   
Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-executive-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Executive Entree:13}&dessert1={Executive Dessert 1:14}&entree2={Executive Entree:71}&dessert2={Executive Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.    
{Party Info:21}

## Executive: Notifications 

 * Admin Notification  -> Sends entry details to admin_email  
 * Client Email Notification of Form Details -> Sends email to client with subject line of **Party Detail Changes Confirmation - The Real Food Academy** (see content below)   
 ><p>Thanks {Name (First):2.3}!</p><p>We got your updated party information, and will adjust your order accordingly.</p><p>Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-executive-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Executive Entree:13}&dessert1={Executive Dessert 1:14}&entree2={Executive Entree:71}&dessert2={Executive Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.</p><p>BIRTHDAY PARTY INFO:<br/>=========================================</p><p>{Party Info:21}</p>

---

# TOP CHEF (id: 9)
>Link to test the form with:  
https://therealfoodacademy.com/birthday-party-package-details-top-chef/?first=Cenay&last=Nailor&email=cenay@cenaynailor.com&phone=9289781919&birthday_name=Tallen&dob1=7/20/2006&location=The+Real+Food+Academy&guests=15&entree=Fettuccine+Pasta&dessert1=Strawberry+Shortcake&entree2=The+Traditional+Pizza&dessert2=Chocolate+Covered+Fruit+Skewers&salmon=1&cheese=1&waldorf=1&proscuitto=1&quiche=1&caprese=1&hummus=1&guacamole=1&tzatziki=1&cookies=1&fondue=1&brownies=1&balloons=1&extra-hour=1&zumba=1&update=true

**Top: Passed parameters names and field ID's**  
first : 2.3 {Name (First):2.3} *  
last  : 2.6 {Name (Last):2.6} *  
email : 3 *  
phone : 4 *  
type : 75 (hidden)
birthday_name 9 *  
dob1 : 11 *  
info1 : 21 * (hidden)
location (dropdown) : 7 *
 * The Real Food Academy
 * My Location
   If My Location
    * address : 74 *
guests : 12 *
entree : 13 *
dessert1 : 14 *
entree2 : 71 *
dessert2 : 15 *

--- 

### Top: Add on's

---
salmon : 22  
cheese : 23  
waldorf : 25  
proscuitto : 30  
quiche  : 33  
caprese : 36  
hummus : 39  
guacamole : 42  
tzatziki : 45  
cookies : 48  
fondue : 51  
brownies : 54  
balloons : 63  
extra-hour : 67  
zumba : 69.1  
special : 70  
update : 76

## Top: Confirmations:  
 * Default - Redirect -> http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA  
 * Client - Thanks - No redirect   
 Conditionally sends based on GF Update Status = true  
 Text on screen - See content below:   

><p>Thanks {Name (First):2.3}!</p><p>We got your updated party information, and will adjust your order accordingly.</p><p>Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-top-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Top Entree:13}&dessert1={Top Dessert 1:14}&entree2={Top Entree 2:71}&dessert2={Top Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.</p><p>{Party Info:21}</p>


## Top: Notifications 

 * Admin Notification  -> Sends entry details to admin_email  
 * Client Email Notification of Form Details -> Conditionally send.  
 Sends email to client with subject line of **Party Detail Changes Confirmation - The Real Food Academy** (see content below)   
 >Thanks {Name (First):2.3}!  
We got your updated party information, and will adjust your order accordingly.   
Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-top-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Top Entree:13}&dessert1={Top Dessert 1:14}&entree2={Top Entree 2:71}&dessert2={Top Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.    
BIRTHDAY PARTY INFO:   
==========================================  
{Party Info:21}

---

## Gravity Form Handling Code (in plugin.php)
This is the custom code to handle the processing the form details into the Party Info field that ActiveCampaign needs. 

```
/** 
 * Package up all the data from the Budding Chef submission and 
 * place into the Party Info field that is passed back to ActiveCampaign
 * @author Cenay Nailor
 * @version 1.0.3
 */
function budding_chef_details_handler( $form ) { 
    
    $childName = rgpost(input_9);
    $dob       = rgpost(input_11);
    $guests    = rgpost(input_12);    
    $package   = 'Budding Chef Birthday Party';
    $entree    = rgpost(input_13);
    $dessert   = rgpost(input_14);
    $dessert2  = rgpost(input_15);
    $location  = rgpost(input_7);    
    $address   = PHP_EOL . rgpost(input_71_1) . PHP_EOL . rgpost(input_71_3) . ', ' . rgpost(input_71_4) . ' ' . rgpost(input_71_5);
    // 71.1 = Address 1
    // 71.3 = City
    // 71.4 = State
    // 71.5 = Zip

    $age       = calcAge($dob);

    $party_info  = 'Party For: ' . $childName . PHP_EOL;
    $party_info .= 'Party location: ' . $location . PHP_EOL;
    
    if (rgpost(input_71_1) <> '') {
        $party_info .= 'Address: ' . $address . PHP_EOL;
    }

    $party_info .= 'Birth date: ' . $dob . PHP_EOL;
    $party_info .= 'Age: ' . $age . PHP_EOL;
    $party_info .= 'Guests: ' . $guests . ' (Estimated Kids)' . PHP_EOL;
    $party_info .= 'Package: ' . $package . PHP_EOL;
    $party_info .= 'Entree: ' . $entree . PHP_EOL;
    $party_info .= 'Dessert: ' . $dessert . PHP_EOL;

    if ($dessert2 != 'None') {
        $party_info .= 'Extra Dessert: ' . $dessert2 . PHP_EOL;
    }
    // Add ons
    $party_info .= PHP_EOL . 'Add on\'s' . PHP_EOL;

    if ( rgpost(input_22) <> 0 ) {
        $party_info .= rgpost(input_22) . ' : Smoked Salmon with Capers' . PHP_EOL;
    }
    if ( rgpost(input_23) <> 0 ) {
        $party_info .= rgpost(input_23) . ' : Cheese Crackers' . PHP_EOL;
    }
    if ( rgpost(input_25) <> 0 ) {
        $party_info .= rgpost(input_25) . ' : Chicken Waldorf Sandwiches' . PHP_EOL;
    }
    if ( rgpost(input_30) <> 0 ) {
        $party_info .= rgpost(input_30) . ' : Proscuitto and Mozzarella Sandwiches' . PHP_EOL;
    }
    if ( rgpost(input_33) <> 0 ) {
        $party_info .= rgpost(input_33) . ' : Quiches' . PHP_EOL;
    }
    if ( rgpost(input_36) <> 0 ) {
        $party_info .= rgpost(input_36) . ' : Caprese Salad' . PHP_EOL;
    }
    if ( rgpost(input_39) <> 0 ) {
        $party_info .= rgpost(input_39) . ' : Hummus with Pita crackers' . PHP_EOL;
    }
    if ( rgpost(input_42) <> 0 ) {
        $party_info .= rgpost(input_42) . ' : Guacamole Dip with chips' . PHP_EOL;
    }
    if ( rgpost(input_45) <> 0 ) {
        $party_info .= rgpost(input_45) . ' : Tzatziki Dip with Pita bread' . PHP_EOL;
    }
    if ( rgpost(input_48) <> 0 ) {
        $party_info .= rgpost(input_48) . ' : Assorted Cookies' . PHP_EOL;
    }
    if ( rgpost(input_51) <> 0 ) {
        $party_info .= rgpost(input_51) . ' : Fruit and Chocolate Fondue' . PHP_EOL;
    }
    if ( rgpost(input_54) <> 0 ) {
        $party_info .= rgpost(input_54) . ' : Flourless Brownies' . PHP_EOL;
    }
    if ( rgpost(input_63) <> 0 ) {
        $party_info .= rgpost(input_63) . ' : Balloons' . PHP_EOL;
    }
    if ( rgpost(input_67) <> 0 ) {
        $party_info .= rgpost(input_67) . ' : Extra Hour' . PHP_EOL;
    }
    if ( rgpost(input_69_1) <> '' ) {
        $party_info .= '1 : Zumba' . PHP_EOL;
    }

    // Add the Special Instructions
    if ( rgpost(input_70) <> '' ) {
        $party_info .= 'Special Instructions: ' . PHP_EOL . rgpost(input_70) . PHP_EOL;
    }
    // Push the value into the field
    $_POST['input_21'] = $party_info;

    // Call an endpoint with the WordPress function -> https://developer.wordpress.org/reference/functions/wp_remote_request/
    
}
add_action( 'gform_pre_submission_7', 'budding_chef_details_handler', 10, 2 );
```
Yields something like this: 
```
Party For: Britene Nicole
Party location: The Real Food Academy
Actual birth date: 5/15/2004
Guests: 14 (Estimated)
Package: Executive Chef Birthday Party
Entree: The Traditional Pizza
Dessert: Chocolate Covered Fruit Skewers
Extra Entree: Fettuccine Pasta
Extra Dessert: Waffle and Fruit Towers

Add on's
1 : Smoked Salmon with Capers
1 : Chicken Waldorf Sandwiches
1 : Quiches
1 : Hummus with Pita crackers
1 : Tzatziki Dip with Pita bread
1 : Flourless Brownies
1 : Extra Hour
1 : Zumba
Special Instructions: 
Ignore Me too.
```

---
This example displays the listed text on all entries for form id 31 when viewing the Entry details.
https://docs.gravityforms.com/gform_entry_detail/

add_action( 'gform_entry_detail', 'add_to_details', 10, 2 );
function add_to_details( $form, $entry ) {
    if ( $form['id'] == 31 ) {
        echo '<div>This hook is used to add additional information to the details page for an entry.</div>';
    }
}

# Constructed Sample
*A sample of the Party info we will construct and send to AC*  
```
Party For: Brad
Party location: The Real Food Academy
Actual birth date: 2/11/1983
Estimated Guests: 48
Package: Budding Chef Birthday Party
Entree: Pizza
Dessert: Chocolate Covered Fruit Skewers
Extra Dessert: Decorate Cupcakes
4 : Smoked Salmon with Capers
3 : Cheese Crackers
2 : Chicken Waldorf Sandwiches
1 : Proscuitto and Mozzarella Sandwiches
2 : Quiches
3 : Caprese Salad
4 : Hummus with Pita crackers
3 : Guacamole Dip with chips
2 : Tzatziki Dip with Pita bread
1 : Assorted Cookies
2 : Fruit and Chocolate Fondue
3 : Flourless Brownies
4 : Balloons
3 : Extra Hour
1 : Zumba
Special Instructions: 
Pine nut allergies all around
```

# Notes from TRFA project Doco
## Kids Party
Process as it currently exists. Customer select the Kids Party from menu or page, select the "package" to buy, and complete the (Gravity Form) which details the items they want to include in the party. Form submission should redirect to the Bookeo page (widget) as defined here: http://bookeo.com/cookingwithkidsmiami?type=214TMEXMR1518911A3CA

### API
The Bookeo to API should send the customer record (or update existing) with Party date and time to appropriate field. Should move 2 up to 1 if the new date is BEFORE the original. Should not insert into date 2 if the date 1 has already passed. 

### Tags
Should add these tags: 
 * Client Type . Kids Party
 * Kids Party . Purchased
  