---
title: MERN Stack 3 - Planning the App
created: '2020-05-08T09:56:43.209Z'
modified: '2020-05-08T10:13:12.914Z'
---

# MERN Stack 3 - Planning the App

## General Planning Steps

* Come up with an idea/solve a problem
* Create a design/sketch - Wireframes for the app
* Plan your data models
  * Which data do you need to send from frontend to backend or backend to database.
* Plan your endpoints and 'pages'

## Understanding the General App Idea

* Build an app where users can share places (with images and location) with other users
  * All CRUD methods covered.
  * Multiple data models, image upload and input validation.
  * Authentication and authorisation required.

## Data & API Endpoints used in our App

* Users
  * Name, Email, Password, Image
* Places
  * Title, description, address, location (latitude,longitued, Image
* one place belongs to exactly one user
* One user can create multiple places

### API Endpoints

  * Two main data models, two main endpoint areas.
  * /api/users/..
    * GET api/users/
      * Retrieve list of all users
    * POST api/users/signup
      * Create new user + log user in
    * POST /api/users/login
      * Log user in
  * /api/places/..
    * GET ../user/:uid
      * Retrieve list of all places for a given user id (uid)
    * GET ../:pid
      * Get a specific place by place id (pid)
  * POST /api/places
    * Create a new place
  * PATCH ../:pid
    * Update a place by id (pid)
  * DELETE .../:pid
    * Delete a palce by id (pid)

### Required SPA Pages for the Frontend

* SPA Pages
  * `/` -> List of Users
    * Always reachable (whether logged in or not)
  * '/:uid/places' - List of places for selected user.
    * Always reachable (whether logged in or not)
  * '/authenticate' - Signup + Login Forms
    * Only un-authenticated
  * '/places/new' - New place form
    * Only authenticated
  * '/places/:pid' - Update place form
    * Only authenticated
