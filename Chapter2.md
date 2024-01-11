# Chapter 2: Getting Fancier 

In this chapter we will set the stage for more complicated selectors by first reviewing the DOM and reminding ourselves about some properties of buttons.

Then we can move on to fancier selectors – first the ones which use slightly more complicated CSS selectors and some general jQuery methods (which have some overlap),  then we learn how to back up the chain, and finally turn to using filters and eq().  This last part includes selecting using CSS pseudo-classes and also using some jQuery extensions to those filters based on those pseudo-classes.

In terms of the lingo associated with jQuery, this discussion includes both "selection" and "traversing the DOM".

In Chapter 3 we will learn how to use attributes and properties to make selections.

## Review of the DOM

- **The DOM is a tree of nodes, used to represent web pages.** <br>
That is, your HTML code is a document and it is very useful to have a model of it, in which each element is an represented as an object on a tree.  That model of your HTML document is the DOM or Document Object Model.

  Conceptually, the root of this tree is the document (or document object),  which has two nodes, head and body.  That is, the head and body are children of the document, so their nodes hang directly off the document node. Other nodes in the tree may be paragraphs, divs, etc. (i.e. elements) but also nodes with the text content of elements. Each node hangs off the node of its parent.
  
  People often refer to the nodes as 'elements' on the tree, although, strictly speaking, some of the nodes are element-nodes, some are text-nodes, etc. (As of DOM4, published in 2015, attribute nodes are no longer part of the DOM node interface, but you may find earlier references to attribute-nodes.)
  
  The DOM is a w3c standard.

  It is useful to bear in mind this distinction between your HTML document and the DOM which is how your browser models the document for purposes of manipulation.  
 
- **The DOM may change as you manipulate it with jQuery (or JavaScript)** <br>
When we add and remove classes, append and delete nodes, change attributes for an element, etc. we are changing the DOM. In JavaScript we used methods such as document.getElementById(). In jQuery we can simplify our code by using the $() operator to get elements by class, tag, ID, etc. And, in Chapter 4 we will also learn about jQuery methods for changing the DOM (usually referred to as manipulating the DOM.)

- **There are tools for examining the DOM ** <br>
Chrome, Firefox, Safari and Opera all use similar engines for rendering a layout of the DOM.  I admit to liking both Chrome and the Firefox Developer Version; when you have a page open in Chrome, ctl+shift+I (cmd+shift+I on Macs) will open the web developer tool, and the Elements panel will show you to DOM tree, which you can expand and collapse as you prefer. A clear and simple explanation of the basic capabilities may be found at 
https://developers.google.com/web/tools/chrome-devtools/dom#view

  In Firefox the same functionality is available with ctl+shift+K (cmd+shift+K on Macs) and its Inspector pane. For example, using Chrome, the file demo_0_2_feedback_with_label_change.html (from Chapter 0), with the head collapsed, looks like

![image](https://github.com/menzin/About_jQuery/assets/144168274/f9659922-bbe6-429d-b645-79d2fefe388e)

Placing the cursor over an element in the upper right panel (highlighted in light blue in the image below) will provide more information about the chosen node.

![image](https://github.com/menzin/About_jQuery/assets/144168274/973fa5be-8d9b-48c7-b24d-d23463925c1a)

> [!NOTE]
> Can change alerts to console.log() & use demo_2_1A_child_vs_descendants_w_lengths_con.html


:warning: Warnings About the DOM

1. The styling for a child (or a descendant) will override that of a parent. For example, in the code above, if we have used classes to style the div with blue text and the outer ul with green text, using removeClass() and addClass() to change the div to red text, the ul will still be green.  This is because the more specific styling in the cascade of CSS always prevails. 

2. A paragraph may not have lists inside it; divs may.

  For example, in the code 
  
    <div>Verbiage          
      <ul>…<ul> 
    <div>  
  
  if we change the div to a p, then the browser will interpret 
    
    <p>Verbiage 
      <ul>…</ul> 
    </p>
    
  as 
  
    <p>Verbiage </p> 
      <ul>…</ul> 
       
  And the ul will no longer be a child of the p. You can see this by looking at the  elements in the code below in the Chrome or Firefox developer tool, and changing the div's to p's.      
  
    <div id= "info”>General Information -Please select your model:
      <ul>Model
        <li>Model1</li> 
        <li>Model2</li>
        <li>Older Models 
          <ul>These are no longer supported:
              <li>Model A</li> 
              <li>Model T</li> 
          </ul>  
        </li> 
      </ul> 
    </div>
    
More examples of this sort may be found at https://stackoverflow.com/questions/10601345/an-ul-element-can-never-be-a-child-of-a-p-element

  
### A reminder about buttons
We will be using a lot of buttons to trigger events, so it's good to have a reminder about some of their properties before we start. <br>
In HTML5 there are two ways to code for a button -  with a `<button>` tag or with an `<input type = 'button'>` tag.

There are a few important differences between these two methods:
- Button tags need to be closed, while the input tag (like the  `<br>` tag ) is empty.
- The clickable text in the input tag is given by the value attribute inside the tag `<input type = 'button' value = 'Click here'>` while the clickable part of the button tag is given between the opening and closing tags `<button>Click here</button>`
- Because everything between the opening and closing button tags is clickable, you may include images, etc. It is also easier to style the button.
- **The `<Input type='button' >` is always inside a form, while the `<button>` may be either inside or outside a form.**
- **The default type for a button tag is a submit button.** If you want a button which is inside a form and not a submit button, you must specify its type:

      <button type = 'button'>Click here</button>
  
(When you use the input type = ... Method you are already specifying the type as a button.)
  
- If you want a `<button>` inside a form and which causes a change in the page appearance, then you must either specify type = 'button' or the onclick event handler must return false. <br>
If you don't do this, the handler will fire twice. (There are a lot of other explanations for this – as you can see by googling jQuery _toggle()twice_. The clearest of these explanations is at https://stackoverflow.com/questions/3070400/jquery-button-click-event-is-triggered-twice but the explanations will make more sense after we discuss events in Chapter 6). The code below illustrates this:

> [!NOTE]
> In this code I have included the parameter for the number of milliseconds; this makes the hide/show happen slowly so that you can see what is happening.

Summary:

The more modern method for coding buttons is to use the button tags. If your `<button>` is outside a form, things will go smoothly. If your `<button>` is inside a form and you do not want to submit the form, you must either specify type='button' inside the opening `<button>` tag or you must have the onclick event handler return false. 

Example – and introducing the toggle() and toggleClass() methods. 

The toggle() function will swap the display state. That is, if the display is block or inline, then it will make the display state none, and if the display state is none, then toggle() will return it to its original (block or inline) state. You can change the speed at which this toggling happens by adding a parameter: either the number of milliseconds for the toggle or 'slow' or 'fast'. For example, the code below 
          
           $(someSelector).toggle(1200) 
           
will take 1.2 seconds to hide/show the selected elements.  

The toggleClass(some list of classes)  will swap the named classes. For example, 

    $(someSelector).toggleClass('redText')  

will add/remove the class redText to the selected elements. This code demos the behavior of buttons we have discussed above. I suggest that you try to predict the behavior of each button, based on the discussion above, and then run the demo to verify your predictions. 

[demo_2_0_toggling_and_buttons.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_0_toggling_and_buttons.html)

  ``` html
    <!doctype html>
    <html lang = 'en'>
    
        <meta charset="utf-8">
        <head>	
           <title>toggle and toggleClass demo</title>
           <script src="jquery.js"></script>
    	   
    	   <style>
    	      .redText {color:red;}
    		  .embolden {font-weight:bold;}
    		</style>  
    	  </head>
        <body>	  
    	<p>This paragraph has a <span id= "disp" class = "redText" >mysterious</span> word.</p>
    	<p>The following buttons are inside a form:</p>
    	<form>
    	   <!-- 3rd and 4th button don't work b/c button inside a form and 
    	        neither button type is given nor false is returned -->
    	    <button type='button' onclick="$('#disp').toggle(1200);">(Button type given)Make mysterious appear or disappear</button> <br />		
    		<button  onclick="$('#disp').toggle(1200);return false;">(Button type NOT given, but return false)Make mysterious appear or disappear</button> <br />
    		<button  onclick="$('#disp').toggle(1200);return true;">(Button type NOT given, but return true)Make mysterious appear or disappear</button> <br />
    		<button  onclick="$('#disp').toggle(1200);">(Button type NOT given, but no return)Make mysterious appear or disappear</button> <br />
    		<button type='button' onclick="$('#disp').toggleClass('redText');">Make mysterious red or black</button>
    	</form><br />
    	<p>The following buttons are outside a form:</p>
    	<!-- All buttons work b/c they are outside a form- i.e. nothing to submit -->
    	<button type='button' onclick="$('#disp').toggle(1200);">(Button type given)Make mysterious appear or disappear</button> <br />		
    		<button  onclick="$('#disp').toggle(1200);return false;">(Button type NOT given, but return false)Make mysterious appear or disappear</button> <br />
    		<button  onclick="$('#disp').toggle(1200);return true;">(Button type NOT given, but return true)Make mysterious appear or disappear</button> <br />
    		<button  onclick="$('#disp').toggle(1200);">(Button type NOT given, but no return)Make mysterious appear or disappear</button> <br />
    		<button type='button' onclick="$('#disp').toggleClass('redText');">Make mysterious red or black</button>
    	 </body>
    </html>  
```

### Fancier selectors and filters 

- **Using hierarchies**  
jQuery provides many ways to use the DOM to find elements.  Let’s start with the simplest ones, _all of which follow the same syntax as in CSS:_
  - **Immediate child** <br>
    `$('selector1 > selector2')` will find all elements that match selector2 and whose immediate parent matches             selector1. Example:  Suppose that your HTML includes the following:
    
        <div id= "info”>General Information -Please select your model:
          <ul>Model
            <li>Model1</li>
            <li>Model2</li>
            <li>Older Models
                <ul>These are no longer supported:
                    <li>Model A</li>
                    <li>Model T</li>
                </ul>
                </li>
          </ul>
        </div>
  
    Then `$("#info > ul ")` will select the ul which begins Model, but _not_ the ul which begins _These are no longer supported:_                            
    Notice that in the query above we were able to mix the types of selectors -i.e. a tag selector which is a child of an ID selector.
    
    We could also have a longer chain of selectors.      
                                                                                
  - **Descendants** <br>
  Just as `$("selector1 > selector2")` gives you the elements which satisfy selector2 and are immediate descendants (children) of the elements which match selector1, `$("selector1 selector2")` gives you the elements which satisfy selector2 and are any descendants (children, grandchildren, etc.) of elements which match selector1.
  
     In the example above `$("#info ul")` will produce the ul which begins Model, and also the ul which begins _These are no longer supported._
  
    The difference between descendants and children is illustrated in the following script
  
    Examine the code just below the next screen shot. In this script there are two identical lists, each with three items; and the second list element has a nested list with 3 items.   
  
    So the lists look like:
    - Item 1
    - Item 2
      - Nested item 1
      - Nested item 2
      - Nested item 3
    - Item 3

     In the upper list (with id topList), we draw red lines around each `<li>` which is an immediate child of topList. In the lower list (with id botList) we draw blue lines around each <li> which is any descendent of botList. 

    The result and code are:

    ![image](https://github.com/menzin/About_jQuery/assets/144168274/2107393a-615f-4ff4-bc19-74386e354923)

    [demo_2_1_child_vs_desc.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_1_child_vs_desc.html)
 
    ``` html
        <!doctype html>
        <html lang="en">
        <head>
          <meta charset="utf-8">
          <title>child vs all descendants demo</title>
          <!-- from https://api.jquery.com/child-selector/ -->
          <style>
          body {
            font-size: 14px;
          }
          	</style>
          <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
          <script>
        		$(document).ready(function() {
        		$( ".topList > li" ).css( "border", "4px solid red" );
        		//Note that don't need ul.topList in selector as topList class appears only once
        		$( ".botList  li" ).css( "border", "4px double blue" );
        		})
        	</script>
        </head>
        <body>
         <h4>Immediate children are outlined in red with $("topList > li") </h4>
        <ul class="topList">topList
          <li>Item 1</li>
          <li>Item 2
            <ul>
            <li>Nested item 1</li>
            <li>Nested item 2</li>
            <li>Nested item 3</li>
            </ul>
          </li>
          <li>Item 3</li>
         </ul> 
         <h4>All descendants are outlined in blue with $("botList  li") </h4>
         <ul class="botList">botList
          <li>Item 1</li>
          <li>Item 2
            <ul>
            <li>Nested item 1</li>
            <li>Nested item 2</li>
            <li>Nested item 3</li>
            </ul>
          </li>
          <li>Item 3</li> 
        </ul>
         

        </body>
        </html>

     ```
  
    We can also see this distinction between children and descendents in the code below which adds the use of the length property to see how many elements are returned in set:
 
    [demo_2_1A_child_vs_descendants_w_lengths.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_1A__child_v_descendants_w_lengths.html)
    
       ``` html
        <!doctype html>
        <html lang = 'en'>
        
            <meta charset="utf-8">
            <head>	
               <title>child vs descendant selector w lengths demo</title>
               <script src="jquery.js"></script>
        	   <script>
        	      $(document).ready(function(){
        		          
        				  $( "div" ).css( "border", "3px double red" );  
        				  $( "ul" ).css( "border", "3px double blue" );  	
        				  
        				  $infoChild = $("#info > ul");  
        				  $infoDesc = $("#info ul"); 
                           				  
           				  var mesg1 = "#info is outlined in red ; it  has " + $infoChild.length + " <ul> child(ren)";
        				  var mesg2 = " and it has " + $infoDesc.length + " , each outlined in blue, <ul> descendant(s).";	  		   			  
        				  alert(mesg1 + mesg2);
        				  
        				  });
               </script>
               
            </head>			  
        			  
        	<body>			 
        	<div id= "info">General Information (with ID "info")-Please select your model:
        		<ul id = "outerUL">Model
        			   <li>Model 1</li>
        			   <li>Model 2</li>
        			   <li>Older Models 
        					 <ul id="innerUl">These are no longer supported:
        							<li>Model A</li>
        							<li>Model T</li>
        					 </ul>
        				</li>            
        		</ul>
        	</div>
            
        	</body>
        </html> 

    And the result looks like:
    ![image](https://github.com/menzin/About_jQuery/assets/144168274/f3e55e7b-4f2b-45cf-9053-60109fd4684b)

  ```

 > [!NOTE]
 > demo_2_1A_child_vs_descendants_w_lengths_con.html is the same demo, but using console.log instead of alert

  - **Siblings** <br>
      jQuery also allows us to find either the next sibling of an element or all following siblings of an element.   
  
      We will  use these when we manipulate the DOM in Chapter 5.
  
      I urge you to load the code and play with the various buttons in the examples below.
  
      
       **Immediate next sibling:** The syntax is as follows:        
       
           $('selector1 + selector2') 
    
      will return all those elements matching selector2 which are immediate next siblings of an element matching selector1.
          
      **All following siblings**:
  
           $('selector1 ~ selector2')
  
      will return all those elements matching selector2 which are any later siblings of an element matching selector1.
  
      Some examples will clarify this.

    [demo_2_2_siblings.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_2_siblings.html)
  
  ``` html
        <!doctype html>
        <html lang='en'>
        	<head>
        		<meta charset="utf-8">
        		<title>demo_0_1</title>
        		<style>
        			.blueText {color:blue; }
        			.redText  {color:red; }
        			.greenText  {color:green; }
        		</style>	  
        		<script src="jquery.js" > </script>        //the jQuery library
        		<script type="text/javascript">
        			function turnBlue(someSet){			
        			$(someSet).removeClass("redText").removeClass("greenText").addClass("blueText");}  
        			
        			function turnRed(someSet){			
        			$(someSet).removeClass("blueText").removeClass("greenText").addClass("redText");}
        			
        			function turnGreen(someSet){
        			$(someSet).removeClass("redText").removeClass("blueText").addClass("greenText");} 	
        			
        			
        		</script>
        	   
          </head>
          </body>
        		<form id="form1" class ="blueText">
        		  <p> This is form1 </p>
        		  <input type ="button" value ="Make next sibling red" onclick = "turnRed($('#form1 + form'));">		  
        		  <input type ="button" value ="Make all siblings green" onclick = "turnGreen($('#form1 ~ form'));">
        		  <input type ="button" value ="Turn me red!" onclick = "turnRed($('#form1'));">
        		  <br /><br />
        		</form>  
        		
        		<form id = "form2" class ="blueText">
        		  <p> This is form2, the immediate (next) sibling of form1. </p>
        		  <br />
        		 </form> 
        		
        
        		<form id = "form3" class ="blueText">
        		<p> This is form3,  a following sibling of form1. </p>
        		<br />
        		</form>
          </body>
        </html>
   ```     

⚠️ Some warnings about relatives: <br>

The first thing to remember is that (just as with humans) siblings must share a parent.  

The second thing is more subtle, so be warned. If you ask for 

            $('selector1 + selector2').someMethod()
            
Then the immediate sibling after selector1 must match selector2 ... Or nothing happens.

To see this, we modify the code above slightly ---- namely we add two paragraphs between form1 and form2 and two new buttons (code is bold).   The first of these paragraphs is the immediate successor of form1.   (See if you can predict what will happen, and then read the explanation after the code.)

[demo_2_3_siblings_playing.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_3_siblings_playing.html)

``` html
    <!doctype html>
    <html lang='en'>
    	<head>
    		<meta charset="utf-8">
    		<title>demo_0_1</title>
    		<style>
    			.blueText {color:blue; }
    			.redText  {color:red; }
    			.greenText  {color:green; }
    		</style>	  
    		<script src="jquery.js" > </script>        
    		<script type="text/javascript">
    			function turnBlue(someSet){			
    			$(someSet).removeClass("redText").removeClass("greenText").addClass("blueText");}  
    			
    			function turnRed(someSet){			
    			$(someSet).removeClass("blueText").removeClass("greenText").addClass("redText");}
    			
    			function turnGreen(someSet){
    			$(someSet).removeClass("redText").removeClass("blueText").addClass("greenText");} 	
    			
    			
    		</script>
    	   
      </head>
      </body>
    		<form id="form1" class ="blueText">
    		  <p> This is form1 </p>
    		  <input type ="button" value ="Make next sibling red, if it is a form" 
    		        onclick = "turnRed($('#form1 + form'));">	
    		  <input type ="button" value ="Make next sibling red, if it is a p" 
    		        onclick = "turnRed($('#form1 + p'));">			  
    		  <input type ="button" value ="Make all siblings which are forms green" 
    		        onclick = "turnGreen($('#form1 ~ form'));">
    		<br />		
    		  <button type = 'button' onclick = "turnRed($('#form1'));">Turn form1 red</button>
    		  <input type ="button" value = "Make paragraph1 red" onclick ="turnRed($('#form1 + p'));">
    		  <input type ="button" value = "Make paragraphs 1 and 2 green"  
    		            onclick= "turnGreen($('#form1 ~ p'));">
    		  <br /><br />
    		</form>  
    		
    		<p>This is the first paragraph after form1 and the next (immediate) sibling of form1</p>
    		<p>This is the second paragraph after form1 and also a sibling of form1.</p>
    		
    		<form id = "form2" class ="blueText">
    		  <p> This is form2, the immediate (next) form sibling of form1. </p>
    		</form> 
    		
    
    		<form id = "form3" class ="blueText">
    		<p> This is form3,  a following form sibling of form1. </p>
    		<br />
    		</form>
      </body>
    </html>
```

 The new buttons work just fine, but look what happened the first button (code in italics; it has the value "Make next sibling red"  ) ...  It looks for `$('#form1 + form')` That is, it looks for a form which is the immediate sibling of form1  ---- and there isn't such an element.  The immediate sibling of form1 is a paragraph!  The button right after the italicized one does work – because it looks for forms which are any siblings of form1.  
Finally, as you should expect, if there are multiple occurrences of paragraphs right after forms, we can handle all of them at once. The code is below. In addition to adding the two paragraphs after form2 we have modified the last two buttons.

[demo_2_4_siblings_playing_more.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter02/demo_2_4_siblings_playing_more.html)

  ``` html
    <!doctype html>
    <html lang='en'>
    	<head>
    		<meta charset="utf-8">
    		<title>demo_0_1</title>
    		<style>
    			.blueText {color:blue; }
    			.redText  {color:red; }
    			.greenText  {color:green; }
    		</style>	  
    		<script src="jquery.js" > </script>        
    		<script type="text/javascript">
    			function turnBlue(someSet){			
    			$(someSet).removeClass("redText").removeClass("greenText").addClass("blueText");}  
    			
    			function turnRed(someSet){			
    			$(someSet).removeClass("blueText").removeClass("greenText").addClass("redText");}
    			
    			function turnGreen(someSet){
    			$(someSet).removeClass("redText").removeClass("blueText").addClass("greenText");} 	
    			
    			
    		</script>
    	   
      </head>
      </body>
    		<form id="form1" class ="blueText">
    		  <p> This is form1 </p>
    		  <button type = 'button' onclick = "turnRed($('#form1 + p'));">
    		     Make next sibling, which is a p,  red</button>  
    		  <button type = 'button' onclick = "turnGreen($('#form1 ~ form'));">
    		     Make all form siblings green</button>
    			 
    		  <button type = 'button' onclick = "turnRed($('#form1'));">
    		     Turn form1 red!</button>
    		  <button type = 'button' onclick ="turnRed($('form + p'));">
    		     Make paragraphs 1 red"</button>
    		  <button type = 'button' onclick= "turnGreen($('form ~ p'));">
    		     Make paragraphs 1 and 2 green</button>
    		  <br /><br />
    		  
    		</form>  
    		
    		<p>This is the first paragraph after form1.</p>
    		<p>This is the second paragraph after form1.</p>
    		
    		<form id = "form2" class ="blueText">
    		  <p> This is form2, the immediate (next) sibling of form1. </p>
    		  <br />
    		 </form> 
    		
            <p>This is the first paragraph after form2.</p>
    		<p>This is the second paragraph after form2.</p>
    		<form id = "form3" class ="blueText">
    		<p> This is form3,  a following sibling of form1. </p>
    		<br />
    		</form>
      </body>
    </html>
  ```  

**Summary**


| Syntaxx        | Meaning      | Example  |
| ------------- |:-------------:| -------:|
| 'Sel1, Sel2'   | either  |'span, div' returns all `<span>s` and all `<div>s`|
| 'Sel1 > Sel2'  | Immediate child | 'div > p' returns the all the <p>s inside a <div> which are children of the < div> (not grandchildren, etc.). |
| 'Sel1 Sel2'     | Descendant  | div p' returns any <p> which is a descendant of a div. |
| 'Sel1 + Sel2'   | Next sibling |'p + **p**' returns any `<p>` whose immediate previous sibling is a `<p>` (Bolding to show correspondence.) |
| 'Sel1 ~ Sel2'   | All following siblings  |'p ~ **p**' returns all `<p>s` which are siblings of a previous p. (Bolding to show correspondence.) |


























### Backing up with end() 
We have made extensive use of the way most jQuery methods return a set. Sometimes we want to back up to the previous set, and the end() method will do just that. 

Suppose that I have code such as   

    $('.reviewClass').addClass('redText' 'embolden').children().addClass('blueText')

We have first gotten the set of all elements with class reviewClass. Then we have added two classes to those elements (which, recall,  doesn't change the jQuery set!). Then we went on to all the children of elements in our set and added a class to them.  Suppose now that we want to go back to those reviewClass elements to do some more stuff with them or find their next sibling etc. Then end() will do exactly that. 

Another way to think of this is imagine that as we use jQuery we produce a stack (most recent at the top of the stack) of the jQuery sets that we have gotten. We are operating on the top jQuery set in that stack.  What end() does is to pop the top set off the stack, so we are operating on the next set down.   

⚠️ Warning: some jQuery methods return the same set that they operated on. Examples of these methods  would be  addClass(), removeClass(), and several such as  css() which we consider in the next chapter.   
 
### Using filters and .eq() 

- **Finding specific elements in the order found with .eq()**

At this point we have seen enough selectors for most uses, but you should know that you can filter the results to get the first, last, nth or nth from last child. 

All of these are documented at https://api.jquery.com/category/selectors/ with examples. Some of these have been deprecated, so the recommended way to do so is to use the index in the returned set. Of course, the set starts counting from 0.

For example,   
  `$('div').eq(0)`  will return the first div and 

  `$('div').eq(1)` will return the second div.  

You may also count from the end by using negative numbers.  
  `$('div').eq(-1)` will return the last div.  (There is no -0.)  

And, should you need it, the length property of a jQuery set will tell you the number of elements returned.         

    var numDivs = $('div').length;

- **Finding all inputs in a form or forms – the :input and :button filters**

Instead of asking for all `<input>`, `<select>`, `<textarea>` and `<button>`  elements, we can collect them all at once using the :input filter.

`$('#myForm  :input')`  for one form 
`$('form :input')`  for all forms 

`$('#myForm  :input')` will  return the form elements where our user has input some information, which may be useful for form validation or processing. 

Please notice that this includes not only all `<input...>`elements, but also drop-down lists and buttons (whether declared with an input tag or a button tag.) 

Similarly   

    $('#myForm  :button')

will collect all buttons (whether declared with an input tag or a button tag), which may be useful for adding event handlers.

⚠️ WARNING:  If you use `<button>`, you must say `<button type = 'button'> `unless you want a submit button. Otherwise you get double bind!!!

There are analogous filters for all radio buttons, passwords, etc.  but as we shall see in the next section, we can also do this by using attributes (rather than learning a host of filters.)

- **Other filters**
  
jQuery supports all the CSS pseudo-classes (such as :first-child) and a few more of its own (such as :button and :input ). You can filter on them.   

I find that I rarely use most of them, And so am not discussing them here. But you can see the list of them at https://api.jquery.com/category/selectors/. If you are already a great user of CSS pseudo-classes , then you can find a list of just the jQuery extensions at https://api.jquery.com/category/selectors/jquery-selector-extensions/  

Do be careful, because quite a few of the filters have been deprecated in favor of newer methods.  (For example the .eq() method replaces the :eq() filter.)  The documentation notes when a filter or method has been deprecated and suggests alternatives.


## Review

The DOM is a tree, and we can find collections of elements on it using the same selectors we used in CSS. 

We can also chain such methods as children(), find(someSelector), parent(), parents() and siblings() to obtain collections of elements. 

We can use end() to back up one step. 

There are also a variety of filters to find particular types of input elements, (see https://api.jquery.com/category/selectors/form-selectors/ ),  first child, (see https://api.jquery.com/category/selectors/child-filter-selectors/) etc. but you must exercise care as some of them have been deprecated.

## Owning it

Look at the code at http://web.simmons.edu/~menzin/CS321/CS321_TOC.html and make sure you know how it works.

Create a page with 3 divisions. Each division should have some content (for example some jpeg's). Start with the divisions hidden, but add a clickable element to the headline for each division which allows you to show/hide its content.
















    

   
