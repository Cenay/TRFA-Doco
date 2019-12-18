# Gingerbread House Decorating
Bookeo ID: 44  
Bookeo Product ID: 21437AH77157D7DDA2E8  
Bookeo Custom Field ID: None  

### Requirements
Once a year, TRFA holds an event for the kids (parents can watch). Customer books the event in the same manner as the Mommy and Me classes (Special Event). Booking the event will insert the client into ActiveCampaign (if not found), add them to the Special Events list (id: 10), tag them (from the pivot table), update the booking history field. Since this event happens only once a year, there are no alternate "sets" of dates. Should push to the only field (id: 101)  

## Bookeo: GINGERBREAD HOUSE DECORATING (id: 44)
Landing Page: https://therealfoodacademy.com/gingerbread-house-decorating/  
Booking link: https://bookeo.com/cookingwithkidsmiami?type=21437AH77157D7DDA2E8  

>Tags: 
20: Client Type . Kids  
190: Gingerbread House Decorating 2019  (title tag logic, not in table)  
274: Gingerbread House Decorating . Purchased  

## Current Process
The process (for the customer) is in this order.  
 1. Visit the landing page and click "Book"
 2. Complete the booking form (no details required)
 3. API -> Creates or updates our ActiveCampaign contact record, adds person type = Customer, adds tags (from pivot table), insert title tag + date to Booking History.    
 4. AC -> Automation (confirmation), FAQ, reminders



## Gravity Form Location
None  

## Gravity Form Info (id: NONE)
The fields documented for this form, including Id's and GF Field Personalization's.  

## Links
Landing Page: https://therealfoodacademy.com/gingerbread-house-decorating/  
Booking link: https://bookeo.com/cookingwithkidsmiami?type=21437AH77157D7DDA2E8  

**Redirects Required**  
None  

# ActiveCampaign 
Settings, field id's, tags related to this booking type.  

List Id: 10 : Special Events  
Custom Field: 101 : Gingerbread House  
Tags related to Booking:  
20: Client Type . Kids  
189: Gingerbread House Decorating 2018  
190: Gingerbread House Decorating 2019  
272: Gingerbread House Decorating . Feedback . Provided  
273: Gingerbread House Decorating . Feedback . Requested  
274: Gingerbread House Decorating . Purchased  
275: Gingerbread House Decorating . Start  
API Should Insert (20, 274)

# Automation Notes

## Automation 1

## Automation 2

## Automation 3

## Automation 4

## Testing Links
The links that can be used to test the endpoint

# DEBUG  
Currently in progress and tasks being debugged. 

 * Import from IFS for Gingerbread House and then tag _Gingerbread House Decorating 2018_ -> Send to List: Special Events
When they don't exist first

List: Special Events, All Contacts  
Tags:  
Gingerbread House Decorating 2018  
Imported from IFS  

Then run reports to see those that have kids class and tag them, adult class, tag them, adult party, tag them, summer camp, etc  

Kids Class 1 (has date) -> Client Type . Kids 
>UPDATE: Created new field to hold the date (id: 101 Gingerbread House)