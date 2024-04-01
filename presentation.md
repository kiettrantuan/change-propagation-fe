---
marp: true
theme: uncover
class: invert
paginate: true
---

## Frontend

# **Change Propagation**

---

# What

![bg right contain](./FE-eco.png)

- Pull/Push change to other parts in FE Eco

---

# **CAUSES**

- Backend
- User interaction
- Daemon
- Web Worker

---

# Backend

- APIs
  - REST API:
    - BUILT-IN: FETCH / GET/POST
    - AXIOS
  - GRAPHQL:
    - APOLLO CLIENT
    - Urql
- WebSockets
  - BUILT-IN: new WebSocket
  - Socket.io

---

- Server-Sent Events (SSE)
  - BUILT-IN: const eventSource = new EventSource('http://localhost:8000/stream');
- Polling and Long-Polling
  - REACT-QUERY

---

| Method                       | Real-time | Bi-directional | Complexity | Browser Support |
| ---------------------------- | --------- | -------------- | ---------- | --------------- |
| HTTP Requests (RESTful APIs) | No        | No             | Low        | High            |
| WebSockets                   | Yes       | Yes            | Medium     | High            |
| Server-Sent Events (SSE)     | Yes       | No             | Medium     | Medium          |
| Long Polling                 | No        | No             | High       | High            |

---

# User interaction

- Event listeners - HTML/JS: document.getElementById('myInput').addEventListener('input', (event) => {
  console.log(event.target.value);
  });
  - REACT: onChange, onClick
  - VUE: v-on:click="handler" / @click="handler".
  - HTMX

---

# Web Worker

- JavaScript feature
- Independent background script
- Can't direct access to the DOM _but indirect through message pipeline_ <- how it propagate change

---

# STATE/ CACHE

=> state / cache change => trigger re-render
