# Chapter 5: Interacting with the Page/ Manipulating the DOM

## jQuery Methods for Insertion into the DOM

jQuery has eight different methods for inserting new material into a page. We will look at one carefully, and then describe the landscape of all eight. (Once you see one, all this will make sense.)

Let's start with the `after()` method. This method is used to insert some html right after a set of selected elements. Suppose that your page includes the code:

      <p>I would like to say: </p>
      <br>
      <div>Now let's have another paragraph:</div>
      <p>Don't say goodbye; say </p>  
      
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

         <p>I would like to say: </p>
         <b>Hello</b>
         <br>
         <div>Now let's have another paragraph:</div>
         <p>Don't say goodbye; say </p>
         <b>Hello</b>

While an **inside** insertion at the end of each `<p>` would result in the body being 

         <p>I would like to say:<b>Hello</b> </p>
         <br>
         <div>Now let's have another paragraph:</div>
         <p>Don't say goodbye; say <b>Hello</b></p>

So our insertion methods break down into groups of outside insertions and inside insertions (four methods in each group.) The 'inside' methods place the new material just inside the receiving elements and the outside methods place the new material just outside the receiving or targeted elements.

Not so surprisingly, we also find that there are methods which correspond to insertions at the start of the selected elements and at the end of the selected element.





