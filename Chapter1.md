# Chapter 1: What Is Happening Here 

This chapter explains the basic mechanisms of jQuery and provides examples of code which is used frequently. Such examples include simple animations, altering the text on a page (e.g. in a quiz) and more complicated versions of Chapter 0 examples. We discuss collections aka jQuery sets, basic chaining, and what methods can and cannot be chained.

In Chapter 2 we will move on to more advanced methods for this.


### What Is Happening Here 

Please explain  

    $(document).ready(function(){ //some code });
    
This line of code has two important things going on here – _**a callback function**_ and an _**anonymous function**_. 

To separate these two issues, suppose that somewhere we had defined a function gettingGoing(). Then we could replace the line above with 

    $(document).ready( gettingGoing() );
    
That is, the ready() method expects to have one parameter and it expects that parameter to be a function. When ready() has finished executing, that function which was passed to ready() will be executed.

The function which has passed to ready() is called a callback function and in the example right above our _**callback function**_ is gettingGoing()  

Again, ready(some call back function) will execute that callback function as soon as ready has finished executing – i.e. as soon as the DOM has loaded. 

Now it would work perfectly well to define gettingGoing() in our script and then use it as the callback function for ready( ), but JavaScript also has something called _**anonymous functions**_.  

Sometimes you have a function which you will use in only one place – and gettingGoing() is a perfect example of that. (Many callback functions fit this pattern). 

So rather than litter the namespace with the names of functions which are used only once, JavaScript allows you to simply place the code there. 

You don’t need to name these functions, because you will never call them else where, you merely need to say 

    function(any parameters){ //the code }

That is what is going on here. Instead of defining

    function gettingGoing( ) {//the code}  //gettingGoing might have parameters                        $(document).ready(gettingGoing())    
 
 we combine these as 
    
    $(document).ready(function() {//the code});

So, to summarize, in the line above:
- $(document) grabs the document element
- $document.ready() applies the ready() function to document
- The ready function requires one parameter – which is the callback function
- The callback function will be executed as soon as ready() finishes – i.e. the DOM has been loaded
- Here we use an anonymous callback function – that is, we define the callback function inside the parentheses of ready()
  
If you have never seen callbacks or anonymous functions before, they may take some getting used to. You can find some more examples at JSNote2A on Functions and Closures at http://web.simmons.edu/~menzin/CS321/Unit_2_JavaScript_and_HTML_Forms/Chapter_3_Basic_Java_Script/  


### Collections or Sets or Objects 

**What $ returns – almost an array** <br>
When you select some elements using $( ), jQuery returns a set of elements.

Depending on whom you read, that set may be referred to using any of the following terms: `set`, `collection`, `object`, `jQuery set`, `jQuery collection`, `jQuery object`, `wrapped set`. The jQuery API documentation refers to a set; I will also use the term collection. And I will use the terms set and collection interchangeably. 

It doesn’t matter what term you use, what matters is that you get a set/collection/bunch of elements, and with that set come a bunch of jQuery methods which you can use to operate on the set.

Fundamental to jQuery is the ability to operate on them all, without explicitly writing a loop. This is referred to as _**implicit iteration**_.

jQuery also provides explicit loops and even a function which will turn your set of elements into an array of elements – and sometimes you may need to do that – but at this stage we should focus on the implicit iteration, which is at the heart of jQuery.  

As an aside, it is common to pass that set you just got into a variable, and the convention is to give that variable a name which begins with a $:  

    var $myCaveats = $(".caveats");
    
The $ at the start of the variable name is just a ‘heads-up’ that we have a jQuery set and we can operate it with jQuery methods.

So, depending on your other needs, you could code:

    $(".caveats").hide() 
    
or           

    $myCaveats = $(".caveats");
    $myCaveats.hide();
    
When would I bother with the extra step? Imagine that I had a complicated form to validate. It might be useful to get the collection of all the text box IDs – and to have a name for that collection since I expect more than one pass will be needed for all of them to be filled in appropriately. 

So far we have seen how to get a collection by selecting for a class or an ID or a tag. Later on we will describe how we can get collections with more specific criteria, and how to manage collections. But first, we turn to what all that implicit iteration can do for us. 

### Chaining

**Chaining – what it is**

The idea behind chaining is fairly simple and very powerful. As we saw in the previous section, the jQuery operator $() returns a set of elements on which you can operate with various jQuery methods. 

When you operate on those elements you get a new set of elements – and you can again operate on them with a new method. 

It’s worth saying this again: A jQuery method returns a set of elements ready to be operated on; each method returns another set of elements ready to be operated on with another method. 

For example, in Chapter 0 we looked at the code:  
       
        $(".blueText").removeClass("blueText").addClass("redText"); 
        
- The first step is $(".blueText") which returns the set of all the elements with the class blueText.  
- We then apply the method removeClass("blueText"). That method removes the class blueText and returns the same set of elements (now missing their blueText class).
- Finally we apply the method addClass("redText"). That method adds the class redText to the set of elements which were passed to it (i.e. the elements which used to have the class blueText) and adds the class redText to those elements – and, you guessed it, returns the set of those elements.

<br>
  
**What methods can you chain?**

- **Any method which returns a jQuery set may be chained.**
  
Fortunately, jQuery has wonderful documentation at https://api.jquery.com/
When you look at the documentation for a particular method, say for addClass(), in the upper right corner you will see that this method returns a jQuery (object) and so you may chain other methods after it. 

<img width="566" alt="addClass()" src="https://github.com/menzin/About_jQuery/assets/144168274/2d0ae008-f0d5-4c3a-a0b9-0f93a254e7d8">

We have already seen that addClass(), removeClass(), show() and hide() all return jQuery sets, as does the jQuery operator $() used to return a jQuery set. 

- **You can chain methods – but not properties**

Obviously, then, when a method returns a string, number or array, then you can not chain it. 

The most frequently returned properties are **length**, and the result of **val()** and **text()**.: 

`length`<br>
The length property is the number of elements in the jQuery collection. It is not a method, and so it can not be chained. <br>
Note: If you query $('#someID') then the collection has only one element and its length is 1. 

`val()`<br> 
The val() method returns the value of the first element in a jQuery set. It is used typically for form elements. <br>
Example: if we have an element <input type = 'text' id = 'givenName' > then $('#givenName').val() will give us whatever has been entered in that text box.  

`text()`<br>
This will return a string of the text content of all the matched elements. <br>
Example: $('h3').text() will give all the h3 headlines "strung together." 


- **Some methods can return either a jQuery set or a property, depending on the parameters **

The .val() method has two variants - $(someSelector).val() will return the value of the _first_ element matched, while $(someSelector).val(newValue) will set the value of _all_ the matched elements to newValue.  

Similarly, the **css()** method has two variants, depending on how many arguments you supply. If you supply only the name of a style property you will get the value of that property.

For example, if our HTML includes

    <p id = "intro" class = "blueText">  …. </p> 
    
then
    
    $("#intro").css("color"); 
    
will return whatever color was set for the class blueText (presumably "blue").


<img width="561" alt="css(propertyName)" src="https://github.com/menzin/About_jQuery/assets/144168274/31463242-af2a-4b89-8294-42d01fec205e">


You can see from the documentation that this returns a string ("blue") and so it can _not_ be chained.  

The other way we can use the css method is to set a property. 

For example, if our HTML includes

    <p id = "intro" class = "blueText"> ... </p> 
    
then

    $("#intro").css("color", "red"); 

will set the color in the selected elements (here just the element with id "intro") to be red and will return those selected elements. So this method may be chained. 

In the documentation you can see that here our method returns a jQuery set.

<img width="559" alt="css(propertyName, value)" src="https://github.com/menzin/About_jQuery/assets/144168274/13fc5b53-9ba9-42e6-9cf7-378b54506681">


Another method which may return different types of things is the val() method. When used with no arguments passed to it, .val() returns the value of a form element. (There are subtleties, which we'll get to later, for checkboxes, radio buttons and select lists; for now think about textarea and input elements.) 

If our form has an element

    <input type = "text" id = "givenName">  

then     

    $("#givenName").val(); 

should return whatever was entered in that text area, and here val() returns a string, number or array. So it may _not_ be chained.

On the other hand

    $("#givenName").val("This field is required"); 
    
Should make _This field is required_ appear in that text element and now it returns whatever matched the selector (in this case just the element with id givenName), and it may be chained.    

For example, the **length property** is the number of elements in the jQuery collection. It is not a method, and so it can not be chained.

> [!NOTE]
> If you query $('#someID') then the collection has only one element and its length is 1.

We will discuss the length property when we review the DOM in Chapter 2.

<br>

**EXAMPLES**

Ideas: <br>
Ask a user to input a color and change the span to that color. Or use a random # generator to choose from an array of colors upon clicking a button.

As we saw in Chapter 0, we can use jQuery to change the attributes of an element – e. g. its style (color) or visibility. 

For example, if we have many items described on a page, we can make the name or picture of that item clickable and show/hide a longer description of that item, or change the size of the image.

What kinds of jQuery methods can we use in our chaining? So far we have seen addClass( ), removeClass( ), show( ), and hide( ). Of course, there are many more methods which will be introduced in chapters 2 and 3.

In Chapters 2 and 3 we will learn about fancier selectors, how to traverse the DOM and how to change the CSS and other attributes of elements. 

In Chapter 4-6 we will see how to bind( ) and unbind( ) event handlers to events, how to manipulate the DOM and how to implement AJAX. 

So far we have blithely introduced changes on our pages (changed color, had error messages appear and disappear) without regard to how screen readers handle such changes (They don't). We have done this in order to focus on simple examples and understand how jQuery acts. But, of course, we need to also write pages which are ARIA compliant. With the understanding we have gained from the earlier chapters, in Chapter 8 we learn how to make our pages ARIA compliant. 


## Summary

We have seen that 
  
    $(document).ready(function() {//do stuff}) 
    
waits until the DOM has been downloaded and then executes the callback function, which is an anonymous function defined inside the `{//do stuff}` brackets. 

We have also seen that when we use a selector 

    $(some selector) 
    
then jQuery will return a set or collection of elements. We can then operate on those elements with a method. When that method also returns a collection of elements (very common) then we can use a new method to operate on them. This is called chaining and typically it looks like 

    $(some selector).method1().method2().method3()….methodN(); 
    
And it is good to remember that the simple selectors 

    $(".someClass"), $("#someID"), $("someTag")
    
All expect the class, ID or tag to be inside quotation marks, or be a variable which holds the relevant string 

    var myClass = ".blueText"; 
    $(myClass)
    
Finally, we have used the methods 
- `addClass("classToAdd")`
- `removeClass("classToDrop")`
- `show()`
- `hide()`
- `css("propertyToGet")`
- `css("propertyToSet", "newValue")` 
- `val()`
- `val("newValue")`

And seen how to chain them. We have also seen how to use
- `.val()` 
- `.text()` 
- `.length`
  
to find the relevant properties.

## Owning it

- What properties are changed by show() and hide()?
- Explain why you can not chain after `$('#myID').val()` or `$('#myID').css("color")`
- Explain why you can chain `$('div').css('color', 'red')`
- Create a page with a list of 3 clickable images. When the image is clicked show a description of the image. Add a "hide all descriptions" button which performs as advertised.  


















