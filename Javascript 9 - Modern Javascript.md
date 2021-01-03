---
title: Javascript 9 - Modern Javascript
created: '2020-03-19T07:03:34.167Z'
modified: '2020-03-19T07:03:41.445Z'
---

# Javascript 9 - Modern Javascript

## A Brief Overview

* Less about the language itself, more about the enviroment we use it in.
* Node.js/NPM
    * This lets us use libraries like Angular, React
    * Development tools that let us use ES5 and other things.
* Babel
    * Convert ESNext back to ES5 so all browsers can understand the code.
* Webpack
    * ES6 Modules lets us write different modules in different files.
    * Webpack lets us bundle these modules together.

## A Brief Introduction to the Command Line

* To create a new file `touch test.js`
* To copy a file `cp test.js ..`
    * This copies a file to the parent folder.
* `mv test.js ..` will move the file to the parent folder.
* `rm test.js` to delete a file
* `rm -r test` to delete recursively a folder and its files
* `open index.html` - will open a file with the default programme.

## Node.js and NPM

* To check the node.js version use `node -v`
* To check if NPM is installed: `npm -v`
* To install packages: `npm install webpack --save-dev`
    * This `--save-dev` means that webpack will be a development package dependency in our project.
* If you use `npm install jquery --save`
    * They are dependencies in the final project.
* To uninstall `npm uninstall jquery --save`
* These packages are at the moment only installed in the project.
    * If you want to install it globally `npm install live-server --global`
        * Or use `-g`

You will also need to change the entry in webpack.config.js (a file we create during the video) from this:

    entry: ['babel-polyfill', './src/js/index.js'],

to this:

    entry: ['./src/js/index.js'],



## Configuring Webpack

* Create a `webpack.config.js` file.
* Entry:
* Output: Where to store our javascript file.
    * Filename: The standard is bundle.js
    * Path: where we want our packages to go.
* Mode: Development mode doesn't do a lot of things to save time, but production mode
* JS Files need an `export default 23`
    * In this case it would export the number 23.
* The entry point then needs a `import x from './test';`
    * This will save what we exported from the previous module to the variable x in index.js

### The Webpack Dev Server

In our `webpack.config.js` file add:

```javascript

    devServer: {
        contentBase: './dist'
    }

```
In our package.json, we should also add the scripts:

"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development --open"
  },

### Webpack Plugins

* This lets us bundle our html files using webpack.
* Run `npm install html-webpack-plugin --save-dev`
* Add `const HTMLWebpackPlugin = require('html-webpack-plugin');` to `webpack.config.js`
* Add:

```javascript

plugins: [
    new HtmlWebpackPlugin({
        filename: 'index.html',
        template: './src/index.html'
    })
]

```
* When we use the dev server, it doesn't save the files on disk.

## Babel

* Loaders allow us to import and process different files.
* The `test` property:
    * It uses regular expressions. In this case we want to test for all the javascript files. `test: /\.js$/,`
    * The regular expression are teh `/ /` at the beginning and end.
* The `exclude` property stops it from reading all the node modules files which don't need to be compiled.

```javascript

module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader'
                }
            }
        ]
    }

```
* We also need a new config file for Babel. It's usaully called `.babelrc`.
* Within it we write:

```
    {
        "presets": [
            ["@babel/env", {
                "useBuiltIns": "usage",
                "corejs": "3",
                "targets": {
                    "browsers": [
                        "last 5 versions",
                        "ie >= 8"
                    ]
                }
            }]
        ]
    }
```
* Within this we write that for the presets, we want the environment to target the last 5 versions of browsers and ie8+ as being able to use ES6.

## Planning Our Project Architecture with MVC

* Model, View, Controller
* Search.js <-> Controller (index.js) <-> searchView.js
    * Search.js - In the search model is where we do api and ajax calls
    * searchView.js - Where we get the search string from the user and the results.
* The model is always concerned about the data and the logic.
* The view gets data from and displays data in the user interface.
* Always one model and one view for each item. All controlled through the controller.

## ES6 Modules

* Two folders in our `./src/js` folder: models and views.
* Model files naming convention is to use uppercase for the model name. `Search.js`

### Imports/Exports

* Default exports are used when we only want to export one thing.
    * `export default 'I am an exported string.'`
    * To import the string `import string from './models/Search'`
        * You don't have to specify .js on the file when importing.
* Named export is to export multiple things from a file.
    * `export const add = (a,b) => a + b; export const multiply = (a,b) => a * b;`
    * To import this: `import { add, multiply } from './views/searchView';`
    * You have to use the exact same name as the exports.
    * You could use `import { add as a }` if you want to rename them.
* To import everything:
    * `import * as searchView from './views/searchView';`
    * You could then call it as `${searchView.add}`


👉 This is how you use forkify-api instead of the food2fork API.

In the Search.js file (as soon as you get there), just replace:

    const res = await axios(`${PROXY}http://food2fork.com/api/search?key=${KEY}&q=${this.query}`);

with this:

    const res = await axios(`https://forkify-api.herokuapp.com/api/search?&q=${this.query}`);


Then, in Recipe.js (as soon as you get there), please replace:

    const res = await axios(`${PROXY}http://food2fork.com/api/get?key=${KEY}&rId=${this.id}`);

with this:

    const res = await axios(`https://forkify-api.herokuapp.com/api/get?rId=${this.id}`);

## Making our First API Calls

* Some browsers don't recognise fetch, so instead we can use a popular HTTP request library, `axios`
    * Install `npm install axios --save`
    * Import the package `import axios from 'axios'`
* Axios also receives the data and converts it from json straight away.

## Building the Search

* When our final app runs, the state of our app is:
    * The current search queries, the current recipes, the current shopping list.
    * All of this data is the 'state'.
    * We want all this data to be in one central variable.
        * Accessible in one central module.

```javascript

/* Global tate of the app
    * - Search Object
    * - Current recipe object
    * - Shopping list object
    * - Liked recipes
*/

const state = {};

```

## Hash Change Event

* There is an event that happens when the `#` changes in the URL.
* `window.addEventListener('hashchange', controlRecipe);`
    * This will only work (so far) when the hash changes rather than when the page is loaded with the hash in the URL.
* Two event types:

```javascript

window.addEventListener('hashchange', controlRecipe);
window.addEventListener('load', controlRecipe);

['hashchange', 'load'].forEach(event => window.addEventListener(event, controlRecipe));

```

## Splice vs Slice

* Splice:
    * Pass in a start index and how many positions to take.
    * It returns the elemtns and deletes them from the original array.
* Slice:
    * Accepts a start and an end index and then returns a new array
    * Doesn't mutate the original array.

## localStorage API

* `localStorage.setItem('key', 'value')`
    * Both key and value have to be strings.
    * To retrieve `localStorage.getItem('key')`
* Built into the browser window object.
* Data stays across page load.