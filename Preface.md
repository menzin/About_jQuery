# Preface to About jQuery
© Margaret S. Menzin 2021

## Why jQuery? 
Once upon a time, a long time ago, there was something called “the browser wars”, whereby many features of HTML and JavaScript were not uniformly implemented across all popular browsers. At that time, jQuery was designed to provide a uniform interface to all browsers.

Those bad days have passed, but jQuery proved to be so helpful and useful, that it remains the most popular library of functions for JavaScript on the web, with over 50% of all web sites using jQuery. In addition there is now a huge eco-system of plug-ins which add additional functionality to jQuery. So, if you believe in the wisdom of crowds, or in not re-inventing the wheel, or if  you simply want to be able to maintain existing web sites, it behooves you to learn jQuery. Plus, it’s a lot of fun and will make your life easier.

The most common uses of jQuery are to add event handlers to multiple elements, to implement AJAX, to animate a page with visual effects, and to manipulate the DOM by changing either what elements appear on a page or how they appear.  

That last category includes such things as reacting to form validation either by showing and hiding error messages or by changing the color or font weight of the label.  It also includes having sections of a page (e.g. on a page of FAQs) open up or close. That is, we can manipulate the DOM either by adding/removing elements or by changing their attributes.

AJAX is used for quizzes and, more generally, for adding new material to a page without re-loading the whole page.  

Visual effects implemented through jQuery run the gamut from coloring the rows of a table to displaying carousels of images to implementing sliders and to user re-sizing an element with drag-and-drop.  

Event handlers, such as reacting to button clicks, are intrinsic to making a page interactive. jQuery makes it easy for the page to load (so the user can see the whole page) while the event handlers are being added silently; thus speeding up the user’s experience.

So, jQuery will make it easier for us to add a lot of useful and amusing features to our pages.

## Why this book?
For over 10 years I have been teaching jQuery to my students at Simmons University. In that time we have tried to use a variety of references, some of which were too basic and some too elaborate or too focused on highly refined visual design.

This book is an attempt to fill the middle ground. It is intended for people who know the basics of HTML, CSS and JavaScript, but haven’t yet tried to use jQuery or other framework, and may (or may not yet) be completely clear on the nuances of callbacks, closures or events.

A detailed set of references is found at the end of this book, and more references on web centric computing may be found at http://web.simmons.edu/~menzin/WebCentricResources.html

## What you should know
In order to use this book you should be comfortable with:
- **HTML**
    You should be able to write basic HTML, including tags for images, anchors , lists, forms, buttons, divs and spans. You should also have used event handlers, such as onclick. How the DOM is represented will be reviewed in Chapter 1, but it will be helpful if you have worked with this material before. It is an advantage but not required that you understand basic issues around responsive web design  and accessibility.

- **CSS**
  You should be able to use basic selectors for a class, id, tag. We will not be using ultra fancy CSS, but the selectors for CSS are used through out jQuery. Using your favorite cheat sheet you should be able to set the color and font properties of the text. We will review the difference between the display and visibility properties in Chapter 1.

- **Javascript**
  You should be completely comfortable with core JavaScript. That is, you should know how to write and use loops, functions, arrays, objects (realized as associative arrays). It will be very helpful if you have written anonymous functions, although there will be a brief discussions of that in Chapter 1. You may or may not have used callbacks and you may or may not be using ES6 (or later – sometimes called ESnext). I had to decide whether or not to use ESnext in this book. My decision to forgo ESnext is based on the importance of this in jQuery, and avoiding the temptation to use arrow functions, which, of course, handle this differently from traditional functions.

## How this book is organized
- **Chapter 0 – Just Get Me Going**
  This chapter shows you how to use jQuery in very simple examples, without a lot of background about what is happening behind the scenes
  
- **Chapter 1 – What Is Happening Here** 
    Please read this short chapter, even if you have used jQuery.
This chapter explains the basic mechanisms of jQuery and provides examples of code which is used frequently.. We discuss collections, chaining, and backing up the chain.

- **Chapters 2 –  4** build your knowledge of the syntax and methods in jQuery.
  
- **Chapter 5- 7** are about the functionality which make jQuery shine.  They discuss respectively manipulating what is on your webpage, event handling and AJAX.
  
- Chapter 8 provides a guide to learning how to maintain ARIA compliance when you are manipulating the page or using AJAX.
  
- **Chapters 9  - 10** respectively point you to next steps with jQuery ( e. g. for plug-ins and mobile devices) and references on jQuery.
  
- **The Table of Contents** is quite detailed about each chapter's topics.
  
- **Code **
  All code examples are at in the same folder as the chapter's text. Please use the downloaded code rather than copying from the Word documents --- we all know that that will lead to all kinds of problems.


## Acknowledgements


