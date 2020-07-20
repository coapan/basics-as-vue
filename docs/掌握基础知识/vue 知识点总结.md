### 什么是 MVVM？
MVVM是 Model-View-ViewModel 的缩写，是一种设计思想。
Model层代表数据模型。也可以在Model中定义数据修改和操作的业务逻辑。
View代表用户的操作界面，
ViewModel 监听模型数据的改变和控制视图行为，处理用户交互，是一个同步View和Model的对象。
viewModel和Model实现数据的双向绑定

### vuex是什么？哪种功能场景使用它？
Vuex是专门为vue.js设计的状态管理模式，可以帮助我们管理共享状态，也就是管理全局变量

### vuex 有哪几种属性
- state: 基本数据
- getters: 从基本数据派生的数据，类似于组件中的 computed
- mutations:是vuex中改变status的唯一途径，且只能是通过同步的方式操作
- actions: 一对对status的异步操作可以放在action中
- modules: 当store对象过于庞大时，模块化 vuex

### 如何让 css 在单文件只在当前组件中起作用？
`<style scoped></style>`

### 请列举出 vue常见的生命周期钩子函数
开始创建、初始化数据、编译模板加载渲染函数、挂载DOM、更新前数据改变页面未改变、更新完成后
beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, boforeDestro, destroyed

**第一次加载会触发哪个钩子？**
beforeCreate, created, beforemount, mounted

**DOM渲染在哪个周期中就已经完成**
在 mounted 中就已经完成了

### Vue 实现数据双向绑定原理
1. 父组件与子组件传值
采用数据支持结合发布者-订阅者模式的方式，通过 `Obecjt.defineProperty()` 来支持各个属性的 setter, getter, 在数据变动时发布消息给订阅者，触发相应监听回调。
2. 非父子组件间的数据传递，兄弟组件传值
eventBus, 就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。或者直接使用vuex（项目很大时）

### Vue 组件间的参数传递
父组件给子组件传参：子组件通过 props 方法接受参数
子组件给父组件传递：$emit('func')方法传递参数，父组件通过 v-on:func=""来接受参数

### 请说出至少4种vue当中的指令和它的用法？
- 文本插值：{{ message }}
- v-html: render html tag,
- v-text: render text
- v-bind: bind data
- v-once: bind only once
- v-model: 输入数据和应用状态间的双向绑定
- v-on：绑定一个事件监听器
- v-if v-show: 条件渲染

### 如何解决 Vue 中的 ajax 中跨域问题？
```text
	'/api': { // 使用 /api 来 代替 http://localhsot:8080
		target: 'http://localhost:8080', // 源地址
		changeOrigin: true, // 改变源
		pathRewrite: {
			'^/api': 'http://localhost:8080' // 路径重写
		}
	}
```
修改完之后的跨域请求就可以直接写成 /api/login 等

### Vue 如何优化首屏加载速度？
- 异步路由加载
- 不打包库文件
- 关闭 sourcemap
- 开户 gzip 压缩

### axios 是什么？怎么使用？
axios是一个基于 promise 的HTTP库，可以get post请求等。

**Promise是什么？**
promise是一个对象用来传递异步操作的信息
promise的出现主要是解决地狱回调的问题，无需多次嵌套
**如何使用？**
`npm install --save axios`

```javascript
	import Axios from 'axios'
	Vue.propertype.$axios = Axios

    //使用 axios 发送请求：
    
	this.$axios.get('/user?id=100').then(function (res) {
		consloe.log(res)
	}).catch(err => {
		console.log(res)
	})
```
**拦截器**
- 请求拦截器
```js
	axios.interceptors.request.use(function(config) {
		return config;
	}, function (error) {
		return Promise.reject(error)
	})
```
- 响应拦截器
```js
	axios.interceptors.response.use(function(response) {
		return response;
	}, function(error) {
		return Promise.reject(error)
	});
```

### vue中的路由拦截器的作用？
例：当用户没有登录权限的时候就会跳转到登录页面，用到的字段：requireAuth:true

### Http 请求中常见的请求方式
- get 向特定的路径资源发出请求，数据暴露在url中）（获取数据）
- post 向指定路径资源提交数据进行处理请求，数据包含在请求体中（发送数据）
- options 返回服务器针对特定资源所支持的http请求方法，允许客户端查看，测试服务器性能
- head 向服务器与get请求相一致的响应，响应体不会返回
- put 从客户端向服务器端传送的数据取代指定的文档的内容（修改数据）
- 
- delete 请求服务器删除指定的内容（删除）
- trace 回显服务器收到的请求，主要用于测试或者诊断
- connect 

### Vue 的路由实现：hash 模式和 history 模式
**hash模式**
在浏览器中符号 #，#以及#后面的字符称之为hash，用 `window.location.hash` 读取
**history模式**
hostory采用HTML5新特性，且提供两个新方法：pushState(), replaceState() 可以对浏览器历史记录栈进行修改，以及 popState()事件的监听到状态变更。

### Vue 的路由钩子函数
- beforeEach 主要有3个参数 to、from、next
	* to：route 即将进入的目标路由对象
	* from： route 当前导航正要离开的路由
	* next: function 一定要调用该方法 resolve 这个钩子。执行效果依赖 next 方法的调用参数。可以控制网页的跳转。


### Vue-cli 如何新增自定义指令
1. 创建局部指令
```javascript
var app = new Vue({
	el: '#app',
	data:{},
	// 创建指令（可以多个）
	directives: {
		// 指定名称
		dir1: {
			inserted(el) {
				// 指令中第一个参数是当前使用指令的DOM
				console.log(el)
				console.log(arguments)
				// 对DOM进行操作
				el.style.width='200px'
				el.style.height='200px'
				el.style.background='#000'
			}
		}
	}
})
```
2. 创建全局指命令
```js
	Vue.directive('dir2', {
		inserted(el) {
			console.log(el)
		}
	})
```
3. 指令的使用
```html
	<div id="app">
		<div v-dir1></div>
		<div v-dir1></div>
	</div>
```

### Vue 中如何定义一个过滤器
```html
	<div id="app">
		<input type="text" v-model="msg" />
		{{msg|capitalize}}
	</div>
```
```js
	var vm = new Vue({
		el: "#app",
		data() {
			msg: ''
		},
		// 局部定义方式
		filters: {
			capitalize: function (value) {
				if (!value) return ''
				value = value.toString()
				return value.chatAt(0).toUpperCase() +value.slice(1)
			}
		}
	})
	
	// 全局定义方式
	Vue.filter('capitalize', function (value) {
		if (!value) return ''
		value = value.toString()
		return value.chatAt(0).toUpperCase() + value.slice(1)
	})
```
过滤器接收表达式的值 msg 作为第一个参数，capitalize 过滤器将会收到 msg的值作为第一个参数

### 对 keep-alive 的了解？
keep-alive是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染
在 Vue 2.1.0 版本之后， keep-alive 新加入了两个属性， include(包含组件缓存)、exclude(排除的组件不缓存，优先级高于include)
使用方法：
```html
<keep-alive include="include_components" exclude="exclude_components">
	<component>
	 // 该组件是否缓存取决于 include 和 exclude 属性
	</component>
</keep-alive>
```
参数解释：
- include 字符串或正则表达式，只有名称匹配的组件会被缓存
- exclude 字符串或正则表达式，任何名称匹配的组件都不会被缓存
- include 和 exclude 的属性允许组件有条件的缓存。二者都可以用 `，`分隔字符串、正则表达式、数组。当使用正则或者是数组时，要记得使用 v-bind

使用示例：
```html
// 逗号分隔字符串，只有组件a和b会被缓存
<keep-alive include="a,b">
	<component></component>
</keep-alive>

// 正则表达式，需要使用 v-bind 符合匹配规则的都会被缓存
<keep-alive :include="/a|b/">
</keep-alive>

// 数组，需要使用 v-bind，被包含的都会被缓存
<keep-alive :include="['a', 'b']">
</keep-alive>
```

### v-if 和 v-show 的区别
v-if 按照条件是否渲染，v-show是 display 的 block或none，无论何时都渲染。
v-if适用于数据不经常变动的时候使用，v-show适用于条件经常变动时使用

### 怎么定义 vue-router 的动态路由？怎么获取传过来的参数值
在 router 目录下的 index.js 文件中，对 path 属性加上 /:id
使用 router 对象的 params.id 获取
示例：
```html
	<router-link :to="{name:'filter', params: {id: 5}}">过滤器</router-link>
```
```js
	console.log(`传送过来的参数ID为：${this.$route.params.id}`);
```
### Vue 常用的修饰符有哪些？
- .prevent 提交事件不再重载页面
- .stop 阻止单击事件冒泡
- .self 当事件发生在该元素本身而不是子元素的时候会触发
- .capture 事件侦听，事件发生的时候会调用

### vue.js 的两个核心是什么？
数据驱动、组件系统

### Vue 中 key 值的作用？
当 vue用 v-for 正在更新已渲染过的元素列表时，它默认用 “就地复用”策略。如果数据项的顺序被改变，vue 将不会移动 DOM 元素来匹配数据项的顺序，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟 DOM

### Vue 中的深拷贝和浅拷贝
**如何区分深拷贝与浅拷贝？**
假设B复制了A，当修改A时，看B是否会变化，如果B也跟着变了，说明这是浅拷贝。如果 B 没变，那就是深拷贝。
普通的变量赋值一般都是浅拷贝：
```js
	let a = 1
	let b = a // 对b来说就是浅拷贝
```
来看一个例子：
```js
export default {
	date() {
		return {
			list: [1,2,3]
		}
	},
	created() {
		let item = this.list
		console.log(item) // 这里就是 list 的值
		
		this.list[0] = 666 // 修改list[0]的值
		console.log(item) // 这时候发现 item 的值也发生了改变，这就是浅拷贝
	}
}

//深拷贝：
	let item = JSON.parse(JSON.stringify(this.list[0]))
```
这样你不管怎么处理复制的变量，都不会影响到原数组的结构。
这里有一篇文章讲得特别的好：
[https://blog.csdn.net/chshyuxyz/article/details/102721464](https://blog.csdn.net/chshyuxyz/article/details/102721464)
**实现深拷贝的方法？**
1. 通过递归原求对象的方式
2. 通过 JSON.parse(JSON.stringify())
3. 通过 jQuery 的 extend 方法。 $.extend([deep], target, object1[, objectN])
deep 表示是否深拷贝，true为深拷贝，false为浅拷贝
target Obejct类型，目标对象，其他对象的成员属性将被附加到该对象上
object1 objectN 可选。 Object类型，第一个以及第N个被合并的对象
```js
	let a = [0, 1,2]
	b = $.extend(true, [], a)
	a[0] = 666
	a[2][0] = 2
	console.log(a,b)
```

### Vue 组件间是如何进行通讯的
1. props/$emit
父组件 A 通过 props 的方式向子欣组件 B 传递，B to A 通过在 B 组件中 $emit('func')，A 组件中通过 v-on:func 或 @func 的方式实现
2. $emit/$on
这种方法通过一个空的 Vue 实例作为中央事件总线，用它来触发事件和监听事件，巧妙而轻量 地实现了任何组件间的通信，包括父子，兄弟，跨级。当项目较大时，可以使用状态管理 Vuex
```js
let vm = new Vue()
vm.$emit('func', data)
vm.$on('func', date => {})
```
3. vuex
4. $attr/$listens
多级组件嵌套需要传递数据时，通常使用的方法是通过 vuex，但如果仅仅是传递数据，而不做蹭处理，使用 vuex 处理，未免有点大材小用。
	- $attr: 包含了父作用域中不被 prop 所识别的特性绑定。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 interitAttrs 选项一起使用
	- $listeners: 包含了父作用域中的 v-on 事件监听器。它可以通过 v-on="$listens" 传入内部组件
我们来看个例子：
[https://www.cnblogs.com/fundebug/p/10884896.html](https://www.cnblogs.com/fundebug/p/10884896.html)
5. provide/inject
6. $parent/$children 与 ref

### axios 和 ajax的区别及优缺点
区别： axios 是通过 Promise 实现对 ajax 技术的一种封装，就像 jQuery对 ajax的封装一样
ajax技术实现了局部数据的刷新，axios实现了对ajax的封装，axios有的ajax都有，ajax有的axios不一定有。
优缺点：
- ajax：
	* 本身是针对  MVC 编程，不符合前端 MVVM的浪潮
	* ajax不支持浏览器的back按钮
	* ajax暴露了与服务器交互的细节
	* 对搜索引擎的支持比较弱
	* 不容易调试
- axios:
	* 从nodejs中创建http请求
	* 支持 Promise API
	* 提供了一些并发请求的接口
	- 可以在浏览器中发送 XMLHttpRequest 请求
    - 支持拦截请求和响应
    - 转换请求和响应的数据 
    - 能够取消请求
    - 自动化JSON格式
    - 客户端支持保护安全免受 XSRF 攻击

### vuex和浏览器缓存的区别及优缺点
Vuex 是 vue 的状态管理器，存储的数据是响应式的，但是并不会保存起来，刷新之后或者浏览器关闭之后 就回到了初始状态，具体做法应该在 vuex 里数据改变的时候把数据拷贝一份保存到 LocalStorage 里面，刷新之后，如果 localStorage 里有保存的数据，取出来再替换 store 里的state

由于 vuex里我们保存的状态是数组，而 localStorage 只支持字符串，所以需要用 JSON 转换
`JSON.stringify(state.xxxx) // array -> string`
`JSON.parse(window.localStorage.getItem('name')) // string -> array`
