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


## Using Properties

We  have seen how to select based on attribute values and also how to get and set attribute values.

We can get and set property values, analogously to attribute values, but there is no jQuery filter which will select based  on property values. (Of course, we can retrieve the property value and test it with traditional JavaScript.)

The syntax for getting and setting properties is exactly like that for attributes.: 
    `$'(someSelector').prop('someProperty')` will get the named property of the _first_ element which matches someSelector. <br>
         $'(someSelector').prop('someProperty', newValue)` will set the named property to newValue for _all_ elements which match someSelector.

If our code included `<input type='text' id='fName'>` and we wished to change what appears in the text box, we might say: 

    $('#fName').prop('value', 'First name is required'); 
    
### Owning it: 

Take a page you have previously designed which has several images, and define an object which gives the same width, height and border to an image. In your initial script (the one which executes when the DOM is ready), use that object to make all you images have that format. Optionally, you can have a button which changes all the images to a larger format.  

Take a page you have previously designed which has a form.  For any input of type text which is required you would like the initial value to be "Required: please enter " or some similar message.  Using either a class req or checking that the required attribute is present, make the value of all those fields be the desired message. As in the previous, example, this should be part of your initial script.

## Check it out --- checked and selectedIndex

The difference between attributes and properties is especially relevant for check boxes, radio buttons and select lists.

- **Checkboxes**

For a checkbox, the attribute 'checked' will always have the value that was set in the (original) HTML. But the property will change based upon user input. The attribute 'checked' will have either the value 'checked' or the value 'undefined'. The property 'checked' will have the value 'true' or 'false'. The two values below will be the same: 
- `$('#someId').prop('checked')`
- `$('#someId').is(':checked')`
Note: the colon in the checked filter!

We can see the attributes and properties in question using the code below: 

[demo_3_2_checked.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter03/demo_3_2_checked.html)

```html
<!doctype html>
<html lang = 'en'>

    <meta charset="utf-8">
    <head>	
       <title>checkbox demo</title>
       <script src="jquery.js"></script>
	   <script>
	      function displayCheck(){
		  var mes1 = "For the initially checked button the value of the attribute checked is  ";
		  mes1 += $('#startChecked').attr('checked');
		  mes1 += " and the checked property is "
		  mes1 += $('#startChecked').prop('checked');
		  var mes2 = "... For the initially unchecked button the value of the attribute checked is ";
		  mes2 += $('#startUnchecked').attr('checked');
		  mes2 += " and the checked property is "
		  mes2 += $('#startUnchecked').prop('checked');
		  alert(mes1 + mes2);
		  //alert(mes2);
		  }
		</script>    
	  
		
	</head>
	<body>
	<form>
	  <input type = 'checkbox' id = 'startChecked' checked>Initially checked <br>
	  <input type = 'checkbox' id = 'startUnchecked'>Initially unchecked <br>
	  You can check and uncheck these boxes and see what happens to the attr and prop 'checked'.<br>
	  <button type = 'button' onclick = 'displayCheck();'>Show the attr and prop </button>
	  
	</form>
	</body>
	</html>

```

- **Radio Buttons**

For a group of radio buttons there is, of course, only one which is checked. Here we can use the :checked filter to identify the one button in the group which has been checked, and then print its value. We could have used the .is(':checked') here (as with checkboxes) instead. 

[demo_3_3_radio.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter03/demo_3_3_radio.html)

``` html
<!doctype html>
<html lang = 'en'>

    <meta charset="utf-8">
    <head>	
       <title>checkbox demo</title>
       <script src="jquery.js"></script>
	   <script>
	      function displayCheck(){
		  var $rButton = $('input[name = "firstGroupName"]:checked');
		  alert($rButton.val());  
		  }
		</script>    
	  
		
	</head>
	<body>
	<form>
	First set of radio buttons:<br />
	  <input type="radio" name="firstGroupName" id="firstGroup1" 		   
	     value="value for first button" checked="true" />Text next to button 1.&nbsp;&nbsp;
	  <input type="radio" name="firstGroupName" id="firstGroup2" 
	      value="value for second button"/ >Text next to button 2.&nbsp;&nbsp;
	  <input type="radio" name="firstGroupName" id="firstGroup3" 
	     value="value for third button" />Text next to button 3.&nbsp;&nbsp;
	  <input type="radio" name="firstGroupName" id="firstGroup4" 
	     value="value for fourth button" />Text next to button 4.<br /><br />
	  You can changed the checked and see the value of the checked.<br>
	  <button type = 'button' onclick = 'displayCheck();'>See the value</button>
	  
	</form>
	</body>
	</html>
 ```

- **Select lists are discussed after we look at text() below.**

**For some attributes and properties we have more specific methods ----- val(), html(), and text(). **

All three of these methods follow the same general structure, with some subtleties: 				 
 - `$(someSelector).oneOfTheseMethods()`  returns a value (not chainable)
 - `$(someSelector).oneOfTheseMethods(newValue)`  sets the value & returns a jQuery set (is chainable.)
 
We have already seen that **val()** is used to get and set the value of any element (button, textarea, or input type) of a form. 

Example: 

	<input type ='text' id = 'team'> 
 	$('#team').val('Please enter the team you are on'); 
  
The css() method is similar, in that  css() may be used to get and set a specific css property. With css() however you must always specify what property you are interested in. 
- `css('someProperty')`  may be used to get **that property** (not chainable)
- `css('someProperty', 'newValue')` set the style properties of an element.

In other words, for all these methods
- when you use it as a getter, it returns a value & is not chainable;
- when you add a parameter and use it as a setter, it returns a jQuery set and is chainable.

As with css() you can set multiple properties or attributes by putting the name-value pairs in an object. For example,  
        
	$('.conditions').prop({'color':'gray', 'font-size':'small'}) ;  
 will make all items with the conditons class be in small, gray text.

_Please notice that_ 
- Unlike the other methods we are discussing in this section, css(), attr() and prop() require a specific property or attribute  whose value we will get or set.
- When we are setting the multiple properties, our object of key-value pairs needs quotes around both the property names and their values!

[demo 3_4_css_method.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter03/demo_3_4_css_method.html)

``` html
<!doctype html>
<html lang = 'en'>

    <meta charset="utf-8">
    <head>	
       <title>css() demo</title>
       <script src="jquery.js"></script>
	   
	   <style>
	      .redText {color:red;}
		  .blueText {color:blue;}
		  .embolden {font-weight:bold;}
		  .plain {font-weight:normal;}  		  
	</style>  
		
	  </head>
	<body>
	Open the console to see how the css() method operates.<br />
	After any change, use the buttons which show the elements' properties.<br ><br>
	   <div class = 'redText'>The div heading is red
       <p class = 'embolden'>The paragraph inside the div.  </p>
	   </div>
	   <button onclick="console.log($('div').css('color'));">Show div color</button> 
	      <!--returns rgb(255,0,0)--><br>
	   <button onclick="console.log($('p').css('color'));">Show paragraph color</button>
	      <!-- returns rgb(255,0,0)--> <br><br>
	   
	   <button onclick = "var pr= $('div').css(['color','font-weight']);
	                      console.log(pr)">
	      Show div styles using an array of properties</button>
		  <!-- returns {color: "rgb(255, 0, 0)", font-weight: "400"} --><br />		 
		<button onclick = "var pr= $('p').css(['color','font-weight']);
	                      console.log(pr)">
	       Show paragraph styles using an array of properties</button>
			<!-- returns {color: "rgb(255, 0, 0)", font-weight: "700"}  --><br><br>
		<button onclick="$('p').css('font-weight', 'normal');"> 
		       Remove bold from paragraph with css method</button>	 <br>
		<button onclick ='$("div").css({"color": "blue", "font-weight": "bold"});'>Change div properties</button>		
  
	 </body>
</html>	 
```

Next we turn to text() and html(). Both of these methods return the "inner" text at the identified selector, but the returned material is interpreted differently, and they return different numbers of matches.

This will become useful in Chapter 5 when we learn how to manipulate the DOM in sophisticated ways. For now, let's use them in straightforward ways.
 
The **text()** method, like the other methods we have examined, may be used to get content or  to set it!  

If the jQuery set that the selector returned has multiple elements in it _then the content of all those elements will be strung together in the return value or will be reset_, depending on whether you are using text() to get or set the text. 

This is a very important difference from all the other methods we have been discussing  - attr(), prop(), css() and html()  -  which get or set only the first match!   

Again, text() will retreive all the text in the selected elements, while html() retreives only the text from the first match.  This makes a get with the text() method different from html(), val(), etc. 

Warning: _you may not use text() to get the value of form elements_. For form elements you need to use val()

[demo_3_5_text_method.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter03/demo_3_5_text_method.html) 
``` html

<!doctype html>
<html lang = 'en'>
  <head>
    <meta charset="utf-8">	
    <title>select list  demo</title>
    <script src="jquery.js"></script>	   
	</head>
	<body>
	<div id='div1'>
	  <p id='p1'>Paragraph 1</p>
	  <p>Paragraph 2</p>
	</div>
	
	<div id='div2'>
	  <ol>Ordered List
	     <li>Item A</li>
		 <li>Item B</li>
		 <li>Item c</li>
	  </ol>
	</div>
	<button type = 'button' onclick = "console.log($('#div1').text());"> Show text for first div</button>
	<button type = 'button' onclick = "console.log($('#div2').text());"> Show text for second div</button><br>
	<button type = 'button' onclick = "$('#p1').text('New stuff');">Change first paragraph</button>
	<button type = 'button' onclick = "$('p').text('Surprise!');">Change all paragraphs</button>
	
	
	</body>
</html>
```

As always, you should predict what will happen, based on the material just above this code, and then verify your predictions by running the demo.

Finally, we turn to **html()**. The html() method is just like the text() method except that it gets only the first match. For setting, it operates on all matches.

**Summary table**

Notes: You may use `$(this)` as the selector. 

See code exmples for checked (above) and selectedIndex (below). 

Remember: You get **ONE** value but can set **MANY**: **html()** and **val()** as getters have no parameters. **text()** is different from **html()** in that it retreives all the matched content.

**The content of `<input type='text'…>` is accessed with val()**. You may use `$(this).val()` in its onclick handler, because, as always, the value of _this_ is determined by where the handler is called from.

<img width="549" alt="SummaryTable1" src="https://github.com/menzin/About_jQuery/assets/144168274/c13bfa3f-7bb0-486d-a50f-04a331d0dbb4"><img width="546" alt="SummaryTable2" src="https://github.com/menzin/About_jQuery/assets/144168274/ab159490-682a-494b-9f4a-8b511f07f7ad">

Warning: When you replace a value make sure that you are leaving you html as valid html. 

- **Select lists**

While checkboxes and radio buttons have a _checked_ attribute, the options in a select list have a _selected_ attribute. 

You can find the chosen option using either the _selectedIndex property_ (as in displayOption1() below) or you can use the _:select filter on the option_  (as in displayOption3() below). 

Please note that there is **no space** between the _option_ selector and the _:selected_ filter. 

Of course, text() will return what is between the opening and closing option tags, and val() will return the value of the value attribute (inside the opening option tag.)  

And, as noted above, text() will string together all the texts of matching items (which is important when multiple selections are allowed), while val() will return only the first match.

[demo_3_6_selectList.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter03/demo_3_6_selectList.html)

``html

	<!doctype html>
	<html lang = 'en'>
	
	    <meta charset="utf-8">
	    <head>	
	       <title>select list  demo</title>
	       <script src="jquery.js"></script>
		   <script>
		      function displayOption1(){
			  //This list has no default option selected
			  var $chosenOption = $('#dropDownName1').prop('selectedIndex');
			  //Line below does not work
			  //var $chosenOption = $('#dropDownName1 [selected = "selected"]');
			  
			  alert($('#dropDownName1 option').eq($chosenOption).val());  
			  }
			  function displayOption2(){
			  var $chosenOption = $('#dropDownName2 option:selected');
			  //NOTE: There should be NO space before the semi-colon		  
			  alert("The text for your option is " +$chosenOption.text());    		  		  
			  }
			  function displayOption3(){
			  var $chosenOption = $('#dropDownName3 option:selected');
			  //NOTE: There should be NO space before the semi-colon
			  
			  alert("The text for your option is " +$chosenOption.text());
			  alert("The value for your option is " +$chosenOption.val());		      		  		  
			  }
			  
			</script>    
		  
			
		</head>
		<body>
		<form>
		<!-- The value is what is passed to the web server .. e.g. it might be MA when the text says Massachusetts.  -->
		<!-- It is possible to find ready made drop down lists for all states, all countries, etc.  on line.  -->
		Instructions for drop-down list goes here!<br />
		This drop down list has no default value selected.<br />
		<select name='dropDownName1' id='dropDownName1' size='maxNumberItems' multiple='false'>
			<option value='info for choice 1 for useful procesing' >Text appearing on list for choice 1</option>
			<option value='info for choice 2 for useful procesing' >Text appearing on list for choice 2</option>
	
			<option value='info for choice 3 for useful procesing' >Text appearing on list for choice 3</option>
			<option value='info for choice 4 for useful procesing' >Text appearing on list for choice 4</option>
	</select>
	<br /><br />
	
	<!-- Here is a drop down list which allows for multiple selections -->
	<!-- Notice that the multiple attribute of select is set to multiple or true -->
	Instructions for multiple selection drop-down list goes here!<br />
	<select name='dropDownName2' id='dropDownName2' size='maxNumberItems' multiple='true'>
	    <option value='info for choice 1 for useful procesing' selected='true'>Text appearing on list for choice 1 </option>
	
	    <option value='info for choice 2 for useful procesing' >Text appearing on list for choice 2 </option>
	    <option value='info for choice 3 for useful procesing' >Text appearing on list for choice 3 </option>
	    <option value='info for choice 4 for useful procesing' >Text appearing on list for choice 4 </option>
	</select>
	<br /><br />
	
	Instructions for third drop-down list goes here!<br />
	This drop down list has the second item selected as the default value.<br />
	<select name='dropDownName3' id='dropDownName3' size='maxNumberItems' multiple='false'>
	    <option value='info for choice 1 for useful procesing'>Text appearing on list for choice 1</option>
	
	    <option value='info for choice 2 for useful procesing' selected='selected'>Text appearing on list for choice 2</option>
	    <option value='info for choice 3 for useful procesing' >Text appearing on list for choice 3</option>
	    <option value='info for choice 4 for useful procesing' >Text appearing on list for choice 4</option>
	</select>
	<br /><br />
	   
		  You can changed the checked and see the value of the checked.<br>
		  <button type = 'button' onclick = 'displayOption1();'>See the option on the first list</button>
		  <button type = 'button' onclick = 'displayOption2();'>See the option(s) on the second list</button>
		  <button type = 'button' onclick = 'displayOption3();'>See the option on the third list</button>
		  
		</form>
		</body>
		</html>

	```



### Owning it: 

Using a form you have previously designed, find the values for the checked radio buttons and checkboxes and for the selected option on you select lists. For example, you may store those values in variables for puporses of validating your form.
 
