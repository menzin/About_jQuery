# Chapter 3: Using Attributes and Properties: I get it – and sometimes I set it.

## The difference between attributes and properties

**Attributes** are specified in the (original) html document.  

For example
    
    <p id = "generalInfo">…..</p>  
    
specifies the id attribute of the paragraph.  

The **properties** are the corresponding _current_ values in the DOM.  

An easy way to remember this is that **pr**operties have the **pr**esent values.  

In our example, the node for the paragraph object has an id property with the value "generalInfo".

For example, when a check box is checked or unchecked, the attribute "checked" was specified in the HTML and it stays the same, but the property "checked" changes as the user checks or unchecks a box. 

Similarly with an `<input type = 'text' value ='Please enter your name'>`, the value attribute  is set to 'Please enter your name', but once the user enters something that value changes.

As you will see shortly, there are jQuery functions `attr()` and `prop()` which are just like `css()` in that they may be used both to retreive a value and to set a value. For all three methods, when only one parameter is passed to the function a value is returned, typically as a _string_:     
- `attr(someAttributeName)`  returns the value of that attribute for the first match
- `prop(somePropertyName)`   returns the value of that property for the first match
- `css(someCSSProperty`)     returns the value of that CSS property for the first match

And all three of these functions may also be used to set that value:    
- `attr(someAttributeName, newValue)`   resets the value of that attribute for all matches
- `prop(somePropertyName, newValue)`    resets the value of that property for all matches
- `css(someCSSProperty, newValue)`      resets the value of that CSS property for all matches

In all three cases, when you pass in newValue, it can be the value returned by a function, and in all three cases you can set multiple attributes/properties/css properties by passing in an object of name: newValue pairs.   


For example, we might, very conveniently, have:

    $('#myLogo').css({"width": 200px, "height":200px,  "border-color":"red"} )

And to repeat, if you have many images on your page, then
  
    $('img').prop('src')   
    
will give the source (src) for the _first_ image 

but

     $('img').prop('src', 'me.jpg') 

will make all your images be the same file.

That said, there are several subtleties and 'gottchas' to discuss (below).  

They revolve around three issues:
- first there are some situations in which the attribute and property have different names. 
- second attributes are almost always strings (after all they come from your HTML document), although there are boolean attributes.
  On the other hand,  properties may be strings, Booleans or computed values. For example, 
  - if you have a style attribute which sets the color red, the property in Chrome and Firefox will be rgb(255,0,0) although other browsers may use FF0000 etc. 
  - for href attributes with a relative address (like href= "fileInThisFolder.html" )  the property will have an absolute address (http://mySite/myFolder/fileInThisFolder.html).

- third, there are some properties where we should not use prop() but should use a different method to retrieve and set the property.  

Before turning to these special situations, let's look at the general cases, and then we'll summarize in a chart.

## Theory, practice and more subtleties

In theory, an attribute stays fixed when you change the corresponding property.  But it is not always so straightforward. Some attributes/properties always **reflect** each other (so that changing either the attribute or the property will change both of them), while other attributes stay fixed when the property changes.  

Demo 2.5 below shows that for the id, title, and href attributes, they change with the corresponding properties. The clearest explanation I have seen of this is at https://stackoverflow.com/questions/6003819/what-is-the-difference-between-properties-and-attributes-in-html (the long response, which explains that id and href are reflected, but value is not.) But that provides more information than we need for our routine coding.

So, what should we do in practice in face of these subtleties? 

- If you are going to need the original value of an attribute, save it in a var. Then you don't need to worry about prop() changing the attribute. (Note: Do NOT save the jQuery code to get the attribute, not  even in a const since const will save the function call but not the value.) 
- Make any need changes using prop(). Some attributes are no longer set-able with attr(), but prop() will always work. 
- Read (below) carefully how attributes and properties differ for check boxes and select (drop-down) lists and use the appropriate method.

Okay, now we are ready to turn to the general situation of using attributes and properties. 


## Using Attributes

Like the val() we saw in Chapter 1, attributes may be used in two different ways – either to select based on their value or to retrieve and use their value. Fortunately for us, the syntax is sufficiently different to make it easy for us to keep these use cases clear. 

**Selecting with attributes**

We can select all elements whose specific attribute has a desired value with  
        
        $("someSelector [someAttribute = 'someValue'] ")  
        
Example: Suppose I have a number of links on a page to a Table of Contents (or to a cart on an eCommerce site) and I want to either add an event handler or some styling to all those links.  

If I have named my Table of Contents with 

           <a name = "ToC"></a>  

then I can select all the anchors which link to it with 

           $("a [href = '#ToC'] ")

And operate on them with a method (for example, add styling or an onclick event handler). 

Please notice the syntax here --- 
- the double quotes" " surround the whole selector `a [href='#ToC']`
- the square brackets [ ] surround the phrase setting the attribute (here href) to the desired value (here #ToC)
- the single quotes ' ' surround the value we want for our attribute 
- we do not put quotes around the name of the attribute. 

Please notice also that this code will return the set of all the elements which match the desired attribute-value pair.

We can, if needed, get fancier here – e.g. have  multiple attribute-value selectors or ask that the value merely contain a substring.  I haven't needed to do this, but if you need to do such things then you should refer to https://api.jquery.com/category/selectors/attribute-selectors/  

**Retrieving and using the values of attributes** 

Now that we seen how to make a selection using the value of an attribute, let's turn to retrieving the value of an attribute for other uses.

`$'(someSelector').attr('someAttribute')` will return the value of the attribute someAttribute in the first element which matches someSelector.

Example: `$('img').attr('alt')` will return the alt value in the first image. If you want the alt values for all the images you will need to use an explicit loop (using the each() or map() method – see Chapter 4 on When You Absolutely Must Write a Loop.)  

If the attribute is undefined, then jQuery will return undefined and you can test for this in the usual way:

    var altAttr = $('img').attr('alt'); 
    if (altAttr === undefined) {// do stuff so this is fixed };

Obviously, once the value of an attribute is retrieved it may be used elsewhere in the script.

Again, please notice that 
- attr() returns a value and so is not chainable, 
- the name of the attribute is a string (i.e. in quotes or a string variable), 
- the value of the attribute that is returned is the original value  (i. e. from the html)  and 
- attr() returns the value of the attribute for only the first element matched.
- In other words, this is quite different from selecting with an attribute-value pair in that only one value is returned (not a set) and we need to put the name of the attribute inside quotes.

`$'(someSelector').attr('someAttribute', newValue)` will change the attribute someAttribute to newValue for all elements matching someSelector.   

This may also be used to add an attribute to selected elements. 

For example, if we need to add an alt value to <img id='logo'…> we could say 

    $('#logo').attr('alt', 'company logo');






