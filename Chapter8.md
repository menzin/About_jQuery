# Chapter 8: Making Page Changes ARIA Compliant

Design of accessible web sites is an important topic, and worthy of a separate treatment outside this book. Fortunately, there are a number of excellent sites, referenced below.

Accessibility and jQuery capabilities have some interaction, especially when you use jQuery to change the appearance of a web page. That is to say, 
- there are accessibility features which you should incorporate even on static pages
- there are jQuery functions which don't impact accessibility (e.g. adding event listeners), 
- there is the overlap --- when you use jQuery to modify a page (e.g. to hide/display error messages during form validation or use AJAX to add some HTML to a page.)

The rest of this page has general information on making your site accessible, followed by information about testing for accessibility, information about specific topics in site accessibility (links, buttons, etc.), then information about accessibility and dynamically changing pages (such as from ajax or from showing and hiding content) and finally some newsletters/blogs.

## General resources on accessibility:

The standard that is used for accessibility is ARIA or the Accessible Rich Internet Applications Suite and the standards for it are at the Web Accessibility Initiative of the w3c: https://www.w3.org/WAI/standards-guidelines/aria/ with more information at https://www.w3.org/standards/webdesign/accessibility

As Mozilla points out, HTML5 incorporates many of these issues, so one should start with good semantic HTML. Mozilla has a series of excellent articles and tutorials on its site: https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA. 

Google has a similar site https://developers.google.com/web/fundamentals/accessibility

The U.S. and U.K. governments also have published standards, checklists and tutorials:

- https://section508.gov/create/web-content (Federal standards from the U.S. government)
- https://section508.gov/create/software-websites (Advice on creating accessible websites)

The Carnegie Museums have a very helpful website on accessibility, with everything from an overview of the types of disabilities to consider to best practices and specific code to incorporate on your site. http://web-accessibility.carnegiemuseums.org/

The Do-It project at the University of Washington https://www.washington.edu/doit/ has a list of resources, of which accessible web pages is only one part. The most relevant part for web design are at https://www.washington.edu/doit/programs/center-universal-design-education/overview , their center for universal design, and https://www.washington.edu/doit/programs/center-universal-design-education/resources/published-books-and-articles-about-universal , their books and articles about universal design.


Ryerson University in Canada has published several free books on this topic: https://pressbooks.library.ryerson.ca/catalog/accessibility

Laura Carlson at the U. Minnesota – Duluth publishes a newsletter which has many current articles about accessibility and she maintains a very complete list of resources on this subject https://www.d.umn.edu/itss/training/online/webdesign/accessibility.html. It is an excellent go-to place for specific topics, with sections for buttons, forms, links, carousels and slide shows, semantic HTML, tables, and, of course, testing and checklists.



