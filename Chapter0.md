# Chapter 0: Just Get Me Going

**jQuery is a library of functions & you need a copy.** </br>
There are two ways to get a copy of jQuery – you can link to an online copy in a CDN (Content Delivery Network) or you can download a copy and link to your own copy inside your script. 
I will show you both ways.  Both ways work; you decide!  

The other decision you need to make is whether you want to always access the most recent version of jQuery (via a CDN), or whether you want to stick to a specific version, so that your code won’t be effected by future updates. 

To keep things simple, we will download the current version (3.4.1 as of this writing)  and store it in the same folder as our webpages.  That also means that we have decided to stick to a specific version. 

All downloads may be found at https://jquery.com/download/ and we will download the compressed or minified version  at https://code.jquery.com/jquery-3.4.1.min.js   (You can see the ‘min’ for minified in its URL.   It is missing comments, etc.  and, at another time, you may find it interesting to look at the uncompressed, development version.) 

You can save the file as **jquery.js** (as I have done) or as **jquery_3_4_1_min.js**  in the same folder where you will store your scripts. 


### Template For Your Webpage:

    <!doctype html>
    <html lang='en'>
    	<head>
      	    <meta charset="utf-8">
    	    <title>jQuery template</title>
         	<script src="jquery.js"> </script>  <!-- the jQuery library  -->
    	<script>
    			        //This entire script could be in an external file
    			        //Your code which uses jQuery will go here	
    	 </script>
         <!--  links to style sheets go here  -->
        </head> 
        </body>
               <!-- Your HTML and any links to other scripts  go here. -->
         </body>
    </html> 

**If you would rather link to a CDN:**  
The advantage of doing this is that if your user’s device already has a copy of jQuery from the same CDN, then it won’t be downloaded again. The disadvantage is that it can require an additional http request.

Instead of downloading jQuery and linking to it in the line     

     <script src = "jquery.js"> </script>  //the jQuery Library

You replace that line with: 
           
      <script src = "https://code.jquery.com/jquery-3.4.1.min.js"> </script>

Either way, you have now made the jQuery library available to your web page and we are now ready to use it.


**The jQuery script should be the very first script your page executes – so put it in the head or at the top of the body.**
- (Aside: Some people put all scripts at the very end of the body, but the jQuery official site uses the head for jQuery and then the <script ...> which makes use of jQuery.)

To summarize, in Notepad++ the template looks like:

![image](https://github.com/menzin/About_jQuery/assets/144168274/267af744-3abb-4e8f-8eaa-ddcb3f6f4b7a)

Lines 1-5 and 12-17 are just your typical html, line 6 brings in the jQuery library of functions, and lines 7-11 are where you will make use of those functions. That’s it. Now you’re ready to find out how to use those jQuery functions. 

* $( ) calls that library
   * That is, $ is a shortcut for calling  all the functions in the jQuery library.
  
* Tell jQuery to wait until the DOM is loaded and then we’ll use the library of functions
    * Often jQuery is used to manipulate the DOM, so we tell jQuery to wait for the DOM to load.
    * We do this by typing:

          $(document).ready(function() {//We’ll put our specific instructions here.});

    * And then inside the { ... } we will tell jQuery what to do once the DOM has loaded.
    * So we next need to see what to put inside the { }.
    * As an aside,  please notice that jQuery does not need to wait for all the images to load – it just needs to know the structure of the DOM. This        can help speed things up.
      
* Use $() again - Put a selector inside the parentheses of $( )

      $(document).ready(function() {                $(some CSS selector)        });

    * This asks jQuery to return a collection of all the elements which match that selector.
    * jQuery uses the same selectors as CSS (Basic ones are reviewed below.)
    * For example, `$(".smallText")` will return all the elements in the class smallText. `$("#terms")` will return the (one) element whose ID is             terms.   



