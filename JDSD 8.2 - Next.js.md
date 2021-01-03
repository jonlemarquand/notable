---
title: JDSD 8.2 - Next.js
created: '2020-04-28T09:38:21.913Z'
modified: '2020-04-28T11:49:08.976Z'
---

# JDSD 8.2 - Next.js

## Setting Up Next.js

* `npm init -y`, `npm install next react react-dom`
* With next it includes routing, webpack, css features, typescript etc.
* `create a pages directory`

```javascript

const Index = () => (
<div>
  <h1>SSR Magician</h1>
</div>
);

export default Index

```
## Next.js Pages

```javascript

import Link from 'next/link';

const About = () => {
  return (
    <div>
      <h1>About</h1>
      // could also use <a href='/about'>About</a>
      <Link href='/'>
        <a>Home</a>
      </Link>
    </div>
  );
}

export default About;

```
* All you had to do was create a new page in the pages folder with 'about'.
* With next links we just give it the end point '/about' for a tags rather than the 'about.js'

### Client-Side Routing

* Happens when a route is handled internally by javascript.
  * Any time the URL changes, the state of our application changes.
  * With client-side rendering, you don't have to refresh the entire page.
* The link tag using 'next/link' makes the routing happen client-side rather than the `<a> href>` tag.

## Shared Components

```javascript
// components > image.js

const Image = () => (
  <img src="http://testimg.com">
);

export default Image;

// about.js

import Image from '../components/Image'

...
<Image />
...

```

## Dynamic Apps with Next.js

* An API called: GetInitialProps - It gets the data and passes it down to the component as props.
  * We can either do API calls on the server or on the client.
* If you're working with next use: `npm install isomorphic-unfetch`. We can then use Fetch on the server.

```javascript

//robots.js

import Link from 'next/link';
import fetch from 'isomorphic-unfetch';

const Robots = (props) => {
  return (
    <div>
      <h1>Robots</h1>
      <Link href='/'>
        <button>Home</button>
      </Link>
      <div>
        {
          props.robots.map(robot => (
            <li key={robot.id}>
              <Link href={`robots/${robot.id}`}>
                <a>{robot.name}</a>
              </Link>
            </li>
          ))
        }
      </div>
    </div>
  )
}

Robots.getInitialProps = async function() {
  const res = await fetch('http://jsonplaceholder.typicode.com/users')
  const data = await res.json();
  return {
    robots: data
  }
}

export default Robots;

```

## Latest Version of Next.js

* `npx create-next-app`
