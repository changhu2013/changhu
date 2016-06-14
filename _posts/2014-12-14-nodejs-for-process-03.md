---

layout: default
title: Node Js 多进程 pp
abstract: 
comments: false

---


pp_master.js


```javascript


var config = require('./config'),
    http = require('http'),
    numCPUs = require('os').cpus().length,
    cp = require('child_process'),
    net = require('net'),
    http = require('http'),
    httpProxy = require('http-proxy');

//---------------------------------------------------------
var workers = [];
var proxys = [];
var clearWorkers = function (pid) {
    for (var i = 0; i < workers.length; i++) {
        if (workers[i].pid == pid) {
            workers.splice(i, 1);
        }
    }
};
var clearProxys = function (pid) {
    for (var i = 0; i < proxys.length; i++) {
        if (proxys[i].pid == pid) {
            proxys.splice(i, 1);
        }
    }
};
var createWorker = function () {
    var worker = cp.fork('pp_worker.js');
    //当接收到子进程启动成功的消息后,启动代理服务
    worker.on('message', function (message) {
        if (message.act === 'ready') {
            var port = message.port;
            var proxy = httpProxy.createProxyServer({
                target: {
                    host: 'localhost',
                    port: port
                }
            });
            proxy.pid = worker.pid;
            proxy.on('error', function(){
                var pid = proxy.pid;
                console.log('http代理服务:' + pid + '发生错误');
                clearWorkers(pid);
                clearProxys(pid);
                createWorker();
            });
            proxys.push(proxy);
            console.log('启动http代理服务,端口:' + port + ' pid :' + worker.pid);
        }
    });
    //当接收到自杀信号的时候重启新的进程
    worker.on('message', function (message) {
        if (message.act === 'suicide') {
            var pid = worker.pid;
            console.log('http服务:' + pid + '宕机了');
            clearWorkers(pid);
            clearProxys(pid);
            createWorker();
        }
    });

    //退出时重启新的进程
    worker.on('exit', function () {
        var pid = worker.pid;
        console.log('http服务:' + pid + '退出了');
        clearWorkers(pid);
        clearProxys(pid);
        createWorker();
    });

    //退出时重启新的进程
    worker.on('error', function () {
        var pid = worker.pid;
        console.log('http服务e:' + pid + '退出了');
        clearWorkers(pid);
        clearProxys(pid);
        createWorker();
    });

    workers.push(worker);
};
//根据CPU数量创建子进程
for (var i = 0; i < numCPUs; i++) {
    createWorker();
}

//http服务
var port = config.port;
http.createServer(function (req, res) {
    var proxy = proxys.shift();
    proxy.web(req, res);
    proxys.push(proxy);
}).listen(port);

//---------------------------------------------------------
//web socket服务，其端口为http服务端口 + 1
var webSocket = http.createServer(function (req, res) {
    res.end('这个是web socket服务');
});
require('./socket.io.js')(webSocket);
webSocket.listen(port + 1);
console.log('启动web socket服务');
//---------------------------------------------------------

//主进程关闭后关闭所有子进程
process.on('exit', function () {
    for (var i = 0; i < workers.length; i++) {
        var worker = workers[i];
        console.log('关闭http服务:' + worker.pid);
        worker.kill();
    }
    proxys = [];
});



```


pp_worker.js


```javascript



var portfinder = require('portfinder');

var app = require('./app.js');
var server = require('http').createServer(app);

portfinder.getPort({
    port : 9000
}, function(err, port){
    server.listen(port);
    console.log('启动http服务,端口:' + port);
    process.send({
        act:'ready',
        port : port
    });
});

process.on('uncaughtException', function(err){

    //记录日志
    console.log(err);

    //向主进程发送自杀信号
    process.send({act:'suicide'});

    //停止接收新的连接
    server.close(function(){
        //所有连接断开后,退出进程
        process.exit(1);
    });

    //5秒后退出进程, 当连接的是长连接时，设置定时退出
    setTimeout(function(){
        process.exit(1);
    }, 5000);
});


```

