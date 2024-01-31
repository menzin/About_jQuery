# Chapter 5:  Interacting with the Page/ Manipulating the DOM

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













