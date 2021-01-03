---
title: GatsbyJS (LevelUpTutorials)
created: '2020-11-21T10:07:10.958Z'
modified: '2020-11-21T17:32:52.632Z'
---

# GatsbyJS (LevelUpTutorials)

* A blazing fast way to build static sites without a backend.
* Gatsby node-based and tied in with React.
* `npm install --global gatsby-cli`
* `gatsby new folderName`

## Gatsby Files Overview

* `gatsby-config.js`: What kind of plugins + site title.
* `src` folder has: Pages and Layouts

## Adding Plugins & Using Sass

* There's a load of official Gatsby plugins
* After npm installing it, in the plugins config:

```js

plugins: [
  `gatsby-plugin-react-helmet`,
  `gatsby-plugin-sass`
],

```
* When you add a new plugin, you need to restart your development server.

## Building a Blog with Markdown

* Need to use a source plugin, the gatsby source filesystem.
  * `npm install --save gatsby-source-filesystem`
  * Also need to add it in the gatsby-config file:

```javascript

plugins: [
  {
    resolve: 'gatsby-source-filesystem',
    options: {
      path: `${__dirname}/src/pages`,
      name: 'pages',
    }
  },
  'gatsby-transformer-remark'
]

```
* Initialising a plugin and giving it an option of a path where the files live and a name.
* Next, we need a transformer plugin.
  * Transform the data into a format which GraphQL can query.
  * `npm install --save gatsby-transformer-remark`
* Restart the site to add the changes.
* Create a folder in pages: `08-24-2017-first-blog`
  * A new file in it: `index.md`

```markdown

---
path: '/first-post'
title: 'First Blog Post'
---

# Hello!

```
* Make a templates folder > post.js

```javascript

import React from 'react';
import Helmet from 'react-helmet';

export default function Template({data}) {
  const {markdownRemark: post} = data;
  // const post = data.markdownRemark;
  return (
    <div>
      <h1>{post.frontmatter.title}</h1>
      <div dangerouslySetInnerHTML = {{__html: post.html}} />
    </div>
  )
}

export const postQuery = graphql`
  query BlogPostByPath($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        path
        title
      }
    }
  }
`
```

* So far we've told gatsby where to look for posts but not that they should be pages.
* Make a file at the root of the project called: `gatsby-node.js`
  * We'll be using the create-pages api

```

const path = require('path');

exports.createPages = ({boundActionCreators, graphql}) => {
  const {createPage} = boundActionCreators;

  const postTemplate = path.resolve('src/templates/post.js');

  return graphql(`{
    allMarkdownRemark {
      edges {
        node {
          html
          id
          frontmatter {
            path
            title
          }
        }
      }
    }
  }`)
  .then(res => {
    if(res.errors) {
      return Promise.reject(res.errors);
    }
  
    res.data.allMarkdownRemark.edges.forEach( ({node}) => {
      createPage({
        path: node.frontmatter.path,
        component: postTemplate
      })
    })
  })
}

```
* BoundActionsCreators allows you to use gatsby's action creators.

## Testing Queries with GraphQL

* There is a graphql debugger


