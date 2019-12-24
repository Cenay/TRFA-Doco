# Bookeo Image Insertion Project 
As the chef updates the classes in Bookeo, there is some HTML, image resizing and other clean up that needs to take place. This document will describe those steps. 

## GOALS: 
These are the overall goals of the project, and something to keep in mind as you work. 
 1. File size reduced (faster load times)
 1. Images square and 200 x 200 natively
 1. Description consistent in appearance
 1. HTTPS protocol used on all links and images (prevent mixed contents errors)
 1. Remove unnecessary HTML like span tags, extra bolding, etc. 


## Image Sizing 
As chef Maria finds the images, she will place them in the shared Dropbox folder called /TRFA Website Files/Bookeo Images in either the adult or kids subfolder. Each image should be named the same as the class. 


 * Pull the images to your computer
 * Create squares out of them by either cropping or enlarging. 
 * Resize the squares down (or up) to 200 x 200
 * Resolution should be 72 ppi and saved at 80% compression.
 * Name them `bookeo-` and then the class name (_if Maria hasn't already_)
   * Samples `bookeo-fun-kitchen-gadgets.jpg` and `bookeo-kids-brunch.jpg`
 * Upload them to TRFA's site folder. (/images/)
 * Move the image you just completed into the /_completed/ version  

 **The only images that should remain in the original folder (_if any_) are those you haven't yet processed and attached to a class. It is possible that she will add more images than she needs.**

## HTML Updates
 * Edit each class that has an "attached" image and remove it, then save the record
 * Edit the HTML of each class description to embed the image into the content (_see below_)
 
 ```
 <p><img src="https://therealfoodacademy.com/images/CHANGEME.jpg" alt="CHANGEME" width="200" style="float: left; margin-right: 10px;"/></p>
 ```
   * The link should ALWAYS be the `https` protocol, and a lot of the older entries still say `http` on the image source. Change to `https` please. 
   * Change the ALT text of each to match the class name
   * Copy the image name from your scratch area into the CHANGEME of the image name.
   * Save the class and review for correctness

## Style Rules and Edits
 * Top line is always the date
   >Format month name as `upper case`, the numeric part of the date, and then the `date modifier` as lower case, followed by the year. (ie: JANUARY 18th, 2020)
 * Next line is always the day of the week and time of the class
   >Format should be `upper case` on the day of the week. The time should be separated by a dash, and the am/pm portion of the time is lower case. (ie: WEDNESDAY 11:30am - 1:30 pm)
 * Remove the price **unless it's a special price**. Normal prices are currently:
   * Kids Classes - $35
   * Adult Day - $45
   * Adult Evening - $65
 * There should be NO span tags inside the HTML. If you find any, remove them. 
 * The menu items should never be bold or italized.
 * When chef refers to making a class vegan, paste in this code (In the HTML editor) instead of her statement: (_Optionally, you can copy the sentence of the WYSIWYG editor that contains the link_)
    ```
    This class can be 100% vegan upon <a href="mailto:maria@therealfoodacademy.com" target="_top">prior request</a>.
    ```
 * The class name (on the line called "Name" outside of the description), should be in this format, please correct any errors:
   * Title of the class is `upper case` and then a dash `-` and then the class type in `sentence case`. (ie: FUN KITCHEN GADGETS - Kids Class)
   * The only one's that have them are these types: 
     * Kids Class
     * Adult Day
     * Adult Evening
     * Day Camp (_when it is one the school holiday's_)


## Quality Control Review
Since the chef isn't a native English speaker, there are sometimes errors in her content. Review the description for grammar errors, misspellings, incorrect language usage, etc. Correct if it's clear what it should be. Notify me if it's "feels wrong" but you are unsure how to correct it. 

A little creative editing can also make the class description look better when it wraps just a couple of words to below the image. If you can edit the content without changing the "flavor" of her description, do so. 


## Instructions Video
This video contains a sample of how to perform the task. Some of the things done are subjective so remember the goal is a pleasing appearance, consistant image sizes and reduced file sizes so phone users don't get upset at the download speed/time. 

 * [Video Instructions for Project](https://cn-team-videos.s3.us-west-2.amazonaws.com/bookeo-class-editing.mp4)

## Alternatives to Photoshop
If Photoshop isn't in your toolbox, here are some alternatives to use. 
 * Photopea : https://www.photopea.com/
 * Pixlr : https://pixlr.com/e
