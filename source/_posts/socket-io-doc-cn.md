---
title: Socket.IO中文文档
tags: [socket,socket.io,文档]
categories: [服务器相关]
date: 2018-10-16 09:53:54
---

尝试翻译socket.io官方文档, 没有时间线, 不知道何时完成...


水平有限, 如果有人看到, 并且发现其中错误, 欢迎您给我指出, 谢谢!  
<!--more-->
***
# 目录
- [目录](#目录)
- [概述](#概述)
    - [Socket.IO是什么](#socketio是什么)
        - [可靠性](#可靠性)
        - [支持自动重连](#支持自动重连)
        - [断线检测](#断线检测)
        - [支持二进制](#支持二进制)
        - [支持多路技术(待翻译)](#支持多路技术待翻译)
        - [支持房间模式](#支持房间模式)
    - [Socket.IO不是什么](#socketio不是什么)
    - [Socket.IO的安装](#socketio的安装)
        - [服务器](#服务器)
        - [JavaScript客户端](#javascript客户端)
        - [其他的客户端实现](#其他的客户端实现)
    - [与http模块搭配使用](#与http模块搭配使用)
        - [服务器(app.js)](#服务器appjs)
        - [客户端(index.html)](#客户端indexhtml)
    - [与express模块搭配使用](#与express模块搭配使用)
        - [服务器(app.js)](#服务器appjs)
        - [客户端(index.html)](#客户端indexhtml)
    - [发送和接收事件](#发送和接收事件)
        - [服务器](#服务器)
    - [Restricting yourself to a namespace(待翻译)](#restricting-yourself-to-a-namespace待翻译)
    - [Sending volatile messages(待翻译)](#sending-volatile-messages待翻译)
    - [发送和获取数据(acknowledgements)(待翻译)](#发送和获取数据acknowledgements待翻译)
    - [广播消息](#广播消息)
        - [服务器](#服务器)
    - [作为跨浏览器的WebSocket使用它](#作为跨浏览器的websocket使用它)
        - [服务器(app.js)](#服务器appjs)
        - [客户端(index.html)](#客户端indexhtml)

# 概述
## Socket.IO是什么
Socket.IO是浏览器与服务器之间的实时的、双向性的、基于事件的通讯库，它由两部分组成：
* Node.js服务器：[源码](https://github.com/socketio/socket.io) | [API](https://socket.io/docs/server-api/)
* 供浏览器使用的Javascript库(也可以在Node.js中运行)：[源码](https://github.com/socketio/socket.io-client) | [API](https://socket.io/docs/client-api/)

它的主要特点是：
### 可靠性
即使在以下情况下依然可以建立链接：
* 有代理和负载均衡。
* 个人防火墙和杀毒软件。

为了达到这个目的， Socket.IO依赖于 [Engine.IO](https://github.com/socketio/engine.io)，先建立一个长轮询连接，然后尝试将其升级为经测试过更可靠的传输方式， 比如WebSocket。更多信息请访问 [Goals](https://github.com/socketio/engine.io#goals) 板块.
### 支持自动重连
除非被通知，否则一个断开连接的客户端会一直尝试重新连接服务器， 知道服务器再次可用。请在 [这里](https://socket.io/docs/client-api/#new-Manager-url-options) 查看可用的重连选项。
### 断线检测
在Engine.IO层面实现了心跳机制，允许客户端和服务器双方都能知道另一方是否不再响应。

这个功能是通过在服务端和客户端设置的计时器实现的，在连接建立是的握手阶段通过超时值共享（参数pingInterval和pingTimeout）。Those timers require any subsequent client calls to be directed to the same server, hence the sticky-session requirement when using multiples nodes.
### 支持二进制
所有可以序列化的数据结构都可以发送，包括：
* 浏览器中的 [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 和 [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
* Node.js中的 [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 和 [Buffer](https://nodejs.org/api/buffer.html)
### 支持多路技术(待翻译)
In order to create separation of concerns within your application (for example per module, or based on permissions), Socket.IO allows you to create several Namespaces, which will act as separate communication channels but will share the same underlying connection.
### 支持房间模式
在任一命名空间内，你可以定义任意的频道， 我们称之为房间（Rooms），socket连接可以加入或离开放假。你可以向一个指定的房间广播消息，所有加入该房间的客户端都会受到广播消息。

这是一个很有用的功能， 可以向一个群组的用户或者连接到多个设备的指定用户发送通知。

这些功能都有一个简单方便的API，例如：
```javascript
io.on('connection', function(socket){
    socket.emit('request', /* */); //向socket连接发送事件
    io.emit('broadcast', /* */); //向所有连接发送事件
    socket.on('reply', function(){ /* */ };); //监听reply事件
});
```
## Socket.IO不是什么
Socket.IO **不是** WebSocket的一种实现方法。虽然Socket.IO确实在WebSocket可用的时候使用其作为传输方式，它向每一个数据包添加了一些元数据：数据包类型（packet type）、命名空间（namespace）和确认id（ack id）如果需要消息确认的话。这就是为什么一个WebSocket客户端不能连接到Socket.IO服务器， 反过来一个Socket.IO客户端也不能连接到WebSocket服务器。请在[这里](https://github.com/socketio/socket.io-protocol)查看协议规范。
```javascript
// 警告：客户端不能连接！
const client = io('ws://echo.websocket.org');
```
## Socket.IO的安装
### 服务器
```javascript
npm install --save socket.io
```
[查看源码](https://github.com/socketio/socket.io)
### JavaScript客户端
默认在服务端中提供的/socket.io/socket.io.js文件。

也可以从某个CDN上获取，比如 [cdnjs](https://cdnjs.com/libraries/socket.io)

若果要在Node.js中使用，或者搭配像 [webpack](https://webpack.js.org/) 或者 [browserify](http://browserify.org/) 这样的工具包使用，可以通过npm来安装
```
npm install --save socket.io-client
```
[查看源码](https://github.com/socketio/socket.io-client)

### 其他的客户端实现
还有一些其他语言的客户端实现，它们是由社区维护的：
* Java: https://github.com/socketio/socket.io-client-java
* C++: https://github.com/socketio/socket.io-client-cpp
* Swift: https://github.com/socketio/socket.io-client-swift
* Dart: https://github.com/rikulo/socket.io-client-dart
* Python: https://github.com/invisibleroads/socketIO-client
* .Net: https://github.com/Quobject/SocketIoClientDotNet
## 与http模块搭配使用
### 服务器(app.js)
```javascript
var app = require('http').createServer(handler);
var io = require('socket.io')(app);
var fs = require('fs');

app.listen(80);

function handler(req, res) {
    fs.readFile(__dirname + '/index.html',
        function(err, data) {
            if(err) {
                res.writeHead(500);
                return res.end('Error loading index.html');
            }

            res.writeHead(200);
            res.end(data);
        }
    );
}

io.on('connection', function(socket) {
    socket.emit('news', { hello: 'world' });
    socket.on('my other event', function(data) {
        console.log(data);
    });
});
```
### 客户端(index.html)
```html
<script src="/socket.io/socket.io.js"></script>
<script>
    var socket = io('http://localhost');
    socket.on('news', function(data) {
        console.log(data);
        socket.emit('my other event', { my: 'data' });
    });
</script>
```
## 与express模块搭配使用
### 服务器(app.js)
```javascript
var app = require('express')();
var server = require('http').Server(app);
var io = require('socket.io')(server);

server.listen(80);
//注意: app.listen(80)在这里不可用

app.get('/', function(req, res) {
    res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket) {
    socket.emit('news', { hellp: 'world' });
    socket.on('my other event', function(data) {
        console.log(data);
    });
});
```
### 客户端(index.html)
```html
<script src="/socket.io/socket.io.js"></script>
<script>
    var socket = io.connect('http://localhost');
    socket.on('news', function(data) {
        console.log(data);
        socket.emit('my other event', { my: 'data' });
    });
</script>
```
## 发送和接收事件
Socket.IO允许你发送和接收自定义事件。除 ***connect*** 、***message*** 、***disconnect***之外，你可以发送自定义事件：
### 服务器
```javascript
//io(<端口>)会创建一个http服务器
var io = require('socket.io')(80);

io.on('connection', function(socket) {
    io.emit('this', { will: 'be received by everyone' });

    socket.on('private message', function(from, msg) {
        console.log('I received a private message by ', from, ' saying ', msg);
    });

    socket.on('disconnect', function() {
        io.emit('user disconnected');
    });
});
```
## Restricting yourself to a namespace(待翻译)
## Sending volatile messages(待翻译)
## 发送和获取数据(acknowledgements)(待翻译)
## 广播消息
如需广播消息，只需要在调用 ***emit*** 和 ***send*** 方法之前添加 ***broadcast*** 标记即可。广播的意思是向除发起广播之外的所有客户端发送消息。
### 服务器
```javascript
var io = require('socket.io')(80);

io.on('connection', function(socket) {
    socket.broadcast.emit('user connected');
});
```
## 作为跨浏览器的WebSocket使用它
当然你也可以只是用标准的WebSocket语法。只使用 ***send*** 方法并且监听 ***message*** 事件即可：
### 服务器(app.js)
```javascript
var io = require('socket.io')(80);

io.on('connection', function(socket) {
    socket.on('message', function() {});
    socket.on('disconnect', fuction() {});
});
```
### 客户端(index.html)
```javascript
<script>
    var socket = io('http://localhost/');
    socket.on('connect', function() {
        socket.send('hi');
        socket.on('message', function(msg) { });
    });
</script>
```
如果你不关心诸如重连逻辑之类的事情，可以看一下 [Engine.IO](https://github.com/socketio/engine.io)，这是Socket.IO使用的WebSocket语法的传输层。