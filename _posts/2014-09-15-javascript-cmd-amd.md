---

layout: default
title: Javascript CMD & AMD
abstract: 
comments: false

---

## 1. AMD & Require JS

- AMD (Asynchronous Module Definition) 异步模块定义，模块加载不影响后续语句执行，所有依赖某些模块的语句均放在回调函数中
- AMD 定义了一个自由变量或全局变量define的函数

```javascript 

	define(id?, dependencies? factory)

```

[AMD 规范](https://github.com/amdjs/amdjs-api/wiki/AMD)

- id 字符串类型，表示模块标识，可选参数

- dependencies 是一个当前模块依赖，已被模块定义的模块标识的数组自变量

- factory 是一个需要实例化的函数或对象

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