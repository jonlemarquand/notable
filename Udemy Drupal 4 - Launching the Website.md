---
title: Udemy Drupal 4 - Launching the Website
created: '2019-12-17T14:41:57.848Z'
modified: '2020-04-29T12:51:52.719Z'
---

# Udemy Drupal 4 - Launching the Website

## Roles &amp; Permissions

* We can set access to anything between viewing the site and all the admin roles.
* Permissions is how we set who can view/do what they do on the site.

## Database Transfer

* Back-Up the Database:
  * phpmyAdmin > drp > export > custom > save output to file > go
* New Server
  * create new database on production server
  * doesn't have to be the same name.
  * create user + password

## File Transfer

* Need to update `/sites/default/settings.php`
* When moving websites, flush the cache prior to making the database backup and then after everything has been moved.

## Updates & Reports

* Managing Module Updates:
  * Check the release notes.
  * Run `update.php`
* Managing Core Updates:
  * delete sites, themes &amp; modules
  * delete everything else from your main folder
  * run `update.php`

## Reports

* Recent log messages can see various information about what's happening with the website.



