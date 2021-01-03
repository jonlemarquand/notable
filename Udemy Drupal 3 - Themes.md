---
title: Udemy Drupal 3 - Themes
created: '2019-12-17T14:08:53.525Z'
modified: '2020-04-29T12:51:44.305Z'
---

# Udemy Drupal 3 - Themes

* When installing a theme remember to check the blocks that are used by the theme and that they line up with what you are currently using.

## Custom Themeing Part 1 - The Info File

* Without the info file, Drupal won't recognise the theme.
* The info file has to be called `foldername.info.yml`
* There are a specific set of lines that need to be in the info file:

```
name: Fluffiness
type: theme
description: 'A cuddly theme that offers extra fluffiness.'
core: 8.x
libraries:
  - fluffiness/global-styling
base theme: classy
regions:
  header: Header
  content: Content
  sidebar_first: 'Sidebar first'
  footer: Footer
```
* Don't use tabs only use spaces.
* Properties and lists MUST be indented by two (2) spaces.
* Libraries: where we define the stylesheets used by the themes.
* Regions: Where we declare the different block regions.

## Custom Themeing Part 2 - File & Folder Structure

* `fluffiness.breakpoints.yml` - In this file we will be defining all our breakpoints for responsive stylesheets.
  * Mobile, tablet etc.
* `fluffiness.libraries.yml` - In this file we'll point to all the stylesheets we have on our website.
* `fluffiness.theme.yml` - Declare any PHP conditional logic and data preprocessing code.
  * Optionally, we could create a config folder with settings and schema files.
* Create a subfolder called CSS with a `style.css` file inside.
* Also make a js folder with a file called `flufiness.js` containing the javascript that makes the theme.
* Images folder - This isn't the images that get uploaded via the form but the images that are used in the themes.
* Templates folder - This is where we override the default page layouts with files in this folder.
  * How to style pages and blocks
* A logo.png and screenshot.png file in the root and these will be used by default on the theme.

fluffiness.libraries.yml:


```
global-styling:
  version: 1.x
  css: 
    theme:
      css/style.css: {}
  js:
    js/fluffiness.js: {}
```
* Any time you make a massive change to the info files etc you need to go to Permissions > Clear cache

## Custom Themeing Part 3 - Overriding Templates

* Copy files from basic drupal theme.

