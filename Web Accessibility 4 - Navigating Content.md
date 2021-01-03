---
title: Web Accessibility 4 - Navigating Content
created: '2020-09-09T10:58:58.648Z'
modified: '2020-09-12T11:00:26.530Z'
---

# Web Accessibility 4 - Navigating Content

* The level heading tags `<h1>` etc, they have meaning when selecting the headings in a voiceover command.
  * They then use ctrl+option+rightarrow after selecting the heading focus
  * Move heading on the page by ctrl+option+cmd+h

## Using Headings

* DOM Order matters
  * The heading list follows the dom order not the page.

```javascript

for (var i = 0, headings = $$('h1,h2,h3,h4,h5,h6');
     i < headings.length; i++) {
   console.log(headings[i].textContent.trim() + " " +  
               headings[i].tagName,
               headings[i]);
}

```

* This snippet lists the H tags in a page.
* H2 section, h3 subsection.
* An option could be to have headings offscreen to add to content off the page, but don't go overboard with this technique.

## Other Navigational Options

* Other features - Links, form controls and landmarks
* Don't use span or a class without href.
  * Always use anchor tag with href.
    * It will show up in the links list
    * Work with a keyboard.
* Use a button rather than an a tag without href.
* A link around an image:

```html

<a href="/">
  <img alt="Udacity" src="logo_wordmark.svg">
</a>

```
* Link text should give enough information on whether they want to click it.
* Avoid 'learn more' kind of links, because they aren't descriptive.

## Landmarks

* Main - Typically there should be only one main element.
* Header - page banner or else a grouping elemenet for introductory content.
* Footer - page footer or a grouping element for a section of a page.
* Nav - This represents a navbar for a page.
* Article - self-contained sections of content.
  * Would its content make sense in another context?
* Section - generic section of another document.
  * Typically includes a heading.
* Aside - Tangentially related to an article around it.
