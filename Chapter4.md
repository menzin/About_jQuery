# Chapter 4: Loops
**Sometimes you absolutely must write a loop- or to each his own **

## $.each Versus $(someSelector).each()

## $.each

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
