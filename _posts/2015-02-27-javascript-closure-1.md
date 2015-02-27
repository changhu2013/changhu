---

layout: default
title: Javascript 闭包(Closure)
abstract: 
comments: false

---

## 1. 闭包


- 函数闭包(Function Closure)

_在内部保存数据和对外无副作用_

- 对象闭包(Object Closure)

with语句实现直接相关的一种闭包

## 2. 闭包与函数实例

- 函数：只是一段静态的代码，脚本文本，因为它是一个代码书写时，以及编译期的、静态的概念。 

- 闭包则是函数的代码在运行过程中的一个动态环境，是一个运行期的、动态的概念。引擎对每个函数建立其独立的上下文环境，因此当函数被再次执行或通过某种方法进入函数体内时，就可以得到闭包内的全部信息。


## 3. 什么是闭包

### 闭包的两个特点：

- 闭包作为与函数成对的数据，在函数执行过程中处于激活(即可访问)状态

- 闭包在函数运行结束后，保持运行过程的最终数据状态

因此函数的闭包总的来说决定两件事：

_闭包所对应的函数代码如何访问数据，以及闭包内的数据何时销毁, 对于前者来说，涉及作用域(可见性)的问题；对于后者来说，涉及数据引用的问题_


```javascript

function MyFunc(){// 闭包 0 开始
  
  var data_1 = 123;
  var data_2 = new Array();

  function func_1(){//闭包 1 开始
     
	//...
  
  }//闭包 1 结束
  
  function func_2(){ //闭包 n 开始
    
	//...

  }//闭包 n 结束

}// 闭包 0 结束


```

_上面的代码仅能从静态的视觉效果上说明闭包、子函数闭包、upvalue之间的关系_ 


- 在运行过程中，子函数闭包(闭包1和闭包n)可以访问upvalue（data_1和data_2）

- 同一个函数中的所有子函数(闭包1和闭包n)访问一份相同的值upvalue


```javascript

function MyFunc(){
   
  var data = 100;

  function func_1(){
    data = data * 5;
  }

  function func_n(){
    
    alert(data);

  }
  
  func_1();
  func_n();
} 

//由于func_1和func_n使用相同的upvalue变量data，因此在func_n中可以显示func_1对该值的修改，返回结果为500

MyFunc();

```








