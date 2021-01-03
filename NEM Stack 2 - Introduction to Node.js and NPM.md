---
title: NEM Stack 2 - Introduction to Node.js and NPM
created: "2020-03-30T09:51:06.173Z"
modified: "2020-03-30T10:26:13.089Z"
---

# NEM Stack 2 - Introduction to Node.js and NPM

## What is Node.js and Why Use it?

- Single-threaded, based on event driven, non-blocking I/O model
- Perfect for building fast and scalable data-intensive apps.
- Not great for applications with heavy server-side processing.
- Javascript across the entire stack: faster and more efficient development.

## Running Javascript Outside the Browser

- To exit node in terminal type `.exit`.
  - Can also use `Ctrl + D`
- If you press TAB you can see all the global variables that are available in Node.
- Quick use: `_ + 6` would be the previous result plus 6.
- Use `String.` will show all the string properties that are available to string at the time.

## Using Modules 1: Core Modules

- To run a file, write `node index.js`.
- To enable files we can use: `const fs = require('fs')`.
  - This will return an object that can be used later, in this case to open files.

## Reading and Writing Files

- `const textIN = fs.readFileSync(link to source, character encoding)`
- To write a file:
  - `fs.writeFileSync('./txt/output.txt', textOut);`
  - Location of the file to be created, and what the file should contain.

## Blocking and Non-Blocking: Asynchronous Nature of Node.js

- Synchronous code is blocking code. It can only do one thing at once.
  - Asynchronous code can run things in the background and then be called into the main thread when needed.
- In a simplified way, all the users that are using your node.js server are using the same thread.
  - Thereforer all users would have to wait for something to finish.

### Reading and Writing Files Asynchronously

```javascript
fs.readFile("./txt/start.txt", "utf-8", (err, data) => {
  fs.readFile(`./txt/${data1}.txt`, "utf-8", (err, data2) => {
    console.log(data2);
    fs.readFile("./txt/append.txt", "utf-8", (err, data3) => {
      console.log(data3);
      fs.writeFile("./txt/final.txt", `${data2}\n${data3}`, "utf-8", err => {
        console.log("Your file has been written");
      });
    });
  });
});
console.log("Will read file!");
```

## Creating a Simple Web Server

- Include the package: `const http = require('http');`
- We need to create the server and then start the server.
  - `const server = http.createServer((req, res) => {res.end('Hello from the server');});`
  - This function runs when the server is accessed.
- We can then run the listen command:
  - `server.listen(8000, '127.0.0.1', () => {console.log('Listening to requests');})`
  - A port is basically a sub-address on a certain host.
  - Localhost is generally on 127.0.0.1
- When we start a server, node doesn't close once the script has run because it has to listen.
  - To break from the programme we use ctrl+c (Not command+c).

## Routing

- We use another built in node module:
  - `const url = require('url');`

```javascript
const server = http.createServer((req, res) => {
  const pathName = req.url;

  if (pathName === "/overview" || pathName === "/") {
    res.end("This is the overview");
  } else if (pathName === "/product") {
    res.end("This is the product");
  } else {
    res.writeHead(404, {
      "Content-type": "text/html"
    });
    res.end("<h1>Page not found!</h1>");
  }
});
```

- Always have to send headers before content.
- These routes have nothing to do with files and folders.

## Building a Very Simple API

- An API is a service from which we can request some data.

```javascript

} else if (pathName === '/api') {
  fs.readFile(`${__dirname}/dev-data/data.json`, 'utf-8', (err, data) => {
    const productData = JSON.parse(data);
    // console.log(productData);
    res.writeHead (200, { 'Content-type': 'application/json'});
    res.end(data);
  })
}

```

- Usually the `.` is where the script is running and the `__dirname` is where the file is located.
- However, the version above would require the file to be read every time a user requests the page '/api' which isn't efficient.
- Instead, we can use:

```javascript
const data = fs.readFileSync(`${__dirname}/dev-data/data.json`, "utf-8");
const dataObj = JSON.parse(data);

// In the /API link

res.writeHead(200, { "Content-type": "application/json" });
res.end(data);
```

- This ensures the file is only read once (outside of the server requests) and then stored as an object which can be used multiple times.

## HTML Templating:
