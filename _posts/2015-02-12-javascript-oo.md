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

## 3. undefined

- 除了直接赋值和typeof()之外，其余任何对undefined的操作都将导致异常,如果要知道一个对象是不是undefined，只能采用typeof()的方法.
- undefined是一个已经实现的系统保留字,可以使用undefined来比较和运算.

```javascript

	var v;
	if(typeof(v) === 'undefined'){
		alert(typeof(v));
	}

	if(v === undefined){
		alert(v === undefined);
	}

	if(!v){
		alert(!v);
	}

```

## 4. void 运算符
- void运算符，对表达式求值，并返回undefined.在希望求表达式的值,但又不希望脚本剩余部分看到这个结果,该运算符最有用,

```javascript

	var v2 = void 12;
	alert(v2);

	var v3 = void function(){ return 13; }();
	alert(v3);

	var v4 = void(v3 = 4);
	alert(v4);

	var v5 = (v3 = 'v3');
	alert(v5);
	alert(v3);

```

## 5. number

- Number.MAX_VALUE：返回 JScript 能表达的最大的数。约等于 1.79E+308。
- Number.MIN_VALUE：返回 JScript 中能够表示的最接近零的数。约等于 2.22E-308。注意不是最小的数。
- 由于没有整形的缘故。可以使用parseInt()方法。
- NaN:表示算术表达式返回非数字值的特殊值。
- Infinity:返回比在js中能够表示的最大的数(Number.MAX_VALUE)更大的值。在数学运算中与正无穷大一样。
- isNaN: 返回一个 Boolean 值，指明提供的值是否是保留值 NaN （不是数字)

## 6. boolean (略)

## 7. string
- link():把一个有 HREF 属性的 HTML 锚点放置在 String 对象中的文本两端。
- big():把 HTML <BIG> 标记放置在 String 对象中的文本两端。
- 另外类似的方法还有：anchor(), blink(), bold(), fixed(), fontcolor(),fontsize(), italics(), small(), strike(), sub(), sup()

```javascript
	
	var s1 = 'link';
	document.write(s1.link("http://changhu2013.github.io/changhu"));

	var s2 = new Object();
	s2.name = 'changhu';
	s2.age = 28;
	
	document.writeln(s2);
	document.writeln(s2.toString());

	s2.toString = function(){
		return 'name :' + this.name + ' age:' + this.age;
	}

	document.writeln(s2.toString());

```

