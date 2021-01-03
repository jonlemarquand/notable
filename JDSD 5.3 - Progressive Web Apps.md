---
title: JDSD 5.3 - Progressive Web Apps
created: '2020-04-15T07:58:16.575Z'
modified: '2020-04-23T17:43:59.179Z'
---

# JDSD 5.3 - Progressive Web Apps

* A web app is a website which allows users to interact in many ways.
  * However, it is inside a browser window.
  * A native app is an app on a phone.
  * Progressive web apps are aiming to have a similar experience to apps on a phone.
* Apps have everything on the phone.
  * There isn't a server/client relationship in the same way.
  * They can send push notifications, work offline, or get a blank screen.
* You can now make web apps behave like native apps.
* Checklist: https://developers.google.com/web/progressive-web-apps/checklist

## HTTPS

* HTTPS prevents bad actors from tampering with communications.
  * If we're sending a username/password to the server, we can encrypt it.
  * A requirement for progressive web apps, but also should just be standard practice.

## App Manifest

* Making the site as much like an app as possible.
* `manifest.json`
* Favicon generator: realfavicongenerator.net

## Service Worker

* A script that your browser runs in the background, separate to the web app.
  * Another work along with the main thread.
  * A programmable proxy.
  * Allows us to work on what happens on a request by request basis.
  * Also helps with background syncs and push notifications.
* Change serviceWorker.unregister(); to .register();
* The service worker intercepts any request made to the network, to see if they already have the files in the cache API.
  * The cache API is something that the browsers give you. A box where static files are stored.
  * The worker checks the box first - regardless of network activity.
  * If not, we have to talk to the network.
* The Cache Storage API.
  * We cache the shell of the website via the service worker.
  * So on repeat visits, the app shell can be loaded instantly.

## Deploying our React App

* `npm install gh-pages`.
  * Package.json
    * In scripts: use `"predeploy: "npm run build"` + `"deploy": "gh-pages -d build",`
    * Also add, `"homepage": "https://jonlemarquand.github.io/name-of-repo"`
