---
title: VueJS 6-8 - Introducing Components
created: '2020-10-30T16:00:05.228Z'
modified: '2020-11-01T09:50:25.004Z'
---

# VueJS 6-8 - Introducing Components

* Every Vue app you have that controls each individual part of the page would work independently.
* Vue components are connected Vue instances.

```javascript

app.component('user-contact', {
  template: `
    <li>Some Html code here</li>
    <li>Some more code here</li>
  
  `,
  data() {
    return { 
      deatailsAreVisible: false 
      friend: {
        name: 'Jon'
      }
    };
  },
  methods: {
    toggleDetails() {
      this.detailsAreVisible = !this.detailsAreVisible;
    }
  }
});

```
```html
<section id="app">
  <ul>
    <friend-contact>
    </friend-contact>
  </ul>
</section>
```
* You should alwaysx use a component identifier that uses a dash. 
  * This means you will avoid official html elements.
* A Vue component is essentially just another app, but one that belongs to the main app.
  * They can have data, methods etc.

## A Better Development Set-Up

`npm install -g @vue/cli`
`vue create vue-app-name`
`npm run serve`

* The main.js file is the main file where the app is loaded.
* .vue files allow us to write vue components in a much nicer way.
  * You can split template, script, and style in to different blocks.
* Add `vetur` extension.


```javascript

// App.vue

<script>
export default {
    data() {
      return {
        friend: "Jon"
      }
    }
  }
</script>

// main.js

import App from './App.vue'

createApp(App).mount('#app');
```

### Adding a Component

* create a folder called components.
* create a file called CapitalCamelCase.vue also known as PascalCase

```javascript

// FriendContact.vue
<template>
  <li>
    <h2>{{ friend.name }}</h2>
    <button @click="toggle-details">
  </li>
</template>

<script>
export default {
  data() {
    return {
      detailsAreVisible: false,
      friend: {
        id: "manuel",
        name: "Manuel Lorenz"
      }
    };
  }
};
</script>

// main.js

const app = createApp(App);
app.component('friend-contact', FriendContact);
app.mount('#app');

// App.vue
<template>
  <friend-contact></friend-contact>
</template>

```

## Introducing Props

```javascript

export default {
  props: [
    'name',
    'phoneNumber',
    'emailAddress'
  ],
}

```

* Props should be defined in camelCase in the component, but when feeding props in html use this-case
* In the template of the component, we just use `name` rather than `this.name` or `props.name`
* With props, we can't change the data in `FriendsContact` received from App.
  * You can only pass data down.
* You can type your props, to validate what kind of data should be received by this prop:

```javascript

props: {
  name: {
    type: String,
    required: true
  },
  isFavourite: {
    type: String,
    required: false,
    default: '0',
    validator: function(value) {
      return value === '1' || value === '0';
    }
  }
},

```
* You can bind props, like any other html value.

```html
v-bind:is-favourite="true"
<friend-contact
  v-for="friend in friends"
  :key="friend.id"
>
</friend-contact>

```

### Child -> Parent Communication (Events)

```javascript
//child component
//emits: ['toggle-favourite']
emits: {
  'toggle-favourite': function(id) {
    if (id) {
      return true;
    } else {
      console.warn('Id is missing');
      return false;
    }
  }
},
methods: {
  toggleFavourite() {
    this.$emit('custom-event-name', this.id);
  }
}

```
```html
<!--parent component-->
@toggle-favourite="toggleFavouriteStatus"

<script>
  methods: {
    toggleFavouriteStatus(friendID) {
      const identifiedFriend = this.friends.find(friend => friend.id === friendId);
      identifiedFriend.isFavourite = !identifiedFriend.isFavourite
    }
  }
</script>

```

### Provide &amp; Inject

* You can only inject what has been provided on a higher parent/ancestor level.

```javascript
//app.vue

provide() {
  return {
    topics: this.topics
  }
}

//KnowledgeGrid.vue

inject: ['topics']

```
