# Chapter 0: Just Get Me Going

**jQuery is a library of functions & you need a copy.** </br>
There are two ways to get a copy of jQuery – you can link to an online copy in a CDN (Content Delivery Network) or you can download a copy and link to your own copy inside your script. 
I will show you both ways. Both ways work; you decide!  

The other decision you need to make is whether you want to always access the most recent version of jQuery (via a CDN), or whether you want to stick to a specific version, so that your code won’t be effected by future updates. 

To keep things simple, we will download the current version (3.4.1 as of this writing) and store it in the same folder as our webpages. That also means that we have decided to stick to a specific version. 

All downloads may be found at https://jquery.com/download/ and we will download the compressed or minified version at https://code.jquery.com/jquery-3.4.1.min.js (You can see the ‘min’ for minified in its URL. It is missing comments, etc. and, at another time, you may find it interesting to look at the uncompressed, development version.) 

You can save the file as **jquery.js** (as I have done) or as **jquery_3_4_1_min.js** in the same folder where you will store your scripts. 


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

* **$( ) calls that library**
   * That is, $ is a shortcut for calling all the functions in the jQuery library.
  
* **Tell jQuery to wait until the DOM is loaded and then we’ll use the library of functions**
    * Often jQuery is used to manipulate the DOM, so we tell jQuery to wait for the DOM to load.
    * We do this by typing:

          $(document).ready(function() {//We’ll put our specific instructions here.});

    * And then inside the { ... } we will tell jQuery what to do once the DOM has loaded.
    * So we next need to see what to put inside the { }.
    * As an aside, please notice that jQuery does not need to wait for all the images to load – it just needs to know the structure of the DOM. This can help speed things up.
      
* **Use $() again - Put a selector inside the parentheses of $( )**

      $(document).ready(function() {
  		$(some CSS selector)
       });

    * This asks jQuery to return a collection of all the elements which match that selector.
    * jQuery uses the same selectors as CSS (Basic ones are reviewed below.)
    * For example, `$(".smallText")` will return all the elements in the class smallText. `$("#terms")` will return the (one) element whose ID is terms. 

* **Add a method – or do something to the elements in that collection.**
    * Once you have all the elements in the collection, you add a method which does something to them.
    * For example, $(".smallText").hide() will find all the elements in the class smallText and then hide them.
      
* **Putting this all together:**

   [demo_0_0_hiding.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter00/demo_0_0_hiding.html)
  
      <!doctype html>
        <html lang='en'>
        <html>
            <head>
                <meta charset="utf-8">
                <title>Demo</title>
                 <script src="jquery.js" > </script>       <!-- the jQuery library 
        
                 <script type="text/javascript”>
                        $(document).ready( function() {
        
        
                               $(".smallText").hide();
                       });
                   </script>
                   <!--links to style sheets go here- but for this example the CSS is here  
               <style>
                     .smallText {font-size: 5px; }        //the smallText class
               </style>
         
              </head>
              </body>
                    Lots of sales verbiage
                   <span class = smallText>The terms are onerous.</span>
                </body>
           </html>
     
The smallText class will get hidden as soon as the DOM is loaded – too fast for you to see it before it hides – but you can still see that it’s there in the source code.

>[!WARNING]
> you must either grab the code from the Chapter 0 folder or paste the code above into a text editor and save it as demo_0_0.html before trying to run it.** 


### Summary So Far 

The first thing we need to do is to get a copy of the jQuery library, either by downloading jQuery and linking to that copy, or by linking to a copy of jQuery in a CDN. We do that as the very first script in our head. If you download a copy of jQuery and store it with your web page files you will link to it with: 

    <script src="jquery.js" > </script>  

Immediately following that we write a script which asks jQuery until the DOM of our page has downloaded and then execute a script. The script for this (inside script tags) looks like: 

    $(document).ready(function() {         //wait for the DOM to download & then... 
        $(some selector1).someMethod1();     //do stuff
        :                       
        $(some selectorN).someMethodN();
        });

***Short review of very basic selectors***

* `#thisThing` selector for element with ID thisThing (There may be only one element with a given ID.)
    * Example: If you have an element with ID homepage, then $("#homePage") will return that element.
      
* `.thisClass` selector for all elements with class thisClass.
    * Example: If some of your elements (paragraphs, spans, etc.) have the class hot then $(".hot") will return all of them (even if they are different types of elements.)
      
* `"thisTag"` selector for all elements with tag <thisTag>.
    * Example: $("h3") will return all headline elements of size h3.
      
* Multiple selections: `selector1`, `selector2` will selector all elements that meet either selector.
    * Example: $('h2, h3') will return all headline elements of size h2 or h3.
      
* `'*'` __selects all elements.__

  
>[!WARNING]
> You need quotation marks around the selectors.
    * If, however, you create a variable `var thisClass = ".hot"` or pass in an element with id of someID and create a variable `var thisElement = "#” + someID` then you can use `$(thisClass)` or `$(thisElement)`.
       
> Whether you use single quotes or double quotes, be sure they are straight ones, not leaning left or right.
    * This is easy in a text editor, but doesn’t happen in Word (unless the options were reset to use straight quotes) --- so beware of copying text from a Word document. 


***Changing the appearance with addClass() and removeClass()***

One of the easiest and most common ways to change the appearance of an element on a page is by adding and removing classes.  

Example: In your style sheet suppose that you have classes </br>
`.redText   {color: red; }`    </br>
`.blueText  {color: blue;}`  </br>

(Recall that the attribute color sets the color of the text.)  

Then you can change everything which has blue text to red text by coding:

    $(".blueText").removeClass("blueText").addClass("redText");  
    
Please notice what happened here:  

* $(".blueText") returned a collection of all elements with the class blueText, then 
* removeClass("blueText") removed that class and returned those elements and then 
* addClass("redText") added the redText class to those elements.
  
This is called _chaining_ (the methods) and we will see much more of this in Chapter 1. 

We can refine this example so that when our user clicks a button only one specific element will change from blue to red. To do this, the onclick handler for that button will need to pass the ID of the element to be changed to a turnBlue() function.

In this example we will make the changes in appearance happen when the user clicks a button – so we won’t need to wait for the DOM to load but we will use jQuery to add and remove classes. (Look at lines 9 – 15.)   

Please notice the semi-colons at the end of the lines of JavaScript. While semi-colons are often casually omitted in writing JS, it is dangerous to do so in jQuery --- your code may not get parsed in the way you hoped.

Here is a complete example:  

![image](https://github.com/menzin/About_jQuery/assets/144168274/5827f418-1a0f-4703-97fa-17455ac37c71)


As noted above, the code is written in such a way that you could use turnRed and turnBlue for many different elements, so long as the onclick event handler sends the correct ID to the functions. 

In black and white the code is:

[demo_0_1_colors.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter00/demo_0_1_colors.html)

    <<!doctype html>
    <html lang='en'>
    	    <head>
      	        <meta charset="utf-8">
    	        <title>Demo 0_1 Colors</title>
         	    <script src="jquery.js" > </script>     <!-- the jQuery library -->
    	        <script type="text/javascript">
    	            function turnBlue(someID){
    				thisID = "#"+someID;
    				$(thisID).removeClass("redText").addClass("blueText");}  
    	            function turnRed(someID){
    				thisID = "#"+someID;
    				$(thisID).removeClass("blueText").addClass("redText");}
    	        </script>
               <style>
    		      .blueText {color:blue; }
    			  .redText  {color:red; }
    			</style>  
          </head>
          </body>
                <p class =  blueText id = "someP">This text starts in blue.</p>
    			<button onclick = 'turnRed("someP");'>Click me to turn the paragraph red</button>
    			<button onclick = 'turnBlue("someP");'>Click me to turn the paragraph blue</button>
    	        </body>
    	</html>
            

In this example we have changed the color of the text, but you could use the same approach to change the font size, bold or unbold it, etc.

>[!WARNING]
> When you wish to change classes _ which are in conflict_ with each other, _it is good practice to remove the class you don't want_, and not to just add the new class. Failure to do this may give unpredictable result. 

For example, with classes redText and blueText as above, if you simply addClass one of them to an element with the other class you might produce code like:

    <p class = "redText blueText">Blue comes after red</p>
    <p class = "blueText redText">Blue comes before red</p>
    
And, oddly enough, both paragraphs will appear in blue! (You can verify this by looking in the Elements panel of the console.)  
If your classes include both properties which are may conflict with other classes and properties which won't (e.g. color and margin where you change only color) then you might want to use multiple classes for those two types of properties. 

To see this, examine:

[demo_0_3_classConflict.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter00/demo_0_3_classConflict.html) 

    <!doctype html>
    <html lang = 'en'>

    <meta charset="utf-8">
    <head>	
       <title>class conflictdemo</title>
       <script src="jquery.js"></script>
	   
	   <style>
	      .redText {color:red;}
		  .blueText {color:blue;}  
		  
		</style>  
		
	  </head>
	<body>
	   <p class = 'redText blueText'>red first</p>
       <p class = 'blueText redText'>blue first</p>
	 </body>
    </html>	 



***Changing a form when you validate the entries – with colors or with error messages***

- Please note: The example below does not work with screen readers. A discussion of jQuery and ARIA compliance is deferred to Chapter 8.
  
Also, while HTML5 now provides built-in capabilities for form validation (see https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Form_validation for example), the context of reacting to form input is a simple, clear example where we may want to react to what the user has done. 

Giving feedback to users (e.g. after filling in a form) is a very common task. There are a variety of ways to do this – either by changing the color of a label or by showing an error message. Here are two simple ways. 

The point of this example is to provide useful feedback. 

Even though our demo script has only one element, we will write the code so that our function is useful for many different text elements. 

To do this we will follow a pattern of labeling each element and making the id for the label be the id for the input element, concatenated with “Label”. For example, we have an input element with the id “givenName” and its label has the id “givenNameLabel”.  



***Changing the color of the label with removeClass() and addClass():*** 

Suppose that you have a text box:

	<label id="givenNameLabel" class ="blueText"> 
 		Please enter your given name: 
   	<input type = "text" name = "givenName" id= "givenName" > 
    </label>  


Then your code for validating this text box might include: 

	gName = $("#givenName").val()   //Value in the givenName field
 	if (gName.length == 0) {                
  		//empty text box; change label to red                 								$("#givenNameLabel").removeClass("blueText").addClass("redText"); 
   	}  else { 
    	 //make sure label is blue, in case it had been previously made red               				   $("#givenNameLabel").removeClass("redText").addClass("blueText");
      	};

Here it is in a complete program:

[demo_0_2_feedback_with_label_change](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter00/demo_0_2_feedback_with_label_change.html) 

	<!doctype html>
	<html lang='en'>
	    <head>
  	        <meta charset="utf-8">
	        <title>Demo 2 Feedback by Changing Colors</title>
			<style>
		      .blueText {color:blue; }
			  .redText  {color:red; }
			</style>  
     	    <script src="jquery.js" > </script>      <!-- the jQuery library -->
	        <script type="text/javascript">
	            function validate(someTextID){
				thisElement = "#"+someTextID;         //the text element
				thisLabel = thisElement + "Label"; //the label for the text element
				thisValue = $(thisElement).val() ; //The value stored in the text element
				
				if (thisValue.length == 0)    //if statement works
				   {//empty element; change label to red					    
				    $(thisLabel).removeClass("blueText").addClass("redText");						
				   return;
				   } else {
				   //change label to blue in case there was a previous error					   
	       			   $(thisLabel).removeClass("redText").addClass("blueText"); 
				   return;}
				   }	            
	        </script>
             
      </head>
      <body>
           <form name="demoForm" id="demoForm">
		<label id='givenNameLabel' class ='blueText'>
	    	Please enter your given name:
	   	<input type = "text" name = "givenName" id= "givenName"  value ="">
		</label>
		<button type="button" onclick = 'validate("givenName");'>Validate this entry</button>
	  </form> 
	</body>
	</html>


***Changing the visibility /display of an error message with hide() and show():*** 

This approach uses error messages – and the fact that a form element may have more than one label. 

Before we start, please recall that there are two different ways to make an element disappear – through either the display property or the visibility property:       

- `display`: **none** will hide the element without taking up any space.
- `visibility`: **hidden** will hide the element but still allocate space to it.
  
 The hide() and show() methods work on the **display** property. That is, when you hide () elements the space they used to take up disappears (temporarily – until you show() the elements again), but with the added benefit that when you hide() them, the original display value (inline or block) is remembered and used when you again show() them. 
 
	The possible values for _visibility_ as _hidden_ and _visible_.
 	The possible values or _display_ are _none_, _block_, _inline_.

In this code, we will display the error messages inline, but hide them immediately, as soon as the DOM has loaded. We will do it this way in case you had a fancy layout you wanted to maintain.

Also, since there maybe several error messages, we will give them all an ErrorMsg class, so that we can hide() them all when the DOM loads. In other words, our script in the head (right after linking to jQuery.js) would be:

	<script>
 		$(document).ready(function () {
   			$('.errorMsg').hide();
      	}); 
       
We extend our pattern of making the id for the first label (as just above) the id for the input element with “Label” appended to it: the id for the error message will be the id for the input element with “ErrorMsg” appended to it. 

We can then use exactly the same approach as in our demo_0_0_hiding.html.

Specifically, suppose your code contains:   

      <label for="givenName"> Please enter your given name:</label> 
      <input type = "text" name = "givenName" id= "givenName" > 
      <label for="givenName" id="givenNameErrorMsg" class = "errorMsg"> 
      		You must fill in this box. </label>

Then your code can say: 
	
 	gName = $("#givenName").val()    //Value in the givenName field 
  	if (gName.length == 0) {                 
   		//empty text box; make the Error message visible 
     		$("#givenNameErrorMsg").show();
       	}  else {   
	//make sure the Error message is not visible 
 	   $("#givenNameErrorMsg").hide();
     	};   
      
While these examples validate one element, givenName, we could easily pass the ID of the element to validate to a function, as we did in the previous example, and have a more general function for validating text boxes – see Chapter One.


## Review 

jQuery is a library of functions which is useful for manipulating the appearance and functionality of web pages.  

The first thing you must do is make that library accessible to your web page – either by downloading jquery.js. saving it  and linking to it right after your title tag, or by linking to it in a CDN such as Google’s or Microsoft’s.  

$( ) is a shorthand for accessing the jQuery functions. 


If you are going to use jQuery to manipulate the DOM or bind event handlers to elements then you must wait for the DOM to be loaded. You do this by coding:
    
    $(document).ready(function() {   //We’ll put our specific instructions here.
    });
    
Typically jQuery acts by selecting a collection of element(s) and then applying a method to them. The code looks like:     

    $(some CSS selector).someMethod();  

Sometimes we chain methods and then the code looks like: 

    $(some CSS selector).someMethod1().someMethod2();  
    
Four very popular jQuery methods are: </br>
`hide()` </br>
`show()`  </br> 
`addClass(“someClass”)` </br> 
`removeClass(“someClass”)`
    
When we write code using jQuery it is important to put a semi-colon `;` at the end of each statement.  

## Owning it 

Design a page which has a paragraph and a set of buttons – two to turn the paragraph red or blue and two to make the text bold face or normal.
