# Info Field Expired
Starting in Jan 2020, we are now expiring the Party and Field Trip info fields once they've been online for 30 days (new field in the info called Created: yyyy-mm-dd). Once the info field is removed as a part of the daily clean up process, a customer who clicks on the link to see the "edit party info" link, will not work because the info field is blank. 

In this case, the URLGenerator will pull out what we still know (first, last, email, phone, etc) and the redirect to the base URL and pass the above parameters PLUS `&expired=true`

Each form will need to be updated to display the expiration warning so the customer understands why the "edit" ability is gone and they have to start over. 

### Adult Party Warning
```
<div class="expire-warning">
<h3>We're sorry, but the party details you are trying to edit, have expired.</h3>

<p>You will need to complete the details again. We only hold details for 30 days without a booking. You can refer to your original confirmation email for the details supplied.</p>
<p>Thanks for understanding.</p>
</div>
```

### Field Trip Warning
```
<div class="expire-warning">
<h3>We're sorry, but the Field Trip details you are trying to edit, have expired.</h3>

<p>You will need to complete the details again. We only hold details for 30 days without a booking. You can refer to your original confirmation email for the details supplied.</p>
<p>Thanks for understanding.</p>
</div>
```

### Kids Party Warning
The kids party is a little different since the "base" url is not available to the API (all the data in info went away that would have directed us to the correct form), so the API will redirect to the base page where the party type is selected. 

The warning is in a CODE BLOCK
```
<?php
if( $_REQUEST['expired'] ) { ?>
<div class="expire-warning">

  <h3>
  	We're sorry, but the party details you are trying to edit, have expired.
  </h3>

  <p>
    You have been redirected back to the Kids Birthday starting point. 
    You will need to select the party type and complete the details again. 
    We only hold them for 30 days.
  </p>
  <p>You can refer to your original confirmation email for the details you provided us originally.</p>
  <p>Thanks for understanding.</p>
  
</div>

<?php } ?>
```

# Warning Styles
The "child" theme created via a plugin (Add Assets) contains this styling for the warning box.  
```
.expire-warning {max-width:800px;margin:30px auto 60px auto;background-color:#ddd;padding:20px 30px;border:1px sollid #ccc;}
.expire-warning h3, .gform_wrapper.expire-warning h3 {font-weight: bold !important;}
.expire-warning p {font-family: 'Raleway', sans-serif;}

```
