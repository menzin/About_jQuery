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
