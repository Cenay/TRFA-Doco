# Mommy (And Daddy) And Me
Bookeo ID: 46  
Bookeo Product: 2147NUJWL15695777311  

Customers can book this event with TRFA on one of the four available dates during the year. There is a [TRFA landing page](https://therealfoodacademy.com/mommy-and-me/) that explains the service, but booking **begins** on Bookeo. The customer checks out on Bookeo, and Bookeo sends a payload to our webhook /booking/create. 
 
Bookeo link: https://bookeo.com/cookingwithkidsmiami?type=2147NUJWL15695777311  

## Current Process
The process (for the customer) is in this order:
 1) Visit Mommy and Me page, select "Register for our Mommy and Me Class"
 2) Complete booking form with number of adults, kids    
 3) API -> Creates or updates our ActiveCampaign contact record and adds tags
 4) AC -> Automation, FAQ, reminders

API should insert this contact into list id: 1 (All Contacts)  

Tags: 
 * 52: Mommy and Me . Purchased
 * 17: Client Type . Adults  
 * 20: Client Type . Kids  


# Tag Handling
## API Adds These Tags
Currently, the tags being added are hardcoded, however, since the class name is unique and not one of the multi-titled types, we can insert the tagging in the bookeo_ac_tag pivot table. I have done so.    

_Currently being hardcoded to the ActiveCampaign record._  
 * 52: Mommy and Me . Purchased

> UPDATE REQUEST:  
Update the API so that it "looks up" and creates the tags from the bookeo_ac_tags table, and inserts what it finds there. 

## API to use bookeo_ac_tags
Now contains the three tags shown below.  
 * 52: Mommy and Me . Purchased
 * 17: Client Type . Adults  
 * 20: Client Type . Kids  

## Title Tag
API will also construct the title tag from parsing the title, removing the price (and everything to the right of it), converting to Camel case, and adding the year.  
> NOTE: This is NOT currently implemented in either V1 or V3  

_See example below:_  
MOMMY (OR DADDY) AND ME $35 PER PERSON   
becomes  
Mommy (or Daddy) and Me 2019  

> If the title tag doesn't exist, it should be created in ActiveCampaign.  

# Links
These are the various related links for the Mommy and Me event/service

Mommy and Me: https://therealfoodacademy.com/mommy-and-me/  
Booking button: https://bookeo.com/cookingwithkidsmiami?type=2147NUJWL15695777311  

# Booking History
Use the title created in the Title Tag logic above as the name to insert into the booking history (in addition to the date).  

> Should appear in history as:  
 Mommy (Or Daddy) and Me 2019 - 12/06/2019  

# Automation Notes  
This is the text representation of the automation steps and logic for the Mommy and Me sequence(s). This section is coming soon.  

### Mommy and Me / Init

### Mommy and Me / Reminder
### Mommy and Me / Request Feedback
### Mommy and Me / FU Next year