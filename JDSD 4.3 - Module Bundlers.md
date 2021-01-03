---
title: JDSD 4.3 - Module Bundlers
created: '2020-05-05T06:14:15.739Z'
modified: '2020-05-05T06:58:46.795Z'
---

# JDSD 4.3 - Module Bundlers

* Bundlers are able to manipulate and combine CSS files, or JS files etc and minify them.
* Alternatives to webpack: Parcel/Rollup.js
* Webpack does have a lot of set up, because you have to tell it every setting.

## Webpack

### Intro to Webpack

* Webpack bundles our files into static files that we send to the browser.
* `npm install --save-dev webpack webpack-dev-server webpack-cli`
  * webpack-dev-server will let us be able to test a build on a localserver.
  * In scripts: `"start": "webpack-dev-server --config ./webpack.config.js --mode development"`
* In `webpack.config.js`:

```
module.exports = {
  entry: [
    './src/index.js'
  ],
  output: {
    path: __dirname + '/dist',
    publicPath: '/'
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist'
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      },
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['eslint-loader']
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  }
}

```
* The module rule in the config file allows only the js or jsx files (excluding the node-modules folder) to be run through babel and eslint as well.
  * This uses regular expressions.
* The resolve rule allows react to import 'file' rather than 'file.js' assuming it would be automatically compiled.
* Babel allows us to write code in ES6 and then put it through Babel to the exact code in ES5 for older browsers.
  * `npm install --save-dev babel-core babel-loader babel-preset-env`
  * `npm install --save-dev babel-preset-react`
  * In package.json file we can add:

```json

"babel": {
  "presets": [
    "env",
    "stage-2",
    "react"
  ]
}

```
* `npm install react react-dom` - For React
* `npm install --save-dev install eslint eslint-loader`
* We also need an `.eslintrc` file:

```
{
  "parser": "babel-eslint",
  "rules": {

  }
}


```
* `npm install --save-dev babel-eslint`
* `npm install --save-dev eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y`
  * These are just some of the eslint configurations that are used to make better code.
  * Airbnb open-sourced their eslint config that is fairly popular.


## Updating Libraries: Babel 7 + ESlint

* Babel
  * Babel 7.9 is the latest version (May 2020).
  * `npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader`
  * All you need to do is:

```json
"babel": {
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

* ESlint
  * The way of making a `.eslintrc` file is deprecated.
  * We could use `.eslintrc.json` to keep the file the way it was in the previous video.
* Webpack Build Config: https://createapp.dev/


## Parcel

* With Parcel, in the "scripts" section, we can just use "start": "parcel index.html" and parcel will take care of the rest.
* Really quick set-up time, zero configuration.
