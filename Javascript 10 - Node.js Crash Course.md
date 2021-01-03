---
title: Javascript 10 - Node.js Crash Course
created: '2020-03-24T16:02:39.551Z'
modified: '2020-03-24T16:02:48.133Z'
---

# Javascript 10 - Node.js Crash Course

## An Overview of Node.js

* Javascript can be used for backends with node.js
    * Like a container that can execute javascript code.
* To check your node version: `node -v`
* To enter node: `node` in terminal.
* To exit node: Ctrl + C and then Ctrl + C again.
* To run a node file: `node index.js`
* `sudo npm install nodemon -g`
    * This installs nodemon. It will continually run a js file which saves us having to type in the `node index.js` every time.

### To read a file

* `const fs = require('fs');` - This is one of the core node.js files.
* `const json = fs.readFileSync(`${__dirname}/data.data.json`, `utf-8`)`
    * `__dirname` is the absolute file directory used in node.
    * `utf-8` is needed to specify the character format.
* To parse this data:
    * `const laptopData = JSON.parse(json)`;
    * This converts a string to javascript keys and objects which can be used.

## The Server

* `const http = require('http');`
* This callback function gets access to the request and response objects of an http request.

```javascript

const server = http.createServer((req, res) => {
    console.log("Someone has accessed the server!")
    res.writeHead(200, { 'Content-type': 'text/html'});
    res.end('This is the response');
});
server.listen(1337, '127.0.0.1', () => {
    console.log('Listening for requests now.');
})
```

* This section of code listens out on the server for requests.
* When the request is made, the server will respond in different ways to different urls.

### To direct URLs

```javascript

const url = require('url');

const server = http.createServer((req, res) => {

    const pathName = url.parse(req.url, true).pathName;
    const query = url.parse(req.url, true).query;

    if(pathName === '/products' || pathName === '/') {
        res.writeHead(200, { 'Content-type': 'text/html'});
        res.end('This is the PRODUCTS page');
    }

    else if (pathName === '/laptop') {
        res.writeHead(200, { 'Content-type': 'text/html'});
        res.end('This is the LAPTOPS page');
    }

    else {
        res.writeHead(404, { 'Content-type': 'text/html'});
        res.end('URL was not found on the server!');
    }
});

```

* When someone makes a request we get a lot of data.
    * One of these parts is `url`.
* Always have the `/` in the pathname.
* The `writeHead` section returns a url code (200 normal, 404 not found)
* The `.query` function will return an object.
    * `?id=4&name=Jon` -> `{ id: '4', name: 'Jon'}`

## Templating

* In an HTML file, set the template names as `{%TEMPLATE%}`
    * This prevents other text having it in the HTML file (or at least makes it unlikely).
* In a laptop route:

```javascript

if (pathName === '/laptop' && id < laptopData.length {
    res.writeHead(200, { 'Content-type': 'text/html'});

    fs.readFile(`${__dirname}/templates/template-laptop.html`, 'utf-8', (err, data) => {
        const laptop = laptopData[id];
        let output = data.replace(/{%PRODUCTNAME%}/g, laptop.productName);
        output = output.replace(/{%IMAGE%}/g, laptop.image;
        output = output.replace(/{%PRICE%}/g, laptop.price;
        res.end.(output);
    });
}

```

* In Node, the concept of files and folders doesn't really exist. It's all request and returns.
* So we have to return images based on the request.
* A general route for images:

```javascript

if (/\.(jpg|jpeg|png|gif)$/i/).test(pathName)) {
    fs.readFile(`${__dirname}/data/img${pathName}`, (err, data) => {
            res.writeHead(200, { 'Content-type': 'image/jpg'});
            res.end(data);
    })
}

```
* This tests if the pathname has an image ending in its pathname.