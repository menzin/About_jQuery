# Chapter 6: Events

Management of event handlers is one of JQuery's strengths. As intermediate web development frequently goes lightly on this topic, we begin with a review of events, which goes deeper than our reviews of other topics.

## Review of Events

_**Browser and Mouse Events**_ 

There are many events built into the browser API – most notably **click**, **blur**, **focus**, **mouseup**, **mousedown**. The structure for all is similar, so we will focus on the click event. There are other events associated with the document, such as its being loaded. We will not be discussing them here. 

_**An event listener**_

Listens for an event to happen and then fires an **event handler**. Many people use the terms event listener and event handler as synonyms (though not always.)
  
We also speak interchangeably of **adding an event listener** or **registering an event handler**; both expressions mean that we have specified what happens when a particular event happens.

For browser and mouse events, those events typically are associated with an action (such as a click or a mouseover) and an element where it takes places. So the event handler will specify what happens when that action happens to that element; the event handler then becomes a property/attribute of the element and the name of that property tells you the action- e.g. `someElement.onclick`  

In classic JavaScript, there are many ways to register an event handler. We will, in a minute, show you how to do this with jQuery, but here are the methods you may have seen in the past. The only purpose of reminding you of these ways to register an event handler is to tie the topic into your previous knowledge. We will not be using these approaches; we will get a better way to do this with jQuery. 

These methods fall into three general categories: 

<ins>**General category 1**: Use the onclick attribute inside the tag for the element in question.</ins>

> [!NOTE]
> 1. The Mozilla Developer Network recommends not using this. Among other sins, you are mixing structure (HTML) and behavior (JavaScript). 
> 2. If your code ends with return false then the default action will be prevented. (See below for information about preventDefault.) You have actually seen this before when the validation function for a submit button returns false in order to prevent the form's submission.

- Put the code into the onclick attribute:

      <button type ='button' id="But1" onclick ="alert('But1 was clicked');" >Click Me!</button>       

- Write an IEFE (immediately executable function expression) for the onclick attribute:

      <button type ='button' id="But2" onclick ="(function() {alert('But2 was clicked');})()" > Click Me!</button>

- Write a function and set the onclick attribute in the button tag to be that function:
  ```html
      function But3Handler() {alert('But3 was clicked')};
      <button type ='button' id = "But3" onclick = "But3Handler() ;" >
      ClickMe! </button>
  ```
> [!NOTE]
> This actually makes the But3Handler function run. <br> 
> The But3Handler() is defined separately in a script.


<ins>**General category 2**: Use JavaScript to set the onclick attribute for the element</ins>  

> [!NOTE]
> The element is identified by its id.  <br> 
> The oncllick attribute is set inside a script and after the DOM is loaded.  <br> 
> You may either define the handler directly, as in the But5 example, or you may set it to another function, as in the But4 example. <br> 
> You may remove the handler by coding myElement.onclick = null.

- Write a function and set the button's onclick property (in the DOM) to be that function:
``` html
function But4Handler() {alert('But4 was clicked')};     <button type ='button' id = "But4">ClickMe! </button>
```
And then, inside a script and after the DOM is loaded:

    But4.onclick = But4Handler;     //NO parentheses after But4Handler 
    
> [!NOTE]
> But4Handler can be either a function: function But4Handler(evt) {...} <br> 
> Or But4Handler can be a function expression: But4Handler = function(evt) {...}

- Set the button's onclick attribute to be an anonymous functions (i.e. a function expression):

      <button type ='button' id = "But5" >ClickMe! </button>
      But5.onclick = function() {alert('But5 was clicked');};
  
> [!NOTE]
> Again, the last line must be inside a script & after the DOM is loaded.

<ins>**General category 3**: Use addEventListener and removeEventListener </ins>  

- Use addEventListener (and removeEventListener).

> [!NOTE]
> As with the approach of general category 2, the event handlers are registered inside a script and after the DOM is loaded.

- But6.addEventListener('click', But6Handler); or using an anonymous function for the second parameter.

> [!NOTE]
> This may be used to add multiple handlers to the same event. Classic JS does not allow for multiple handlers on the same event unless you use this approach. jQuery does allow for multiple handlers on the same event.  

- We will see that jQuery makes this easy and more powerful – using the `on()` method to add a listener and the `off()` method to remove it. (Note: The old bind() and unbind() methods are deprecated as of version 3.0 of jQuery)

- The code for all these examples is in [eventHandlerDemo.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter06/eventHandlerDemo.html)

``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Classic Registration of Event Handlers Demo</title>
  
  <script src="jquery.js"></script>
  
<script>
function But3Handler() {alert('But3 was clicked')};
function But4Handler() {alert('But4 was clicked')};
function But6Handler() {alert('But6 was clicked')};

//Once the DOM is loaded can assign event handlers etc.
$(document).ready(function() {
	But4.onclick = But4Handler;     //NO parentheses after But4Handler
	But5.onclick = function() {alert('But5 was clicked');};
	But6.addEventListener('click', But6Handler);  
});

</script>
</head>
<body>
<h4>The buttons described in the section of Chapter 6 on the classic methods for registering event handlers:</h4>

<button type ='button' id="But1" onclick ="alert('But1 was clicked');" >
          But1 - Click Me!  Uses: Codes the onclick handler directly in the button tag</button><br/><br />
<button type ='button' id="But2" onclick ="(function() {alert('But2 was clicked');})()" >
          But2 - Click Me! Uses: Codes the onclick handler as an anonymous function in the button tag</button><br /><br />
<button type ='button' id = "But3"   onclick = "But3Handler() ;" >
          But3 - ClickMe! Uses: The handler is the result of a function defined elsewhere</button><br /><br />
<button type ='button' id = "But4">But4 - ClickMe! Uses: The handler is a function defined elsewhere</button><br /><br />
<button type ='button' id = "But5" >But5 - ClickMe! Uses: The handler is a function expression defined elsewhere</button><br /><br /> 
<button type ='button' id = "But6" >But6 - ClickMe! Uses: Handler is added with addEventListener</button><br /><br /><br />     		   
 
</body>
</html>
```

_**Events have properties**_

- An event listener can always take a parameter evt which refers to the event object and is automatically passed to the event handler.
- The most often used properties are evt.target which is the object on which the original event (e.g. the click) took place and evt.type which is the name of the event (e.g. 'click').
- `this` in an event handler refers to the element on which the event is operating (see the next paragraph.)
- `evt.clientX` and `evt.clientY` give the coordinates of the event relative to what is visible in the application window. These coordinates might be useful for drag-and-drop.
- `evt.pageX` and `evt.pageY` give the coordinates coordinates of the event relative to the whole document, including parts of it which may have been scrolled out of view. For example, if you are looking at coordinates on a map then you want to use the pageX and pageY coordinates, since you have no control of (or interest in ) how the user has resized or scrolled the window.
- There are also properties (see next paragraph) which related to how events bubble or propagate up through the DOM.
- A full list of all the properties of an event may be found at https://developer.mozilla.org/en-US/docs/Web/API/Event. From there you can also follow the links to more specific types of events, such as mouse events.
- If you have never worked much with events, a good, fuller discussion of events may be found at https://javascript.info/events (although jQuery will simplify some of the issues discussed in the second half of that chapter.)  


_**Events propagate aka bubble up**_

- If you fire an event on an element which is nested inside another element, then that event will first work on that element, and then on its parent, and so on up the DOM. We say that the event **propagates** or **bubbles** up.
  
	For example, suppose that you have
  ```html
  <div id='myDiv' onclick='expose(this);'>
  	<ul id = 'myList' onclick='expose(this);'>
  	  <li id = 'item1' onclick='expose(this);'>See my details</li>
  	  <li id = 'item2' onclick='expose(this);'>And  my details</li>
  	</ul>
  </div>
  ```
  	and have defined `function expose(evt) {alert(evt.id)};`

	When you click on myItem1 or myItem2, then all the onclick handlers going up the DOM will be fired – firing whatever onclick handler has been attached to that element.

	On the other hand, it you have
	```html
	<div id='yourDiv' onclick = 'expose(this);'>
		<ul id = 'yourList'>
		 <li id = 'yourItem1' onclick='expose(this);'>See my details</li>
 		 <li id = 'yourItem2' onclick='expose(this);'>And my details</li>

		</ul>
	</div>
	```
	
 	then clicking on yourItem1 (while it propagates up the DOM) – notice that yourLIst does not have any onclick event handlers to fire, but yourDiv does (and it fires.).  
  
  	In other words, the event bubbles up the DOM, firing the onclick event handler when one exists (& skipping the elements where there is no onclick event handler.)

- **Distinguishing between and using both the event object and its current target.** <br>
  	There is another thing to notice here: be clear about whether your handler is a function of an element or of an event!
  
	- **When the onclick event handler calls expose(this) from the element, as in**
	
	  ``` html
	  	<li id = 'yourItem1' onclick='expose(this);'>See my details</li>
	  ```
	  **then expose() is passing <ins>_this_</ins>, which is the <ins>_element_</ins> where the event is firing**.
	
	  We can think of this as expose(el) knowing that the parameter el being passed in is an <ins>_element_</ins>. Remember that **_this_** refers to an element.
	
	  If needed, we can still access the actual event and all its properties: the browser will automatically create an event object (named _event_) so we can use event.type, event.clientX and event.clientY for the coordinates of the click, etc.


	-  **When we code the onclick event handler with**

    	``` html
     		yourItem1.onclick = function(evt){ …}  
     	```
		**then evt is a _event_**. An event handler expects one parameter – namely the event.

		We can recover the element it fired on as evt.target and we can recover the current target as evt.currentTarget or as this. In other words, as the event bubbles up, the value of this refers to the place where you are in the bubbling – i.e. to the currentTarget. 

> [!NOTE]
> If you want to know what the current target is you should ask for evt.currentTarget.id

In the example in this paragraph, (since we wrote a handler specifically for clicking on yourItem1) we know what the target is but in a few paragraphs we will see how to write one handler for clicking on multiple elements – and then we will want to know what the target is. 
	  

- **The target vs. the currentTarget** <br>
	When the event is bubbling up `evt.target` refers to the element on which the event (e.g. the click) originally took place and `evt.currentTarget` refers to the element on which the event is taking place now (i.e. as it bubbles up.) _**this**_ will also refer to the currentTarget. 

- **Stopping propagation** <br>
	Sometimes you want to stop this propagation. `event.stopPropagation()` will stop the event from bubbling up the DOM. `event.stopImmediatePropagation()` will stop any other event handlers which are attached to this same event from firing. Both of these are supported in jQuery – with the same names as in JavaScript.

	⚠️: Some events – notably focus and blur – do not bubble.

	All these are illustrated in the code for [eventPropagationDemo.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter06/eventPropagationDemo.html)

	```html
	<!doctype html>
	<html lang="en">
	<head>
	  <meta charset="utf-8">
	  <title>Event Propagation demo</title>
	  
	  <script src="jquery.js"></script>
	  
	<script>
	//Various functions which will be used as event handlers
	function expose(element) {
	   alert(element.id )
	   };
	function exposeOnce(element) {
	   alert(element.id );
	   event.stopPropagation();  
	   };
	   
	exposeEvent = function(evt) {
	   alert("The event.type is " + evt.type + ". Its event.target is " + evt.target.id + " and its currentTarget "+evt.currentTarget.id);
	   alert("Inside the event handler the value of this is " + this.id);
	   }
	
	//Once the DOM is loaded can assign event handlers etc.
	$(document).ready(function() {
	    //The onclick handlers for myItem3 and myItem4 are passed the event
		myItem3.onclick = exposeEvent;
		myItem4.onclick = exposeEvent;
		
		//In yourList we have handlers for the div and the first 2 items, but not the list
		yourDiv.onclick = exposeEvent;
		yourItem2.onclick = exposeEvent;
		});
		</script>
	</head>
	<body>
	
	<h4>myDiv: with clickable items inside myList</h4>
	<div id='myDiv' onclick='expose(this);'>
		<ul id = 'myList' onclick='expose(this);'>myList with its clickable list items:
		   <li id = 'myItem1' onclick='expose(this);'>myItem1: See the onclick propagate from the li to the list to the div</li>
		   <li id = 'myItem1A' onclick='expose(this);'>myItem1A: Same as myItem1; just showing the propagation from another li</li>
		   <li id = 'myItem2' onclick='exposeOnce(this);'>myItem2: For this item we stopped the propagation</li>
		   <li id = 'myItem3'>myItem3: This time we use an onclick handler which is passed the event, but still bubbles up</li>
		   <li id = 'myItem4'>myItem4: Same as myItem3; just showing the propagation from another li</li>
	    </ul>
	</div>
	</br>
	<h4>yourDiv with some clickable items inside yourList: </h4>
	The first two items and the div have onclick handlers, third item and the list do not.<br>
	Notice that the bubbling goes from the list item directly to the div.<br> 
	Also notice that as the click on yourItem2 propagates, the value of this is the currentTarget.
	<!-- yourList doesn't have an onclick handler, but clicks bubble up to yourDiv -->
	<div id='yourDiv' onclick = 'expose(this)'>
	   <ul id = 'yourList' >
	       <li id = 'yourItem1' onclick='expose(this);alert("From here I can see the type is "+event.type);'>yourItem1 has an onclick handler</li>
	       <li id = 'yourItem2' >yourItem2 also has an onclick handler</li>
		   <li id = 'yourItem3' >yourItem3: Has no onclick, but watch clicks bubble up.</li>
		   
	  </ul>
	</div>
	<br>
	 
	</body>
	</html>

 	```
- **Events may be delegated** <br>
	Rather than write and attach an event handler for each of many elements, it may be more convenient to attach the event handler to their parent (or other ancestor) and then use the target to make the response specific to the element where the event fired.

	For example, suppose that we have a list of items and clicking on one of them adds it to a shopping cart. Then our code might look like:

	``` html
	<ul id = 'books'>Books
	  <li id ='jsgp'>JavaScript: the Good Parts by Douglas Crawford</li>
	  <li id ='ydkjs'>You Don't Know JavaScript by Kyle Simpson</li>
	  <li id ='ydkjsyet'>You Don't Know JavaScript Yet by Kyle Simpson</li>
	</ul>
 	```


	Rather than write an onclick event handler for each of the three books, we could write one for the whole ul and then test to see which book was clicked. Our event handler would look like:

  	``` html
  	 var bookList = $('#books');   //or use document.getElementById('book'): if no jQuery
   	bookList.addEventListener('click',
   		function(evt) {
   			trgt = evt.target;
   			if (trgt.tagName == "LI")  //Respond only to a click on a list item
   				{addToCart(trgt.id)'}
   			};
   	```

	where we have previously defined a function addToCart which takes the id and processes it. <br>
	Reminder: tagName returns the tag for an element node in all upper case; nodeName does the same for all types of nodes (elements, attributes, etc.) 


	(Actually, we can improve this by adding a custom attribute isbn to each of our books, and then pass the isbn to an appropriate function.)

	The important point here is that we have delegated the event handling to the parent element (here, the ul) and written only one event handler.

	Further, because of bubbling, if we add more books to our list then clicking on a new book will again cause bubbling up to the #books onclick, and the desired steps will take place.

	So event delegation has two powerful advantages: we write the code only once (making it easier to maintain) and we can have that code work on elements which we have yet to add.

	While this example, has a relatively simple event handler, there may be situations where we want a more complex function. And, obviously, we could define that function separately outside the event handler.

	As you will see shortly, jQuery also makes if possible to do this, with some extra bells and whistles. You will see that with proper coding, jQuery allows us to have event handlers that will also work for new DOM elements as they are added.


	⚠️: The currentTarget is always the element to which the event handler is attached. In the case of delegated events, that is the element at top of piece of the DOM to which you attached the delegated event handler! For instance, in the example above the currentTarget is #books and the target is the list item which was selected. 
	
	In some situations the browser may not identify a currentTarget for delegated events, so it is safest to code around that by using this.
	
	A detailed example of event delegation is in the jQuery paragraph about attaching events with delegation, as jQuery makes this easy for us to do. 

- **Events have default behaviors, which you can prevent.** <br>
	The most common default behavior is for anchors, where your browser will follow the link provided. There are other default behaviors, too. For example, submit and reset buttons have default behaviors.

	Every event comes with a `preventDefault()` method.

 	You can also prevent the default behavior with **return false**, but that obviously doesn't allow you to have other things happen once you have returned from the function. With preventDefault() you can code for the other things you want to have happen.

  You will see that in jQuery the preventDefault method works in the standard way - but you can use jQuery to select a collection of target elements on which this behavior is implemented.

  When you have an event **evt** then `evt.preventDefault()` will (not surprisingly) prevent the default action from taking place. Typically, this would be one step in an event handler - e.g.
  ``` html
    <p id = 'foil'>Foiled again!</p>
    $('#foil').onclick = function(evt) {evt.preventDefault; alert('Sorry!'); } 
    ```
   A more useful example would be:

	**Owning it:** <br>
	Write an event handler for an anchor which puts up a confirm dialog box, asking if the user wants to leave the site, and responds appropriately – i.e. prevents the default behavior if the user fails to confirm a willingness to leave the site.

	<ins>Hint:</ins> It will be easier if you use a separately coded event handler as in But4 of demo_6_3_using_on.html, below, since then you will be able to get ahold of the event itself. If you need to force a way to re-direct the user to a page they have confirmed they wish to go to, https://css-tricks.com/redirect-web-page/ (about a third of the way down the page) will show you several ways to make this happen. The simplest is to set:

  		window.location = "http:// put new url here ";

## jQuery and events 

jQuery allows us to do all the standard things with events and also some additional things. For example, jQuery supports attaching an event handler to multiple events (e.g. a click on a class of elements) and supports defining your own custom event. In addition, by using jQuery we can attach event handlers to new DOM elements as they are added to the page! Of course, jQuery does preserve our ability to access event attributes (such as target and currentTarget) and to stop propagation, prevent default behavior, etc. 

There are many advantages of using jQuery to handle events.
- First of all, please remark that in our three general categories of how to register event listeners using classic methods, the first categories of how to register event listeners using classic methods, the first category (using onclick inside the element tag) is not recommended, and the second and third categories require us to wait until the whole webpage is loaded (including images and scripts), not just when the DOM is loaded. But jQuery's `$(document).ready()` method does precisely that for us (and is faster and more robust than the classic onload event.) - that is `$(document).ready()` gets to work when just the DOM has been loaded. 

- Second, jQuery allows us to easily assign multiple handlers to the same event – which in classic JS we could do only with addEvent Listener.

- Third, as you will see, jQuery allows us to define and handle our own (custom) events.

- Fourth, by using jQuery selectors we can assign handlers to many events at once – e.g. all events with a certain class. And this assignment will also be made automatically to new elements as we add them to our DOM.

- Fifth, using jQuery we can trigger an event – i.e. make it happen under script control.

- Finally, jQuery allows us to pass other data to the event handler.

So on to using jQuery with events!

_**Attaching an event handler to an event – basic**_

The basic format for attaching a handler to some events is: 
	
	 $(someSelector).on('nameOfEvent', handlerFunction) 
 
For example:

	$('.bigDealClass').on('click', function() {$(this).addClass('embolden') } ); 
      
If you have defined function myHandler() { …} elsewhere then your code might be:

	$('.bigDealClass').on('click', myHandler);      //NO parentheses after myHandler  

Either line of code will look for all elements with the class bigDealClass and when they are clicked will execute the anonymous function which adds the class embolden to them.  

Please notice that <ins> _in the code for the event handler, this refers to the element for which the handler is being attached_. </ins>   

In other words, whether you define the handler function separately or code it as an anonymous function inside on(), the handler function 'knows' that it is an event handler and so it automatically gets this referring to the element on which it attached, but may have as a parameter the event which fired it. Thus, you can reference this.target, etc.  

To see this, examine:

[demo_6_3_using_on.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter06/demo_6_3_using_on.html)

```html

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Using on()demo</title>
  
  <script src="jquery.js"></script>
  <script>
	function But3Handler(){alert(typeof(this));alert(this.id);} ;
	function But4Handler(evt) {alert(typeof(this));alert(this.id);alert(evt.target);alert(evt.target.id);}
	$(document).ready( function() {
			          $('#But1').on('click',function(){alert(typeof(this));alert(this.id);})
				  $('#But3').on('click', But3Handler);
				  $('#But4').on('click', But4Handler);
               });

</script>
</head>
<body>
 
<button id="But1"  type ='button'>
          Click But1 on() is used to code an anonymous event handler</button><br>
<button id = "But3"   type = 'button'>
           Click But3! onclick is set with on()to a separately defined function</button><br>

<button id = "But4" type = 'button'>Click But4! onclick property is set with on() to a separately 
defined function</button>

</body>
</html>
```

To repeat, then, the on() method expects (at least) two parameters – the first is the name of the event and the last is a function which 'knows' the value of the event (if that is passed as a parameter) and of this, i.e. 'knows' the event and hence the value of event.target, the element to which the event handler is being attached. 

> [!WARNING]
>  1. This will attach the onclick handler only to existing elements which match the selector. To make this work for all elements which match the selector you need to use a delegated event handler – see example below.
>  2. Put the script which adds the event handlers at the bottom of the page, so that all the elements are loaded, or (my preference) use $(document).ready().
>  3. You can use a named function or function expression for the event handler but it should be defined before you attach the event handler. Recall that function definitions are hoisted, but function expressions are not hoisted.

The example in the next paragraph demonstrates **direct** (not delegated) event handling, and two different ways to code for delegated event handling.


_**Attaching an event handler designed for delegation**_

Because delegation is such a useful way to attach event handlers to multiple events, jQuery has a special way to make this easy. 

Our basic way to attach event handlers is:

	$(someSelector).on('nameOfEvent', handlerFunction)  
 
Notice that I rather slyly refered to the parameters of on() as the first and last parameter. Now we will add another parameter between them.  

	$(someSelector).on('nameOfEvent', descendentSelector(), handlerFunction) 

Now, the handlerFunction will be called only on elements which are descendants of our someSelector and match the descendantSelector. 

For example, if we wish to execute handlerFunction on any list item inside a ul we would code: 

	$('ul').on('click', 'li', handlerFunction); 

This is much cleaner than the code we wrote without jQuery (where we had the test that the target – the li element - was not the same as the currentTarget – the ul element or test that target had nodeName of LI.) 

On the other hand, by restricting our event handler to fire only for li elements, we lose the opportunity to do something for other elements (unless we write handlers for them specifically.) 

[eventDelegationWithjQueryplusDemo.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter06/eventDelegationWith_jQueryPlusDemo.html)

``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Event Delegation withjQuery demo</title>
  
  <script src="jquery.js"></script>
  
<script>
//Various functions which will be used as event handlers

var counter = 1;    //used to generate new id's as add list items

//As you examine the event handlers, remember:
//this is the target element, NOT the event; the event is named event
//The id and nodeName properties belong to this, which is also equal to event.currentTarget
//Methods like preventDefault() belong to event.

function expose() {
   console.log(this.id )
   };
   
function delExpose(){ 
   //This handler is attached at the ul level.  As the <ul> is NOT inside a div etc.
   //There is no bubbling up from the ul.   
  var $tgt = event.target;   //Delegated event - target is the ul or li
  var $ourTag = $tgt.nodeName;   //Shows as tag, as it should
  console.log("At an element with tag "+ $ourTag);
  console.log("In delExpose with target " + $tgt.id) //shows the id for the UL.
  console.log("Ready to see if we are at an LI and if so handle appropriately.")		  
  if ($ourTag == "LI")   //so not at the ul
	 {console.log($tgt.id);
	 //console.log(event.delegateTarget.id);
	 }
	 else     //should bubble and doesn't!!
	 {console.log("Now you are not at an LI")};
	 }   
	 
function delExpose2() {
    //This is like delExpose() but takes advantage on the additional parameter in on()
	//So we do not need to test for being at an li.
	var $tgt = event.target;   //Delegated event - target is the ul or li
    var $ourTag = $tgt.nodeName;   //Shows as tag, as it should
    console.log("At an element with tag "+ $ourTag);
    console.log("In delExpose2 with target " + $tgt.id) //shows the id for the UL.
	//Now, knowing we are at an li, we can handle the event
	console.log($tgt.id);
	}	 

function newItem(idForMyUL){
  //Function to attach another list item to the end of a ul
  var li2= document.createElement('li')
  li2.innerHTML = 'new list item';
  li2.id = 'new' + counter;
  counter ++;
  idForMyUL.appendChild(li2);
}   


//Once the DOM is loaded can assign event handlers etc.
//We have to use the each() method here -rather than say $('li').onclick = expose;
//Because a handler is attached to each li in tthe stdUL, new li's don't get the handler.
//Because the handler for delUL is attached at the ul level and delegated, new li's get handled.
$(document).ready(function() {
    $('#stdUL li').each(function(i){this.onclick = expose;});
	$('#delUL').on('click', delExpose);
	$('#delUL2').on('click', 'li',delExpose2);
	});
</script>
</head>
<body>
<ul id='stdUL'>Our list starts with two items
  <li id='item1'>Original clickable item 1</li>
  <li id='item2'>Original clickable item 2</li>
</ul>  
<br />
Click on the items above to verify that their onclick handlers are attached.<br>
Then click on the button below to add a new item to the list and see if it has an onclick handler.<br>
<button type = 'button' onclick = "newItem(stdUL);">Click to add an item to the list above</button><br>
Check to see that the new item does NOT have the onclick event handler.<br><br>

Now we repeat this with event delegation.
<ul id='delUL'>Our list starts with two items
  <li id='item1del'>Original clickable item 1 from list with delegation</li>
  <li id='item2del'>Original clickable item 2 from list with delegation</li>
</ul>  <br>
Here is the button to add to the list with event delegation:
<br />
<button type = 'button' onclick = "newItem(delUL);">Click to add an item to the list above</button><br>
Check to see that the new item does NOT have the onclick event handler.<br><br>

And we repeat again, using event delegation for only list items.<br />
Notice that we were able to get a 'not a list item' message in the previous list, because 
we tested for being at an li or not in our onclick handler.<br />
Here, because that testing is managed by the on() method, we have no handler for clicking on the ul.<br>
<ul id='delUL2'>Our list starts with two items
  <li id='item1del2'>Original clickable item 1 from second list with delegation</li>
  <li id='item2del2'>Original clickable item 2 from second list with delegation</li>
</ul>  <br>
Here is the button to add to the list with event delegation:
<br />
<button type = 'button' onclick = "newItem(delUL2);">Click to add an item to the list above</button><br>
Check to see that the new item does NOT have the onclick event handler.<br><br>
</body>
</html>
```

jQuery also provides a **delegateTarget** property. For any event evt, evt.delegateTarget is the element to which the event handler is attached. (For an event which is not delegated, then evt.delegateTarget is the same as evt.currentTarget.) 

Although both the jQuery api and the w3schools site show code which use event.delegateTarget, this property is not part of the official DOM specification for events at https://dom.spec.whatwg.org/#introduction-to-dom-events, and when using jQuery, the use of this property is not robust. Finally, you don't really need it. With a delegated event handler, the event bubbles up until it gets to the element where the handler is attached, and that element is then the event.currentTarget. 

Finally, the on() method also allows you to have a parameter in which you pass in additional data. https://api.jquery.com/on/#on-events-selector-data-handler has examples of this, as well as many examples about default behavior for submit buttons, etc. 


- **_Removing event handlers_**

Just as on() will attach event handlers, off() will remove them.  

The syntax for off() is exactly the same as that for on(), except that in order to remove an event handler it must have been specified with a name – i.e. not as an anonymous function.  

For example, in the code above we have:

	$('#delUL').on('click', delExpose);  
 
and we could remove that event handler with 

	$('#delUL').off('click', delExpose);


- ***You can have custom events and even pass them data***

Using jQuery you can define your own events, and attach event handlers. 

You can also pass data to these event handlers. The discussion of custom events is beyond the scope of this book, but you can learn more at https://learn.jquery.com/events/introduction-to-custom-events/.


- ***Stopping the bubbling/propagation and preventing defaults***

As noted above, jQuery provides a uniform interface for all browsers for the standard methods
- `evt.stopPropagation()`
- `evt.stopImmediatePropagation()`
- `evt.preventDefault()`
where evt is any event.

In addition, jQuery provides methods which return boolean variables and which allow us to test to see if these actions have taken place.

Again, with evt being any event, the methods are:     
- `evt.isPropagationStopped()`
- `evt.isImmediatePropagationStopped()`
- `evt.isDefaultPrevented()`

As a reminder, these are methods on the _event_.  

As noted in the earlier part of this chapter, preventDefault() is part of the event API, and jQuery makes it available and robust.  A more detailed discussion of these boolean properties and propagation methods  is beyond the scope of this book; documentation can be found at https://api.jquery.com/category/events/event-object/ ; specification of the standard behavior is at https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation etc. and a discussion of preventing default behavior without the use of jQuery is discussed at https://javascript.info/default-browser-action 

As outlined in the "Owning It" exercise above, the preventDefault() method is potentially useful in a complex event handler.  

In fact, we also are all familiar with this general idea from submit buttons, where a false is returned (and the form is not submitted) when validation fails.   


- ***Event capturing (going down the DOM)*** 

Rarely used and so beyond the scope of this book. Default value is false, which means you don't capture. 


- ***Multiple event handlers***

An element may have multiple handlers for the same event. For example, if you have a catalog of items for sale, focusing on some of them might bring up a dialog box which asks for size. Those items would have an identifying class, and you could attach an appropriate event handler to all items in that class. Then, there might be another event handler which fires for clicking on any item. 

When an item has multiple handlers, they fire in the order in which they were bound to the element. 


- ***Triggering an event with trigger() or triggerHandler(), and event.result***

Using jQuery we can force an event to fire – i.e. to trigger it. For example, if a user leaves a text box empty we might want to triggger the onfocus event on it.

`$('myID').trigger('click')` will cause the onclick event handler to fire and then go on to the default behavior for the click event. If you do not want the default behavior to execute, then use `triggerHandler()`. That is triggerHandler is like `trigger()` and then `preventDefault()`. Events fired with either `trigger()` or `triggerHandler()` will bubble up the DOM. If your event handler returns a value, then the most recently returned value is stored in `event.result` (Remember that an event may have multiple handlers.) 


- ***Shortcuts – for common events***

Some events are so common that there are shortcut names for binding event handlers.

For example, the following are equivalent

	$(someSelector).on('click', someHandler) and $(someSelector).click(someHandler)  

The click() method without any handler is also a shortcut for triggering a click event. For example, `$('#myID').click()` will trigger the onclick handler on `#myID`. 

The other events with shortcuts include hover, focus, blur, submit, mouseover, mouseout, mouseup, mousedown and dblclick. Some of these shortcuts you will use so often that they will be second nature to you; others you will either look up at  https://api.jquery.com/category/events/ or use the slightly longer on('someEvent', handler) syntax. 




 
