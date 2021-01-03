---
title: NEM Stack 3 - Introduction to Back-End Web Development
created: '2020-04-01T17:58:32.249Z'
modified: '2020-04-01T18:28:38.586Z'
---

# NEM Stack 3 - Introduction to Back-End Web Development

## An Overview of How the Web Works

### What Happens When we Access a Webpage

* Client (e.g. Browser) -- > Request -- > Server
* Server --> Response --> Client
* https://www.google.com/maps
  * Protocol: https - 
  * Domain name: google.com
  * Resource: /maps
    * This is not the real address of the server but easy to memorise.

#### 1 - Request goes to DNS Server

* DNS: special servers which are like the phonebooks of the internet.
  * The DNS converts the domain address to the real address.
  * It returns: protocol://ipaddress:port number
    * The port number is just to identify a specific service running on a server.
    * Like a subaddress.

#### 2 - TCP/IP socket connection

* A connection is now established between the browser and the server. 
  * This connection is typically kept alive for how long it takes all the information to br transmiteed.
* TCP/IP splits the request into packets which are then sent and reassembled on the other side.
  * Going through traffic with motorbikes rather than a coach.

#### 3 - Make HTTP request

* Another communication protocol (allowing 2 or more parties to communicate)
* HTTP Requests:
  * GET - get data
  * POST - send data
  * PUT/PATCH - Modify data
* This is where the server would be told that you want to access the 'maps' resource.
* Start line: HTTP method + request target + HTTP version
* HTTP request headers: such as host, user-agent, language
* Request Body: only when sending data to server, e.g. POST

#### 4 - Make HTTP response

* Start line: HTTP version + status code + status message
* Headers: Information about the response itself
* Body: Response body (most responses)
  * Used response.end in previous section.

#### 5 - Index.html is loaded

* This is then scanned for js, css, images.
* The process is then repeated for each file.

#### 6 - Website is then rendered in the browser.

## Front End vs Back End Web Development

* Front end development is about everything that happens in the browser.
  * HTML/CSS/Javascript
  * Also React/Redux etc.
* Back end is everything that happens on the server.
  * A server is really just a computer that is connected to the internet.
  * Stores files and is running http server software.
    * This is more of a static server.
  * Dynamic web applications have an app running, an http server and the files.
    * This is also linked to a database.
    * Logins, payments etc are all done on the back end.

## Static vs Dynamic vs API

### Static Websites vs Dynamic Websites

* Static: Sites and files are stored ready to go on the server. The websites are literally just served files over the browser.
* Dynamic: Websites are usually built on the server before sending the file.
  * Database -> Get Data -> Build website from template -> HTML/CSS/Javacript -> Browser
  * This is known as server-side rendering.
* Web application = Dynamic website + Functionality.
* APIs
  * Database -> Get Data -> JSON Data -> Browser
  * The API is consumed on the client side.
    * The website is then plugged into a template on the client side.
    * From the server to the client.
    * Client-side rendered.
  * A piece of software that can be used by another piece of software that allows them to talk to each other.
* One API, many consumers
  * Server sends the data so it could be used in browsers or apps.
  * If the site was rendered server-side we are trapped into a platform.
  * Some people just build APIs and then sell them to companies. 

