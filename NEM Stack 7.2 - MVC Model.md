---
title: NEM Stack 7.2 - MVC Model
created: '2020-04-10T10:37:53.297Z'
modified: '2020-04-10T10:46:28.523Z'
---

# NEM Stack 7.2 - MVC Model

## Intro to Back-End Architecture: MVC, Types of Logic, and More

* Model
  * Business Logic
* View
  * In this case, the templates responsible for the view.
  * Presentation logic
* Controller
  * Interact with requests, send back responses.
  * Application logic

* Request -> Router -> Controller -> Interact with the models -> Controller -> View -> Controller -> Response

### Application vs Business Logic

* Application Logic
  * Code that is only concerned about the application's implementation, not the underlying business problem we're trying to solve.
  * In Express, this is often about managing requests and responses.
  * The bridge between model and view layers.
* Business Logic
  * Code that actually sovles the business problem we set out to solve.
  * Directly related to business rules, how the business works, and business needs.
  * Examples:
    * Creating new tours in the database.
    * Checking if the user's password is correct.
    * Validating user input data.
    * Ensuring only users who bought a tour can review it.
* Fat models / thin controllers
  * offload as much logic as possible into the models, and keep the controllers as simple and lean as possible.


## Refactoring for MVC

* Folders for models, routes and controllers.

