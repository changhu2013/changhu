---

layout: default
title: Node Js 多进程 pm
abstract: 
comments: false

---


pm_master.js


```javascript


var app = require('pm').createMaster({
    pidfile : 'bench.pid',
    statusfile : 'status.log',
    heartbeat_interval : 2000
});

app.on('giveup', function(name, fatals, pause){
    console.log('Master giveup to restart "%s" process after %d times. pm will try after %d ms.', name, fatals, pause);
});

app.on('disconnect', function (worker, pid) {
    // var w = cluster.fork();
    console.error('[%s] [master:%s] wroker:%s disconnect! new worker:%s fork',
        new Date(), process.pid, worker.process.pid); //, w.process.pid);
});

app.on('fork', function () {
    console.log('fork', arguments);
});

app.on('quit', function () {
    console.log('quit', arguments);
});


//除socket.io以外的其他所有服务
var cpus = require('os').cpus().length;

if(cpus >= 2){
    app.register('http', __dirname + '/pm_worker.js', {
        listen : 3000,           //主进程绑定的端口
        children : cpus - 1   //主机有多少CPU则启动多少子进程
    });
    //用于socket.io
    app.register('socket.io', __dirname + '/pm_socket.io.js', {
        listen : 3001,           //主进程绑定的端口
        children : 1   //主机有多少CPU则启动多少子进程
    });
} else {
    //用于所有服务
    app.register('http', __dirname + '/pm_socket.io.js', {
        listen : 3001,           //主进程绑定的端口
        children : 1             //主机有多少CPU则启动多少子进程
    });
}

app.dispatch();


```


pm_worker.js


```javascript


var app = require('./app.js');
var http = require('http').createServer(app);


//以下注释内容为使用pm管理进程的代码,不支持windows系统
var worker = require('pm').createWorker();
var battleIo = require('./models/BattleIo.js');
battleIo.setWorker(worker);
worker.on('message', function(msg, from, pid){
    console.log('接收到socket.io服务发送来的消息 ' + from + ' ' + pid);
    console.log(msg);

    var method = msg.method;
    var args = msg.args;
    battleIo[method].call(battleIo, args[0], args[1], args[2], args[3], args[4], args[5], args[6]);
});
worker.ready(function(socket, port) {
    http.emit('connection', socket);
});
worker.on('suicide', function (by) {
    console.log('suicide by ' + by);
});


```


pm_socketio.js


```javascript


var http = require('http').createServer(function(req, res){
    res.end('这个是web socket服务');
});

var worker = require('pm').createWorker();

var io = require('socket.io')(http);
var battleIo = require('./models/BattleIo.js');

battleIo.setSocketIo(io);

worker.on('message', function(msg, from, pid){
    console.log('接收到http服务发送来的消息 ' + from + '   ' + pid);
    console.log(msg);

    var method = msg.method;
    var args = msg.args;
    battleIo[method].call(battleIo, args[0], args[1], args[2], args[3], args[4], args[5], args[6]);
});

worker.ready(function(socket, port) {
    http.emit('connection', socket);
});

worker.on('suicide', function (by) {
    console.log('suicide by ' + by);
});


```
