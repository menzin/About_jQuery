# Chapter 5: Interacting with the Page/ Manipulating the DOM

## jQuery Methods for Insertion into the DOM

jQuery has eight different methods for inserting new material into a page. We will look at one carefully, and then describe the landscape of all eight. (Once you see one, all this will make sense.)

Let's start with the `after()` method. This method is used to insert some html right after a set of selected elements. Suppose that your page includes the code:
```html
<p>I would like to say: </p>
<br>
<div>Now let's have another paragraph:</div>
<p>Don't say goodbye; say </p>  
```      
and you would like to insert Hello right after each paragraph. 

To make things simple we'll create a variable material to hold what we want to insert, and then write a script to do the insertion:

     <script>
     var material = "<b>Hello</b>"
     $( "p" ).after( material );
     </script> 

Please notice that after is a method which is on the jQuery collection we have selected (here on $("p") collection.)

Here is the complete example:
[afterDemo3.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter05/afterDemo3.html) 

This example is based on the example at https://api.jquery.com/after/ 

``` html


<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>after demo</title>
  <style>
  p {
    background: yellow;
  }
  </style>
  <script src="jquery.js"></script>
</head>
<body>
 
<p>I would like to say: </p>
<br>
<div>Now let's have another paragraph:</div>
<p>Don't say goodbye; say </p>
 
<script>
var material = "<b>Hello</b>"
$( "p" ).after( material );
</script>
 
</body>
</html>
```

If you look at the elements in the console after the function has executed you will see:

![image](https://github.com/menzin/About_jQuery/assets/144168274/1469a06f-d58d-4764-a483-32fc161f64f0)


That is, the material has been inserted _directly after the matched elements_.

We say that after() is an **outside** method because the line 
       
        $( "p" ).after( material );
        
inserts material _outside_ the paragraph (and after it.)  

You can easily see that the material is outside the paragraph because the paragraph has a yellow background, but the material we added doesn't.

If you want to see the move actually take place (it's pretty cool) run 

[after_2nd_demo.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter05/after_2nd_Demo_v3.html)

``` html


<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>after 2nd demo </title>
  <style>
  p {
    background: yellow;
  }
  </style>
  <script src="jquery.js"></script>
</head>
<body>
 
<p>This is paragraph 1. </p>
<p>This is paragraph 2.</p>
<br>
<div>(This is div 1) Now let's have another paragraph:</div>

<br>
<div>(This is div 2) And here's my second div. </div>
 
<script> 
$(document).ready(function(){
  confirm('Ready for the move now?');
  $( 'div' ).after($('p'));})
</script>
 
</body>
</html>
```

Now that we have seen after() in action, let's talk about the whole set of insertion methods.

First of all, the insertion methods may be either **outside** methods or **inside** methods. The outside methods insert the new material just outside the elements in the jQuery collection and the inside methods insert the new material just inside the tags of the selected elements. 

For example, an **outside** insertion of `var material = "<b>Hello</b>"` at the end of each `<p>` in afterDemo.html results in the body being
```html
<p>I would like to say: </p>
<b>Hello</b>
<br>
<div>Now let's have another paragraph:</div>
<p>Don't say goodbye; say </p>
<b>Hello</b>
```
While an **inside** insertion at the end of each `<p>` would result in the body being 

 ```html
<p>I would like to say:<b>Hello</b> </p>
<br>
<div>Now let's have another paragraph:</div>
<p>Don't say goodbye; say <b>Hello</b></p>
```
So our insertion methods break down into groups of outside insertions and inside insertions (four methods in each group.) The 'inside' methods place the new material just inside the receiving elements and the outside methods place the new material just outside the receiving or targeted elements.

Not so surprisingly, we also find that there are methods which correspond to insertions at the start of the selected elements and at the end of the selected element.

For another example, suppose we have a `div with id = 'results'`: 

     <div id = 'results'> Our results ... </div> 
     
Inserting `<h3>Results</h3>`  with an '_outside_' insertion at the beginning of this div will results in 

     <h3><Results</h3>  <div id = 'results'> Our results...</div>  
     
While an '_inside_' insertion will result in:

     <div id = 'results'><h3>Results</h3>Our results...</div>

So, there are 4 different place you might want to place the new material – just inside or just outside; at the start or the end. That gives us the need for 4 methods.  

While `after()` is an **outside** method, `append()` is an **inside** method.  Consider what happens when we replace after() with append().

Now you can see (from the yellow background) that the material we added is at the end of the paragraph but inside it (and the yellow background)!


 <img width="701" alt="Screenshot 2024-02-22 at 12 48 04 AM" src="https://github.com/menzin/About_jQuery/assets/144168274/ed57d020-9a55-4786-bc03-05ce2c47fc12">


Similarly, `before()` is an **outside** method and `prepend()` in an **inside** method.

### Owning It

Modify the code to try it before() and prepend().You should be able to predict what will happen.

To summarize, we have four new methods - `after()`, `append()`, `before()` and `prepend()` - all of which work on the general form: 


     $(someSelector).ourNewMethodWhichSpecifiesWhereTheNewStuffGoes(theNewStuff);


Warning ⚠️: I stored "the new stuff" in a variable, but if you choose to type in that string directly, then you must put it inside quotation marks: 

     $('p').after('<b>Hello</b>')

Sometimes, it is useful to switch the order in which we specify what the new stuff is and where it is going.  

There are four methods – `insertAfter()` , `appendTo()`, `insertBefore()`, `prependTo()` – which correspond to our four previous methods and do this for us.

Their general format is:

     $(theNewStuff)).ourNewMethodWhichSpecifiesWhereTheNewStuffGoes(someSelector).

For example, the following are equivalent, where we have stored our new stuff in the var material:

        $('p').after(material); 
        $(material).insertAfter('p');

Here is another example. In order to get

        <h3 id='resultsHead'><Results</h3>  <div id = 'results'> Our results...</div>

We could either select for `#resultsHead` and use a method on that set to put `#results` after it,  

       $('#resultsHead').after($('#results');

or we could select for `#results` and use a method on that set to put `#resultsHead` before it

          $('#results').insertBefore( $('#resultsHead'));

So, all together we get eight methods – four which are methods that belong to the targeted elements and four which belong to new elements. Each set of four has two for insertion at the beginning (inside and outside) and two for insertion at the end (inside and outside.)   

Now this is a lot of options – so one way to keep all these methods straight in your mind is to notice that the first four methods, the ones which belong to the receiving/targeted elements – `after()`, `before()`, `append()` and `prepend()` = all have one word names; the second group of methods, those which belong to the new material – `insertAfter()`, `insertBefore()`, `appendTo()`, `prependTo()` – all  have two word names.

Here is the  summary chart below with some examples of what they do. In this chart I have listed first the four outside methods and then the four inside methods. And I have paired them so that you see the method which belongs to the receiving element followed by the method which belongs the the new material (& both methods do the same thing.) 

<img width="595" alt="Screenshot 2024-02-22 at 12 57 08 AM" src="https://github.com/menzin/About_jQuery/assets/144168274/75e44dee-b342-4595-8abc-0bf318174c64">

<img width="595" alt="Screenshot 2024-02-22 at 12 57 36 AM" src="https://github.com/menzin/About_jQuery/assets/144168274/012cd08b-ac45-413c-a976-ccf5572b08f3">


I have tried to keep the examples above relatively straightforward, but the material you insert might be more complex – e.g. an entire div, identified by an id. 

And, of course, you might be adding elements with onclick event handlers, etc. The jQuery site warns us that using any of these methods to add code from an unknown source to our page may leave the page vulnerable to malware.


### Owning It

Suppose that you have an unordered list `<ul id = 'myDogs'>` with three items on it. Suppose also that you have an icon of a dog.   Place that icon at the beginning of each list item. You should be able to do this in two different ways.

### Removing elements from the DOM

There are several ways to remove elements using jQuery. A detailed discussion is beyond the scope of this book, but you can find details at https://api.jquery.com/category/manipulation/dom-removal/ 

Very briefly:
- `empty()` takes all the descendants (and text)  of the selected collection
- `remove()` takes away the elements in the collection and their descendants , and all the content and bound events of the elements which are removed.
- `detach()` removes the element but not its data or events

### Cloning and wrapping elements

Again, this is beyond the scope of the book, but details may be found at https://api.jquery.com/category/manipulation/

Very briefly:
- `clone()` makes a deep copy of the indicated elements. We might, for example, clone an icon which, on being clicked,  collapses/expands part of a menu, save that in a var, and then insert it before any element with a class='collapse'.
- `wrap("someCode")` may be used to wrap someCode around the selected elements – e.g. put the elements in a `<div>` or `<li>`.
- `unwrap()` undoes `wrap()`. 




