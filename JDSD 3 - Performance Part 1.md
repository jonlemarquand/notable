---
title: JDSD 3 - Performance Part 1
created: '2020-02-20T19:20:56.768Z'
modified: '2020-02-21T07:56:13.288Z'
---

# JDSD 3 - Performance Part 1

* If a page loads slow, people leave the site.

## 3 Keys to Performance

* Improve what happens on the client-side
* Improve the transfer of the files
  * Network latency: the time it takes to do that transfer.
  * Average website makes 100 requests.
* Improve backend processing
  * Needs time to process and build the site it sends over.

## Network Performance

* Shrinking the files
  * Minimize the text files (HTML/CSS/Javascript)
    * UglifyJS - Part of the build process.
    * Using something like webpack to package files and make them smaller.
  * Optimising images.
* 'The Travelling Deliveryman'
* To view a webpage, we have to download all the related files.
* We also have to carry the files across the internet. Less work = quicker load.

### Image File Formats

* The primary way to change an image size is to change the file format.
* Pick the file format that is best for the job.
  * JPG, PNG, GIF and SVG
* JPG
  * Photos, images and things with many colours
  * Don't allow for transparency.
  * Complex images, lots of colours.
  * Generally a bit big in file size.
* GIF
  * Normally limit the number of colours you can use
  * Reducing the colour count saves a lot of size
* PNG
  * Normally limit the number of colours you can use.
  * Logos, etc.
* SVG
  * Vector Graphics
  * You can expand the svg to several times its original size and minimal increase
  * Really small for what they do, good for 4k displays.
  * Usually quite simplictic
* Pick the right format of images and then compress them as much as you can without losing the quality.

### Image Optimisations

* If you want transparency: use a PNG
* If you want animations: use a GIF
* If you want colourful images: use a JPEG
* If you want simple icons, logos, and illustrations, use SVGs
* Reduce PNG with TinyPNG
* Reduce JPG with JPEG-optimizer
* Try to choose simple illustrations over highly detailed photographs.
* Always lower JPEG image quality (30-60%)
* Resize image based on size it will be displayed
  * Match the resolution to the size displayed on a website.
  * Don't just host a 1500px width image in a 500px box.
* Display different sized images for different backgrounds.
  * Everybody has different screen sizes.
  * Use media queries to send the right size image with CSS.
* Use CDNs like imigx
  * Content delivery networks
  * An external site that has faster access and optimisation
* Remove image metadata
  * see verexif.com/en/index.php
  * photos often save data like location, device info etc.

### Delivery Optimisation

* Do you need to use the librarires like bootstrap/javascript libraries?
* Less files and less trips.
* Less GET requests will speed up the page loading process.

## Critical Render Path

* Here's what happens:
  * We request the website and the browser returns the HTML file.
  * The browser starts reading the HTML and creates the DOM.
    * Document Object Model
    * It incremently generates this tree model of the tags that are needed for the website.
  * As its doing that it encoutners the css file for the style, so it goes to retrieve that.
    * It also then builds the CSS obj file and is building this tree.
  * If it hits upon a javascript file, it will modify the html/dom.
  * It then constructs the rendering tree to know exactly what is on the page. It uses this to layout the page, forgetting about the other files.

### How do we optimise the HTML file?

* We need to get the CSS files as soon as possible and the javascript files as late as possible.
* By placing the javascript files at the bottom, it gives the styles and media a chance to start loading.
* There are some exceptions, such as google analytics.

### CSS

* Only load whatever is needed
* Above the fold loading

```javascript

const loadStyleSheet = src => {
  if(document.createStylesheet) {
    document.createStylesheet(src)
  } else {
    const stylesheet = document.createElement('link');
    stylesheet.href = src;
    stylesheet.type = 'text/css';
    stylesheet.rel = 'stylesheet'
    document.getElementsByTagName('head')[0].appendChild(stylesheet)
  }
}

window.onload = function(){
  loadStyleSheet('./style3.css')
}

``` 

* Media Atrributes
  * In the css link tag you can use media attributes to load the file:
  * `<link rel="stylesheet" href="./style2.css" media="only screen and (min-width:500px)">`
* Less specificity
  * The more information you send, the more bytes it is.
  * When it arrives in the browser, it has to do more work if the specificity is really complicated.

### Javascript

* Once a script is loaded, it can't be executed before all the css is compiled.
  * Only once all this is done will a page load.
* Load Scripts asynchron
  * When a html file is parsing the script tag,
  * `<script async>` - While I'm working on this HTML parsing, can you download the javascript file. It's a lower priority but will still be done.
    * It could be loaded at any time.
    * Add them to anything that doesn't effect the dom.
      * Analytics or tracking scripts
  * `<script defer>` - It won't block loading of the page but won't execute until the HTML has been parsed.
* Minimise DOM manipulation
* Avoid long running Javascript
  * If you have Javascript code that is running for a long time, it will block the page from working properly.

  ### Render Tree/Layout/Paint

  * If javascript events change any parts of the page, it forces us to change the render tree, layout and paint.
