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
- 单词必须为驼峰形式，或 ".", ".."
- 模块名不允许文件扩展名的形式，如 ".js"
- 模块名可以为"相对的"或"顶级的"，如果首字符为 "." 或 ".." 则为相对的模块名
- 顶级的模块名从根命名空间的概念模块解析
- 相对的模块名从"require"书写和调用的模块解析


相对模块名解析示例：

- 如果模块 a/b/c 请求 ../d 则解析为 a/d

- 如果模块 a/b/c 请求 ./e  则解析为 a/b/e


### dependencies 依赖

- 定义模块所依赖的模块的数组，依赖的模块必须根据模块的工厂方法优先级执行，并且执行的结果应该按照依赖数组中的位置顺序以参数的形式传入工厂方法中。

- 依赖的模块名如果是相对的，应该解析为相对定义中的模块。换句话来说，相对名解析为相对于模块的名字，并非相对于寻找该模块的名字的路径。

- 本规范定义了三种特殊的依赖关键字。如果"require","exports", 或 "module"出现在依赖列表中，参数应该按照CommonJS模块规范自由变量去解析。

- 依赖参数是可选的，如果忽略此参数，它应该默认为["require", "exports", "module"]。然而，如果工厂方法的形参个数小于3，加载器会选择以函数指定的参数个数调用工厂方法。


### factory 工厂方法

- 为模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值

- 如果工厂方法返回一个值（对象，函数，或任意强制类型转换为true的值），应该为设置为模块的输出值。




创建一个名为"alpha"的模块，使用了require，exports，和名为"beta"的模块:

```javascript

define('alpha', ['require', 'exports', 'beta'], function(require, exports, beta){
		
  exports.verb = function(){
		
    return beta.verb();

    // OR

    return require('beta').verb();

  };

});

```

一个返回对象的匿名模块

```javascript

define(["alpha"], function (alpha) {
  return {
    verb: function(){
      return alpha.verb() + 2;
    }
  };
});


```

一个没有依赖性的模块可以直接定义对象：

```javascript

define({
   add: function(x, y){
      return x + y;
   }
});

```

一个使用了简单CommonJS转换的模块定义：

```javascript

define(function (require, exports, module) {
    
    var a = require('a'),
    b = require('b');

    exports.action = function () {};

});


```

### require();

[require API](https://github.com/amdjs/amdjs-api/wiki/require)


```javascript

define(['require'], function (require) {

    //the require in here is a local require.

});

define(function (require, exports, module) {

    //the require in here is a local require.

});


define(function (require) {
    
    require(['a', 'b'], function (a, b) {

        //modules a and b are now available for use.

    });

});


//cart.js contents:
define (function(require) {
        
	//module ID part is './templates/a'
        
	//'.extension is '.html'

	//templatePath may end up to be something like
	
	//'modules/cart/templates/a.html'
	
	var templatePath = require.toUrl('./templates/a.html');

});


```
