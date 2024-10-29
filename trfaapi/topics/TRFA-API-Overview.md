# TRFA API Overview

This API is used by The Real Food Academy (TRFA) to allow communication between TRFA's Bookeo and ActiveCampaign accounts. When a customer purchases a party or event through their corporate site, Bookeo passes a payload to our webhooks, which processes the data and sends to ActiveCampaign (TRFA).   

> Do NOT Include any sensitive information in this documentation.  

## Versions

The TRFA API is located at https://trfaapi.com and has three versions.  

### V1  
https://trfaapi.com/v1/  

Originally written to take Bookeo data and send it to InfusionSoft. The Field Trips event type is still on version 1, so all the webhooks are still on version 1 as Bookeo doesn't allow us to receive one type of booking to one webhook, and another on a different one.
Once all the booking types are present in V3 (see below), all of Bookeo's webhooks can be updated to point to the new version.  

### V2
https://trfaapi.com/v2/  

___ 

Originally written to take FareHarbor booking data and send it to InfusionSoft. This version was never fully complete and was abandoned when TRFA decided not to pursue the relationship with FareHarbor. There will be no documentation on it.

### V3
https://trfaapi.com/v3/  

---  

Written to take Bookeo data and send it to ActiveCampaign for his business. Ported from V1 to V3 and completed and maintained by Khurram and Cenay.  

### V4
https://trfaapi.com/v4/  

---
Written to inject Franchises into our work flow. Art will be selling franchises to TRFA and will need each to have it's own Bookeo and ActiveCampaign accounts. When a purchase happens, the franchisee's Bookeo account will send to our webhooks for processing, and then be sent to the franchisee's ActiveCampaign account. This version is still under construction.  

---
## Markdown Shortcuts and Methods  
_Using this area to put examples to help me remember the methods to add something to the MD files. Found most of this [at this link:](https://www.jetbrains.com/help/phpstorm/markdown.html)_  

[Logs of V4 TRFA API](https://trfaapi.com/v4/ "V4 Logs")
