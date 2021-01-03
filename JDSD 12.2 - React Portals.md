---
title: JDSD 12.2 - React Portals
created: '2020-05-03T09:46:04.895Z'
modified: '2020-05-03T10:53:06.509Z'
---

# JDSD 12.2 - React Portals

## React Portals

```javascript

// index.html

<body>
  <div id="root"></div>
  <div id="modal-root"></div>
</body>

// app.js

import Modal from './components/Modal/Modal';

// modal.js

import React from 'react';
import ReactDOM from 'react-dom'l
import './Modal.css';

const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    modalRoot.appendChild(this.el)
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el)
  }

  render() {
    return ReactDOM.createPortal(this.props.children, this.el)
  }
}

export default Modal;

```
* A portal element in React inserts into the DOM tree after the Modal's children are mounting.
  * Attached at the root to the HTML div.
* if you have a form, create a new state within the form component that it keeps.
  * When the user clicks submit, or does an important action we then update the main App.js state.
  * This stops the state being updated everytime they type a key.
