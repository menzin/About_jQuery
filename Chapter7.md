# Chapter 7: AJAX

## Review of AJAX and its Key Components

The basic process of AJAx (which originally meant Asynchronous JavaScript and XML) is to be able to retrieve content so that it may then be massaged by your script and inserted in your page. 


It is important to remember that AJAX is **asynchronous**. In practical terms, this means that your browser will try to execute the rest of your script while it is retrieving the data (html, xml, json, etc.) that you requested. We will see, when we discuss $.getJSON, how to make sure that the retrieval has been completed before you use try to use that data. 

Here, we review the basic components of AJAX without dwelling too much on the details, since jQuery makes it much easier to write AJAX requests.

To make an AJAX request in vanilla JavaScript, we create an an XMLHttpRequest object. This object has several methods and properties:

- The `open()` method opens a channel between your page and the server for communication. It specifies the URL on the server and whether any data will be sent with a GET or with a POST.

    Recall that in a GET any data is in key-value pairs which are appended to the URL (i.e. open) and that in a POST the key-value pairs are in the body of the HTTP message. See https://www.w3schools.com/tags/ref_httpmethods.asp if you need a review.

- The `send()` method initiates the actual sending of data, and waiting for a response.

- The **readyState** property holds a value between 0 and 4, which tells you where you are in the cycle of opening the channel, sending and receiving data. A value of **4** indicates that the return data has been received.

- A **status** property (which may be accessed only when you are in state 3, receiving, or 4, done) which tells you whether or not the transaction was successful. A value of 200 means success, and of course the infamous 404 means the page was not found.

- The **onreadystatechange** event handler which fires whenever the readyState property changes. The user needs to write the code for this event handler --- typically you check to see that you have reached state 4, then do some error handling if the status is not 200, and proceed with using the data you got if the status is 200. <br>
Depending on whether the data was XML or JSON it will be placed in a different property of the XMLHttpRequest object. Either way, you will use JavaScript and DOM methods to do whatever you need to with the data.

## AJAX in jQuery

jQuery has a jqXHR object, which is an XMLHttpRequest object with some additional properties and methods. These additions will simplify our lives.


### The .ajax() method

There are a number of shortcut methods for simple requests (see next paragraph) , but the basic structures for the general ajax() method are:  

- `$.ajax(url_where_the_data_is [, optional object with settings] )` //executes an Ajax request

- `$(someSelector).ajax(url_where_the_data_is [, optional object with setting] )` //executes an Ajax equest, typically putting the data at someSelector  

The versatility of the ajax() method lies in the settings. We won't describe all of them, but here are the most important ones: 

> [!NOTE]
> This lists the keys in the setting object – the programmer needs to provide the values, which may be a string (as for method) or a function to execute (as for done, failed and always) or some other object (as for data).

- **method**: Possible values are "GET", "POST" and "PUT" ---- it defaults to "GET" if omitted.

- **done** (replaces **success** ): Function to call after a successful completion of retrieving the data. Typically this handles whatever it is you want to do with the data

- **failed** ( replaces **error**): Function to call when the data retrieval is not successful

- **always** (replaces **complete**): an optional function to be executed after done or failed has finished executing.

- **data**: The data (object, string or array) you are sending to the server. Details of how the data is sent are discussed below.

- **username** and **password** are the obvious things if the server requires your script to log on

> [!NOTE]
> In older code you may find code which uses success, error or complete. These are deprecated as of jQuery3.0 and you should now use done, failed, and always.
> One can also get detailed control over the stages in Ajax by using Ajax Events (See https://api.jquery.com/Ajax_Events/ ). We will not be discussing them here.

### The shortcut methods:

The shortcut methods (which we describe in more detail below, with examples) are:

- `load()` - which loads a file with HTML (typically not an entire html document, but html which you wish to insert)
- `getJSON()` – which retrieves some JSON using GET
- `getScript()` – which retrieves a JavaScript script using GET

_For these three methods above you must provide the URL on the server where the data is , and you can optionally provide data to send to the server and/or the function to execute if the request succeeds._

There are also two general shortcut methods:

- `get()` - which retrieves an arbitrary type of file/data using GET
- `post()` - which retrieves an arbitrary type of file/data using POST

For these last two methods, in addition to the required URL and optionla parameters for the first three shortcut methods you may able (optionally) provide the dataType of what you are retrieving.

We are now ready to examine all these methods in more detail, starting with the simpler one and working our way up in complexity.

In order to execute these examples you will need to make use of a <ins>_server_</ins> – e.g. a computer where you make your home page publicly available, not just your standalone computer.

For security reasons you will <ins>_not_</ins> be able to import data from a different domain. So we will put our home page and the files with our data in the same folder.

## Use of jQuery for a simple retrieval of data

**Retrieval of some HTML which we are going to insert on our page with load()**

> [!NOTE]
> These files need to be on a server – e.g. wherever you put your web pages.

One of the simplest things to do is to get some html from another file and insert it on your page. Specifically we do the following: 

- Identify a div or other element where we want the new html to go. <br>
  In this example we have identified the div with id _divForLoad_

- Use the load method for that div or other element to identify the html we will pass in; the url is passed to load as its parameter in string form. <br>
In this example we are going to get the contents of the file newHTML.html which is in the same folder as our script, although you could write a more complex path here.

- That html file is just a snippet of html – not a whole web page!

- Please notice that the entire contents of our div or other element will be _replaced_  by the new html. Obviously, if you don't want to replace anything, then you put the new html in an empty element. <br>
In our example, the phrase "New HTML will go here" gets replaced.

- The new html you are inserting will get any styling that was specified in your style sheets.

	[ajaxDemo1Load.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter07/ajaxDemo1load.html)

    ``` html
	<!doctype html>
	<html lang='en'>
		<head>
	  	    <meta charset="utf-8">
		    <title>AJAX Demo 1</title>
	     	<script src="jquery.js"> </script>   <!-- the jQuery library in the same folder  -->
		    <script>
				//We can put functions here
		
		    </script>
	        <!--  links to style sheets go here  -->
	    </head>
	    </body>
	        <div id = 'divForLoad'>New HTML will go here</div>
			<button type = 'button' onclick = "$('#divForLoad').load('newHTML.html');">
			    Click here to load the newHTML contents</button>
		</body>
	</html>
	``` 

The html file which this page uses is:

[newHTML.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter07/newHTML.html)

  	<h3>This is the contents of the newHTML file</h3>

Load the demo, open the debugger to see the elements, and watch what happens to the div when you click the button. 


### More complex retrieval of html data using load()

Rather than loading all of an html file you might choose to load only part with some particular ID. For example, we might have a page of thumbnails for shopping with each image having an id that corresponded to a catalog number – e.g. Fall123. If the html file we load from had divs with the same numbering system for their IDs, then when we clicked on the image with ID Fall123, we would construct a variable 'newHTML.html #Fall123' and use it as the parameter for the load:

[ajaxDemo2load.html]()

``` html

<!doctype html>
<html lang='en'>
	<head>
  	    <meta charset="utf-8">
	    <title>AJAX Demo 2</title>
     	<script src="jquery.js"> </script>   <!-- the jQuery library  -->
	    <script>
		function loadURL(someImage){
		var newURL = 'newHTML2.html #' + someImage.id;
		$('#emptySpot').load(newURL);
		}
	
	    </script>
        <!--  links to style sheets go here  -->
    </head>
    </body>
        <p>General text</p>
		<img src = Fall123Thumbnail.jpg alt = 'red umbrella from Anil Kumar Gridhar on pexels.com' 
		    height="200" width="125" id = 'Fall123' onclick = 'loadURL(this);'>
		    Some text about Fall123 <br>		
		<img src = Fall456Thumbnail.jpg alt = 'blue umbrella from Zach Jarosz on pexels.com' 
		    height="200" width="125" id = 'Fall456' onclick = 'loadURL(this);'>
		    Some text about Fall456 <br>
        <div id = 'emptySpot'></div>					
	</body>
</html>
                            
```

Of course our newHTML.html file would now look like: 

[newHTML2.html for ajaxDemo2load](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter07/newHTML2.html)

``` html
<div id='Fall123'>Whatever image and info you want about Fall123. 
This photo is by Anil Kumar Grindhar is from pexels.com</div>
<div id='Fall456'>Whatever image and info you want about Fall456.
This photo is by Zach Jarosz is from pexels.com</div>
```
As with the previous demo, you should run this demo, watching the div contents in the debugger!

So far we looked at `$('#someID').load('urlForTheHTML')`, retrieving either part or all of the html file. The load() method, however, has two optional parameters: an object which holds information for the server, and a callback function to execute if the load is successful. 

Note that because one of these parameters is an object and the other a function, you may include either, neither or both – i.e. jQuery can tell what parameters you included. 

As an example of the use of the callback function, suppose that if your user retrieved some html about an item which is on sale, you had an alert box pop up: 

	$('#divForLoad').load('newHTML.html', function(){alert('On sale today only!')} ) ; 

The complete set of possibilities for the load() method is (see https://api.jquery.com/load/#load-url-data-complete:): 

	$('#someID').load(someURL [,someData] [, functionUponCompletion]) 

That said, load() is usually used for straightforward applications. We will see these optional parameters used for extensively in `.getJSON()` and `.ajax()`

### Owning It

Instead of using and accordion in the Table of Contents for http://web.simmons.edu/~menzin/CS321/CS321_TOC.html we could have shown just the Units, and made them clickable, to bring in the chapter headings.

Modify that page so that the first three Units are clickable (and create the appropriate html file for the chapters you will load.)


## Retrieval of some JSON data – which we will massage with our callback function

**Reminder about JSON and objects in JavaScript.**

JSON is actually a subset of JavaScript objects, but with a few details that must be taken care of – especially the requirement that every key be a string written with double quote marks. For example, 365 is not an acceptable key in JSON, but "365" is. Fortunately, the JSON module will make the needed conversions for us.  

If you have a JavaScript object someObject in your code then `JSON.stringify()` will turn it into JSON - i.e. JSON.stringify(someObject) is a new JSON variable. 

If you have a JSON object someJSON then `JSON.parse ()` will turn it into a JavaScript object -  i.e. JSON.parse(someJSON) is a new JavaScript object.  

There are also utilities, external to your script, which you may find useful if you want to prepare some JSON for AJAX to retrieve: 
- Utility to convert a csv file to json is at https://www.csvjson.com/csv2json
- Utility to convert a string to json is at https://www.json-generator.com/
- Several utilities, including for XML conversion, are at https://onlinejsontools.com/stringify-json

| State | Capital |
| ------------- | ------------- |
| Massachusetts  | Boston  |
| Texas  | Austin  |
| New York  | Albany  |
| New Hampshire  | Concord  |

I used the first of these utilities to convert my csv file to a JSON object: 

``` html
{
"Massachusetts": {
	"Capital": "Boston"},
"Texas": {
	"Capital": "Austin"},
"New York": {
	"Capital": "Albany"},
"New Hampshire": {
	"Capital": "Concord"}
}
```

> [!NOTE]
> If our CSV file did not have headings, we would get a much simpler json object. I am showing you how to handle the more complex json since you will want the headings when there are more than two columns – e.g. state capital and population (in millions). Then you will want the value of "Massachusetts" to be the object {"Capital": "Boston", "Population": 6.902}

Remember how we handle JSON:
[jsonReminders.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter07/jsonReminders.html)

``` html
<!doctype html>
<html lang='en'>
	<head>
  	    <meta charset="utf-8">
	    <title>JSON reminders Demo 2</title>
     	<script src="jquery.js"> </script>   <!-- the jQuery library  -->
	    
        <!--  links to style sheets go here  -->
    </head>
    </body>
        <!-- Manipulating a JSON object so as to put its key-value pairs in an array -->
		<script>
			ourJson = {
			  "Massachusetts": {
				"Capital": "Boston"
					},
			  "Texas": {
				"Capital": "Austin"
				  },
			  "New York": {
				"Capital": "Albany"
				  },
			  "New Hampshire": {
				"Capital": "Concord"
				  }
			};
			arr = [];
			
			$.each(ourJson, function(k, v){console.log(k)});  //Logs the keys - e.g. Massachusetts			
			
			$.each(ourJson, function(k, v){console.log(v)}); //Logs the object values - e.g.
			                   //  {Capital:"Boston"}
						
			$.each(ourJson, function(k,v){console.log(v.Capital)}) //Logs the value of Capital - e.g. Boston
			
			//construct the array of states and their capitals
			
			//Reminder: In JavaScript an ordered pair is an array.
			
			$.each(ourJson, function(k, v){elem = [k, v.Capital]; console.log(elem); arr.push(elem);})
			
		
		</script>

        <div id = 'emptySpot'></div>			
			
			
	</body>
</html>
```

Now, let's adapt this to using getJSON to retrieve our JSON from another file. Then, after I retrieve this JSON object I will make the array of ordered pairs of the form (State, Capital) so that I can write a quiz to randomly select a state, show its name, and ask for the capital. (NOTE: My utility offers me the option of an array of objects, which would use slightly different processing.) 

The general form for getJSON is `$.getJSON(someURL [,someDataForServer][,callbackFunction]);`

We'll defer the explanation of the optional Data parameter until we discuss the ajax method in the next section.

Please notice that getJSON is a global function (unlike load() which is a method of a specific element). Suppose that our JSON is stored in a file capitals.json in the same folder as our webpage. (Note the json extension!) 

Our webpage will ask for 

	$.getJSON('capitals.json', callbackFunction) 
 
We will write the callback function as an anonymous function; it will have one parameter, namely the json which getJSON has retrieved and put into that one parameter. So our code will look like:

	 $.getJSON('capitals.json', function(myJSON) {//process myJSON }) 

and now we need to process myJSON. The each() method is perfect for this task, just like in our code for jsonReminders.html (In fact, it is the identical processing function.) 

Once again, because we are using the server to access a file, we need to upload our files to an appropriate site. That said, the code below will work:

``` html
var arr=[];
	function manip(){
	<!-- Manipulating a JSON object so as to put its key-value pairs in an array -->
				
		$.getJSON('capitals.json', function(ourJson){
		   $.each(ourJson, function(k, v){                           
		   elem = [k, v.Capital];                            
		   arr.push(elem);
		  })
		});  //end of getJSON
		} //end of manip
```

When we put this in a full script and try to actually use the entries we have loaded into arr, however, we have problems: 

 Using ajaxDemoPartlyWorks_v7.html

``` html
<!doctype html>
<html lang='en'>
	<head>
  	    <meta charset="utf-8">
	    <title>getJSON Demo</title>
            <script src="jquery.js"> </script>   <!-- the jQuery library  -->
            <script> 
		var arr=[];
		function manip(){
		<!-- Manipulating a JSON object so as to put its key-value pairs in an array -->
					
			$.getJSON('capitals.json', function(ourJson){
			   $.each(ourJson, function(k, v){                           
                           elem = [k, v.Capital];                            
                           arr.push(elem);
                          })
			});

		     //Now comes some interesting issues: 
		      console.log(arr);   //Shows an array w/ 4 elements, each of which is an array

//And the contents of the 4 inner arrays are just what you expect
		    //The next 2 lines don't work- See explanation right below the code

			for(i=0; i<4; i++){alert(arr[i]);}  //Says undefined
			for(i=0; i<4; i++){alert('The capital of '+ arr[i][0] + ' is ' + arr[i][1] )}

	 }
				  
	</script>   		
    </head>
    </body>
	<button type = 'button' onclick = "manip()">Click for the capitals</button>       		
	</body>
</html>

```

The problem is that getJSON is ***asynchronous***. That means that it will work in the background, and meanwhile our script will go on to do other things. In fact, what happens is that the script tries to access the values in arr[i] _before getJSON_ has finished filling that array.

For this code, the asynchronicity causes troubles even when we are retrieving json on the same web site. Clearly, if we are retrieving json from a remote website, it might take even longer for getJSON to complete its work.

> [!NOTE]
> This may be a problem in load() also, since load uses an asynchronous GET. In our load() example, however, we just stared at the new HTML and didn't try to interact with it.

So, how do we resolve our getJson problem? .getJSON(), like the more general .ajax() function, has a method done() which will execute after the getJSON (or ajax) is done executing. At that point, of course, we know that arr[] has all the values we retrieved using our asynchronous function and we can interact with them.

To make this happen, we have added the done method (in bold) to the object which `$.getJSON()` returns.

[getJSONDemo.html](http://web.simmons.edu/~menzin/CS321/Unit_5_jQuery_and_Ajax/About_jQuery/Chapter07/getJsonDemo.html)

``` html
<!doctype html>
<html lang='en'>
	<head>
  	    <meta charset="utf-8">
	    <title>getJSON Demo</title>
     	<script src="jquery.js"> </script>   <!-- the jQuery library  -->
		<script> 
		var arr=[];
		var gJobject;
		function manip(){
		<!-- Manipulating a JSON object so as to put its key-value pairs in an array -->
					
			gjObject = $.getJSON('capitals.json', function(ourJson){
			   $.each(ourJson, function(k, v){                           
                           elem = [k, v.Capital];                            
                           arr.push(elem);
                          })
			})
			.done(function()
			      {for(i=0; i<4; i++){alert('The capital of '+ arr[i][0] + ' is ' + arr[i][1] )}}
				  );			
			};
			
				  
	     </script>   		
    </head>
    </body>
	<button type = 'button' onclick = "manip()">Click for the capitals</button>       
			
			
	</body>
</html>
```
More generally, `$.getJSON` has three methods which you may specify: `done()`, which is exectuted when getJSON finishes successfully, `fail()`, which is executed when there is an error, and `always()` which is executed in either case and after done() or fail() if they have been specified. 

Because each of these methods returns the same jqxhr object, the syntax for them is chained: 

``` html
$.getJSON('the_url' [optional data for the server[], function(retrievedJSON){//process the JSON}] )               .done(function(retrievedJSON) {//after success })
.fail(function(retrievedJSON){//after error})
.always(function(retrievedJSON){//after everything else};
```
You may omit one or more of the methods done, fail, always if they are not needed. It is also fine, if you find it easier to maintain, to write: 

``` html
 var myGJ =
	$.getJSON('the_url' [,optional data for the server [], function(retrievedJSON){//process the JSON}] );          	myGJ.done = function(retrievedJSON) {//after success }); 
 	myGJ.fail = function(retrievedJSON) {//after error });
	myGJ.always = function(retrievedJSON) {//after everything else });
```

Also, you may choose to put all the processing (upon success) in the done() method. When we talk about `$.ajax()` you will see that the processing upon success is put in the done() method.

Reference: https://stackoverflow.com/questions/33946699/iterating-over-collection-from-getjson-and-pushing-new-objects-into-an-array-l

Use of jQuery function `$.getScript` for retrieval of a script is touched upon at the end of this chapter.

### Sending data, including deciding between GET and POST

As a reminder, in a GET the data which is sent to the server is url-encoded and then appended to the URL, while in a POST the data is in the body of the message.

This makes the GET less secure: for example, the data which was appended the the request is stored in the history of the user's browser and the server's logs. Obviously, no requests which includes passwords should be made using a GET.

On the other hand, a GET request may be bookmarked, and often the results are cached in the user's browser. With a POST, the data must be re-submitted. 

Finally, in a GET the data, because it will be url-encoded, can be only in ASCII and is limited in length. A request of up to 2,000 characters in a GET is safe, but some browsers support longer requests.  

The` $.get()` function is exactly like the `$.getJSON()` function, except that it may be used to retrieve various kinds of data. 

`$.get` has an optional last parameter to specify the data type; if that parameter is omitted then jQuery will guess among the types html, xml, json, text and script; if that parameter is 'json' then your `$.get()` is the same as `$.getJSON()`. 

Another way to phrase this is that the following are equivalent: 

	$.getJSON('capitals.json', function(ourJSON){// process}}   

 	$.get('capitals.json', function(ourJSON){//process}, 'json') 

It is also possible to write `$.get({object with settings})` where one of the keys in the _object with settings_ <ins>must</ins> be url. 

`$.get` is a shorthand method for `$.ajax` where the method is specified as GET. 

<br>

#### Use of $.post() 

This is just like the `$.get()` function, except that the http POST method is used. 

Again, this is a shorthand function for the more general `$.ajax()`. 

With `$.post()` it is common to use the optional second parameter to specify the data that is being POSTed to the server, as well as any other properties for the server.

We turn next to $.ajax, which is the most general function. 

<br>

**`$.ajax()` and `$(someSelector).ajax()`**

The `$.ajax()` function has a simple signature:

	$.ajax(url_where_the_data_is [ , optional object with settings] )

The secret is that all the information which the server needs is in the optional object. As noted at the start of this chapter, there are many keys which may appear in this object, but here are the most important ones:

- **method**: Possible values are "GET", "POST" and "PUT" ---- it defaults to "GET" if omitted.

- **done** (replaces **success** ): Function to call after a successful completion of retrieving the data. Typically this handles whatever it is you want to do with the data

- **failed** ( replaces **error**): Function to call when the data retrieval is not successful

- **always** (replaces **complete**): an optional function to be executed after done or failed has finished executing.

- **data**: The data (object, string or array) you are sending to the server. Details of how the data is sent are discussed below.

- **username** and **password** are the obvious things if the server requires your script to log on


A few other options which you may see mentioned, but will not need at this point, are: 

- **dataType**: As discussed in the section on $.get(), if you do not specify the dataType, jQuery will make an informed guess about the type of data in the response. This guess is normally just what you want, and you would set the dataType only if you want to override the kind of processing which jQuery would normally do for your response code. The types you may specify are 'json', 'html', 'xml' and 'script'. 

- **accepts**: This may be used to limit the type of the response you are willing to accept. Normally you know what type of code the response will be and you don't need to set this.

- **context**: This allows you to set the value of **this** to be used in your callback. 

Let's start by noticing that the _done_, _failed_, and _always_ methods, which we had previously written as properties of `$.getJSON()` have now been moved into the object which is $.ajax() 's second parameter.

The _method_, _username_, and _password_ keys are self-explanatory, and we have previously discussed the _dataType_ one. So, let us look at the possible kinds of values for data and how they are handled. The value for data may be a string, an object, or an array.  

In the simplest case, the value for data is a string. Then jQuery will convert it to a query string and append it to the url for the GET request. Please notice that when the data is a string, the default method is a GET. 

Next suppose that the value of data is an object. As https://api.jquery.com/jQuery.ajax/ explains: "The data option can contain either a query string of the form `key1=value1&key2=value2`, or an object of the form `{key1: 'value1', key2: 'value2'}`. If the latter form is used, the data is converted into a query string using `jQuery.param()` before it is sent. This processing can be circumvented by setting `processData` to `false`. "

Here it is useful to note the jQuery also has a function **serialize()** which will convert to a query string all the "successful" controls in a form (i.e. the input elements and text areas, but not buttons and for types where there is a choice only the chosen elements - only the checked radio buttons and checkboxes and selected elements in a select list.) The syntax for using serialize on a form with id myForm is

	var myFormSerialized = $('#myForm').serialize()

Now you can pass data:myFormSerialized in your `$.ajax()` call. (That is, in the object where you set the various key-value pairs for the $.ajax() call, the value for data is myFormSerialized.) 

Another method is to use FormData, which is straight JavaScript. In that case you would write: 

	var myF = $('#myForm'); 
	var myFD = new FormData(myF);

Now use you pass data:myFD in your $.ajax() call. 

> [!WARNING]
> While you won't have any problems with the new FormData() constructor (as in the code above here), some older and mobile browsers do not support all the methods of FormData objects. A current table describing this is at https://developer.mozilla.org/en-US/docs/Web/API/FormData (You can find more about FormData at https://www.javascripttutorial.net/web-apis/javascript-formdata/)

Finally, suppose we the data we wish to send to the server is an array. In this situation jQuery will serialize the array, but the result may require careful processing server-side. For example,  arr = [10, 20, 30] will get serialized as : arr=10&arr=20&arr=30 or arr[]=10&arr[]=20&arr[]=30.  You may find it easier to loop through your array and construct the query string prior to calling ajax. 

The `$.ajax()` function may also be used to upload whole files, but that is beyond the scope of this book.

A description of all the keys whose value may be specified in $.ajax() is found at https://api.jquery.com/jQuery.ajax/ or, in slightly less detail but with examples, at https://www.geeksforgeeks.org/jquery-ajax-method/?ref=rp 

<br>

**`$.getScript()`** 

This method is used to retrieve a script and then allow the (optional) success function to run. The syntax is 
	
 	$.get('url_where_script_file_is' [,success_function]);

The success_function will expect parameters that hold data and the jqxhr object. Further discussion of this is beyond the scope of this book.

## Owning it 

You want to create a multiple choice quiz on each of two possible topics. The user will select the topic and then (with a click) start the quiz.

Your web page should have a button to start the quiz and a piece of code with a place for the question and check boxes for the 4 possible answers. It will also need to hold an input element (not shown) for the correct answer. Of course, this will not be seen until the user starts a quiz.

Starting the quiz will also cause the loading of the questions and answers for the topic chosen, and the storing of that information in an array. Then, one, by one, the questions will be displayed and after the user has selected an answer (and indicated that the user is ready to check the answer) a function will provide the appropriate feedback to the user. At the end of it would be nice to tell the user the number of questions answered and the number correct.
