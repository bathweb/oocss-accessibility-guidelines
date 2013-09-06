# OOCSS Accessibility Guideline

### Accessibility is an important aspect of web development. This guideline outlines the ways in which we are striving to make the OOCSS open source project accessible. All contributions will be checked against these guidelines.

Note: The guidelines are themselves open source and open for contributions, so if there is something you disagree with, there are steps you can take to remedy it: 

1. The best option is to fork the guidelines and submit a pull requests with your desired change. 
1. A less good, but still valuable option is to open an issue to discuss the change you would like. 

We prefer to have discussions around working examples, so if you can, please do the former. Each commit needs examples of good and bad usage to be accepted, but we’ll happily work with you to get your pull request ready to be merged.

## General guideline

### Provide meaningful alternative text for your images

Alternative text provides textual alternative to images to AT (assistive technology) users. 

```
/* Good */
<img src=”img/logo.png” alt=”Stubbornella”>

/* Bad */
<img src=”img/logo.png” alt=”Logo”>
```

### Only provide alternative text for important content

Only provide alternative text for images that are important to the content, and not to images that are for decorations only. If an image doesn’t require alt attribute, give it an empty value. If we leave out the alt attribute, screen readers will read out the URL of the image instead.

```
/* Good */
<img src=”ad.jpg” alt=””>

/* Bad */
<img src=”ad.jpg”>
```

### Use the correct heading hierarchy

Create an appropriate document structure by using the correct heading level, i.e. `<h1>` follows by `<h2>`, then `<h3>` etc. However, do not use heading level as a mean of formatting text, e.g. don’t style a heading as `<h3>` simply because it has the `same font size and style. Instead, use the appropriate heading level but add a class of the heading that matches the design.


```
/* Good */
<h1>Heading 1</h1>
<h2>Heading 2</h2>

/* Good - when your h2 looks like the styling of a h3 */
<h1>Heading 1</h1>
<h2 class=”h3”>Heading 2</h2>

/* Bad- when you try to match the styling of a h2 with a h3 by using the wrong heading level */
<h1>Heading 1</h1>
<h3>Heading 2</h3>
```

In the latest [Screen Reader Survey](http://webaim.org/projects/screenreadersurvey4/) conducted by WebAIM, 47% of surveyed people still find using [heading level](http://webaim.org/projects/screenreadersurvey4/#levels) to navigate around the site useful. 

### Provide meaninful link description

By having descriptive links, when screenreader users set their screen reader to only read out links, they’ll be able to understand what they’re for when the links get read out. Avoid having links with generic descriptions like “click me” or “read more”. However if you must use generic descriptions, add a `title` attribute with a better description, or hide the description off screen using helper classes.

```
/* Good  - hide description using helper classes*/
<a href=”http://www.stubbornella.org”>
Read more <span class=”hideVisually”>about Stubbornella</span>
</a>

/* Good - use title attribute to add description */
<a href=”http://www.stubbornella.org” title=”Read more about Stubbornella”>
Read more
</a>

/* Bad - generic description*/
<a href=”http://www.stubbornella.org”>Read more</a>

/* Bad - generic description */
<a href=”http://www.stubbornella.org”>Click here</a> for more information about Stubbornella.
```

### Ensure your content follows a logical tab order

Having a logical tab order is important to keyboard users, so avoid setting tabindex specifically. 

```
/* Bad */
<a href=”http://demo.com” tabindex=”2”></a>
<a href=”http://www.stubbornella.org” tabindex=”3”>Stubbornella</a>
```

### Provide focus state

Provide focus state on any focusable elements e.g. links, buttons. Anywhere hover state is declared, focus style should be declared as well, and at least have it matching the hover style if there isn’t specific focus style. Also, never set [outline: none](http://outlinenone.com/) in your CSS.

```
/* Good - specific focus style */
a:focus {
  ...styles that will clearly indicate to user the focused element
}

/* Good - focus style matching hover style*/
a:hover,
a:focus {
  ...styles that will clearly indicate to user the hovered or focused element
}

/* Bad - removing focus outline */
a:focus {
   outline: none;
}
```

### Ensure your site works without JavaScript

With components that require JavaScript (e.g. tabs), ensure you can still access the content after JavaScript has been turned off. You can achieve this by using the class .no-js to create styling specifically for non-JS users. The .no-js class gets replaced with .js when JavaScript is turned on. We use the script developed by [Paul Irish](http://paulirish.com/2009/avoiding-the-fouc-v3/) to do the detection. It needs to be set in the `<head>`.

```
/* Good */
.tabContent {
    display: none;
}
.no-js .tabContent {
    display: block
}

/* Bad*/
.tabContent {
    display: none;
}
/* No non-JS styles to make the content visible */
```

### All form fields should have an associated label

Form fields must have a label associated to it, otherwise they will not be accessible to screenreaders. The label’s for attribute needs to match the id of the form field. This will also help to increase the click area of the form field. If the label is not visible in the design, use the helper class .hideVisually to hide the label off screen.

```
/* Good */
<label for=”search”>Search</label>
<input type=”text” id=”search” />

/* Good - hide label off screen */
<label for=”search” class=”hideVisually”>Search</label>
<input type=”text” id=”search” />

/* Bad - label’s for attribute doesn’t match the form field’s id */
<label for=”searchField” class=”hideVisually”>Search</label>
<input type=”text” id=”search” />

/* Bad - no label */
<input type=”text” id=”search” />
```

## Template guideline

### Provide skip link

All templates should include a skip link that's hidden off screen but when tab into it via keyboard, it'll become visible to the users. This is important for keyboard users and AT users, and is to allow them to skip over navigations and less important information in the header. The skip link should have a href value that matches the id of the main container.

In cases where you want to hide the skip link off screen but make it appear on screen when focused, you can add the [helper classes](#css-helpers) `.hideVisually` and `.focusable` to achieve this. 

```
<a href=”#main” class=”hideVisually focusable”>Skip to content</a>

...

<!-- The skip link’s href should match the id of the main content conatiner -->
<div id=”main”> 
```

### ARIA roles
A template is generally made up of layout with the following major blocks:

* `<header>` with `role=banner` : usually contains site-wide specific content such as logo, navigation, global search..
* `<nav>` with `role=navigation`: a list of navigational links that allow users to navigate to other parts of the site/page. Not all links need to use a `<nav>` element. It should only be used on major navigational blocks. 
* `role=main` on container around the main content.
* `role=complementary` on container (the aside element) with supporting information of the document
* `role=contentinfo` on container (the footer element) around metadata of the document, e.g. footnote, copyright statement

```
/* Example: */

<div role="header">
    ...site wide specific content e.g. logo, global search, navigation

    <nav role="navigation">
        <ul>
            <li>Nav item 1<li>
            <li>Nav item 2<li>
        </ul>
    </nav>
</div>


<div role="main">
    ...your main content
</div>

<aside role="complementary">
    ...secondary content
</aside>

<footer role="contentinfo">
    ...footnote, copyright statement
</footer>
```

ARIA roles information from: [Using WAI-ARIA landmark roles](http://www.paciellogroup.com/blog/2010/10/using-wai-aria-landmark-roles/)

## Accessibility guideline references:
[WebAIM](http://webaim.org/intro/)

[HTML5 Doctor](http://html5doctor.com/nav-element/)
