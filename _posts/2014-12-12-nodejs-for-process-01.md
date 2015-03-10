---

layout: default
title: Node Js 多进程 ps
abstract: 
comments: false

---

ps_master.js

```javascript

var http = require('http'),
    numCPUs = require('os').cpus().length,
    cp = require('child_process'),
    net = require('net');

var workers = [];

function eachWorker(callback) {
    for (var idx in workers) {
        callback(workers[idx]);
    }
}

for (var i = 0; i < numCPUs; i++) {
    var worker = cp.fork('ps_worker.js');
    worker.on('message', function(msg){
        console.log('接收到消息:' + JSON.stringify(msg));
        if (msg.type == 'broadcast') {
            var from = msg.from;
            //发送给其他的子进程
            eachWorker(function (w) {
                console.log(w.pid);
                if (w.pid !== from) {
                    msg.type = 'send';
                    msg.to = w.pid;
                    w.send(msg);
                }
            });
            //发送给socket.io
            msg.type = 'send';
            msg.to = socket.pid;
            socket.send(msg);
        }
    });
    workers.push(worker);
}

//http请求
net.createServer(function (s) {
    s.pause();
    var worker = workers.shift();
    worker.send('c', s);
    workers.push(worker);
}).listen(3000);

//web socket请求
var socket = cp.fork('ps_socket.io.js');
net.createServer(function (s) {
    s.pause();
    socket.send('c', s);
}).listen(3001);

//主进程关闭后关闭所有子进程
process.on('exit', function(){
    eachWorker(function(w){
        w.kill();
    });
    socket.kill();
});


```

ps_worker.js

```javascript

var app = require('./app.js');
var server = require('http').createServer(app);

var io = require('socket.io')(server);
var battleIo = require('./models/BattleIo.js');
battleIo.setSocketIo(io);


process.on('message', function (msg) {
    process.nextTick(function () {
        if (msg.type == 'send' && msg.to == process.pid){
            console.log('接收到消息:' + JSON.stringify(msg));

            var data = msg.data;
            var method = data.method;
            var args = data.args || [];
            var a = args[0] ? args[0] : undefined;
            var b = args[1] ? args[1] : undefined;
            var c = args[2] ? args[2] : undefined;
            var d = args[3] ? args[3] : undefined;
            var e = args[4] ? args[4] : undefined;
            var f = args[5] ? args[5] : undefined;
            var g = args[6] ? args[6] : undefined;
            var h = args[7] ? args[7] : undefined;

            battleIo[method].call(battleIo, a, b, c, d, e, f, g, h);
        }
    });
});


process.on("message", function (msg, socket) {
    process.nextTick(function () {
        if (msg == 'c' && socket) {
            socket.readable = socket.writable = true;
            socket.resume();
            //server.connections++;
            //server.getConnections();
            socket.server = server;
            server.emit("connection", socket);
            socket.emit("connect");
        }
    });
});


```

ps_socketio.js

```javascript

var server = require('http').createServer(function (req, res) {
    res.end('这个是web socket服务');
});

var io = require('socket.io')(server);
var battleIo = require('./models/BattleIo.js');
battleIo.setSocketIo(io);

process.on('message', function (msg) {
    process.nextTick(function () {
        if (msg.type == 'send' && msg.to == process.pid){
            console.log('接收到消息:' + JSON.stringify(msg));

            var data = msg.data;
            var method = data.method;
            var args = data.args || [];
            var a = args[0] ? args[0] : undefined;
            var b = args[1] ? args[1] : undefined;
            var c = args[2] ? args[2] : undefined;
            var d = args[3] ? args[3] : undefined;
            var e = args[4] ? args[4] : undefined;
            var f = args[5] ? args[5] : undefined;
            var g = args[6] ? args[6] : undefined;
            var h = args[7] ? args[7] : undefined;

            battleIo[method].call(battleIo, a, b, c, d, e, f, g, h);
        }
    });
});

process.on("message", function (msg, socket) {
    process.nextTick(function () {
        if (msg == 'c' && socket) {
            socket.readable = socket.writable = true;
            socket.resume();
            //server.connections++;
            //server.getConnections();
            socket.server = server;
            server.emit("connection", socket);
            socket.emit("connect");
        }
    });
});


```

