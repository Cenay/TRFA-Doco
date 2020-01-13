# Adult Party 
Bookeo ID: 47  
Bookeo Product ID: 214EWHUMF1491EC82C71  
Bookeo Custom Field (Details): 214EWHUMF1491EC82C71_4J4AMMPP 

## Requirements
Adult's and businesses can book a party with TRFA, selecting any one of (currently 3) 5 different cuisines. This starts the process and will insert the client into ActiveCampaign if not found, add them to the Adult Party list, and then tag them to await the API call that indicates the completion of the $200 booking deposit.  
 
## Current Process
The process (for the customer) is in this order:
 1) Visit Adult party page, select "cuisine"
 2) Complete cuisine form with details
    Gravity Form completion inserts/updates ActiveCampaign: 
    TAG: 179: Adult Party . Details Received      
    LIST: 7: Adult Party
 3) Book date and time and pay deposit
 4) -> Automation, FAQ, reminders, final head count
 5) Provide final head count closer to the party date

Gravity Form located here:
---
https://therealfoodacademy.com/provide-private-party-details/?menu=Asian+Cuisine  
Where the `?menu=Asian+Cuisine` or `?Latin+Cuisine` or `?Italian+Cuisine` as selected on the book an adult party form from the screen prior.   

Form submission should redirect to the Bookeo page (widget) as defined here: https://bookeo.com/cookingwithkidsmiami?type=214EWHUMF1491EC82C71  

>UPDATE: Adding new conditional logic to redirect them to a message on page, not the Booking page again (to eliminate confusion).  

## Bookeo: PRIVATE PARTY (id: 47)
API should insert these tags into the contact record:  
Tags:  
17: Client Type . Adults  
68: Client Type . Adult Party  
173: Adult Party . Booking Complete (fires: Adult Party / Init)  
174: Adult Party . Purchased  

Bookeo Custom field (Details):  
214EWHUMF1491EC82C71_4J4AMMPP  

## API
The Bookeo to API should send the customer record (or update existing) with Party date and time to appropriate field (Adult Party 1 set or Adult Party 2 set). Should move 2 up to 1 if the new date is BEFORE the original. Should not insert into date 2 if the date 1 has already passed. (Use Date 1 instead after bubbling up the info). 

## Format Of Push To Bookeo Content
The adult party details that needs to be pushed to Bookeo via the webhook `https://trfaapi.com/v3/class/adultsparty` is below. 

>Updated: Added new line to top, space between the date/time and the "type", remove the "Party Info:" from the heading, make all headers Title case.  (SPECIAL request to match the way you did it on the Cuisine line)

```
(new line)
Date: 99/99/9999
Time: 7:00pm
(new line)
Adult Party - Latin Cuisine
-------------------------------
Guests: 5 (Estimated)
Occasion: Friends Get Together
Wine Option: Silver Tier
Beer Option: None

LATIN Cuisine
--------------------------------
Salad: Mojito Tropical Salad
Appetizer: Seafood Causa with Huancaina Sauce
Entree: Lomo Saltado
Dessert: Pineapple Rum Cake and Ice Cream

SPECIAL Request
--------------------------------
Ignore me, more testing.
```

# Process On Final Head Count
ActiveCampaign automations send out reminders (if no Final value present) to customers that direct them to a TRFA page that requests the Final Headcount, which should update the customers record in AC, cause the automation to fire which would update the Bookeo booking record with the new "build and push" of info.  

Original page: https://therealfoodacademy.com/cooking-party-final-headcount-and-instructions/  
New page: https://therealfoodacademy.com/adult-party-final-headcount/  

Gravity Form: 13 (Adult Party Final Headcount)
ActiveCampaign Feed updates the Adult Party Final 1 field and sets the tag: Adult Party . Final Headcount Provided  
Form submission should display message on screen like below: 
```
<h3>Got It, Thanks!</h3>
<p>We'll update your party information to reflect that final headcount. Please note your final bill is based on this number.</p>
<p>We'll see you soon.</p>
```

## Links
These are the various links from the site:

**Original**  
Adult Party: https://therealfoodacademy.com/cooking-parties-in-miami/  
Business Party: https://therealfoodacademy.com/cooking-parties-for-businesses/  
Booking Latin: https://therealfoodacademy.com/book-a-cooking-party-latin-cuisine/  
Booking Asian: https://therealfoodacademy.com/book-a-cooking-party-asian-cuisine/  
Booking Italian: https://therealfoodacademy.com/book-a-cooking-party-italian-cuisine/  
Headcount: https://therealfoodacademy.com/cooking-party-final-headcount-and-instructions/  

**New**  
Adult Party: https://therealfoodacademy.com/cooking-parties-in-miami/  
Business Party: https://therealfoodacademy.com/cooking-parties-for-businesses/  
Booking Form: https://therealfoodacademy.com/provide-private-party-details/?menu=Latin+Cuisine  
Headcount: https://therealfoodacademy.com/adult-party-final-headcount/  

### Redirects Required
If necessary, these pages will be redirected through .htaccess (and related)
All three original booking forms to the new method

# Update Party Details Link
This is used in the email's sent from AC to allow customer to update their details
https://trfaapi.com/v3/util/url/adults-party/%SUBSCRIBERID%

QUESTION: What happens if client clicks the link, updates the party details and submits the form again, which pushes to the ActiveCampaign Temp fields again! The API isn't being called, so how do this get triggered/checked? 


To Complete Implementation: 
 * Done: Adult Party Form -> Party Info construction
 * Done: Adult Party automation in ActiveCampaign
 * Create new Business Party front page that leads to same "cuisine" pages (update tags/list)

Business Party Info that Differs
 * Gravity Form: Business Party 14 -> Contains different form (cloned) and additional tag
 * UPDATE: Business Party uses the same form (id: 2) as Adult Party and has conditional logic to 

# Automation Notes
This is the text representation of the automation steps and logic for the Adult Party sequences. The process is exactly the same for the Business Party. 

# Notifications Sent
When final headcount is received, send this notification to Art and Maria (Cenay)

**Subject line:**  
Adult Party Final Headcount received for %FIRSTNAME%  %LASTNAME% of %ADULT_PARTY_FINAL_1%  

**Body**
```
%FIRSTNAME% %LASTNAME% just submitted their final headcount of %ADULTS_PARTY_FINAL_1% for the party planned on %ADULT_PARTY_DATE_1% at %ADULT_PARTY_TIME_1%. 

The details of their party: 

%ADULT_PARTY_INFO_1%
```


# Adult Party / Init 
 * TRIGGER: Tag: (Adult Party . Booking Complete) added by API
 * SEND: Email to confirm the booking  
 * LIST: Subscribe customer to list "All Contacts"     
 * FIRE: Automation: Adults Party / Push Info To Bookeo (https://trfaapi.com/v3/class/adultsparty)  
 * TAG-: Adult Party . Booking Complete (resets for next booking)  
 * END: -> Exit automation  

# Adult Party / Nag To Book
This situation happens when the customer completes the Adult Party form, and does not then book the date/time through Bookeo. This will ask for a completion twice before giving up.  

  * TRIGGER: Tag (Adult Party . Details Received) added (comes from GF)  
  * WAIT: 120 Minutes  
  * IF/ELSE -> Adult Party Info 1 is not blank  
     * Yes -> IF/ELSE -> Do we have the tag (Adult Party . Purchased) (API  
       * Yes -> SEND: email reminding them to complete booking  
       * No -> End Automation (goal met, they booked)  
     * No -> Go back to the WAIT: 120 minutes  
  * SEND: Email to remind to book   
    > Link:  
    Book the date: https://bookeo.com/cookingwithkidsmiami?type=214EWHUMF1491EC82C71   
    Update party details: https://trfaapi.com/v3/util/url/adults-party/%SUBSCRIBERID%     
  * WAIT: 2 days  
  * SEND: 2nd email reminding them to book  
    > Link:  
    Book the date: https://bookeo.com/cookingwithkidsmiami?type=214EWHUMF1491EC82C71  
    Update party details: https://trfaapi.com/v3/util/url/adults-party/%SUBSCRIBERID%   
  * GOAL: (->) They completed the purchase (Adult Party . Purchased)  
  * IF/ELSE: contact still doesn't have (Adult Party . Purchased)  
    * Yes -> Send notification to Cenay (check what we know so we can construct a clean up routine)  
    * No -> END: -> Exit  
  *  END: -> Exit automation  

# Adult Party / Nag For Details
This situation happens when the customer books an Adult Party directly from the Bookeo widget or from the Facebook Bookeo widget, without completing the birthday party selection first. 

 * TRIGGER: Tag added (Adult Party . Purchased) <- comes from Bookeo/API
 * WAIT: 10 minutes
 * IF/ELSE: Contact does not have Adult Party . Details Received tag
   * YES Path: 
     * WAIT: 10 minutes
     * IF/ELSE: Adult Party Info 1 is blank
       * YES Path: 
         * SEND: Email reminding them to provide the details
           >Link: (Adult Private Party details)  
           https://therealfoodacademy.com/cooking-parties-in-miami/
         * WAIT: 2 days
         * SEND: Email reminding them to prodive the details
           >Link: (Adult Private Party details)  
           https://therealfoodacademy.com/cooking-parties-in-miami/
         * GOAL: Adult Party . Details Received tag is present
         * IF/ELSE: Contact does not have Adult Party . Details Received tag
           * YES Path: 
             * NOTIFICATION: Send TRFA email about lack of details
             * END: -> Exit
           * NO Path: 
             * END: -> Exit
       * NO Path:
         * END: -> Exit
   * NO Path: 
     * END: -> Exit


# Adult Party / Headcount Reminder
  * TRIGGER: Current date is 5 days before Adult Party Date 1
  * IF/ELSE -> We have (Adult Party . Start Reminder Sequence) tag
     * Yes -> Exit because contact is already in sequence
     * No -> Continue
  * TAG+: Adult Party . Start Reminder Sequence
  * WAIT: Until current date is 3 days before Adult Party Date 1
  * SEND: email requesting headcount -> 
  > Link:  
  Headcount form https://therealfoodacademy.com/adult-party-final-headcount/%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%  
  * WAIT: Until current date is 1 days before Adult Party Date 1  
  * SEND: email requesting headcount -> 
  > Link:  
  Headcount form https://therealfoodacademy.com/adult-party-final-headcount/%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL% 
  * GOAL: Tag inserted: (Adult Party . Final Headcount Provided)
  * TAG-: (Adult Party . Start Reminder Sequence)
  * END: -> Exit

# Adult Party / Push To Bookeo
  * TRIGGER: None - called by Adult Party / Init 
  * WEBHOOK: https://trfaapi.com/v3/class/adultsparty
  * END: -> Exit

# Adult Party / Request Feedback
  * TRIGGER: Date of the Adult Party Date 1 event  
  * IF/ELSE -> We have (Adult Party . Feedback . Requested) tag  
    * Yes -> End automation (they are already here)  
    * No -> Continue  
  * TAG+: (Adult Party . Feedback . Requested)  
  * SEND: Email requesting them to leave a review ->
  > Links:  
  Leave review: https://therealfoodacademy.com/feedback-on-adult-parties/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%  
   Recipes: https://therealfoodacademy.com/our-recipes/  
 * WAIT: 3 days
 * SEND: Email 2 requesting them to leave a review ->
  > Links:  
  Leave review: https://therealfoodacademy.com/feedback-on-adult-parties/?first=%FIRSTNAME%&last=%LASTNAME%&email=%EMAIL%  
  Recipes: https://therealfoodacademy.com/our-recipes/
  * GOAL: Either tag: (Visited Adult Party Feedback Page) or (Adult Party . Feedback . Provided)  
  * TAG-: (Adult Party . Feedback . Requested)
  * END: -> Exit

# Adult Party / Headcount Received
  * TRIGGER: Tag (Adult Party . Final Headcount Provided) is added  
  * IF/ELSE -> Adult Party Date 1 is on or before current date plus 1 (1 day before party date)  
    * Yes -> Continue YES Path  
    * No -> Continue NO Path  
  * YES PATH:  
    * WAIT: Until 1 day before Adult Party Date 1   
    * SEND: Email Reminder of party and FAQ info (email from Party > Reminder - The Real Food Academy -> Maria)  
    > LINK:  
    FAQ for Adult party: https://therealfoodacademy.com/frequently-asked-questions/#adult-party   
    * TAG-: (Adult Party . Final Headcount Provided) -> Resets for next party   
    * END: -> Exit  
  * NO PATH: (It's too late to send email)  
    * TAG-: (Adult Party . Final Headcount Provided) -> Resets for next party  
    * END: -> Exit  

_Screen grab of the Adult Party / Headcount Received ActiveCampaign automation_
![Headcount Received](/img/adult-party-headcount-received.jpg)


# Adult Party / FU Next Year


# ADULT PARTY (id: 2)
>Link To Test the form with:  
https://therealfoodacademy.com/provide-private-party-details/?first=Cenay&last=Nailor&email=cenay%40softwarethatrocks.com&phone=%28928%29+978-1919&guests=12&wine=Silver+Tier&beer=&menu=Latin+Cuisine&latin-salad=Romaine+Lettuce+Boats&latin-appetizer=Caribbean+Ceviche&latin-entree=Lomo+Saltado&latin-dessert=Flan&update=true&type=Business+Party&other=Retirement+and+Promotion&occasion=Other

**Adult Party: Passed parameters names and field ID's**  
first : 1.3 {Name (First):1.3} *  
last  : 1.6 {Name (Last):1.6} *  
email : 2 *  
phone : 3 *  
type : 29 (hidden) [choices: "Adult Party", "Business Party"]
info : 30 * (hidden) [holds party info]
guests : 4 *
occasion : 31 * (when type = adult party)
occasion : 34 * (when type = business party)
other : 32 (when they select other, this holds their answer)
wine : 22 * 
beer : 25 * 
menu : 5 (drop down)
asian-salad : 10 *
asian-appetizer : 11 *
asian-entree : 8 * 
asian-dessert : 12 *
latin-salad : 13 *
latin-appetizer : 14 * 
latin-entree : 15 * 
latin-dessert : 16 * 
italian-salad : 17
italian-appetizer : 18 *
italian-entree : 19 * 
italian-dessert : 20 * 
special : 27 
update : 33 * (default false) 
extra-hour : 36 (qty)
PRODUCT: extra hour : 35 (extra-hour field above tied to this field)

## Adult Party: Confirmations:  
 * Default - Redirect -> https://bookeo.com/cookingwithkidsmiami?type=214EWHUMF1491EC82C71
 * Client - Thanks - No redirect  
 Conditional (based on GF Update Status = true)  
 Type: Text on screen - See content below. Please note, because there are multiple menu's in use, we're using the gravity forms shortcode conditional's to display the proper links and merge tags. Because conditional shortcodes can not be nested, it's not possible to ALSO include the special logic for the occasion drop down, so it will be ignored. If they need to update, they can select it again.   

```
<p>Thanks {Name (First):1.3}!</p>  
<p>We got your updated party information, and will adjust your order accordingly.</p> 
[gravityforms action="conditional" merge_tag="{Choose Your Cuisine:5}" condition="is" value="Asian Cuisine"]<p>Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/provide-private-party-details/?first={Name (First):1.3}&last={Name (Last):1.6}&email={Email:2}&phone={Phone:3}&guests={Estimated # Guests:4}&wine={Wine Addon:22}&beer={Beer Addon:25}&menu={Choose Your Cuisine:5}&asian-salad={Asian Salad:10}&asian-appetizer={Asian Appetizer:11}&asian-entree={Asian Entree:8}&asian-dessert={Asian Dessert:12}&update=true&type={Party Type:29}&other={Other:32}">update party link</a> again to make changes.</p>
[/gravityforms]  

[gravityforms action="conditional" merge_tag="{Choose Your Cuisine:5}" condition="is" value="Latin Cuisine"]<p>Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/provide-private-party-details/?first={Name (First):1.3}&last={Name (Last):1.6}&email={Email:2}&phone={Phone:3}&guests={Estimated # Guests:4}&wine={Wine Addon:22}&beer={Beer Addon:25}&menu={Choose Your Cuisine:5}&latin-salad={Latin Salad:17}&latin-appetizer={Latin Appetizer:18}&latin-entree={Latin Entree:19}&latin-dessert={Latin Dessert:20}&update=true&type={Party Type:29}&other={Other:32}">update party link</a> again to make changes.</p>
[/gravityforms]

[gravityforms action="conditional" merge_tag="{Choose Your Cuisine:5}" condition="is" value="Italian Cuisine"]<p>Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/provide-private-party-details/?first={Name (First):1.3}&last={Name (Last):1.6}&email={Email:2}&phone={Phone:3}&guests={Estimated # Guests:4}&wine={Wine Addon:22}&beer={Beer Addon:25}&menu={Choose Your Cuisine:5}&italian-salad={Italian Salad:17}&italian-appetizer={Italian Appetizer:18}&italian-entree={Italian Entree:19}&italian-dessert={Italian Dessert:20}&update=true&type={Party Type:29}&other={Other:32}">update party link</a> again to make changes.</p>
[/gravityforms]
<p>PRIVATE PARTY INFO:<br/>
======================================<br/>
{Party Info:30}</p>

```

## Budding: Notifications: 

 * Admin Notification  -> Sends entry details to admin_email  
 * Client Email Notification of Form Details -> Sends email to client with subject line of **Party Detail Changes Confirmation - The Real Food Academy** (see content below)   
    >Thanks {Name (First):2.3}!  
We got your updated party information, and will adjust your order accordingly.   
Please <strong>carefully</strong> review your order below. This is your current order, if you need to make additions or changes, please visit the <a href="https://therealfoodacademy.com/birthday-party-package-details-budding-chef/?first={Name (First):2.3}&last={Name (Last):2.6}&email={Email:3}&phone={Cell Phone:4}&birthday_name={Birthday Child First Name:9}&dob1={Date of Birth:11}&location={Party Location:7}&guests={Estimated Guests:12}&entree={Budding Entree:13}&dessert1={Budding Dessert 1:14}&dessert2={Budding Dessert 2:15}&salmon={Qty Smoked Salmon:22}&cheese={Qty Cheese:23}&waldorf={Qty Waldorf:25}&proscuitto={Qty Proscuitto:30}&quiche={Qty Quiches:33}&caprese={Qty Caprese:36}&hummus={Qty Hummus:39}&guacamole={Qty Guacamole:42}&tzatziki={Qty Tzatziki:45}&cookies={Qty Cookies:48}&fondue={Qty Fondue:51}&brownies={Qty Brownies:54}&balloons={Qty Balloons:63}&extra-hour={Qty Extra Hour:67}&zumba={Add Zumba:69.1}&update=true">update party link</a> again to make those changes.    
{Party Info:21}

# DEBUG
In order to help determine which party set to "update" after a client updates the form data, we are adding a new Temp Event Update field. If the form is submitted initially, it's default value of false will be present, meaning pick 1 or 2 based on the current presence of values. The URL generator will be updated to add &update=true when the link is created, meaning that the Update status, passed back to ActiveCampaign will be true. 


Sample of what the Adult URL Generator creates  
```
https://therealfoodacademy.com/provide-private-party-details/?first=Cenay&last=Nailor&email=cenay%40cenaynailor.com&phone=%28928%29+978-1919&guests=45&occasion=Other&other=Combination+Birthday+Party/Holiday+Party&wine=Silver+Tier&beer=&menu=Asian+Cuisine&asian-salad=Vietnamese+Beef+Salad&asian-appetizer=Pan+Fried+Dumplings&asian-entree=Sushi+Galore&asian-dessert=Flourless+Brownies&update=true
```

ActiveCampaign: Add new field TEMP EVENT UPDATE
 * Push from Gravity Forms for Kids Parties (all three) and Adult Parties
 * Inserting new Temp Event Update (hidden GF field) Params: update

Forms to complete:  
 * Kids Budding Chef: GF Update Status -> Field ID 73 -> input_7_73
 * Kids Executive Chef: GF Update Status -> Field ID 76 -> input_8_76
 * Kids Top Chef: GF Update Status -> Field ID 76 -> input_9_76
 * Adult Party: GF Update Status -> Field ID 33 -> input_2_33
 * Business Party: GF Update Status -> Field ID 33 -> input_14_33
