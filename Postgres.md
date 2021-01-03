---
title: JDSD 10 - Code Analysis/Postgres
created: '2020-05-01T17:37:21.357Z'
modified: '2020-05-06T16:52:55.219Z'
---

# JDSD 10 - Code Analysis/Postgres

* Coming on board to an existing project.

## Setting Up Your Environment

* Fork rather than clone.
* Look over the package.json file.
* `npm install`

## Setting Up PostgreSQL

* PSequel for Mac - Good GUI Postgres Database
* `brew update` and `brew doctor` to make sure everything is updated and working.
* `brew install postgresql`
* To start postgres: `brew services start postgresql`
* To create a database we can then use `createdb 'test'`
* You can then log in through Psequal - with localhost/test port: 5432
  * Or you could log in to postgres with `psql 'test'`

## How to Analyse Code

* Pref: Going through the API first to analyse the endpoints.
  * Look at the folder structure and the readme.
  * Look at the entry point and see what kind of packages are being used.
  * This should then give a basic layout of the backend.
* When you analyse the code, don't just dive into criticism, look at it neutrally.
  * Your job is to integrate with the project, not make it super efficient straight away.
  * Make a note of rooms for improvement to discuss later on.
  * Try and understand high level concepts. 
    * Always start with the big picture and narrow down your focus.
  * Use console.log to see what's happening at different points.
