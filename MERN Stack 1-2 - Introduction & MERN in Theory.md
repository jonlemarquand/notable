---
title: MERN Stack 1-2 - Introduction & MERN in Theory
created: '2020-05-08T08:31:23.164Z'
modified: '2020-05-08T09:34:21.104Z'
---

# MERN Stack 1-2 - Introduction & MERN in Theory

## What is the "MERN Stack"?

* MongoDB - Database Solution
  * A NoSQL Database which stores "Documents" in "Collections" (instead of "Records" in "Tables" as in SQL)
  * Store application data (Users, products)
  * Enforces no data schema or relations.
  * Easy to connect to Node/Express
* Express - Framework for NodeJS
  * A node framework which simplifies writing server-side code and logic.
  * Based on Node
  * Middleware-based: Funnel Requests through functions.
  * Includes Routing, view-rendering and more.
* React - Browser-side Javascript Library
  * All about user interfaces.
  * Responsible for controlling what users see on the screen.
  * Render UI with Dynamic Data
  * Handle User Input
  * Communicate with backend services
  * Provides a "Mobile-App" like User Experience
* Node.js - Serverside Javascript Runtime
  * Javascript on the Server
  * Listen to requests and send responses
  * Execute server-side logic
  * Interact with databases and files

## MERN - The Big Picture

* Client (Browser)
  * React
    * Based on Javascript, javascript exectued in the browser.
  * Presentation/UI
  * Single-Page-Application
    * Anyone can see the code that is in the browser.
    * Data is not persistent in the browser.
    * Info is only visibile in the browser.
* Server
  * Node/Express/MongoDB
  * Node/Express writes javascript code
    * Business Logic
    * More intensive.
  * Database Server
    * Can run on the same server as node or separate.
    * Not a file storage.
    * Node/Express sends database queries to the database server.
  * Business Logic
  * Persistent Data Storage
  * Authentication Logic
  * Runs on a separate machine.
* To link them we do HTTP requests from React to Node. Exchanging responses in JSON format.
  * AJAX requests and responses.
  * Done without reloading the page.

### Diving into the Frontend

* React SPA - Single Page Application
  * React is in charge of rendering everything in the browser.
  * Only one HTML page is served to the browser.
  * Client-side routing
    * Routes (with react-router-dom)
    * Feeling of having multiple pages but in reality it is just one.
    * Route config + Page components
  * Front-end State
    * State is data that influences what is shown on the screen.
    * Hooks, Redux.
    * Redux Logic, React Hooks, Custom Hooks
  * Components + Styling
    * Utility/UI Components

### Understanding the Backend

* Decoupled Ends - Our backend is built as an API - Application Programming Interface.
* We build a Node/Express application that has only some entry points that can be accessed by the React App.
  * This allows us to keep control over the API and say what actions we want to allow.
* REST API or a GraphQL API
  * Rest - Representational State Transfer
  * Work differently on how requests are received.
    * Both execute code on the server.
  * Rest
    * Different URLs + HTTP verbs (endpoints) for different actions.
    * API is stateless and decoupled from any frontend.
    * The most common type of API because of ease of use.
  * GraphQL API
    * One URL + HTTP Verb that accepts query commands.
    * Typically `POST /graphql`
      * This contains a query request (to define the data that should be returned)
    * Query expression identifies a resource and action.
    * API is stateless and decoupled from any frontend.
    * Popular but less common, because you need to learn the query language.

## Two Ways of Connecting Node + React

* Three Big Blocks: React, Node, Database
* Option 1: Server hosts Node API + React SPA
  * Node (Express) API handles incoming requests.
  * Requests not targeting API routes return React SPA.
  * Data is exchanged between React app and Node API in JSON format.
* Option 2: Two Separated Servers
  * One for React
    * Served from separate static host.
    * Just returns HTML/CSS/Javascript files
  * One for API
    * Handling incoming requests.
* In both cases: Logically separated apps (SPA + API)!
* If you get a lot of requests separating the Node + Database is a good idea.
* Option 3: Server-side rendered Pages (via EJS, Pug etc)
  * Possible but likely less interactive as you have to reload pages.

## Creating our Development Environment & the Development Servers


