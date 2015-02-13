---

layout: default
title: Javascript 面向对象编程
abstract: 本文将详细讲解Javascript语言面向对象编程的一些特性
comments: true

---

## 1. 数据类型

JS是弱类型的，内置类型简单且清晰

- undefined
- number
- boolean
- string
- function
- object 


## 2. type instanceof & constructor

- typeof 返回值有六种可能 "number", "string", "boolean", "object", "function", "undefined" _注意这里都是字符串_
- constructor 表示创建对象的函数
- instanceof 返回一个Boolean值，指出对象是否特定类的一个实例

```javascript
	
	var a = 12;
	alert(typeof a);
	alert(typeof(a));

```

执行上面的代码
<input type="button" onClick="f1()" value="点我" />
<script type="text/javascript">
function f1(){
   var a = 12;
   alert(typeof a);
   alert(typeof(a));
}
</script>


```javascript

	function Person(age){
		this.age = age;	
		this.getAge = function(){
			alert(this.age);
		}
	}
	
	var p = new Persion(28);
	
	(p.constructor === Person)?p.getAge():null;

	alert(p instanceof Person);

```

执行上面的代码
<input type="button" onClick="f2()" value="点我"/>
<script type="text/javascript">
function Person(age){this.age = age;this.getAge = function(){alert(this.age);}}
function f2(){
var p = new Person(28);
(p.constructor === Person)?p.getAge():null;
alert(p instanceof Person);
}
</script>

