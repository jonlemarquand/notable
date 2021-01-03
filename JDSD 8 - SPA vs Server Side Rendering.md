---
title: JDSD 8 - SPA vs Server Side Rendering
created: '2020-04-27T17:00:48.273Z'
modified: '2020-04-28T09:31:46.675Z'
---

# JDSD 8 - SPA vs Server Side Rendering

* Single page applications vs server side rendered applications
* The Old Way:
  * Every time you switch links, a new request to the server is made.
* SPA:
  * All done within Javascript programatically.
  * Parts of the page refresh.
  * Client-side rendered.
  * 2 Major Issues:
    * More javascript sent to the client initially.
    * SEO performance. Harder to do good SEO on SPA.

## CSR vs SSR

* CSR: A barebones HTML file that links to javascript.
  * Blank page whilst HTML file loads.
  * We then process all the JS.
  * Once that is done, the page is then loaded.
* SSR: The initial request renders a lot faster.
  * Our server responds with a fully rendered page.
  * The page doesn't become interactive until all the JS has run.

## CSR vs SSR Part 2

* CSR: Very rich interactions.
  * We load part of the page and works well for web applications.
  * The server responds a lot faster.
  * Faster website experience after the initial load.
  * Low SEO Potential
    * The initial load might make the google robot only see a div. Or see a full render.
  * Longer initial load
    * The loading screen or blank page will be displayed for longer.
* SSR
  * Pro: Static sites
    * Static sites/ sites that are mainly text work well
  * Pro: SEO
    * The google bot will already see eveyrthing rendered. Because the content is already provided before you get it.
  * Pro: Initial page load
  * Cons: Full page reload
    * We have to request a new page from the server.
  * Cons: Slower page rendering.
  * Cons: Request to server

  ## Server Side Rendering React

  * We render the app on the server, and then send it over to the browser which 'hyrdrates' the page with the javascript and event listeners etc.
  * SSR React Libraries:
    * Gatsby.js
      * Static site generator for React
    * Next.js
      * Dynamic tool to build react applications.
      * Kind of like create-react-app, it builds a lot of things out of the box.
