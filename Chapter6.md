# Chapter 6: Events

Management of event handlers is one of JQuery's strengths. As intermediate web development frequently goes lightly on this topic, we begin with a review of events, which goes deeper than our reviews of other topics.

## Review of Events

_**Browser and Mouse Events**_ 

There are many events built into the browser API – most notably **click**, **blur**, **focus**, **mouseup**, **mousedown**. The structure for all is similar, so we will focus on the click event. There are other events associated with the document, such as its being loaded. We will not be discussing them here. 

_**An event listener**_

Listens for an event to happen and then fires an **event handler**. Many people use the terms event listener and event handler as synonyms (though not always.)
  
We also speak interchangeably of **adding an event listener** or **registering an event handler**; both expressions mean that we have specified what happens when a particular event happens.

For browser and mouse events, those events typically are associated with an action (such as a click or a mouseover) and an element where it takes places.  So the event handler will specify what happens when that action happens to that element; the event handler then becomes a property/attribute of the element and the name of that property tells you the action- e.g. `someElement.onclick`  

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
>  This actually makes the But3Handler function run.
> The But3Handler() is defined separately in a script.


<ins>**General category 2**: Use JavaScript to set the  onclick attribute for the element</ins>  

> [!NOTE]
> The element is identified by its id.
> The oncllick attribute is set inside a script and after the DOM is loaded.
> You may either define the handler directly, as in the But5 example, or you may set it to another function, as in the But4 example.
> You may remove the handler by coding myElement.onclick = null.

- Write a function and set the button's onclick property (in the DOM) to be that function:
``` html
function But4Handler() {alert('But4 was clicked')};     <button type ='button' id = "But4">ClickMe! </button>
```
And then, inside a script and after the DOM is loaded:

    But4.onclick = But4Handler;     //NO parentheses after But4Handler 
    
> [!NOTE]
> But4Handler can be either a function: function But4Handler(evt) {...} or
> But4Handler can be a function expression: But4Handler = function(evt) {...} 
