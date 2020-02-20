# Kids Class
There are two situations we will be dealing with. The current situation with Bookeo sending the class called "SATURDAY 90 MINUTES KIDS CLASS" and the upcoming method of sending a "menu" name as a part of the class name (ie: Farm To Table - Kids Class)

UPDATE: The new title tag method is now fully implemented and there should not be any new bookings for the Saturday 90 Minutes Kids Class. All current and future bookings will tag with the title of the class with the year of the date added in this form: "Farm To Table 2020"  

The class menu or title will be in upper case and the class category will be in title case. Example below:
```
FARM TO TABLE - Kids Class
```

Currently V1 of the API takes data from Bookeo and inserts it into InfusionSoft, with the exception of Summer Camp, which gets redirected to the V3 endpoint and passes the payload received on the V1 endpoint.

UPDATE: The API is complete and ALL events, classes, parties and field trips are now being processed by version 3 and are sending into ActiveCampaign. 


## Special Handling
We have a situation we need special handling for, since we are now tracking multiple "dates" that a parent can sign a kid up for. We are now tracking up to three different dates for a single contact record, for kids class. We mark them into the fields Kids Class 1, Kids Class 2 and Kids Class 3. 

The *special handling* is when a booking comes in for a date (7/29/2019), the API should check FIRST to see if there is a date in Kids Class 1. It should write it to the blank field Kids Class 2 **unless** it's the SAME date as the Kids Class 1 date. 

### SAMPLES 
Kids Class 1: 7/29/2019 <- Date present AND it's the same (no update)  
Kids Class 2:  
Kids Class 3:  

Kids Class 1: 7/22/2019 <- Date present and it is NOT the same  
Kids Class 2: 7/29/2019 <- New date goes here  
Kids Class 3:  

Kids Class 1: 6/15/2019 <- Date present and it is in the past, push 7/29/2019 over this date. 
Kids Class 2:  
Kids Class 3:  

## Requirements

API Should insert these tags (for scenario 1):
20 Client Type . Kids
 7 Saturday Class . Purchased
40 Saturday Class . Start

API Should create/add this additional "tag" for scenario 2 (examples below)
Farm To Table 2020 
Summer Delight 2020

#### Fields sent via API for Kids Class  
   * First Name
   * Last Name
   * Email
   * Person Type -> Customer  (default, every booking)
   * Phone

#### Set one or more of these dates (Don't let them repeat)
 * Kids Class 1 
 * Kids Class 2
 * Kids Class 3

## DEBUG
1) Do we need the kids class title for email marketing for any reason -> Need a place to store that. 
2) https://trfa.activehosted.com/app/contacts?limit=100&waitid=240 -> Kids Class Reminders, why do I have 78 records?

## BOOKEO DEBUG
1) Why can't we delete an old "date" for an old class where the bookings have already happened?
2) How can the entries be displayed in current date order?


## Payload Samples
Kids Class 
```
{"itemId":"14906242111608","timestamp":"2019-06-24T14:28:00-04:00","item":{"bookingNumber":"14906242111608","eventId":"214R9AAAC14450221C13_214EUNKAK15FB825AB8E_2019-07-27","startTime":"2019-07-27T09:45:00-04:00","endTime":"2019-07-27T11:30:00-04:00","customerId":"214PLNNN4163747A6FB2","customer":{"credit":{"amount":"1360","currency":"USD"},"acceptSmsReminders":true,"numBookings":4,"numCancelations":3,"numNoShows":0,"member":false,"id":"214PLNNN4163747A6FB2","firstName":"Cenay","lastName":"KidsClass","emailAddress":"kidsclass@bycenay.com","phoneNumbers":[{"number":"928-978-1919","type":"mobile"}],"streetAddress":{"countryCode":"US"},"creationTime":"2018-05-18T14:19:00-04:00","gender":"unknown"},"title":"Cenay KidsClass","participants":{"numbers":[{"peopleCategoryId":"Cchildren","number":1}],"details":[{"personId":"PUNKNOWN","peopleCategoryId":"Cchildren","categoryIndex":1}]},"canceled":false,"accepted":true,"sourceIp":"67.61.95.121","creationTime":"2019-06-24T14:16:00-04:00","productName":"SATURDAY 90 MINUTE KIDS COOKING CLASS ","productId":"214R9AAAC14450221C13","price":{"totalGross":{"amount":"35","currency":"USD"},"totalNet":{"amount":"35","currency":"USD"},"totalTaxes":{"amount":"0","currency":"USD"},"totalPaid":{"amount":"35","currency":"USD"},"taxes":[]},"options":[{"id":"214R9AAAC14450221C13_JECKTJWU","name":"My Child\u0027s Experience In The Kitchen","value":"Beginner"},{"id":"214R9AAAC14450221C13_Y9F967TT","name":"Details For Saturday Cooking Class","value":"Not sure where this is going?"}],"privateEvent":false}} 
```

Thursday Evening Adult
```
{"itemId":"14906081754627","timestamp":"2019-06-08T05:29:00-04:00","item":{"bookingNumber":"14906081754627","eventId":"214JPKR77157446E581A_214UTWF4U16AF5690C71_2019-07-11","startTime":"2019-07-11T19:00:00-04:00","endTime":"2019-07-11T22:00:00-04:00","customerId":"214MRJEEL163E05F4C2A","customer":{"credit":{"amount":"0","currency":"USD"},"acceptSmsReminders":true,"numBookings":3,"numCancelations":1,"numNoShows":0,"member":false,"id":"214MRJEEL163E05F4C2A","firstName":"ELLEN","lastName":"BERGER","emailAddress":"D007078C@YAHOO.COM","phoneNumbers":[{"number":"305-213-3934","type":"mobile"}],"streetAddress":{"countryCode":"US"},"creationTime":"2018-06-08T13:08:00-04:00","startTimeOfPreviousBooking":"2018-09-20T19:00:00-04:00","gender":"unknown"},"title":"ELLEN BERGER","participants":{"numbers":[{"peopleCategoryId":"Cadults","number":2}],"details":[{"personId":"PUNKNOWN","peopleCategoryId":"Cadults","categoryIndex":1},{"personId":"PUNKNOWN","peopleCategoryId":"Cadults","categoryIndex":2}]},"canceled":false,"accepted":true,"sourceIp":"23.115.36.165","creationTime":"2019-06-08T05:29:00-04:00","creationAgent":"Maria Cummins","productName":"NAUGHTY VEGAN ","productId":"214JPKR77157446E581A","price":{"totalGross":{"amount":"130","currency":"USD"},"totalNet":{"amount":"130","currency":"USD"},"totalTaxes":{"amount":"0","currency":"USD"},"totalPaid":{"amount":"130","currency":"USD"},"taxes":[]},"privateEvent":false}}
```

Wednesday Lunch Adult
```
{"itemId":"14904058698806","timestamp":"2019-04-05T14:57:00-04:00","item":{"bookingNumber":"14904058698806","eventId":"214FHK39F15B00C6F2AE_214JXNJUU16979D3553E_2019-04-10","startTime":"2019-04-10T11:30:00-04:00","endTime":"2019-04-10T13:30:00-04:00","customerId":"214RTPP33169EEDD0988","customer":{"credit":{"amount":"0","currency":"USD"},"acceptSmsReminders":true,"numBookings":1,"numCancelations":0,"numNoShows":0,"member":false,"id":"214RTPP33169EEDD0988","firstName":"Valquiria","lastName":"B Dores","emailAddress":"vbdores@gmail.com","phoneNumbers":[{"number":"7863401546","type":"mobile"}],"streetAddress":{"countryCode":"US"},"creationTime":"2019-04-05T14:57:00-04:00","gender":"unknown"},"title":"Valquiria B Dores","participants":{"numbers":[{"peopleCategoryId":"Cadults","number":1}],"details":[{"personId":"PUNKNOWN","peopleCategoryId":"Cadults","categoryIndex":1}]},"canceled":false,"accepted":true,"sourceIp":"174.61.1.157","creationTime":"2019-04-05T14:56:00-04:00","productName":"KOREAN FEAST","productId":"214FHK39F15B00C6F2AE","price":{"totalGross":{"amount":"45","currency":"USD"},"totalNet":{"amount":"45","currency":"USD"},"totalTaxes":{"amount":"0","currency":"USD"},"totalPaid":{"amount":"45","currency":"USD"},"taxes":[]},"privateEvent":false}}
```
