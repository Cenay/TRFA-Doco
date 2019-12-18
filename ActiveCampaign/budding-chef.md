# Budding Chef
The API will need an endpoint I can call via a link (within an email), that gets as a param, the contact id. With that, the API should retrieve the info from the contact record, and parse the data into params that are then added to the URL and "redirect" the visitor. 

These params will complete the values for the fields they completed last visit to the form. 

### Important values

Contact ID in AC
%SUBSCRIBERID%

Endpoints in API  
/util/budding
/util/executive
/util/top

### Array of Fields to Party Info Names

BUDDING CHEF
===============================
Party For: 
Party Location: 
Address: 
Actual birth date: 
Estimated Guests: 
Package: 
Entree: 
Dessert: 
Extra Dessert:

Addon's will be in the format of number : name 
I only need the NUMBER to the named field
#   Name you'll find -> param
========================================
X : Smoked Salmon with Capers -> salmon
x : Cheese Crackers -> cheese
x : Chicken Waldorf Sandwiches -> waldorf
x : Proscuitto and Mozzarella Sandwiches -> proscuitto
x : Quiches -> quiche
x : Caprese Salad -> caprese
x : Hummus with Pita crackers -> hummus
x : Guacamole Dip with chips -> guacamole
x : Tzatziki Dip with Pita bread -> tzatziki
x : Assorted Cookies -> cookies
x : Fruit and Chocolate Fondue -> fondue
x : Flourless Brownies -> brownies
x : Balloons -> balloons
x : Extra Hour -> extra-hour
1 : Zumba -> zumba [note: they can only have a single hour so if there is a value, I am putting 1. ]

Special Instructions: 
 (this will be a paragraph. How will this be handled?)

Note Added at time of form completion: 
BUDDING CHEF
===============================
Party For: {Birthday Child First Name:9}
Party Location: {Party Location:7}
Address: {Address of party:8}
Actual birth date: {Date of Birth:11}
Guests: {Guests:12}  Update: {Guest Type:71}
Package: {Budding Chef Package Base [$550] (Name):57.1}
Entree: {Budding Entree:13}
Dessert: {Budding Dessert 1:14}
Extra Dessert: {Budding Dessert 2:15}

Add Ons: 
{Qty Smoked Salmon:22} : Smoked Salmon with Capers
{Qty Cheese:23} : Cheese Crackers
{Qty Waldorf:25} : Chicken Waldorf Sandwiches
{Qty Proscuitto:30} : Proscuitto and Mozzarella Sandwiches
{Qty Quiches:33} : Quiches
{Qty Caprese:36} : Caprese Salad
{Qty Hummus:39} : Hummus with Pita crackers
{Qty Guacamole:42} : Guacamole Dip with chips
{Qty Tzatziki:45} : Tzatziki Dip with Pita bread
{Qty Cookies:48} : Assorted Cookies
{Qty Fondue:51} : Fruit and Chocolate Fondue
{Qty Brownies:54} : Flourless Brownies
{Qty Balloons:63} : Balloons 
{Qty Extra Hour:67} : Extra Hour
Zumba: {Customize (Zumba (1 hour)):69.1}