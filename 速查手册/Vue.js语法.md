

# Vue.js语法

[TOC]


## 概述

### 要点
* 核心库只关注视图层，是一个响应式的UI框架
* 核心是一个允许采用模板语法来声明式地将数据渲染进DOM的系统
* 使用了基于HTML的模板语法，



### 实例
* 每个Vue应用都是一个Vue实例，通过Vue函数创建
	* 一个Vue应用：根Vue实例 + 组件树
	* 所有的Vue组件也都是Vue实例
* 实例被创建时，数据区中的属性会被加入到响应系统中

#### 生命周期
* `beforeCreate` 实例初始化后，数据观测和事件配置之前
* `created`      数据观测和事件已配置
* `beforeMount`  挂载前
* `mounted`      挂载后，不会保证所有子组件都已经挂载好
* `beforeUpdate` 数据更新时，虚拟DOM打补丁前
* `updated`      虚拟DOM重新渲染和打补丁后
* `beforeDestroy` 实例销毁前
* `destroyed`     实例被销毁后

## 模板

### 指令
* 格式：`v-<name>:<parameter>.<qualifier>=<value>`
* 一个带有`v-`前缀的特殊属性
* 值是单个js表达式
* 一条指令只能接收一个参数
* 修饰符英语指定绑定方式 

#### 缩写
* `v-bind:href` => `:href`
* `v-on:click` => `@:click`

#### 示例
```html
<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>
<form v-on:submit.prevent="onSubmit">...</form>

<a v-on:click="doSomething">...</a>
<a @click="doSomething">...</a>

<a v-bind:href="url">...</a>
<a :href="url">...</a>
```

### 语法

#### `{{}}` 文本插值
* 文本插值 `<span>{{ content }}</span>`
* 文本一次插值 `<span v-once>{{ content }}</span>`


#### `v-bind` 属性插值
* HTML属性插值 `<div v-bind:id="variable"></div>`
* 绑定HTML Classs属性 
  * 对象方式：`<div v-bind:class="{ className: variable }"></div>`
  * 数组方式：`<div v-bind:class="[ className1, className2 ]"></div>`

##### 示例
```html
<div v-bind:class="{ active: isActive }"></div>
<!-- 若isActive为true，则渲染为<div class="active"></div> -->

<div v-bind:class="[activeClass, errorClass]"></div>
<!-- 若activeClass: 'active'，errorClass: 'text-danger'
  则渲染为<div class="active text-danger"></div> -->
```


#### `v-if` 条件渲染
* 条件渲染  `<div v-if="cond">content</div>`
* 带分支条件渲染 
	```html
		<div v-if="cond">content</div>
		<div v-else>content</div>
	```
* 带多条件的条件渲染
	```html
	<div v-if="cond-one">content-one</div>
	<div v-else-if="cond-two">content-two</div>
	<div v-else-if="cond-three">content-three</div>
	<div v-else>content-other</div>
	```

##### 特点
* 真正的条件渲染：切换过程中子组件会被销毁、重建
* 惰性的：直到条件第一次满足时，才会开始渲染
* 支持复用元素，使用`key`属性可以独立元素


#### `v-show` 条件显示
* 条件显示 `<div v-show="cond">content</div>`

##### 特点
* 元素会保留在DOM中，只切换CSS的dispaly属性
* 不支持复用元素


#### `v-for` 列表渲染
* 列表渲染 `<li v-for="item in items"> content </li>`
* 带索引的列表渲染  `<li v-for="(item, index) in items"> content </li>`

##### 示例
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>ddd
```

#### `v-on` 监听事件
* 直接执行js代码  `<button v-on:click="js-code"></button>`
* 绑定到方法 `<button v-on:click="method-name"></button>`
* 直接调用方法 `<button v-on:click="method(arg)"></button>`

##### 特点
* 用于监听DOM事件

##### 修饰符
* `.stop` 阻止单击事件继续传播
* `.prevent` 提交事件不再重载页面
* `.capture` 使用事件捕获模式，内部元素触发的事件先处理
* `.self`  只当在 event.target 是当前元素自身时触发处理函数
* `.once`  点击事件只触发一次

##### 示例
```html
<div id="example-1">
	<button v-on:click="counter += 1">Add 1</button>
	<p>The button above has been clicked {{ counter }} times.</p>
</div>


<div id="example-2">
  <button v-on:click="greet">Greet</button>
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
```

#### `v-model` 数据绑定
* 绑定到文本 `<input v-model="variable" />`
* 绑定到多行文本 `<textarea v-model="variable"/>`

##### 特点
* 在表单及元素上创建双向数据绑定
* 根据控件类型自动选取正确的方法来更新元素

##### 修饰符
* `.lazy`   将同步模式转成change事件，而非input事件
* `.number` 自动将用户的输入值转为数值类型
* `.trim`   自动过滤用户输入的首尾空白字符

##### 示例
```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>

<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>

<textarea v-model="message" placeholder="add multiple lines"></textarea>
```


## 组件功能


### 定义组件

#### 格式
```ts
@Component({
  //注册子组件
  components: { xxxComponent, yyyComponent, ... } 
})
class Component extends Vue {

  //定义组件数据
  dataName = <value>  

  //定义组件函数
  methodName() { ... } 
}
```

#### 示例
```ts
@Component({
  components: {  //注册其他组件
    OtherComponent
  }
 })
export default class HelloWorld extends Vue {
  //定义一个组件数据，带有响应属性
  message = 'Hello World!'

  hello() {
    console.log('Hello World!')
  }
}
```


### 计算属性
* 用于复杂逻辑运算
* 计算出的值会进行缓存处理

#### 格式
```ts
//定义属性对应的获取函数
get <attrName>() { ... }
//定义属性对应的设置函数
set <attrName>(value) { ... }
```

#### 示例
```ts
@Component
export default class HelloWorld extends Vue {
  firstName = 'John'
  lastName = 'Doe'

  // Declared as computed property getter
  get name() {
    return this.firstName + ' ' + this.lastName
  }

  // Declared as computed property setter
  set name(value) {
    const splitted = value.split(' ')
    this.firstName = splitted[0]
    this.lastName = splitted[1] || ''
  }
}
```


### 侦听属性
* 用于在数据变化时执行异步操作、大开销操作

#### 格式
```
@Watch(
  <attrName>,
  {
    immediate: <bool>, //侦听开始后，是否立刻调用该函数
    deep: <bool>       //被侦听对象的属性改变时，是否调用该函数
  }
)
<methodName>(newValue, oldValue) { ... }
```

#### 示例
```ts
@Component
export default class YourComponent extends Vue {
  @Watch('child')
  onChildChanged(val: string, oldVal: string) {}

  @Watch('person', { immediate: true, deep: true })
  onPersonChanged1(val: Person, oldVal: Person) {}

  @Watch('person')
  onPersonChanged2(val: Person, oldVal: Person) {}
}
```


### 参数属性

#### 格式
```ts
@Prop(<type>) <attrName>
@Prop([<type>, <type>]) <attrName>
@Prop(
  {
    type: <type>,     //指定类型
    required: <bool>, //是否必须的
    default: <value>,      //默认值
    validator: (value) => <bool>  //校验函数
  }
) <attrName>
```

#### 示例
```ts
@Component
export default class YourComponent extends Vue {
  @Prop(Number) readonly propA: number | undefined
  @Prop({ default: 'default value' }) readonly propB!: string
  @Prop([String, Boolean]) readonly propC: string | boolean | undefined
}
```

### 同步参数属性
* 类似于参数属性
* 不同点
  * 创建一个计算属性
  * 属性名从装饰符参数中获取
* 父组件使用时，可以增加`.sync`修饰符

#### 格式
```ts
@PropSync(<name>, {
    type: <type>,     //指定类型
    required: <bool>, //是否必须的
    default: <value>,      //默认值
    validator: (value) => <bool>  //校验函数
}) <attrName>
```

#### 转换成
```ts
@Prop() <name>

get <attrName>() { this.name }
set <attrName>(value) {
  this.$emit('update:name', value)
}
```

#### 例子
```ts
@Component
export default class YourComponent extends Vue {
  @PropSync('name', { type: String }) syncedName!: string
}
```


### 自定义v-model

#### 格式
```ts
@Model(<event>, {
    type: <type>,     //指定类型
    required: <bool>, //是否必须的
    default: <value>,      //默认值
    validator: (value) => <bool>  //校验函数
}) <attrName>
```

#### 转换成
```js
export default {
  model: {
    prop: 'checked',
    event: 'change',
  },
  props: {
    checked: {
      type: Boolean,
    },
  },
}
```




#### 例子
```ts
@Component
export default class YourComponent extends Vue {
  @Model('change', { type: Boolean }) readonly checked!: boolean
}
```


### 发送事件
* 用于组件向外部通知状态变化
* 使用`v-on`来监听事件

#### 格式
```ts
@Emit(<eventName>) 
<methodName>(eventArg2, eventArg3) {
  return eventArg1
}
```

#### 示例
```ts
@Component
export default class YourComponent extends Vue {

  //事件名: add-to-count
  //事件参数: n
  @Emit()
  addToCount(n: number) { ... }

  //事件名：rest
  @Emit('reset')
  resetCount() { ... }

  //事件名: return-value
  //事件参数：10
  @Emit()
  returnValue() { return 10 }
  
}
```










