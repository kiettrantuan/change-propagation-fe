---
theme: blood
height: "560"
margin: "0.6"
highlightTheme: css/vs2015.css
transition: slide
---
<style>
	.h-50 {
		height: 50%
	}
</style>
# **Change Propagation In Frontend Environment**


---

# What

  

 Pull/Push change to other parts in FE Ecosystem <!-- element class="fragment" data-fragment-index="1" -->
note:
1. _Hiện "What"_
Change propagation (lan truyền thay đổi) là gì:
- Khi thay đổi 1 phần trong hệ thống sẽ dẫn đến sự thay đổi ở các phần khác có liên quan đến nó.
- Như các bánh răng liên kết với nhau khi ta xoay 1 bánh bất kì sẽ lan truyền động năng làm xoay các bánh còn lại.
2. _Hiện "Pull/Push.." và hình_
- Ở FE ta có sơ đồ lan truyền gồm các thành phần chính là User, Display, Webworker và Backend.
- Ngoài Webworker và Backend có thể tương tác trực tiếp thì các phần lan truyền thay đổi qua Display.
NOTE (ko nói):
- Như khi chiên trứng -> tăng lửa -> chảo nóng -> trứng chín -> tắt lửa -> chảo giảm nhiệt -> trứng không bị khét.

  

---

  

# DATA FLOW
![[Pasted image 20240403142749.png]]
 <!-- element class="fragment" data-fragment-index="1" -->

note:
- **Backend**: This is the server-side of the application where data processing and storage occur.
- **Browser**: This represents the client-side of the application where user interactions take place. It contains two parts: “Display (HTML/JS FE Framework)” and “State/Cache”. The “Display” part is responsible for rendering the user interface, while the “State/Cache” part manages the application’s state and cached data.
- **Webworker**: This is a separate thread in the browser that can perform tasks in parallel with the main browser thread. It helps to offload heavy computations from the main thread, keeping the user interface responsive.
- **User**: This represents the end-user interacting with the application through the browser.

---
## User interaction 
- Click, touch
- Typing
- Scrolling
- ...
note:
1. Đầu tiên em xin nói về Thay đổi đến từ Tương tác của User:
- User thông qua các thiết bị vật lý như chuột, bàn phím, màn hình... Push và Pull thay đổi từ Display.
2. Ví dụ:
- User nhập form login rồi submit thì Display gọi lên Backend, đợi nhận kết quả login rồi Display render kết quả lại cho User.
- Trong ví dụ này, thay đổi đến từ User đã lan truyền qua Display tới Backend và ngược lại.
---
## Display
- HTML: Structure & Content
- CSS: Style
- JS: Functionality 
--
![[Pasted image 20240403141814.png]] 
note:
1. Display giữ vai trò nhận thay đổi từ các phần còn lại, xử lý rồi push thay đổi đến các phần cần thiết trong sơ đồ.
2. Display tạo thành từ 3 ngôn ngữ có chức năng khác nhau:
- HTML: Cấu trúc và nội dung -> tĩnh -> xương sống
- CSS: Giao diện -> tĩnh -> da, quần áo, vẻ ngoài
- JavaScript: Chức năng, tương tác -> đông -> bộ não, điều khiển mọi chức năng, hoạt động sống
NOTE (ko nói):
- Khi user truy cập web trên browser thì HTML sẽ load thành DOM cung cấp các method cho phép JavaScript truy cập và thay đổi.
- Get từ server (qua HTTP req/res) hoặc cache;
- HTML có thể qua <form action method> <button submit> để giao tiếp server mà ko cần JS, server process -> trả về HTML mới

--
### Propagate change:
- User: DOM Event -> JS -> ... -> DOM
- Webworker: DOM message Event -> JS -> ... -> DOM
- Backend: Network method -> JS -> ... -> DOM
note:
3. Vậy làm sao để Display nhận biết (pull) thay đổi:
- Phía User: Gán event, state vào các element mà user có thể tương tác như button, input,...
- Phía Webworker: Event onmessage và postmessage
- Phía Backend: api, websocket...
4. Các DOM Event hay response từ BE sẽ được JS xử lý, tính toán lan và lan truyền đi cuối cùng quay lại update DOM cho User nếu cần thiết.
5. Ví dụ: User login thì JS lấy username và pass từ DOM gửi lên BE, đợi response login thành công thì JS update State của user thành đã login và thay đổi các Element tương ứng như đổi button login thành sign out, hiển thị username...
--
### Framework:
- React: Virtual DOM
- Vue: data-binding
- HTMX
note:
React:
React uses a virtual DOM to track changes. When state changes in a component, React creates a new virtual DOM and compares it with the old one1.
This process is called “diffing”. React identifies the differences (or “diffs”) between the old and new virtual DOMs and updates only those parts in the real DOM1.
React uses synthetic events, which are wrappers around the browser’s native events. You can stop event propagation on synthetic events because React handles propagation of these events internally2.
However, stopping the propagation of one type of event (e.g., a change event) won’t affect the propagation of a different type of event (e.g., a click event)3.
React’s one-way data flow means that state is passed down from parent components to child components through props1. This makes state changes predictable but can make complex state management more challenging1.
Vue:
Vue also uses a virtual DOM and a diffing algorithm, similar to React4.
However, Vue extends this with a reactivity system. When you change a Vue instance’s data, the view automatically updates4.
Vue uses an event system that allows components to communicate with each other. Events in Vue can be triggered by user actions, component methods, or other components4.
Vue provides the .stop modifier for v-on to stop event propagation5.
Vue supports two-way data binding (with the v-model directive), which can make handling user input more straightforward4.
For complex state management, Vue relies on Vuex, a state management library6.
HTMX:
HTMX allows you to access modern browser features directly from HTML, rather than using JavaScript7.
It extends and generalizes the core idea of HTML as a hypertext, opening up many more possibilities directly within the language7.
Any element, not just anchors and forms, can issue an HTTP request7.
Any event, not just clicks or form submissions, can trigger requests7.
Any HTTP verb, not just GET and POST, can be used7.
Any element, not just the entire window, can be the target for update by the request7.
When a successful action occurs during a POST, the response includes an HX-Trigger response header that looks like this: HX-Trigger:newContact. This will trigger the table to issue a GET to /contacts/table and this will render the newly added contact row (in addition to the rest of the table)
---
## Backend
- HTTP Requests <!-- element class="fragment" data-fragment-index="1" -->
- WebSockets <!-- element class="fragment" data-fragment-index="2" -->
- Server-Sent Events (SSE) <!-- element class="fragment" data-fragment-index="3" -->
- Polling and Long-Polling <!-- element class="fragment" data-fragment-index="4" -->
note:
- HTTP Requests: This is the most common way to connect frontend to backend. The frontend makes HTTP requests to the backend, which responds with the requested data. This data can be in various formats, such as JSON or XML
- WebSockets: This is a protocol that provides full-duplex communication between the client (frontend) and the server (backend) over a single, long-lived connection. This allows the server to push updates to the client as soon as they happen, without the client needing to request the updates.
- Server-Sent Events (SSE): This is a standard that allows a server to push updates to a client whenever a new event occurs on the server. Unlike WebSockets, SSE is a one-way communication channel from server to client.
- **Polling** simply means checking for new data over a fixed interval of time by making API calls at regular intervals to the server. It is used to get real-time updates in applications. There are many applications that need real-time data and polling is a life savior for those applications.
--

| Method                   | Real-time  | Bi-directional  | Complexity  | Browser Support  |
| ------------------------ | ---------- | --------------- | ----------- | ---------------- |
| HTTP Requests            | No         | No              | Low         | High             |
| WebSockets               | Yes        | Yes             | Medium      | High             |
| Server-Sent Events (SSE) | Yes        | No              | Medium      | Medium           |
| Polling                  | No         | No              | High        | High             |

--
**HTTP Requests**:

- Real-time: No. The client must send a request to receive a response.
- Bi-directional: No. The server cannot send data to the client without a client request.
- Complexity: Low. This is the standard way of sending data over the web.
- Browser Support: High. All modern browsers support this.
--
RESTful APIs
```js
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));

fetch('https://api.example.com/user',
{
    method: "POST",
    body: JSON.stringify({ name:'Flint' })
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
<!-- element class="fragment" data-fragment-index="1" -->
```js
import axios from 'axios';
axios.get('https://api.example.com/data')
    .then(response => response.json())
    .catch(error => console.error('Error:', error));
    
axios.post('https://api.example.com/user', { name: 'Flint' })
	.then(response => response.json())
    .catch(error => console.error('Error:', error));
```
<!-- element class="fragment" data-fragment-index="2" -->

--
Graphql
- Apollo client, Urql, v...v...

```js
  import { gql, useQuery } from "urql";
  const TodosQuery = gql`
    query {
      todos {
        id
        title
      }
    }
  `;
  const UpdateTodo = `
	mutation ($id: ID!, $title: String!) {
		updateTodo (id: $id, title: $title) {
			id
			title
		}
	}
	`;
  const Todos = () => {
    const [result, reexecuteQuery] = useQuery({
      query: TodosQuery,
    });
    const [updateTodoResult, updateTodo] = useMutation(UpdateTodo);
    const submit = newTitle => {
	const variables = { id, title: newTitle || '' };
	updateTodo(variables).then(result => {
		if (result.error) {
			console.error('Oh no!', result.error);
		}});
	};
    const { data, fetching, error } = result;
    if (fetching) return <p>Loading...</p>;
    if (error) return <p>Oh no... {error.message}</p>;
    return (
      <ul>
        {data.todos.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    );
  };
```
note:
- Just like REST, GraphQL allows the client (frontend) to request data from the server (backend). However, there are some key differences between GraphQL and REST:
- Single Endpoint: In GraphQL, there’s typically a single endpoint that accepts queries, as opposed to multiple endpoints in REST.
- Client-Specified Responses: The client specifies exactly what data it needs, which can reduce over-fetching and under-fetching issues common in REST.
- Real-Time Updates: GraphQL has built-in support for real-time updates with subscriptions(Websocket). But only receive changes from server (one-way)
--
**WebSockets**:
![[Pasted image 20240403032325.png]]
--
- Real-time: Yes. The server can send data to the client at any time.
- Bi-directional: Yes. Both the client and server can send data at any time.
- Complexity: Medium. Requires more setup than HTTP requests. Uses a custom ws protocol to transport messages, which works at a lower level than HTTP
- Browser Support: High. All modern browsers support this.
--
Js:
```js
const socket = new WebSocket('ws://localhost:8080');
socket.addEventListener('message', function (event) {
    console.log('Message from server: ', event.data);
});
socket.send('Hello Server!');
```

Alternative: socket.io <!-- element class="fragment" data-fragment-index="1" -->

```html
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <script src="/socket.io/socket.io.js"></script>
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
    <script>
      $(function () {
        const socket = io();
        $('form').submit(function(e){
          e.preventDefault(); // prevents page reloading
          socket.emit('chat message', $('#m').val());
          $('#m').val('');
          return false;
        });
        socket.on('chat message', function(msg){
          $('#messages').append($('<li>').text(msg));
        });
      });
    </script>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```
<!-- element class="fragment" data-fragment-index="2" -->

--

**Server-Sent Events (SSE)**:
![[Pasted image 20240403032448.png]]
--
- Real-time: Yes. The server can send data to the client at any time.
- Bi-directional: No. The client cannot send data to the server.
- Complexity: Medium. Requires more setup than HTTP requests. Uses XHR streaming to transport messages over HTTP
- Browser Support: Medium. Not all browsers support this.
note:

| **WebSockets**                                                                        | **Server-Sent Events**                                     |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Two-way message transmission                                                          | One-way message transmission (server to client)            |
| Supports binary and UTF-8 data transmission                                           | Supports UTF-8 data transmission only                      |
| Supports a large number of connections per browser                                    | Supports a limited number of connections per browser (six) |
| Can't be polyfilled using JavaScript; you need to fall back to basic HTTP messages    | Can be polyfilled using JavaScript                         |
| Some enterprise firewalls with packet inspection have trouble dealing with WebSockets | No blocking by enterprise firewalls                        |

--
Js
```javascript
const eventSource = new EventSource("api/events");

eventSource.onmessage = function(event) {
    console.log("New message: ", event.data);
};
```
--
**Polling**:
- Real-time: No. The client must send a request to receive a response.
- Bi-directional: No. The server cannot send data to the client without a client request.
- Complexity: High. Requires more setup and resources than other methods.
- Browser Support: High. All modern browsers support this.
--
Short Polling

![[Pasted image 20240403032811.png]] <!-- element class="h-50" -->
- Involves the client sending HTTP requests at regular intervals to check for updates.
- The server processes each request and returns a response immediately, regardless of whether new data is available.

note:
- Key Features:
	- Simple Implementation: Easier to implement than long polling.
	- High Frequency Polling: Can be set to poll at very short intervals.
	- Independent Requests: Each request is independent, reducing server memory usage.
- Use Cases:
	- Periodic Data Updates: Useful for data that changes at predictable intervals.
	- System Health Checks: Regularly checks the status of systems or services.
	- Content Feeds: Updates feeds where real-time data is not critical.
--
Long polling

 ![[Pasted image 20240403033241.png]]<!-- element class="h-50" -->
- A variation of the traditional polling technique where the server holds a client request open until new data is available.
- Upon receiving new data, the server responds and closes the connection. The client then immediately sends another request, thus creating a near-continuous connection.
note:
Key Features:
- Reduced Latency: Delivers updates with minimal delay.
- Server-Triggered Responses: Server sends data as soon as it's available.
- Efficient for Real-Time Data: Ideal for applications requiring real-time updates.
Use Cases:
- Chat Applications: Delivers messages in real-time.
- Live Sports Updates: Instantly updates scores and events.
- Notification Systems: Alerts users to new events or changes immediately.
## Key Differences[​](https://www.svix.com/resources/faq/long-polling-vs-short-polling/#key-differences "Direct link to Key Differences")

1. **Connection Duration**: Long polling holds the connection open, while short polling involves repeated separate requests.
2. **Data Latency**: Long polling provides data with lower latency compared to short polling.
3. **Server Load**: Long polling can be more demanding on the server, especially with many open connections.
4. **Implementation Complexity**: Long polling is generally more complex to implement than short polling.
5. **Real-Time Capability**: Long polling is better suited for real-time applications.
--
Short polling
```html
 <script>
	function loadInformation() {
		setInterval(function () {
			// Request
			var request = new XMLHttpRequest();
			request.open("GET", "./data.json");
			request.send();
			// Response
			request.onreadystatechange = function () {
				if (this.readyState == 4 && this.status == 200) {
					// Also checked status==200 to 
					// verify its status is OK or not
					console.log(this.responseText);
				}
			}
		}, 1000);
	}
</script>
```
Long polling
```html
<script>
	function loadInformation() {
		// Request
		var request = new XMLHttpRequest();
		request.open("GET", "./data.json");
		request.send();
		// Response
		request.onreadystatechange = function () {
			if (this.readyState == 4 && this.status == 200) {
				// Also checked status==200 to 
				// verify its status is OK or not
				console.log(this.responseText);
				loadInformation();
			}
		}
	}
</script>
```


note:
- short polling: In this example, the client sends a GET request to the server every second to check for new data. If the server has new data, it sends it back and the client logs it to the console
- long polling: In this example, the client sends a GET request to the server. If the server has new data, it sends it back, the client logs it to the console, and then immediately sends another request

--
### Use Cases: 

---
## Web Worker

- JavaScript feature

- Independent background script

- Can't direct access to the DOM _but indirect through message pipeline_ <- how it propagate change

