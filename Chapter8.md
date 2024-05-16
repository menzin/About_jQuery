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


## Testing

Google has a page on reviewing a site for accessibility at https://developers.google.com/web/fundamentals/accessibility/how-to-review. 

Google and Microsoft use the axe tool https://www.deque.com/axe/. This site has a suite of tools to help with development and testing.

Another such tool for this is pa11y https://github.com/pa11y/pa11y, which is also described at https://medium.com/angular-in-depth/test-for-accessibility-and-help-millions-of-people-97d86f72e2c4. <br> A related tool (focusing on sites which use Angular) is a11y – see https://medium.com/angular-in-depth/test-for-accessibility-and-help-millions-of-people-97d86f72e2c4, although this article also has a lot of good general information.  

Utah State University has published wave https://wave.webaim.org/

You will also find tools about testing for color blindness at http://web.simmons.edu/~menzin/WebCentricResources.html#tools_for_color_blind_viewing

## Specific topics in web page accessibility

https://websitesetup.org/web-accessibility-checklist/ has an accessibility checklist. Here Bruce Lawson, a great web site guru, has a useful article on how to improve your site's accessibility, and demos these practices on this page. 

https://www.ovl.design/text/inclusive-inputs/ shows how to make your form inputs accessible. Another similar article is at https://bitsofco.de/labelling-form-elements/, by Ire Aderinokun, who writes quite a bit about these issues. A similar article, but which includes more html elements, is at https://css-tricks.com/a-complete-guide-to-links-and-buttons/

https://www.w3.org/TR/wai-aria-practices/examples/landmarks/HTML5.html is about landmarks and sectioning. https://www.w3.org/TR/wai-aria-practices-1.1/examples/accordion/accordion.html has a useful table of roles and attributes for aria compliance.  

https://www.deque.com/blog/text-links-practices-screen-readers/ shows you how to make your links accessible.

https://github.com/scottaohara/accessible_components has a number of accessible links and other widgets.

Peter-Paul Koch, who is always worth reading, has written about css and accessibility at https://www.quirksmode.org/blog/archives/2019/04/css_and_accessi.html. Jared Spool, who is also always worth reading, has written about css grid and accessibility at https://medium.com/adventures-in-ux-design/css-grid-accessibility-dd9cf7790dff <br> The article continues on to Mozilla's page on the same issues https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_Layout_and_Accessibility

https://www.freecodecamp.org/news/how-to-make-an-accessible-form-its-easier-than-you-think-672d3f4ff573/ is about designing accessible forms. 

## Resources specific to pages which change dynamically:

https://bitsofco.de/using-aria-live/ shows you how to make pages which change and are aria-compliant. 

Some accessible accordions are at https://github.com/nico3333fr/jquery-accessible-accordion-aria and https://github.com/DavideTriso/aria-accordion

https://scottvinkle.me/blogs/blog/how-html-microdata-helps-with-accessibility is about using microdata to improve accessibility.

https://www.accessibility-developer-guide.com/examples/sensible-aria-usage/hidden/ is about hiding elements and accessibility

https://a11y.nicolas-hoffmann.net/ has plug-ins for accessible jQuery – tool tips, accordions, hide and show, etc.

http://juicystudio.com/article/making-ajax-work-with-screen-readers.php is an older article about making ajax work for accessibility.

http://accessibility.athena-ict.com/aria/Javascript-jquery-accessibility.shtml is focused on keyboard accessibility and jQuery.

## Newsletters which follow this field:

A list apart has many articles on this topic: https://alistapart.com/article/getting-to-the-heart-of-digital-accessibility/

Briana Blaser at the U. of Washington's Do It project maintains an email list for interested people blaser@uw.edu

As mentioned above, there is an excellent newsletter from U. Minnesota- Duluth. You can subscribe by emailing lcarlson@d.umn.edu


