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


***The .ajax() method***

There are a number of shortcut methods for simple requests (see next paragraph) , but the basic structures for the general ajax() method are:  

- `$.ajax(url_where_the_data_is [, optional object with settings] )` //executes an Ajax request

- `$(someSelector).ajax(url_where_the_data_is [, optional object with setting] )` //executes an Ajax equest, typically putting the data at someSelector  

