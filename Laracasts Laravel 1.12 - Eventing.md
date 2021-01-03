---
title: Laracasts Laravel 1.12 - Eventing
created: '2020-08-21T05:57:32.635Z'
modified: '2020-08-21T06:09:06.465Z'
---

# Laracasts Laravel 1.12 - Eventing

* Key things: For example, process the payment, generate a liscense code.
  * There are then a series of side effects that need to occur: Notify the user, award achievement, schedule a sharable coupon
* The most obvious solution: keep it all inline in a controller action.
  * Very easy to come back to however potentially hard to add to.
* Could create a service class or a use case class.
* Another option: Events and listeners.

## Events

* An event represents an action that just took place in your system.
* The listeners are the side effects that listen for the event.
  * Think of it as an if/then relationship
