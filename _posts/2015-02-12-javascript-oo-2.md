---

layout: default
title: Javascript 面向对象编程（第二部分）
abstract: 本文将详细讲解Javascript语言面向对象编程的一些特性
comments: true

---

## 1. object

- object 被包含在所有其他的JS对象中
- object 是一个无序属性的集合,每个属性都有自己的key和value
- 基于原型prototype继承

## 1.1 object对象定义的属性和方法

- constructor属性 引用用于初始化该类的构造函数 == 构造函数.property.constructor
- toString() 返回对象的字符串表示，想调用默认的toString()方法Objct.property.toString.apply(o)
- toLocalString() 返回对象的本字符串表示
- valueOf() 返回对象的数字表示,比toString的优先级高
- hasOwnProperty(property) 判断对象是否有某个特定属性(非继承属性)
- propertyIsEnumberable(property) 对象是否可被枚举(for/in)
- isPropertyOf(object) 判断对象是否是另一个对象的原型

1. new
```javascript

	var o1 = new Object({name:'changhu'})

	o1.age = 28;

	for(var v in o1){
		alert(v + ' : ' + o1[v]);
	}

``` 

2. 对象初始化方法

```javascript

	var o2 = {};
	o2.name = 'changhu';
	o2.age = 28;
	
	var o3 = {
		name : 'changhu',
		age : 28
	};

```

3. 编写一个构造函数,通过new方法来创建对象

```javascript

	function Person(){
		this.name = 'changhu';
		this.age = 28;
	}

	var p = new Person();
	p.name = 'tiger';
	p.age = 25;

```

可以定义私有属性，实例属性，和类属性

- 私有属性：只能在构造函数内部定义和使用
- 实例成员：必须在对象实例化后才能使用
- 类属性：只能通过类名使用

```javascript

	function User(name){
		this.name = name; //实例属性
		var age = 0; //私有属性
		User.prototype.ID = '000001'; //实例属性
	}
	
	User.country = 'china';//类属性

	alert(User.country);	
	alert(User.name);
	
	var u1 = new User('changhu');
	alert(u1.name + ' ' + u1.ID);

```

- 私有方法 ： 只能在函数内部使用
- 实例方法 ： 必须在对象实例化才能使用，语法和对象属性相同
- 类方法 ： 可以直接通过类名调用

```javascript

	function Lover(){
		//私有方法
		var loveU = function(){
			alert('love u');
		}
		//实例方法
		this.loveMe = function(){
			alert('love me');
		}
		//实例方法
		Love.prototype.loveShe = function(){
			alert('miss she');
		}
	}

	//类方法
	Lover.loveHe = function(){
		alert('love him');
	}

	Lover.loveHe();
	var l1 = new Lover();
	l1.loveMe();
	l1.loveShe();

```

<table style="border:1px solid #ccc;">
  <tr style="font-weight:bold;font-size:20px;">
	<td width="50px"></td>
	<td width="50px">可见性</td>
	<td width="80px">定义位置</td>
	<td width="100px">语法</td>
	<td width="100px">引用方式</td>
  </tr>
  <tr>
    <td>私有成员</td>
    <td>对象内部</td>
    <td>构造器内</td>
    <td>在构造器内定义的var变量和function函数属于私有变量</td>
    <td>对象内部直接引用</td>
  </tr>
  <tr>
	<td>实例成员</td>
    <td>全局</td>
    <td>构造器内外均可</td>
    <td>prototype方法和this方法</td>
    <td>对象实例化后引用</td>
  </tr>
  <tr>
	<td>类成员</td>
    <td>全局</td>
    <td>构造器外</td>
    <td>funcName.property = value; funcName.methodName = method / func</td>
    <td>直接通过构造函数名引用</td>
  </tr>
	
</table>


动态删除属性和方法

- delete obj.propertyName;
- delete obj.mehodName;

```javascript

	var user = new User('gaga');
	
	delete user.name;
	alert(user.name);//已无此属性

	var l2 = new Lover();
	delete l2.loveMe();
	l2.loveMe(); //已无此方法 

```

## 致谢
感谢您的阅读，如果其中有任何错误或疏漏，请发送消息给我(微博：@changhu_)，谢谢。