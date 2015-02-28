---

layout: default
title: Javascript CMD & AMD
abstract: 
comments: false

---

## 1. AMD & Require JS

- AMD (Asynchronous Module Definition) 异步模块定义，模块加载不影响后续语句执行，所有依赖某些模块的语句均放在回调函数中

- AMD 只定义了全局变量define的函数

```javascript 

	define(id?, dependencies? factory)

```

[AMD 规范](https://github.com/amdjs/amdjs-api/wiki/AMD)

### id 

字符串类型，表示模块标识，可选参数。 如果没有，模块的名字应该默认为模块加载器请求的指定脚本的名字。如果提供了，模块名必须是“顶级”的和绝对的(不允许相对名字)

模块名字的命名规范
- 模块名是由一个或多个单词以正斜杠为分隔符拼接成的字符串
- 单词必须为驼峰形式， 或 ".", ".."
- 模块名不允许文件扩展名的形式，如 ".js"
- 模块名可以为"相对的"或"顶级的"，如果首字符为 "." 或 ".." 则为相对的模块名
- 顶级的模块名从根命名空间的概念模块解析
- 相对的模块名从"require"书写和调用的模块解析


相对模块名解析示例：

- 如果模块 a/b/c 请求 ../d 则解析为 a/d

- 如果模块 a/b/c 请求 ./e  则解析为 a/b/e



### dependencies 

是一个当前模块依赖，已被模块定义的模块标识的数组自变量

### factory 

是一个需要实例化的函数或对象


创建标识为 alpha 的模块，依赖 require, export, 和标识为 beta 的模块

```javascript

define('alpha', ['require', 'export', 'beta'], function(require, export, beta){
		
  export.verb = function(){
		
    return beta.verb();

    // OR

    return require('beta').verb();

  };

});

```