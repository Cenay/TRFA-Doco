# Reputation
This automation is a part of the utility section and will be labeled as such in the automations section of ActiveCampaign. The goal is to request a follow up from the customer when they provided a 5 Star Review on the site (the tag is added by Luisa when she "approves" the reviews).

## Automation Notes
This is the text representation of the automation steps and logic for the Reputation sequence. A screen grab of the visual flow is below. 

### Reputation 
  * TRIGGER: Tag added (Reviews . 5 Star Added) -> currently manually added when review approved
  * WAIT: 30 minutes
  * TAG+: (Reviews . 5 Star Follow Up Requested)
  * TAG-: (Reviews . 5 Star Added )
  * SEND: Email requesting a third party review on either Google, Facebook or Yelp
    > Google Link:  
    https://www.google.com/search?source=hp&ei=1pcSXZu8NcSU0gLx_qCICw&q=reviews+the+real+food+academy&oq=reviews+the+real+food+academy&gs_l=psy-ab.3..0i22i30.793.7953..8336...0.0..0.179.4172.0j30......0....1..gws-wiz.....0..0i131j0j0i10j33i22i29i30.M9fzmBQ1I-Y#lrd=0x88d9b18f49b7c985:0xd8c415ac59c5bd6a,1,,,
    TAG+: (Visited Google Reviews)  
    TAG-: (Reviews . 5 Star Follow Up Requested)  

    > Facebook Link:  
    https://www.facebook.com/pg/therealfoodacademy/reviews/?ref=page_internal  
    TAG+: (Visited Facebook Reviews)  
    TAG-: (Reviews . 5 Star Follow Up Requested)  

    > Yelp Link:
    https://www.yelp.com/writeareview/biz/B2OawXsomRnCzVFA3IfWQg?return_url=%2Fbiz%2FB2OawXsomRnCzVFA3IfWQg&source=biz_details_war_button  
    TAG+: (Visited Yelp Reviews)  
    TAG-: (Reviews . 5 Star Follow Up Requested)  
  * WAIT: 15 days  
  * SEND: Second email with same request (see above)
  * END: -> Exit

_Screen grab of the Reputation Automation_
![Reputation Automation](/img/reputation.jpg)