### js 闭包是什么？常用的闭包例子
**什么是闭包？**
闭包是指有权访问另外一个函数作用域中的变量的函数，可以理解为：能够读取其他函数内部变量的函数
闭包的作用：正常函数执行完毕后，里面声明的变量被垃圾回收处理掉，但是闭包可以让作用域里的 变量，在函数执行完之后依旧保持没有被垃圾回收处理掉。

闭包的实例：
```js
function addCount() {
	let count = 0
	return function() {
		++count
		console.log(count)
	}
}
let foo = addCount();
foo() // 1
foo() // 2
```

过程：addCount() 执行的时候，返回一个函数，函数是可以创建自己的作用域的，但是此时返回的这个函数内部需要引用 addCount() 作用域下的变量 count，因为这个 count 是不能被销毁的。


### js 内存泄露，如何防止？
[https://segmentfault.com/a/1190000020114344](https://segmentfault.com/a/1190000020114344)
1. 全局变量
```js
function func() {
	name = 'text' // 实际上是 window.name = 'text'
}
```
解决办法：使用严格模式，use strict
2. 未销毁的定时器和回调函数
```js
setInterval(function() {
	let render = document.getElementById('id');
	if (render) {
		render.innerHTML = 'html'
	}
}, 3000)
```
解决办法：在定时器完成工作的时候，手机清除定时器
3. 闭包
```js
let obj = function() {
	let original = thing
	thing = {
		longStr: new Array(10000).join(''),
		someMethod: function() {
			console.log(original)
		}
	}
}
```
解决办法：闭包本身没有错，不会引起内存泄漏，而是使用错误导致的
4. DOM 引用
```js
let ele = {
	image: document.getElementById('image')
}
function doStuff() {
	ele.image.src= 'https://url/image.jpg'
}
function removeImage() {
	ele.body.removeChild(document.getElementById('image'))
}
```
解决办法： image = null

### 定时器的使用 (ES5和ES6对循环的不同写法？)
```js
for (let i = 0; i< 4; i++) {
	setTimeout(function() {
		console.log(i)
	}, 300)
} // 4
```
过程：js 执行的时候首先`会先执行主线程`，异步组件相关的会存到异步队列里，当主线程执行完毕开始执行异步队列，主线程执行完毕后，此时 i 的值为4，所以在执行异步队列队列的时候，打死出来的都是4，这里需要大家对 event loop有所了解。（js的事件循环机制）

**如何修改使其正常打印（使用闭包使其正常打印）**
方法一：
```js
for (let i =0; i<4;i++) {
	setTimeout((function(i) {
		return function () {
			console.log(i)
		}
	})(i), 300)
}

//或者：

for (let i =0;i<4;i++) {
	setTimeout((function() {
		let temp = i
		return function() {
			console.log(temp)
		}
	})(),300)
}

```
方法二：
通过创建一个自执行函数，使变量存在这个自执行函数的作用域里
```js
for (let i=0;i<4;i++) {
	(function(i) {
		setTimeout(function() {
			console.log(i)
		},300)
	})(i)
}
```
闭包的缺陷：
通过上面的例子也发现，闭包会导致内存占用过高，因为变量都没有释放内存。

### promise异步请求执行顺序(先主线程，再异步队列)
### js 原型链
**什么是原型链？**
每个对象都可以有一个原型 _proto_，这个原型还可以有它自己的原型，以此类推，形成一个原型链。
查找特定属性的时候，我们先去这个对象里找，如果没有的话就去它的原型对象里面去找，如果不是没有的话，再去向原型对象的原型对象里去找......这个操作被委托在整个原型链上，这个就是我们说的原型链了。

1. _proto_是原型链查询中实际用到的，它问题指向 Prototype
2. prototype 是函数所独有的，在定义构造函数时自动创建，它总是被 _proto_ 所指


所有对象都有 _proto_ 属性，函数这个特殊对象除了具有 _proto_ 属性，还有特有的原型属性 prototype。
prototype 对象默认有两个属性，constructor 属性和 _proto_ 属性。 prototype 属性可以给函数和对象添加可共享（继承）的方法、属性，而 _proto_ 是查找某函数或对象的原型链方式。
