# UrlGeneratorController
In order to reconstruct the form fields on any one of the party forms, the URI needs to contain the parameters. Since most of the data is inside the "party info" field, and not inside actual fields within ActiveCampaign, a function has to extract them, and build the redirect URI that contains the parameters so the form completes with their prior information. The endpoint is called from emails sent to the contact via ActiveCampaign, with the ID of the contact and the base URL to generate for.  

>Sample Usage:  
https://trfaapi.com/v3/util/url/kids-party/{1335} 
Where {1335} is the contact id within ActiveCampaign 

This is called when contact clicks the link in email within ActiveCampaign inside the automation for the event they have booked.  

**Parameters**
 * {class} -> Possible values: "Kids Party", "Adults Party", "Field Trips"
 * {id} -> Contact ID from ActiveCampaign (passed as %SUBSCRIBERID%)
 * {redirect?} _[Optional]_ -> 
 
 https://trfaapi.com/v3/util/url/adults-party/1048/

1. Is the base URI that ActiveCampaign passes. If it fails, the function calls #3. 
2. The redirect link posted to the view so that contact can find the correct link in the event the "link" fails. (See image below)
3. Old method, no longer used in current emails, left in for backwards compatibility, so that the link in the old emails does not break.  

There are three routes for generating URLs.
```
  1. Route::get('/url/{class}/{id}', 'UrlGeneratorController@ajaxRedirectToUrl');
  2. Route::get('/party/{class}/{id}/{redirect?}', 'UrlGeneratorController@index')->name('urlGenerator');;
  3. Route::get('/{class}/{id}/{redirect?}', 'UrlGeneratorController@index')->name('urlOldGenerator');
```
_Showing the #2 Link reason_
![URL Generator](/img/url-generator-explain-second-route.jpg)

# Methods
Functions used by the URL Generator class.

## ajaxRedirectToUrl
A simple method that just returns url generator view (v3/resources/views/ajax-url-generator.blade.php) where we display the loader until redirected to the correct url. Will also provide a link to the correct URL in the event the program hangs.  
>**RETURNS** view(ajax-url-generator)

## index
A traffic cop that directs to the correct method based on the passed parameter. It takes 3 arguments. 
 * $class: Possible values are "kids-party", "adults-party", and "field-trips". Based on the class name it will redirect to the method responsible to generating url for that class. For example in class of kids class flow will be redirected to `constructKidsPartyUrl` method.
 * $contactId: The ActiveCampaign subscriber ID passed to the base endpoint in the API
 * $redirect (default = null): Internal use only. Used to determine if ajax view is used. 
 >**RETURNS** Nothing

## constructKidsPartyUrl
Builds the actual redirect url, The example of redirect URL is  

>https://therealfoodacademy.com/birthday-party-package-details-executive-chef/?first=Khurram&last=Shahzad&email=khurram_shahzad%40icloud.com&phone=%28305%29+710-4473&birthday_name=Muhammad+Hadi&dob1=03%2F27%2F2017&location=The+Real+Food+Academy&guests=30+%28Estimated+Kids%29&entree=The+Traditional+Pizza&dessert1=Chocolate+Covered+Fruit+Skewers&dessert2=&salmon=&cheese=&waldorf=&proscuitto=&quiche=&caprese=&hummus=&guacamole=&tzatziki=&cookies=&fondue=&brownies=&balloons=1&extra-hour=&zumba=&update=true  

This method takes two arguments.  

* contactId
* redirect

`$contactId` is ActiveCampaign contact's id and `$redirect` is boolean, When redirect is set to true it will redirect the user directly to redirects url without displaying loading view.  
_(This was the initial approach that we implemented and later we added a loader view because generating a url and then redirecting to it was taking some time & we wanted to give user some feedback)._  
This method fetch the contact's details from ActiveCampaing by calling another method `getContactDetailsFromActiveCampaign`.  
`$details = $this->getContactDetailsFromActiveCampaign($contactId, 60);`  
Where `$contactId` is ActiveCamapign's contact id and 60 is the custom field id. In this case **Kids party info 1** id.  
If the **Kids party info 1** field is empty in ActiveCamapign then we redirect with just first name, last name, email, phone & expired parameter. The value of expired is set to **true** to display error/info message on Gravity Forms.  
`expired = true` logic was implemented because there was some cases where user fills out party details form and do not book the party. If there is any contact with such scenario then we have an automation **Cleanup Party Info** which will clear party info data if its in ActiveCampaign for 30 days or more. 
If there is data in **kids party info 1** field then we extract that data. Get the base url by calling **getPackageName** method. The possible return values form **getPackageName** will be  

(If Package value in info field is set to **Budding Chef Birthday Party**)  
>Returned Value: https://therealfoodacademy.com/birthday-party-package-details-budding-chef/  

(If Package value in info field is set to  **Executive Chef Birthday Party**)  
>Returned Value: https://therealfoodacademy.com/birthday-party-package-details-executive-chef/  

(If Package value in info field is set to  **Top Chef Birthday Party**)  
>Returned Value: https://therealfoodacademy.com/birthday-party-package-details-top-chef/  

Once we have the url the we'll build the query string by calling **buildQueryString** method. The working on **buildQueryString** method is mentioned below.

## buildQueryString
Build query string uses a built in PHP method [http_build_query](https://www.php.net/manual/en/function.http-build-query.php)  which generates a URL-encoded query string from the associative (or indexed) array provided.   

The assoc array is generated by calling a method `searchArrayValue` against each key  
*If location is set to "my location" party info then we make a call to another method `getLocationDetails` which will get us the location address, city, state, zip.*

## buildQueryStringForAdultsParty
The working of this method is exact same as of *buildQueryString* method, The only difference is *buildQueryString* deals with kids parties where as *buildQueryStringForAdultsParty* deals with adult parties.

## buildQueryStringForFieldTrips
The working of this method is exact same as of *buildQueryString* method, The only difference is *buildQueryString* deals with kids parties where as *buildQueryStringForFieldTrips* deals with field trips. 

## getContactDetailsFromActiveCampaign
Return ActiveCamapign Contact information and the value of party info field
`getContactDetailsFromActiveCampaign` method takes two arguments

* AC Contact Id
* AC Info Field's Id.  

This method will return array containing **contact information** and **kids party info 1** value. 
If **kids party info 1** field is empty then we just redirect to base url with contact information only. i.e, name, email, phone etc.  
If there is a value in kids party info 1, We'll extract package name from **kids party info 1**. Package name tells us what should be the redirect url because we'll redirect to different urls form each top, budding and executive.
We then build query sending by the information in **kids party info 1** by calling `buildQueryString` method. 

## searchArrayValue
Return part of string by searching for a string if found split the string by ":" and pass back the value by index. 
This method takes 3 parameters

* hayStack
* needle
* index

`'entree' => $this->searchArrayValue($kidsPartyInfoField, 'Entree', 1)`  

In this example it will look for keyword *entree* in *party info custom field* and then split that line by **':'** and takes party depending upon index. 
> Entree: The Traditional Pizza

So from this line we'll have the value of Entree i.e. The Traditional Pizza

## getLocationDetails
Return the address, city, state and zip values from active campaign.  

If in party details location is set to "My Location" then we need location values like address, city, state and zip form ActiveCampaign.
Following are the ActiveCampaign id's for each field we need. 
34: Address 1, 36: City , 37: State ,38: Zip
This method will query active campaign and return back the array containing address, city state and zip

