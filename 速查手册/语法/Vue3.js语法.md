
# Vue3.js语法

## 概述

### 风格
* 选项式API：使用包含多个选项的对象来描述组件
  * 以组件实例为核心
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

## 基础语法

### 响应式对象
* `const xxx = reactive({ xxx: <value> })`    定义
* `cosnt xxx: <type> = reactive({ xxx: <value> })`  定义，带显示类型标注

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
* `const xxx: Ref<type> = ref(<value>)` 定义，带显示类型标注
* `xxx.value`  获取值


#### 示例
```ts
import { ref, Ref } from 'vue'

const yearOne = ref(2020)

cosnt yearTwo： Ref<string | number> = ref('2020')
yearTwo.value = 2020
```

### 计算属性
* `const xxx = computed(() => { ... })` 定义，返回一个ref
* `const xxx = computed<type>(() => { ... })` 定义，带显示类型标注
* `const xxx = computed({ get() { ... }, set(newVal) { ... } })` 定义可写的属性


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
```

### 侦听器
* `watch(<var>, async(newVal, oldVal) => { ... })`  监听ref、响应式对象
* `watch(() => <func>, (resultVal) => { ... })`   监听函数
* `watchEffect(async () => { ... })`    执行一次动作，并自动跟踪依赖

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

watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)


//自动跟踪`url.value`
watchEffect(async () => {
  const response = await fetch(url.value)
  data.value = await response.json()
})
```


## 组件语法


### 注册
* `<script setup> import xxx from 'xxx.vue' `  局部注册

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
* `const xxx = defineProps<{ ... }>()`  定义，带显示类型标注
* `xxx.<name>`  读取

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
  bar: Number
})
</script>
```

### 事件
* `defineEmits([<name>])` 声明
* `defineEmits<{ ... }>()`  定义，带类型标注
* `$emit(<name>)`   发送
* `$emit(<name>, <arg>)` 发送，带参数


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


### v-model实现

#### 语法糖
```html
<CustomInput v-model="searchText">

<!-- 等价于 -->
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
```

#### 实现1
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

#### 实现2
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
``