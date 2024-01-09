# Chapter 1: What Is Happening Here 

This chapter explains the basic mechanisms of jQuery and provides examples of code which is used frequently. Such examples include simple animations, altering the text on a page (e.g. in a quiz) and more complicated versions of Chapter 0 examples. We discuss collections aka jQuery sets, basic chaining, and what methods can and cannot be chained.

In Chapter 2 we will move on to more advanced methods for this.































































## Summary

We have seen that 
  
    $(document).ready(function() {//do stuff}) 
    
waits until the DOM has been downloaded and then executes the callback function, which is an anonymous function defined inside the `{//do stuff}` brackets. 

We have also seen that when we use a selector 

    $(some selector) 
    
then jQuery will return a set or collection of elements. We can then operate on those elements with a method.  When that method also returns a collection of elements (very common) then we can use a new method to operate on them. This is called chaining and typically it looks like 

    $(some selector).method1().method2().method3()….methodN(); 
    
And it is good to remember that the simple selectors 

    $(".someClass"), $("#someID"), $("someTag")
    
All expect the class, ID or tag to be inside quotation marks, or be a variable which holds the relevant string 

    var myClass = ".blueText"; 
    $(myClass)
    
Finally, we have used the methods  
`addClass("classToAdd")`  `removeClass("classToDrop")` <br>
`show()`  `hide()` <br>
`css("propertyToGet")`  `css("propertyToSet", "newValue")`  <br>
`val()` `val("newValue")` <br>

And seen how to chain them. We have also seen how to use 
           .val()         .text()         .length 
to find the relevant  properties.

Owning it
What properties are changed by show() and hide()?
Explain why you can not chain after $('#myID').val() or $('#myID').css("color")
Explain why you can chain $('div').css('color', 'red')
Create a page with a list of 3 clickable images.  When the image is clicked show a description of the image.  Add a "hide all descriptions" button which performs as advertised.  


















