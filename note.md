# **CAUSES**

---

# Backend

* APIs
    - REST API:
        - BUILT-IN: FETCH / GET/POST
        - AXIOS
    - GRAPHQL:
        - APOLLO CLIENT
        - Urql
* WebSockets
    - BUILT-IN: new WebSocket
    - Socket.io
* Server-Sent Events (SSE)
    - BUILT-IN: const eventSource = new EventSource('http://localhost:8000/stream'); 
* Polling and Long-Polling
    - REACT-QUERY

| Method                       | Real-time | Bi-directional | Complexity | Browser Support |
|------------------------------|-----------|----------------|------------|-----------------|
| HTTP Requests (RESTful APIs) | No        | No             | Low        | High            |
| WebSockets                   | Yes       | Yes            | Medium     | High            |
| Server-Sent Events (SSE)     | Yes       | No             | Medium     | Medium          |
| Long Polling                 | No        | No             | High       | High            |
---

# User interaction

* Event listeners
    - HTML/JS: document.getElementById('myInput').addEventListener('input', (event) => {
  console.log(event.target.value);
});
    - REACT: onChange, onClick
    - VUE: v-on:click="handler" / @click="handler".
    
---
# STATE/ CACHE 

---
# Daemon

* Background processes handles tasks *such as maintaining a WebSocket connection*

---

# Web Worker

* JavaScript feature
* Independent background script
* Can't direct access to the DOM *but indirect through message pipeline* <- how it propagate change

---

# **CONCEPTS**

---

# Data Binding

This is the connection between the UI and the business logic. In two-way data binding, if the model state changes, the UI updates automatically and vice versa.

---

# State Management

This involves managing the state of an application - the state is the source of truth that drives the UI. When the state updates, the UI re-renders to reflect those changes.

---

# Component-based Architecture

Modern frontend frameworks like React, Vue, Angular, and Svelte use a component-based architecture. When the state of a component changes, the change propagates to the child components.

---

# Event Propagation

This is the process by which an event propagates or bubbles up from its origin (where it occurred) to the root of the tree (usually the document object). Event propagation can be stopped at any point.

---

# Context and Global State

Context provides a way to pass data through the component tree without having to pass props down manually at every level. Global state libraries like Redux or Vuex handle state changes and propagate these changes across components.

---

# Virtual DOM and Reconciliation

React uses a virtual DOM to keep track of changes. When state changes, React creates a new virtual DOM and compares it with the old one. It then updates the real DOM with only the changes that occurred, rather than re-rendering the entire DOM.

---

# Dependency Tracking

Frameworks like Vue use a reactive dependency system that automatically knows which components need to re-render when state changes.