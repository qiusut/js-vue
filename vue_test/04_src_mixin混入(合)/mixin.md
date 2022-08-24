# mixin混入
1. 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

2. 全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。
不推荐在应用代码中使用。

#### 首先要定义一个混合，通常在src内创建一个mixin.js文件在该文件内定义所需要的混入。
```js
export const mix = {
    methods: {
        showName(){
            alert(this.name)
        }
    },
    mounted() {
        console.log('hello')
    },
}
export const mixe = {
    data() {
        return {
            a:1,
            b:2
        }
    },
}
```
#### 全局混入：混入也可以进行全局注册。使用时格外小心！一旦使用全局混入，它将影响每一个之后创建的 Vue 实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。
```js
import Vue from 'vue'
import App from './App.vue'
import {mix,mixe} from './mixin'

Vue.config.productionTip = false

Vue.mixin(mix)
Vue.mixin(mixe)


new Vue({
	el:'#app',
	render: h => h(App)
})
```
```Vue
<template>
	<div>
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="showName">点击</button>
	</div>
</template>

<script>

	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'女'
			}
		},
	}
</script>
```
> 请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例 (包括第三方组件)。大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为插件发布，以避免重复应用混入。

#### 局部混入：
```Vue
<template>
	<div>
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="showName">点击</button>
	</div>
</template>

<script>
	import { mix } from './mixin'

	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'女'
			}
		},
		mixins:[mix]
	}
</script>
```
> 当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。
比如，数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。