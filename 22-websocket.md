# websocket协议

[http,tcp,websocket的差异](https://developer.aliyun.com/article/1152770)

服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，实现真正的双向平等对话

![](images/img/Snipaste_2024-08-18_21-00-00.png)

## ws

server.js

```
const WebSocketServer = require('ws').WebSocketServer


const wss = new WebSocketServer({ port: 8080 });

wss.on('connection', function connection(ws) {
  ws.on('error', console.error);

  ws.on('message', function message(data) {
    console.log('received: %s', data);
  });

  ws.send('something');
});
```

client.js

```
const WebSocket = require('ws')

// Create WebSocket connection.
const socket = new WebSocket("ws://localhost:8080");

// Connection opened
socket.on("open", function (event) {
  socket.send("Hello Server!");
});

// Listen for messages
socket.on("message", function (event) {
  console.log("Message from server ", event.toString());
});
```



## socket.io

在ws的基础上增加了低版本兼容，可以自动降级处理

客户端有两种引用方式：

<script src='socket.io.js'>   （node包）

cdn方式 