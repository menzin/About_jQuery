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
  **then expose() is passing <ins>_this_</ins>, which is the <ins>_element_</ins> where the event is firing.** 
