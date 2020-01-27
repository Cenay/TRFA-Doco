# API 
The TRFA API is located at https://trfaapi.com and has three versions.  

## V1  
Originally written to take Bookeo data and send it to InfusionSoft. The Field Trips event type is still on version 1, so all the webhooks are still on version 1 as Bookeo doesn't allow us to receive one type of booking to one webhook, and another on a different one.  
Once all the booking types are present in V3 (see below), all of Bookeo's webhooks can be updated to point to the new version. 

## V2
Originally written to take FareHarbor booking data and send it to InfusionSoft. This version was never fully complete and was abandoned when TRFA decided not to pursue the relationship with FareHarbor. There will be no documentation on it. 

## V3
Written to take Bookeo data and send it to ActiveCampaign. As mentioned earlier, Field Trips are still being processed by V1, so for now, all the calls to V1 are being re-rerouted to V3 for those that are present in V3. 


# UPDATES
_Good candidate for queue (?)_  

Endpoint: https://trfaapi.com/v3/bookings/update  

Ideally, when a update happens, we have enough information to correctly update dates (bubble up), booking history (parse and rebuild), deals (maybe), tags (remove one and replace with new one). 

In the case  of manually changed class in Bookeo, needs to remove the first class tag, add the new class tag, and update the Kids Class Date (X) date. 