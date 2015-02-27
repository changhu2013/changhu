---

layout: default
title: Angular JS
abstract : 一个牛X的前端框架
comments: true

---

## 什么是Angular JS

### 1. Hello World

<div ng-app>
请输入您的名字:<input type="text" ng-model="name" />
性别:
<input value="先生" ng-model="gender" type="radio">男
<input value="女士" ng-model="gender" type="radio">女

你好:<span ng-bind="name"></span><span ng-bind="gender"></span><div>

上面这个输入框的实现代码如下：

```html
	<div ng-app>
		请输入您的名字:<input type="text" ng-model="name" />
		性别:<input value="先生" ng-model="gender" type="radio">男 <input value="女士" ng-model="gender" type="radio">女
		你好:<span ng-bind="name"></span><span ng-bind="gender"></span><div>
	<div>
	<script src="{{site.baseurl}}/javascripts/angular.js"></script>
```
只需要如此简单的几行代码就实现了一个看似很复杂的功能，是不是很牛X


<script src="{{site.baseurl}}/javascripts/angular.js"></script>


### 2. Angular Js的特点

1. 使用html作为模板语言
2. 无需对DOM进行显示刷新，因为Angular JS可以通过用户动作、浏览器时间、模型(model)变化来决在何时来刷新何类模板。
3. 有趣的和可扩展的组件子系统能够教会浏览器如何解释自定义的html标签及属性。
4. 依赖注入、可测试性。

### 3. Angular Js的MVC模式

Angular Js中所有能被框架理解和解释的特殊的Html标签和属性,统称为指令(directives)

---
### 资料
1. [Angular GitHub](https://github.com/angular/angular)
2. [AngularJS培训PPT](https://github.com/changhu2013/resume/raw/master/resume/ppt/AngularJS培训-changhu-v1.ppt)
