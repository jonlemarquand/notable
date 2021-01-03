---
title: VueJS 13 - Routing
created: '2020-11-04T20:27:19.366Z'
modified: '2020-11-04T21:00:34.724Z'
---

# VueJS 13 - Routing

* Multiple pages in SPAs

## Routing Setup

* `npm install --save vue-router@next` add @next to the end for vue3.
* In main.js, you can register the router like so:

```javascript

import { createRouter, createWebHistory } from 'vue-router';
import TeamsList from './components/teams/TeamsList.vue';
import UsersList from './components/teams/UsersList.vue';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/teams',
      component: TeamsList
    },
    {
      path: '/users',
      component: UsersList
    }
  ]
});

app.use(router);

// In App.vue

<main>
  <router-view></router-view>
</main>

```
* Every object represents one route and the config for it.

## Navigating with Router-link

* `<router-link to="/teams"></router-link>`
* A special anchor tag that doesn't reload a page and instead the vue router takes over.

### Styling Active Links

* Style the `router-link-active` 
* or in main.js you can use `linkActiveClass: 'active` to change the default name in the router config.

## Programmatic Navigation


