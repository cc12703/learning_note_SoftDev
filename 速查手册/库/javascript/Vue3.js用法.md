
# Vue3.js语法

[TOC]

## 概述

### 风格
* 选项式API：使用包含多个选项的对象来描述组件
* 组合式API：使用导入的API函数来描述组件


#### 示例

##### 选项式
```js
<script>
export default {
  // data() 返回的属性将会成为响应式的状态
  data() {
    return {
      count: 0
    }
  },

  methods: {
    increment() {
      this.count++
    }
  },

  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
</template>
```

##### 组合式
```js
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```


## 模板语法

### 概述
* 一个带`v-`前缀的特殊属性
* 作用：当表达式的值改变时会产生连带影响，响应式地作用于DOM
* 格式：`v-<name>:<parameter>.<qualifier>=<value>`
  * `value`：单个js表达式
  * `parameter`：参数，只能有一个
  * `qualifier`：修饰符，指定绑定方式

### 指令
* `{{ }}`  文本插值：将普通文本插入HTML中
* `v-bind` 属性插值：将值插入HTML属性中
  * 缩写：`v-bind:href` => `:href`
* `v-once` 执行一次性插件
* `v-if`   有条件地渲染HTML元素
* `v-show` 有条件地显示HTML元素，元素总是会被渲染
* `v-for`  通过数组渲染列表
* `v-on`   监听HTML中的事件
  * 缩写：`v-on:click` => `@click`
  * `.stop` 阻止单击事件继续传播
  * `.prevent` 提交事件不再重载页面
  * `.capture` 使用事件捕获模式，内部元素触发的事件先处理
  * `.self`  只当在 event.target 是当前元素自身时触发处理函数
  * `.once`  点击事件只触发一次
* `v-on:xxx="<method-name>"`  事件处理器，绑定一个方法
* `v-on:xxx="func(...)"`      事件处理器，内联js语句
* `v-on:xxx="func(..., $event)"`  事件处理器，传入DOM事件
* `v-on:xxx="(arg) => func(arg)"` 事件处理器，使用lamda代码
* `v-model` 在表单元素上创建双向数据绑定
  * `.lazy`   将同步模式转成change事件，而非input事件
  * `.number` 自动将用户的输入值转为数值类型
  * `.trim`   自动过滤用户输入的首尾空白字符 
  * 语法糖：绑定`value`属性，监听`input`事件


### 写法
* 文本插值 `<span>{{ content }}</span>` 
* 文本一次插值 `<span v-once>{{ content }}</span>` 
* 属性插值 `<div v-bind:id="variable"></div>`  
* 绑定HTML Classs属性 `<div v-bind:class="{ className: variable }"></div>`  
* 绑定HTML Classs属性 `<div v-bind:class="[ className1, className2 ]"></div>` 
* 绑定HTML Style属性 `<div v-bind:style="{ styleName: variable,  styleName: variable }"></div>`  

* 条件渲染 `<div v-if="cond">content</div>` 
* 分组渲染 `<template v-if="ok"> content  </template>`
* 带分支条件渲染 `<div v-if="cond"> content </div> <div v-else> content </div>`   
* 带多条件的条件渲染 
  ```html
    <div v-if="c-one"> content </div> 
    <div v-else-if="c-two"> content </div> 
    <div v-else> content </div>
  ```

* 条件显示 `<div v-show="cond">content</div>` 

* 列表渲染 `<li v-for="item in items"> content </li>` 
* 带索引的列表渲染 `<li v-for="(item, index) in items"> content </li>`  

* 事件发生后直接执行js代码 `<button v-on:click="js-code" />`  
* 将事件绑定到方法 `<button v-on:click="method-name" />`    
* 事件发生后直接调用方法 `<button v-on:click="method(arg)" />`  

* 将数据绑定到文本 `<input v-model="variable" />`   
* 将数据绑定到多行文本 `<textarea v-model="variable"/>`   


### 示例
```html
<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>
<form v-on:submit.prevent="onSubmit">...</form>

<a v-on:click="doSomething">...</a>
<a @click="doSomething">...</a>

<a v-bind:href="url">...</a>
<a :href="url">...</a>


<div v-bind:class="{ active: isActive }"></div>
<!-- 若isActive为true，则渲染为<div class="active"></div> -->

<div v-bind:class="[activeClass, errorClass]"></div>
<!-- 若activeClass: 'active'，errorClass: 'text-danger'
  则渲染为<div class="active text-danger"></div> -->

<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px'}" />


<template v-if="ok"> 
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>

<div v-if="cond">content</div>
<div v-else>content</div>

<div v-if="cond-one">content-one</div>
<div v-else-if="cond-two">content-two</div>
<div v-else-if="cond-three">content-three</div>
<div v-else>content-other</div>

<div v-if="cond-one">content-one</div>
<div v-else-if="cond-two">content-two</div>
<div v-else-if="cond-three">content-three</div>
<div v-else>content-other</div>

<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>

<div id="example-1">
	<button v-on:click="counter += 1">Add 1</button>
	<p>The button above has been clicked {{ counter }} times.</p>
</div>


<div id="example-2">
  <button v-on:click="greet">Greet</button>
  <cascader @change="(val) => changed(val)"></cascader>
</div>

<script>
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },

  methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
    },
    changed: function (val) {
      ...
    }
  }
})
</script>


<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>

<script>
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>

<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>

<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>

<textarea v-model="message" placeholder="add multiple lines"></textarea>



<input  v-model="searchText">

<!-- 等价于 -->
<input 
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value">
```





## 基础语法

### 响应式对象
* `const xxx = reactive({ xxx: <value> })`    定义
* `cosnt xxx: <type> = reactive({ xxx: <value> })`  定义，带类型标注
* `shallowReactive`  浅层作用形式，只有根属性是响应式的

#### 示例
```ts
import { reactive } from 'vue'

//自动推导类型
const state = reactive({ count: 0})

//显示标注类型
interface Book {
    title: string
    year?: number
} 

cosnt book: Book = reactive({ title: "Vue 3" })
```

### 响应式变量
* `const xxx = ref(<value>)`  定义
* `const xxx: Ref<type> = ref(<value>)` 定义，带类型标注
* `xxx.value`  读取值


#### 示例
```ts
import { ref, Ref } from 'vue'

const yearOne = ref(2020)

cosnt yearTwo： Ref<string | number> = ref('2020')
yearTwo.value = 2020
```

### 计算属性
* `const xxx = computed(() => { ... })` 定义
* `const xxx = computed<type>(() => { ... })` 定义，带类型标注
* `const xxx = computed({ get() { ... }, set(newVal) { ... } })` 定义可写属性
* `const xxx = computed({ get: () => { ... }, set: (newVal) => { ... } })` 定义可写属性，lambda格式


#### 示例
```ts
<script setup>
import { ref, computed } from 'vue'


const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一个计算属性 ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})




const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})

//lambda
const fullName = computed({
  get: () => {
    return firstName.value + ' ' + lastName.value
  },
  set: (newValue) => {
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
```

### 侦听器
* `watch(<var>, (newVal, oldVal) => { ... })`  监听响应式变量，ref和reactive
* `watch(() => <var>.<field>, (newVal, oldVal) => { ... })`  监听响应式对象中的属性
* `watch([ <conds> ], ([ <newVals> ], [ <oldVals> ]) => { ... })`  监听多个条件
* `watch(() => <func>, (resultVal) => { ... })`   监听函数
* `watchEffect(() => { ... })`      立即执行，并自动跟踪依赖

#### 回调时机
* 默认回调会在组件更新前调用
* `watch(<source>, <callback>, { flush: 'post' })` 在组件更新后调用
* `watchEffect(<callback>, { flush: 'post' })` 在组件更新后调用
* `watchPostEffect(<callback>)`  在组件更新后调用

#### 示例
```ts
import { ref, watch } from 'vue'

const x = ref(0)
const y = ref(0)


watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

watch(x, (newX, oldX) => {
  console.log(`x is ${newX}`)
})

//监听多个变量
watch([x, y], ([newX, newY], [oldX, oldY])) {
  console.log(`x is ${newX}`)
  console.log(`y is ${newY}`)
}

//使用 getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)


const z = reactive({ count: 0})
watch(() => z.count, (newCount, oldCount) => { ... })


//自动跟踪`url.value`
watchEffect(async () => {
  const response = await fetch(url.value)
  data.value = await response.json()
})
```

### 其他
* `<var> = ref<HTMLElement|null>(null)`  引用html元素


#### 示例
```vue
<template>
    <div class="play" ref="container">
    </div>

</template>

<script lang="ts" setup>

const container = ref<HTMLElement|null>(null)
</script>
```



## 组件语法


### 注册
* `<script setup> import xxx from 'xxx.vue' </script>`  局部注册

#### 示例
```ts
<script setup>
import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```


### 参数
* `const xxx = defineProps([<name>])`  定义，数组形式
* `const xxx = defineProps({ <name>:<type> })` 定义，对象形式
* `const xxx = defineProps<{ ... }>()`  定义，带类型标注
* `xxx.<name>`  读取参数

#### 属性
* `type` 类型，需要使用JS的原生构造器
* `required` 是否必须
* `default`   默认值
* `default() { ... }` 默认值的工厂函数
* `validator(val) { ... }` 校验函数 

#### 示例
```ts
<script setup lang="ts">
const props = defineProps(['foo'])
console.log(props.foo)

defineProps({
  title: String,
  likes: Number
})

const props = defineProps({
  foo: { type: String, required: true },
  bar: { type: Number, default: 100 }
})


const props = defineProps<{
  foo: string
  bar?: number
}>()

//---> 编译为 --->

const props = defineProps({
  foo: { type: String, required: true },
  bar: { type: Number, required: true }
})

</script>
```



### 事件
* `defineEmits([<name>])`   定义 
* `defineEmits<{ ... }>()`  定义，带类型标注
* `$emit(<name>)`           发送
* `$emit(<name>, <arg>)`    发送，带参数

#### 示例
```ts
<script setup lang="ts">
const emit = defineEmits(['change', 'update'])

// 类型标注
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
</script>


<button @click="$emit('someEvent')">点击这里抛出事件</button>

<button @click="$emit('increaseBy', 1)">
  Increase by 1
</button>
```



### v-model

#### 语法糖
```html
<CustomInput v-model="searchText">

<!-- 等价于 -->
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
```
#### 参数
```html
<CustomInput v-model:title="searchText">

<!-- 等价于 -->
<CustomInput
  :title="searchText"
  @update:title="newValue => searchText = newValue"
/>
```

#### 实现-方法1

```vue
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

#### 实现-方法2
* 使用计算属性

```vue
<script setup>
import { computed } from 'vue'

const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const value = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})
</script>

<template>
  <input v-model="value" />
</template>
```


### 状态驱动CSS
* 在\<style>中使用v-bind将CSS值关联到组件状态上
* `<css-name>: v-bind(<var-name>)`
* `<css-name>: v-bind('<express>')`


#### 示例
```vue
<script setup>
const theme = {
  color: 'red'
}
</script>

<template>
  <p>hello</p>
</template>

<style scoped>
p {
  color: v-bind('theme.color');
}
</style>
```


### 依赖注入
* 用于在组件树中传递数据
* 注入名可以是字符串、Symbol
* 可以注入响应式类型
* `provide(name<str>, value)`  组件供给数据
* `cosnt name = Symbol()`  注入名使用Symbol
* `const name = Symbol() as InjectonKey<string>`  给注入值标记类型
* `provide(name, readonly(value))`  组件供给只读数据
* `app.provide(name ,value)`  应用层面供给数据
* `const <var> = inject(name, [default])`  注入数据，带默认值



#### 示例
```html
//provider组件
<script setup>
import { provide, ref } from 'vue'

const location = ref('North Pole')

//更新函数
function updateLocation() {
  location.value = 'South Pole'
}

provide('location', {
  location,
  updateLocation
})
</script>


// injector组件
<script setup>
import { inject } from 'vue'

const { location, updateLocation } = inject('location')
</script>

<template>
  <button @click="updateLocation">{{ location }}</button>
</template>
```





## 状态管理

### 概述
* 使用Pinia
* Actions支持同步和异步
* 扁平结构，每个store相互独立


### 初始化
```ts
import { createPinia } from 'pinia'
createApp(App)
  .use(createPinia())
  .mount('#app')
```

### 创建
```ts
export const xxxStore = defineStore(name, {
  //存储全局状态
  state: () => {   
    return { ... }
  },
  //封装计算属性，缓存数据
  getters: { ... }
  //封装业务逻辑
  actions: { ... }
})
```

### 使用
```ts
import { xxxStore } from 'xxx'
const xxx = xxxStore()
```

### getters
* `name(state) { ... }` 使用参数
* `name(): <type> { ... }` 无参数，使用this

#### 示例
```ts
getters: {
    myCount(state){
        return state.count + 1
    },

    myCount(): number{
        return this.count + 1
    }
}
```

### 更新
* `<store>.stateName`
* `<store>.$patch({ ... })` 批量更新
* `<store>.$patch( statue => { ... } )`
* `actions: { funName() { ... } }`  定义函数

#### 示例
```ts
userStore.count++
    
userStore.$patch({
    count: user_store.count1++,
    arr: [ ...user_store.arr, 1 
})
    
userStore.$patch( state => {
    state.count++
    state.arr.push(1)
})

userStore.changeState(1)

//定义
actions: {
    changeState(num: number) { // 不能用箭头函数
        this.count += num
    }
}
```