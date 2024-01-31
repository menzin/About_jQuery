# Chapter 4: Loops
**Sometimes you absolutely must write a loop- or to each his own**

## `$.each` versus `$(someSelector).each()`

There are two each() methods in jQuery and they are different.  

**$**(someSelector).**each**(someFunction) takes the **collection** returned by $(someSelector) and applies someFunction to each member of the collection in turn.     

**$.each**(someThing, someFunction()) is used to iterate over **arrays** and **objects**. 

Please notice that in **$**(someSelector).**each**() we don't need to specify what the each loop will operate on --- it will operate on the collection which $(someSelector) returned. 

But, with **$.each**(someThing, someFunction()) we need to tell the each loop to operate on someThing. 


## $(someSelector).each() 

So, we have a collection of objects returned by $(someSelector) and we want to do something with them. First of all, if you can use a jQuery method such as addClass() then you should do so. That will be both more efficient and clearer than writing a function to pass to each(). But let us suppose that we need to do something more detailed. Then we will write an anonymous function (a callback) to pass to each(). This anonymous function can have either one parameter or two:  
-  `function(indx, elem) { //do stuff}`
-  `function(indx) {//do stuff}`

Here: 
- **indx** is the index of the element as we iterate through the collection
- In each iteration, **this** will be made to point to the element you are currently operating on
- **elem** if it is used will also be the element you are operating on.

Using the $(someSelector).each() has the incredibly useful property that _this_ is managed for us, and is available to us to indicate exactly what we want it to. The vagaries of using _this_ are too well known to discuss here, so the advantage of having each() take care of this are obvious.

Then, if each() takes care of this, why would we use the function(indx, elem){ } format? 

Because writing elem adds clarity to the code (and because the context for _this_ may change inside your callback, especially with AJAX.)

> [!WARNING]
> You need to refer to the element you are operating on with `$(this)` or `$(elem)` if you are going to apply a jQuery method to it.

See the button which colors the list items (second button) using the css() method and compare it to the one which makes them all black again (third button) using plain assignment of property values. The third button changes a style property and so we can use `this.style.color = whatever`. The second button applies the css() method and so we need to use `$(this).css(stuff)` here.  

[demo_4_0_v3.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter04/demo_4_0_v3.html)

```html
<!doctype html>
<html lang = 'en'>
  <head>
    <meta charset="utf-8">	
    <title>each()  demo</title>
    <script src="jquery.js"></script>	   
	</head>
	<body>

<ul id ='myList'>
	<li>Item 1</li>
	<li>Item 2</li>
	<li>Item 3</li>
	<li>Item 4</li>
	<li>Item 5</li>
</ul>

<button type = 'button' onclick = '$( "li" ).each( function( indx, elem ){
    console.log( $( this ).text() );
});'>Log the list items</button>

<button type = 'button' onclick = '$( "li" ).each( function( indx, elem ){
    if (indx % 3 ==0) {$(this).css("color", "red");}
	if (indx % 3 == 1) {$(elem).css("color", "blue")}
});'>Color the list items</button>

<button type = 'button' onclick = '$( "li" ).each( function(indx){
    this.style.color = "black";
});'>Make the list items black again</button>

</body>
</html>
```

An aside about tables: You will see something like the code above applied to tables, where different rows have different background colors (for legibility). 

The most common situation is to alternate two background colors – and in older code you may see the odd() or even() methods to select all such rows at once.  These methods have been deprecated as of version 3.4 of jQuery. 

You can adapt the code above replacing indx%3 with indx%2. Or you can use the `:even` selector. If you prefer the :even selector then your code will look like: 

```html
<table> <tr><th>ColA</th> <th>ColB</th> <th>ColC</th> </tr>
<tr><td>item1A></td> <td>item1B</td> <td>item1C</td> </tr> <tr><td>item2A></td> <td>item2B</td> <td>item2C</td> </tr>:

</table>

<script>$("tr :even").css("background-color", "yelllow"); </script> 
```

Because table rows are indexed from 0, the heading row will meet the :even criterion. Of course, you may also choose to go back to the heading row and style it separately: 

```html
 $("tr").eq(0).css(["font-weight", "bold","backgound-color", "coral"] );
```

Again, there may be situations where summary or sub-total rows need to be emphasized. Then, adding a class subTotal to such rows will allow you to select those rows using a class selector `$("tr .subTotal")` to change their font-weight or color. This will be preferable to using each().  

Remember that, in general, it is better to use a selector to return all the elements you wish to act upon than to iterate through the elements with `$(someSelector).each()`

This site  https://learn.jquery.com/using-jquery-core/iterating/ also points out when we use attr(), prop(), css() etc. as getters they retreive the appropriate value from only the first match. But we can use `$( ).each()` as a work-around. That is, we can use the each() method to walk through the whole set and retreive the appropriate value for each.

For example, the code below finds the css('color') for each li-element  and logs them to the console. (Of course `$('li').css('color')` would give the color of only the first li.) 

[demo_4_3.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter04/demo_4_3_v1.html)

``` html
<!doctype html>
<html lang = 'en'>

    <meta charset="utf-8">
    <head>	
       <title>$().(each()) demo with getter</title>
        <script src = "jquery.js"> </script> 
        <style>
			.blueText {color:blue; }
			.redText  {color:red; }
			.greenText  {color:green; }
		</style>  		
    </head>
    <body>
	<div>Some colors which you may identify by name: 
	<ul>
	   <li class = 'blueText'>Blue asa the sky</li>
	   <li class = 'greenText'>Green as the grass</li>
	   <li class = 'redText'>Red as a rose</li>
	 </ul>  
	</div> 
	<script>
	  $('li').each(function(index){var cl = $(this).css('color'); console.log(cl);})
	</script>
     </body>
</html>
```

Our script

     $('li').each(function(index){
     	var cl = $(this).css('color');
      	console.log(cl);})

could also have been written as

    $('li').each(function(index, item){
    	var cl = $(item).css('color'); 
     	console.log(cl); })  
## $.each()      
$.each is used to index through an array or object and apply the function passed to each() to every entry in the array or property in the object.  

The formats for arrays and objects respectively are: 
     
     $.each(someArray, function(indx, elem) { //do something} );
     $.each(someObject, function(key, value) {//do something} );  
     
Notice that, whether we are operating on an array or on an object, the first parameter of $.each() is that array or object – either its identifier or the array/object itself. 

For example, we could write
	
 	$.each([2, 3, 5, 7], function(j, elem){console.log(elem*elem);}) )

The second parameter is a function – which takes an index and element of an array, or the key and value of an object.  

Here is a very simple example: 

[demo_4_1.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter04/demo_4_1_v0f.html)
``` html

<!doctype html>
<html lang = 'en'>
    <meta charset="utf-8">
    <head>	
       <title>$.(each) demo</title>
        <script src = "jquery.js"> </script>     
		
	  </head>
	<body>
	<script>
	var A = [1, 2, 3];
	alert(A);
	var sum = 0;
	$.each( A, function( index, value ){
            sum += value;
              }); 
        alert( sum ); 
	</script>		
	</body>
	</html>
```




