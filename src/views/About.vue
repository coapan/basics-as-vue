<template>
	<div>
		<div>
			<div>{{ hello }}</div>
			
			<a v-bind:href="url"> 对象、属性绑定</a>
			<input type="text" v-bind:value="val" />
		</div>
		
		<hr>
		<div>
			<h2>事件绑定、鼠标点击</h2>
			<!-- 点击事件 -->
			<button v-on:click="onClick(123)">点击事件</button>
			<button @click="onClick(123)">点击事件</button>
			
			<!-- 双击事件 -->
			<button @dblclick="doubleClick(222)">鼠标双击事件</button>
			<button v-on:dblclick="doubleClick(333)">鼠标双击事件</button>
			
			<!-- 鼠标移动事件 -->
			<div id="canvas" v-on:mousemove="createData">
				<h3>鼠标移动事件</h3>
				{{x}} {{ y }}
			</div>
		</div>
		
		<hr>
		
		<div>
			<h2>事件修饰符</h2>
			<button @click.once="add(1)">仅触发一次</button>
			<div @click="clickContainer" style="padding: 10px 0">
				<!-- 两个都输出 -->
				<button @click="clickButton">两个都输出</button>
				<!-- 只会输出当前点击 -->
				<button @click.stop="clickButton">只会输出当前点击</button>
			</div>
			<div>this value is {{ onceVal }}</div>
			<div id="canvas" v-on:mousemove="createData">
				<h3>鼠标移动事件</h3>
				{{x}} {{ y }}
				<span v-on:mousemove.stop="stopmoving">(在这里移动阻止了 createData 的数据)</span>
			</div>

			<a href="www.baidu.com" @click.prevent="alert()">有 prevent 阻止不会跳转</a>
		</div>
		
		<hr>
		
		<div>
			<h2>键盘事件及键盘修饰符</h2>
			<!-- 回车后触发方法 -->
			<input type="text" @keyup.enter="clogEnter" />
			
			<!-- alt+ 回车触发 -->
			<input type="text" @keyup.alt.enter="clogAltEnter" />
			
			<input type="text" @keydown="clogKeyDown" />
		</div>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				val: '请输入值',
				hello: 'Hello World.',
				url: 'www.baidu.com',
				x:0,
				y:0,
				onceVal: 10
			}
		},
		methods:{
			add(val) {
				this.onceVal = this.onceVal + val
			},
			alert() {
				alert('Hello')
			},
			onClick(val) {
				console.log(val);
			},
			doubleClick(val) {
				console.log(val);
			},
			createData(event) {
				this.x = event.offsetX
				this.y = event.offsetY
			},
			stopmoving(event) {
				event.stopPropagation()
			},
			clickContainer() {
				console.log('Click Container.');
			},
			clickButton() {
				console.log('Click Button.');
			},
			clogEnter(event) {
				console.log(`${event.keyCode}--${event.target.value}`);
			},
			clogAltEnter(event) {
				console.log(`${event.keyCode}--${event.target.value}`);
			},
			clogKeyDown(event) {
				console.log(event);
			}
		}
	}
</script>