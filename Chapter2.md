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

  
- ** A reminder about buttons** <br>
  We will be using a lot of buttons to trigger events, so it's good to have a reminder about some of their properties before we start. <br>
  In HTML5 there are two ways to code for a button -  with a <button>  tag or with an <input type = 'button'> tag.

  There are a few important differences between these two methods:
  - Button tags need to be closed, while the input tag (like the <br> tag ) is empty.
  - The clickable text in the input tag is given by the value attribute inside the tag  <input type = 'button' value = 'Click here'> while the clickable part of the button tag is given between the opening and closing tags <button>Click here</button>
  - Because everything between the opening and closing button tags is clickable, you may include images, etc. It is also easier to style the button.
  - **The <Input type='button' > is always inside a form, while the <button> may be either inside or outside a form.**
  - **The default type for a button tag is a submit button.** If you want a button which is inside a form and not a submit button, you must specify its type:
  
        <button type = 'button'>Click here</button>
  (When you use the input type = ... Method you are already specifying the type as a button.)
  
  - If you want a <button> inside a form and which causes a change in the page appearance, then you must either specify type = 'button' or the onclick event handler must return false. <br>
  If you don't do this, the handler will fire twice. (There are a lot of other explanations for this – as you can see by googling jQuery _toggle()twice_. The clearest of these explanations is at https://stackoverflow.com/questions/3070400/jquery-button-click-event-is-triggered-twice but the explanations will make more sense after we discuss events in Chapter 6). The code below illustrates this:

> [!NOTE]
> In this code I have included the parameter for the number of milliseconds; this makes the hide/show happen slowly so that you can see what is happening.

Summary:
The more modern method for coding buttons is to use the button tags. If your <button> is outside a form, things will go smoothly. If your <button> is inside a form and you do not want to submit the form, you must either specify type='button' inside the opening <button > tag or you must have the onclick event handler return false. 
Example – and introducing the toggle() and toggleClass() methods. The toggle() function will swap the display state.  That is, if the display is block or inline, then it will make the display state none, and if the display state is none, then toggle() will return it to its original (block or inline) state. You can change the speed at which this toggling happens by adding a parameter: either the number of milliseconds for the toggle or 'slow' or 'fast'.  For example, in the code below 
           $(someSelector).toggle(1200) 
will take 1.2 seconds to hide/show the selected elements.  The toggleClass(some list of classes)  will swap the named classes.  For example,           $(someSelector).toggleClass('redText')  will add/remove the class redText to the selected elements.  This code demos the behavior of buttons we have discussed above.  I suggest that you try to predict the behavior of each button, based on the discussion above, and then run the demo to verify your predictions. 
