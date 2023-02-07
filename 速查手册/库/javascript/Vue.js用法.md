

# Vue.js语法

[TOC]


## 概述

### 要点
* 是一个响应式的UI框架
  * 核心库只关注视图层
* 使用了基于HTML的模板语法
  * 将模板编译成虚拟DOM渲染函数



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





## 模板指令

### 概述
* 一个带`v-`前缀的特殊属性
* 作用：当表达式的值改变时会产生连带影响，响应式地作用于DOM
* 格式：`v-<name>:<parameter>.<qualifier>=<value>`
  * `value`：单个js表达式
  * `parameter`：参数，只能有一个
  * `qualifier`：修饰符，指定绑定方式

### 功能
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
* `v-model` 在表单元素上创建双向数据绑定
  * `.lazy`   将同步模式转成change事件，而非input事件
  * `.number` 自动将用户的输入值转为数值类型
  * `.trim`   自动过滤用户输入的首尾空白字符 
  * 语法糖：绑定`value`属性，监听`input`事件


### 语法
* 文本插值 `<span>{{ content }}</span>` 
* 文本一次插值 `<span v-once>{{ content }}</span>` 
* 属性插值 `<div v-bind:id="variable"></div>`  
* 绑定HTML Classs属性 `<div v-bind:class="{ className: variable }"></div>`  
* 绑定HTML Classs属性 `<div v-bind:class="[ className1, className2 ]"></div>` 

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



## 组件

### 概述
* 是一个带有名字的可复用的Vue实例
* 组件可以进行任意次数的复用



### 功能
* 定义组件 `{ /* options */}`
  * data选项必须是一个函数
* 注册组件：使组件能在模板中使用
  * 方式：全局注册、局部注册
* 使用`prop`注册参数属性，向子组件传递数据  [语法](#参数属性)
  * `type` 数据类型
  * `required` 必须属性
  * `default` 默认值
  * `validator`  验证函数
* 父组件使用`.sync`对自定义属性进行**双向绑定**
* 使用`$emit()`子组件发送一个事件  [语法](#自定义事件)
  * 原型：`$emit(event-name, ...arg)`
  * 事件名推荐使用**kebab-case**
  * 父组件使用`v-on`接收一个事件
* 使用**计算属性**处理复杂逻辑  [语法](#计算属性)
  * 值会基于它们的响应式依赖进行缓存的
  * 在需要可以创建一个**setter**函数
* 使用**侦听器**来响应数据的变化 [语法](#侦听属性)
  * 目的：在数据变化时执行异步操作、大开销操作
* 组件可以自定义v-model操作符 [语法](#自定义v-model)



### 语法

#### 定义组件

##### js方式
```js
{
  data: function() {
    return { ... }
  },
  computed: { ... },

  watch: { ... },

  model: { ... },

  template: '...'
}
```

##### ts方式
```ts
@Component({ })
class Component extends Vue {

  //定义组件数据
  dataName = <value>  

  //定义组件函数
  methodName() { ... } 
}
```

#### 注册组件
```js
//全局注册
Vue.component(name, { 
   // 选项 
})

//局部注册
var CompA = { 
  // 选项 
}

new Vue({ 
  components: { 
    'comp-a': CompA 
  }
})
```
```ts
//局部注册
@Component({
  //注册其他组件
  components: {  OtherComponent }
 })
export default class HelloWorld extends Vue {
    ...
}
```


#### 参数属性
```js
{
  props: [<name>, <name>]

  props: {
    <name>: <type>,
    <name>: <type>,
  }

  prop: {
    type: <type>,
    default: xxx,
}
```

```ts
@Prop(<type>) <attrName>
@Prop([<type>, <type>]) <attrName>
@Prop(
  {
    type: <type>,      
    required: <bool>, 
    default: <value>,       
    validator: (value) => <bool> 
  }
) <attrName>
```

##### 示例
```js
Vue.component('my-component', {
  props: {
    propA: Number,
    propB: [String, Number], // 多个可能的类型
    propC: {
      type: String,
      required: true  // 必填的字符串
    },
    propD: {
      type: Number,
      default: 100  // 带有默认值的数字
    },
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    propF: {
      validator: function (value) {  // 自定义验证函数
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})

```

```ts
@Component
export default class YourComponent extends Vue {
  @Prop(Number) readonly propA: number | undefined
  @Prop({ default: 'default value' }) readonly propB!: string
  @Prop([String, Boolean]) readonly propC: string | boolean | undefined
}
```


#### 双向绑定参数属性
```html
<text-document v-bind:<name>.sync="<attr-name>"></text-document>

<!-- 转换为 -->
<text-document
  v-bind:<name>="<attr-name>"
  v-on:update:<name>="<attr-name> = $event">
</text-document>

```

```js 
{
  this.$emit('update:<name>', newTitle)
}

```

```ts
@PropSync(<name>, {
    type: <type>,     //指定类型
    required: <bool>, //是否必须的
    default: <value>,      //默认值
    validator: (value) => <bool>  //校验函数
}) <attrName>


//转换成
@Prop() <name>    //一个参数属性

get <attrName>() { this.name }   //一个计算属性
set <attrName>(value) {
  this.$emit('update:name', value)
}
```

##### 例子
```ts
@Component
export default class YourComponent extends Vue {
  @PropSync('name', { type: String }) syncedName!: string
}
```


#### 自定义事件
```js
//发送
"$emit(<event-name>)"

//接收
v-on:<event-name>=" ... "
```

```ts
@Emit(<eventName>) 
<methodName>(eventArg2, eventArg3) {
  return eventArg1
}
```

##### 示例
```js
//子组件
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text')>
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})

//父组件模板
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1">
</blog-post>
```

```ts
@Component({})
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

#### 计算属性
```js
Vue.component('<name>', {

  computed: {
    <attr-name>: function() { ... }

    <attr-name>: {
      get: function() { ... }
      set: function(val) { ... }
    }
  }

})
```
```ts
export default class Component extends Vue {

  //定义属性对应的获取函数
  get <attrName>() { ... }
  //定义属性对应的设置函数
  set <attrName>(value) { ... }

}
```

##### 示例
```ts
@Component
export default class HelloWorld extends Vue {
  firstName = 'John'
  lastName = 'Doe'

  get name() {
    return this.firstName + ' ' + this.lastName
  }

  set name(value) {
    const splitted = value.split(' ')
    this.firstName = splitted[0]
    this.lastName = splitted[1] || ''
  }
}
```


#### 侦听属性
```js
Vue.component('<name>', {

  data: function() {
    return { 
      <attr-name>: ...
     }
  },

  watch: {
    <attr-name>: function(newVal, oldVal) { 
      ... 
    }
  }

})
```

```ts
@Watch(
  <attrName>,
  {
    immediate: <bool>, //侦听开始后，是否立刻调用该函数
    deep: <bool>       //被侦听对象的属性改变时，是否调用该函数
  }
)
<methodName>(newValue, oldValue) { ... }
```

##### 示例
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


#### 自定义v-model
```js
export default {
  model: {
    prop: '<propName>',
    event: 'change',
  },

  props: {
    <propName>: {
      type: Boolean,
    },
  },
}
```

```ts
@Model(<event>, {
    type: <type>,     //指定类型
    required: <bool>, //是否必须的
    default: <value>,      //默认值
    validator: (value) => <bool>  //校验函数
}) <propName>
```

```ts
@ModelSync(<propName>, <event>, { ... }) <attrName>


//转换成
@Model(<event>, { ... }) <propName>    //一个v-model

get <attrName>() { this.<propName> }   //一个计算属性
set <attrName>(value) {
  this.$emit('<event>', value)
}
```

##### 例子
```ts
@Component
export default class YourComponent extends Vue {
  @Model('change', { type: Boolean }) readonly checked!: boolean

  @ModelSync('checked', 'change', { type: Boolean })
  readonly checkedValue!: boolean
}
```



## 其他

### 过滤器

#### 功能
* 用于文本格式化

#### 用法
* 文本插值 `{{ message | capitalize }}`
* v-bind表达式 `<div v-bind:id="rawId | formatId"></div>`

#### 语法

##### js组件方式
```js
{
  filters: {
    <name>: function(value) {
      return ...
    }
  }
}
```

##### ts组件方式
```ts
@Component({
  filters: {
    <name>(value: any) {
      return ...
    }
  }
})
export default class ComponentName extends Vue { }
```

##### 全局方式
```js
Vue.filter('<name>', function (value) {
  return ...
})
```





