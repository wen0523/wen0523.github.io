# Vue3组合式API详解


# Vue3组合式API详解

## 概述

Vue3组合式API（Composition API）是一种新的组织组件逻辑的方式，它提供更灵活、更强大的代码组织方式，特别适合复杂组件和逻辑复用。与传统的Options API相比，组合式API让相关逻辑可以集中在一起，提高了代码的可维护性和复用性。

## 1. 组合式API基础

### 1.1 为什么需要组合式API

传统的Options API存在一些问题：

```vue
<!-- Options API - 逻辑分散 -->
<script>
export default {
  data() {
    return {
      count: 0,
      message: 'Hello',
      user: null
    }
  },
  computed: {
    doubleCount() {
      return this.count * 2
    },
    greeting() {
      return this.message + ', ' + (this.user?.name || 'Guest')
    }
  },
  methods: {
    increment() {
      this.count++
    },
    reset() {
      this.count = 0
      this.message = 'Reset'
    },
    fetchUser() {
      // 获取用户数据的逻辑
    }
  },
  watch: {
    count(newVal, oldVal) {
      console.log('Count changed:', oldVal, '->', newVal)
    }
  },
  mounted() {
    this.fetchUser()
  }
}
</script>
```

组合式API解决了这些问题：

```vue
<!-- Composition API - 逻辑集中 -->
<script setup>
import { ref, computed, watch, onMounted } from 'vue'

// 计数器相关逻辑
const count = ref(0)
const doubleCount = computed(() => count.value * 2)
function increment() {
  count.value++
}
function reset() {
  count.value = 0
  message.value = 'Reset'
}
watch(count, (newVal, oldVal) => {
  console.log('Count changed:', oldVal, '->', newVal)
})

// 消息相关逻辑
const message = ref('Hello')
const user = ref(null)
const greeting = computed(() => message.value + ', ' + (user.value?.name || 'Guest'))

// 用户相关逻辑
function fetchUser() {
  // 获取用户数据的逻辑
}

onMounted(() => {
  fetchUser()
})
</script>
```

### 1.2 setup()函数

`setup()`是组合式API的入口函数，在组件创建时执行。

```vue
<template>
  <div>
    <p>计数: {{ count }}</p>
    <button @click="increment">增加</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup(props, context) {
    // props: 父组件传递的属性
    // context: 包含 attrs, slots, emit
    
    const count = ref(0)
    
    function increment() {
      count.value++
    }
    
    // 必须返回要在模板中使用的内容
    return {
      count,
      increment
    }
  }
}
</script>
```

### 1.3 `<script setup>`语法糖

`<script setup>`是Vue3.2引入的语法糖，让组合式API写起来更简洁：

```vue
<template>
  <div>
    <p>计数: {{ count }}</p>
    <button @click="increment">增加</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 自动暴露给模板，不需要return
const count = ref(0)

function increment() {
  count.value++
}
</script>
```

## 2. 响应式系统

### 2.1 ref()

`ref()`用于创建基本类型的响应式数据。

```vue
<template>
  <div class="ref-demo">
    <h2>ref() 演示</h2>
    
    <div class="basic-ref">
      <p>基本类型ref: {{ basicValue }}</p>
      <button @click="basicValue++">增加</button>
    </div>
    
    <div class="object-ref">
      <p>对象ref: {{ objectRef.name }} - {{ objectRef.age }}</p>
      <button @click="updateObjectRef">更新对象</button>
    </div>
    
    <div class="ref-access">
      <p>ref访问: {{ basicValue }} (模板中自动解包)</p>
      <p>ref访问: {{ basicValue + 10 }} (表达式中自动解包)</p>
      <button @click="accessRefInJs">在JavaScript中访问</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 基本类型ref
const basicValue = ref(0)

// 对象ref
const objectRef = ref({
  name: '张三',
  age: 25
})

function updateObjectRef() {
  objectRef.value = {
    name: '李四',
    age: 30
  }
}

function accessRefInJs() {
  console.log('在JavaScript中访问ref:', basicValue.value)
  console.log('在JavaScript中访问对象ref:', objectRef.value)
  alert(`ref值: ${basicValue.value}, 对象: ${JSON.stringify(objectRef.value)}`)
}
</script>

<style scoped>
.ref-demo {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

.basic-ref, .object-ref, .ref-access {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #35495e;
}
</style>
```

### 2.2 reactive()

`reactive()`用于创建对象或数组的响应式代理。

```vue
<template>
  <div class="reactive-demo">
    <h2>reactive() 演示</h2>
    
    <div class="basic-reactive">
      <p>对象reactive: {{ state.name }} - {{ state.age }}</p>
      <p>数组reactive: {{ state.items.join(', ') }}</p>
      <button @click="updateReactive">更新reactive对象</button>
    </div>
    
    <div class="nested-reactive">
      <p>嵌套对象: {{ state.nested.deep.value }}</p>
      <button @click="updateNested">更新嵌套对象</button>
    </div>
    
    <div class="reactive-vs-ref">
      <p>ref计数: {{ refCount }}</p>
      <p>reactive计数: {{ reactiveState.count }}</p>
      <button @click="incrementBoth">同时增加</button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

// 基本reactive对象
const state = reactive({
  name: '王五',
  age: 28,
  items: ['苹果', '香蕉', '橙子'],
  nested: {
    deep: {
      value: '深层嵌套值'
    }
  }
})

// reactive vs ref对比
const refCount = ref(0)
const reactiveState = reactive({
  count: 0
})

function updateReactive() {
  state.name = '赵六'
  state.age = 32
  state.items.push('葡萄')
}

function updateNested() {
  state.nested.deep.value = '更新后的深层值'
}

function incrementBoth() {
  refCount.value++
  reactiveState.count++
}
</script>

<style scoped>
.reactive-demo {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
}

.basic-reactive, .nested-reactive, .reactive-vs-ref {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #0056b3;
}
</style>
```

### 2.3 computed()

`computed()`用于创建计算属性，具有缓存特性。

```vue
<template>
  <div class="computed-demo">
    <h2>computed() 演示</h2>
    
    <div class="basic-computed">
      <p>原始值: {{ baseValue }}</p>
      <p>计算值: {{ doubledValue }}</p>
      <button @click="baseValue++">增加原始值</button>
    </div>
    
    <div class="setter-computed">
      <p>全名: {{ fullName }}</p>
      <input v-model="fullNameInput" placeholder="输入全名">
      <button @click="setFullName">设置全名</button>
    </div>
    
    <div class="complex-computed">
      <p>商品总数: {{ totalItems }}</p>
      <p>总价: {{ totalPrice }}</p>
      <p>平均价格: {{ averagePrice }}</p>
      <button @click="addItem">添加商品</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 基本计算属性
const baseValue = ref(5)
const doubledValue = computed(() => baseValue.value * 2)

// 带setter的计算属性
const firstName = ref('张')
const lastName = ref('三')
const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    const names = newValue.split(' ')
    firstName.value = names[0] || ''
    lastName.value = names[1] || ''
  }
})

const fullNameInput = ref('')

function setFullName() {
  fullName.value = fullNameInput.value
  fullNameInput.value = ''
}

// 复杂计算属性
const products = ref([
  { name: '苹果', price: 5, quantity: 10 },
  { name: '香蕉', price: 3, quantity: 15 },
  { name: '橙子', price: 4, quantity: 8 }
])

const totalItems = computed(() => {
  return products.value.reduce((total, product) => total + product.quantity, 0)
})

const totalPrice = computed(() => {
  return products.value.reduce((total, product) => total + product.price * product.quantity, 0)
})

const averagePrice = computed(() => {
  if (products.value.length === 0) return 0
  return totalPrice.value / totalItems.value
})

function addItem() {
  products.value.push({
    name: '葡萄',
    price: 6,
    quantity: 12
  })
}
</script>

<style scoped>
.computed-demo {
  border: 2px solid #28a745;
  padding: 20px;
  border-radius: 8px;
}

.basic-computed, .setter-computed, .complex-computed {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
}

button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
}
</style>
```

### 2.4 watch() 和 watchEffect()

监听器用于响应数据变化。

```vue
<template>
  <div class="watch-demo">
    <h2>watch() 和 watchEffect() 演示</h2>
    
    <div class="basic-watch">
      <h3>基本watch</h3>
      <p>计数: {{ count }}</p>
      <p>消息: {{ message }}</p>
      <button @click="count++">增加计数</button>
      <button @click="message += '!'">修改消息</button>
      <div class="log">
        <h4>监听日志:</h4>
        <div v-for="(log, index) in watchLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
    
    <div class="watch-effect">
      <h3>watchEffect</h3>
      <p>effect计数: {{ effectCount }}</p>
      <p>effect消息: {{ effectMessage }}</p>
      <button @click="effectCount++">增加effect计数</button>
      <button @click="effectMessage += '?'">修改effect消息</button>
      <div class="log">
        <h4>Effect日志:</h4>
        <div v-for="(log, index) in effectLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
    
    <div class="deep-watch">
      <h3>深度监听</h3>
      <p>用户名: {{ user.profile.name }}</p>
      <p>用户年龄: {{ user.profile.age }}</p>
      <button @click="updateUserProfile">更新用户资料</button>
      <div class="log">
        <h4>深度监听日志:</h4>
        <div v-for="(log, index) in deepWatchLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, watchEffect } from 'vue'

// 基本watch
const count = ref(0)
const message = ref('Hello')
const watchLogs = ref([])

watch(count, (newVal, oldVal) => {
  const log = `count变化: ${oldVal} -> ${newVal}`
  watchLogs.value.push(log)
  console.log(log)
})

watch(message, (newVal, oldVal) => {
  const log = `message变化: "${oldVal}" -> "${newVal}"`
  watchLogs.value.push(log)
  console.log(log)
})

// watchEffect
const effectCount = ref(0)
const effectMessage = ref('Effect')
const effectLogs = ref([])

watchEffect(() => {
  const log = `watchEffect执行: count=${effectCount.value}, message="${effectMessage.value}"`
  effectLogs.value.push(log)
  console.log(log)
})

// 深度监听
const user = ref({
  profile: {
    name: '张三',
    age: 25
  }
})

const deepWatchLogs = ref([])

watch(user, (newVal, oldVal) => {
  const log = `用户资料变化: ${JSON.stringify(newVal.profile)}`
  deepWatchLogs.value.push(log)
  console.log(log)
}, { deep: true })

function updateUserProfile() {
  user.value.profile = {
    name: '李四',
    age: 30
  }
}
</script>

<style scoped>
.watch-demo {
  border: 2px solid #ffc107;
  padding: 20px;
  border-radius: 8px;
}

.basic-watch, .watch-effect, .deep-watch {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.log {
  margin-top: 15px;
  padding: 10px;
  background-color: #e9ecef;
  border-radius: 4px;
  max-height: 150px;
  overflow-y: auto;
}

.log-item {
  padding: 5px 0;
  border-bottom: 1px solid #dee2e6;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}

button {
  background-color: #ffc107;
  color: black;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

button:hover {
  background-color: #e0a800;
}
</style>
```

## 3. 响应式工具函数

### 3.1 toRefs() 和 toRef()

将reactive对象的属性转换为ref。

```vue
<template>
  <div class="to-refs-demo">
    <h2>toRefs() 和 toRef() 演示</h2>
    
    <div class="reactive-object">
      <h3>原始reactive对象</h3>
      <p>姓名: {{ state.name }}</p>
      <p>年龄: {{ state.age }}</p>
      <p>城市: {{ state.city }}</p>
      <button @click="updateOriginal">更新原始对象</button>
    </div>
    
    <div class="destructured">
      <h3>解构后的属性（失去响应性）</h3>
      <p>姓名: {{ name }} (失去响应性)</p>
      <p>年龄: {{ age }} (失去响应性)</p>
      <button @click="updateDestructured">更新解构属性</button>
    </div>
    
    <div class="to-refs">
      <h3>使用toRefs()后的属性</h3>
      <p>姓名: {{ nameRef }} (保持响应性)</p>
      <p>年龄: {{ ageRef }} (保持响应性)</p>
      <button @click="updateToRefs">更新toRefs属性</button>
    </div>
    
    <div class="to-ref">
      <h3>使用toRef()后的属性</h3>
      <p>城市: {{ cityRef }} (保持响应性)</p>
      <button @click="updateToRef">更新toRef属性</button>
    </div>
  </div>
</template>

<script setup>
import { reactive, toRefs, toRef } from 'vue'

// 原始reactive对象
const state = reactive({
  name: '张三',
  age: 25,
  city: '北京'
})

// 解构属性（失去响应性）
const { name, age } = state

// 使用toRefs()（保持响应性）
const { name: nameRef, age: ageRef } = toRefs(state)

// 使用toRef()（保持响应性）
const cityRef = toRef(state, 'city')

function updateOriginal() {
  state.name = '李四'
  state.age = 30
  state.city = '上海'
}

function updateDestructured() {
  // 这不会触发视图更新，因为解构后的属性失去了响应性
  name = '王五'
  age = 35
  console.log('解构属性更新（不会触发视图更新）:', name, age)
}

function updateToRefs() {
  // 这会触发视图更新，因为toRefs保持了响应性
  nameRef.value = '赵六'
  ageRef.value = 40
}

function updateToRef() {
  // 这会触发视图更新，因为toRef保持了响应性
  cityRef.value = '广州'
}
</script>

<style scoped>
.to-refs-demo {
  border: 2px solid #17a2b8;
  padding: 20px;
  border-radius: 8px;
}

.reactive-object, .destructured, .to-refs, .to-ref {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

button {
  background-color: #17a2b8;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #138496;
}
</style>
```

### 3.2 readonly() 和 shallowReadonly()

创建只读的响应式对象。

```vue
<template>
  <div class="readonly-demo">
    <h2>readonly() 和 shallowReadonly() 演示</h2>
    
    <div class="basic-readonly">
      <h3>基本readonly</h3>
      <p>只读值: {{ readonlyValue }}</p>
      <p>原始值: {{ originalValue }}</p>
      <button @click="updateOriginal">更新原始值</button>
      <button @click="tryUpdateReadonly">尝试更新只读值</button>
    </div>
    
    <div class="object-readonly">
      <h3>对象readonly</h3>
      <p>只读对象: {{ readonlyObject.name }} - {{ readonlyObject.age }}</p>
      <p>原始对象: {{ originalObject.name }} - {{ originalObject.age }}</p>
      <button @click="updateOriginalObject">更新原始对象</button>
      <button @click="tryUpdateReadonlyObject">尝试更新只读对象</button>
    </div>
    
    <div class="shallow-readonly">
      <h3>shallowReadonly</h3>
      <p>浅只读对象: {{ shallowReadonlyObject.name }} - {{ shallowReadonlyObject.nested.value }}</p>
      <button @click="tryUpdateShallow">尝试更新浅只读对象</button>
      <button @click="tryUpdateNested">尝试更新嵌套对象</button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, readonly, shallowReadonly } from 'vue'

// 基本readonly
const originalValue = ref('Hello')
const readonlyValue = readonly(originalValue)

// 对象readonly
const originalObject = reactive({
  name: '张三',
  age: 25
})

const readonlyObject = readonly(originalObject)

// shallowReadonly
const shallowReadonlyObject = shallowReadonly({
  name: '李四',
  nested: {
    value: '嵌套值'
  }
})

function updateOriginal() {
  originalValue.value = 'Updated Hello'
}

function tryUpdateReadonly() {
  try {
    readonlyValue.value = 'This will fail'
  } catch (error) {
    console.error('更新只读值失败:', error.message)
    alert('无法更新只读值: ' + error.message)
  }
}

function updateOriginalObject() {
  originalObject.name = '王五'
  originalObject.age = 30
}

function tryUpdateReadonlyObject() {
  try {
    readonlyObject.name = 'This will fail'
  } catch (error) {
    console.error('更新只读对象失败:', error.message)
    alert('无法更新只读对象: ' + error.message)
  }
}

function tryUpdateShallow() {
  try {
    shallowReadonlyObject.name = 'This will fail'
  } catch (error) {
    console.error('更新浅只读对象失败:', error.message)
    alert('无法更新浅只读对象: ' + error.message)
  }
}

function tryUpdateNested() {
  // 浅只读对象的嵌套属性可以被修改
  shallowReadonlyObject.nested.value = '更新后的嵌套值'
  console.log('嵌套属性已更新')
}
</script>

<style scoped>
.readonly-demo {
  border: 2px solid #6f42c1;
  padding: 20px;
  border-radius: 8px;
}

.basic-readonly, .object-readonly, .shallow-readonly {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

button {
  background-color: #6f42c1;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #5a32a3;
}
</style>
```

## 4. 生命周期钩子

### 4.1 基本生命周期钩子

```vue
<template>
  <div class="lifecycle-demo">
    <h2>生命周期钩子演示</h2>
    
    <div class="status">
      <p>当前状态: {{ currentStatus }}</p>
      <p>计数: {{ counter }}</p>
      <p>创建时间: {{ createdAt }}</p>
      <p>更新次数: {{ updateCount }}</p>
    </div>
    
    <div class="controls">
      <button @click="counter++">增加计数</button>
      <button @click="forceUpdate">强制更新</button>
      <button @click="showChild = !showChild">
        {{ showChild ? '隐藏子组件' : '显示子组件' }}
      </button>
    </div>
    
    <div class="logs">
      <h3>生命周期日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in lifecycleLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
    
    <lifecycle-child v-if="showChild"></lifecycle-child>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'
import LifecycleChild from './LifecycleChild.vue'

const counter = ref(0)
const currentStatus = ref('初始化')
const createdAt = ref('')
const updateCount = ref(0)
const showChild = ref(true)
const lifecycleLogs = ref([])

function addLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = `[${timestamp}] ${message}`
  lifecycleLogs.value.push(log)
  console.log(log)
}

// 生命周期钩子
onBeforeMount(() => {
  addLog('onBeforeMount: 组件挂载前')
  currentStatus.value = '准备挂载'
})

onMounted(() => {
  addLog('onMounted: 组件挂载完成')
  currentStatus.value = '已挂载'
  createdAt.value = new Date().toLocaleString()
  
  // 模拟异步操作
  setTimeout(() => {
    addLog('异步操作完成')
    currentStatus.value = '数据加载完成'
  }, 1000)
})

onBeforeUpdate(() => {
  addLog('onBeforeUpdate: 组件更新前')
  currentStatus.value = '准备更新'
})

onUpdated(() => {
  addLog('onUpdated: 组件更新完成')
  currentStatus.value = '已更新'
  updateCount.value++
})

onBeforeUnmount(() => {
  addLog('onBeforeUnmount: 组件卸载前')
  currentStatus.value = '准备卸载'
})

onUnmounted(() => {
  addLog('onUnmounted: 组件卸载完成')
  currentStatus.value = '已卸载'
})

function forceUpdate() {
  // 触发更新
  const temp = counter.value
  counter.value = 0
  setTimeout(() => {
    counter.value = temp
  }, 0)
}
</script>

<style scoped>
.lifecycle-demo {
  border: 2px solid #fd7e14;
  padding: 20px;
  border-radius: 8px;
}

.status {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 20px;
}

.controls {
  margin-bottom: 20px;
}

.controls button {
  margin-right: 10px;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.controls button:first-child {
  background-color: #fd7e14;
  color: white;
}

.controls button:nth-child(2) {
  background-color: #ffc107;
  color: black;
}

.controls button:last-child {
  background-color: #dc3545;
  color: white;
}

.logs {
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
}

.logs h3 {
  margin: 0;
  padding: 10px;
  background-color: #f8f9fa;
  border-bottom: 1px solid #ddd;
}

.log-content {
  max-height: 300px;
  overflow-y: auto;
  padding: 10px;
}

.log-item {
  padding: 5px 0;
  border-bottom: 1px solid #eee;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}
</style>
```

### 4.2 特殊生命周期钩子

```vue
<template>
  <div class="special-lifecycle">
    <h2>特殊生命周期钩子演示</h2>
    
    <div class="error-boundary">
      <h3>错误捕获 (onErrorCaptured)</h3>
      <button @click="triggerError">触发错误</button>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>
    
    <div class="keep-alive-demo">
      <h3>Keep-alive 组件 (onActivated/onDeactivated)</h3>
      <keep-alive>
        <keep-alive-child v-if="showKeepAlive"></keep-alive-child>
      </keep-alive>
      <button @click="showKeepAlive = !showKeepAlive">
        {{ showKeepAlive ? '隐藏' : '显示' }} Keep-alive 组件
      </button>
    </div>
    
    <div class="render-tracked">
      <h3>渲染追踪 (onRenderTracked/onRenderTriggered)</h3>
      <p>追踪次数: {{ trackCount }}</p>
      <p>触发次数: {{ triggerCount }}</p>
      <button @click="triggerRender">触发渲染</button>
      <div class="render-logs">
        <div v-for="(log, index) in renderLogs" :key="index" class="render-log">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onErrorCaptured, onRenderTracked, onRenderTriggered } from 'vue'
import KeepAliveChild from './KeepAliveChild.vue'

const errorMessage = ref('')
const showKeepAlive = ref(true)
const trackCount = ref(0)
const triggerCount = ref(0)
const renderLogs = ref([])

function addRenderLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = `[${timestamp}] ${message}`
  renderLogs.value.push(log)
  console.log(log)
}

// 错误捕获
onErrorCaptured((err, instance, info) => {
  console.error('错误捕获:', err)
  errorMessage.value = `错误: ${err.message}`
  // 返回false阻止错误继续传播
  return false
})

// 渲染追踪
onRenderTracked((e) => {
  trackCount.value++
  addRenderLog(`onRenderTracked: ${e.type} - ${e.key}`)
})

onRenderTriggered((e) => {
  triggerCount.value++
  addRenderLog(`onRenderTriggered: ${e.type} - ${e.key}`)
})

function triggerError() {
  // 故意触发错误
  throw new Error('这是一个测试错误')
}

function triggerRender() {
  // 触发渲染
  const temp = trackCount.value
  trackCount.value = 0
  setTimeout(() => {
    trackCount.value = temp
  }, 0)
}
</script>

<style scoped>
.special-lifecycle {
  border: 2px solid #e83e8c;
  padding: 20px;
  border-radius: 8px;
}

.error-boundary, .keep-alive-demo, .render-tracked {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.error-message {
  color: #dc3545;
  background-color: #f8d7da;
  padding: 10px;
  border-radius: 4px;
  margin-top: 10px;
}

button {
  background-color: #e83e8c;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #d91a72;
}

.render-logs {
  margin-top: 15px;
  max-height: 200px;
  overflow-y: auto;
  background-color: #f1f3f5;
  padding: 10px;
  border-radius: 4px;
}

.render-log {
  padding: 5px 0;
  border-bottom: 1px solid #dee2e6;
  font-family: monospace;
  font-size: 12px;
}

.render-log:last-child {
  border-bottom: none;
}
</style>
```

## 5. 组合式函数（Composables）

### 5.1 基本Composables

```vue
<!-- composables/useCounter.js -->
<script>
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  const doubleCount = computed(() => count.value * 2)
  
  function increment() {
    count.value++
  }
  
  function decrement() {
    count.value--
  }
  
  function reset() {
    count.value = initialValue
  }
  
  function setValue(newValue) {
    count.value = newValue
  }
  
  return {
    count,
    doubleCount,
    increment,
    decrement,
    reset,
    setValue
  }
}
</script>
```

```vue
<!-- composables/useLocalStorage.js -->
<script>
import { ref, watch, onMounted } from 'vue'

export function useLocalStorage(key, defaultValue) {
  const storedValue = ref(defaultValue)
  
  // 从localStorage读取值
  function readValue() {
    try {
      const value = localStorage.getItem(key)
      if (value) {
        storedValue.value = JSON.parse(value)
      }
    } catch (error) {
      console.error(`读取localStorage失败: ${error.message}`)
    }
  }
  
  // 写入localStorage
  function writeValue(value) {
    try {
      localStorage.setItem(key, JSON.stringify(value))
    } catch (error) {
      console.error(`写入localStorage失败: ${error.message}`)
    }
  }
  
  // 监听值变化
  watch(storedValue, (newValue) => {
    writeValue(newValue)
  }, { deep: true })
  
  // 组件挂载时读取值
  onMounted(() => {
    readValue()
  })
  
  return {
    storedValue,
    readValue,
    writeValue
  }
}
</script>
```

### 5.2 在组件中使用Composables

```vue
<template>
  <div class="composables-demo">
    <h2>组合式函数（Composables）演示</h2>
    
    <div class="counter-section">
      <h3>计数器Composable</h3>
      <div class="counter-display">
        <p>计数: {{ counter.count }}</p>
        <p>双倍计数: {{ counter.doubleCount }}</p>
      </div>
      <div class="counter-controls">
        <button @click="counter.increment">增加</button>
        <button @click="counter.decrement">减少</button>
        <button @click="counter.reset">重置</button>
        <button @click="setCounterValue">设置值</button>
      </div>
    </div>
    
    <div class="storage-section">
      <h3>LocalStorage Composable</h3>
      <div class="storage-display">
        <p>存储的值: {{ storedValue.storedValue }}</p>
        <input 
          v-model="inputValue" 
          placeholder="输入要存储的值"
          @keyup.enter="saveToStorage"
        />
        <button @click="saveToStorage">保存</button>
        <button @click="clearStorage">清除</button>
      </div>
    </div>
    
    <div class="multiple-counters">
      <h3>多个独立的计数器</h3>
      <div class="counter-group">
        <div class="counter-item">
          <h4>计数器A</h4>
          <p>{{ counterA.count }}</p>
          <button @click="counterA.increment">+1</button>
        </div>
        <div class="counter-item">
          <h4>计数器B</h4>
          <p>{{ counterB.count }}</p>
          <button @click="counterB.increment">+1</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useCounter } from '@/composables/useCounter'
import { useLocalStorage } from '@/composables/useLocalStorage'

// 使用计数器composable
const counter = useCounter(0)

// 使用多个独立的计数器
const counterA = useCounter(10)
const counterB = useCounter(20)

// 使用localStorage composable
const storedValue = useLocalStorage('user-preferences', { theme: 'light', language: 'zh-CN' })
const inputValue = ref('')

function setCounterValue() {
  const newValue = prompt('请输入新的计数值:')
  if (newValue !== null) {
    counter.setValue(parseInt(newValue) || 0)
  }
}

function saveToStorage() {
  if (inputValue.value.trim()) {
    try {
      const parsedValue = JSON.parse(inputValue.value)
      storedValue.storedValue.value = parsedValue
      inputValue.value = ''
      alert('保存成功!')
    } catch (error) {
      alert('请输入有效的JSON格式')
    }
  }
}

function clearStorage() {
  storedValue.storedValue.value = { theme: 'light', language: 'zh-CN' }
  alert('已清除存储的值')
}
</script>

<style scoped>
.composables-demo {
  border: 2px solid #20c997;
  padding: 20px;
  border-radius: 8px;
}

.counter-section, .storage-section, .multiple-counters {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.counter-display, .storage-display {
  margin-bottom: 15px;
}

.counter-controls, .storage-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.counter-group {
  display: flex;
  gap: 20px;
}

.counter-item {
  padding: 15px;
  background-color: white;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  text-align: center;
  min-width: 120px;
}

.counter-item h4 {
  margin-top: 0;
  color: #20c997;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
  width: 200px;
}

button {
  background-color: #20c997;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #1aa179;
}
</style>
```

## 6. 高级API

### 6.1 依赖注入（Provide/Inject）

```vue
<!-- 父组件 -->
<template>
  <div class="provide-inject-demo">
    <h2>依赖注入（Provide/Inject）演示</h2>
    
    <div class="theme-provider">
      <h3>主题提供者</h3>
      <p>当前主题: {{ currentTheme }}</p>
      <button @click="changeTheme('light')">浅色主题</button>
      <button @click="changeTheme('dark')">深色主题</button>
      <button @click="changeTheme('ocean')">海洋主题</button>
    </div>
    
    <div class="user-provider">
      <h3>用户信息提供者</h3>
      <p>当前用户: {{ currentUser.name }}</p>
      <button @click="updateUser">更新用户信息</button>
    </div>
    
    <child-component></child-component>
    
    <deep-child></deep-child>
  </div>
</template>

<script setup>
import { ref, provide, readonly } from 'vue'
import ChildComponent from './ChildComponent.vue'
import DeepChild from './DeepChild.vue'

// 提供主题
const currentTheme = ref('light')
const themes = {
  light: {
    primary: '#42b983',
    background: '#ffffff',
    text: '#333333'
  },
  dark: {
    primary: '#61dafb',
    background: '#1a1a1a',
    text: '#ffffff'
  },
  ocean: {
    primary: '#0077be',
    background: '#e6f3ff',
    text: '#003d5c'
  }
}

// 提供只读的主题
provide('theme', readonly(currentTheme))
provide('themes', themes)

// 提供更新主题的方法
provide('changeTheme', (theme) => {
  currentTheme.value = theme
})

// 提供用户信息
const currentUser = ref({
  name: '张三',
  role: 'admin',
  permissions: ['read', 'write', 'delete']
})

provide('currentUser', currentUser)

// 提供用户操作方法
provide('updateUser', (newUser) => {
  currentUser.value = { ...currentUser.value, ...newUser }
})

function changeTheme(theme) {
  currentTheme.value = theme
}

function updateUser() {
  const newName = prompt('请输入新的用户名:')
  if (newName) {
    currentUser.value.name = newName
  }
}
</script>

<style scoped>
.provide-inject-demo {
  border: 2px solid #6f42c1;
  padding: 20px;
  border-radius: 8px;
}

.theme-provider, .user-provider {
  margin: 15px 0;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

button {
  background-color: #6f42c1;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

button:hover {
  background-color: #5a32a3;
}
</style>
```

```vue
<!-- 子组件 ChildComponent.vue -->
<template>
  <div class="child-component">
    <h3>子组件</h3>
    <p>注入的主题: {{ theme }}</p>
    <p>注入的用户: {{ currentUser.name }}</p>
    
    <div class="theme-display" :style="themeStyles">
      <p>这是使用注入主题的样式</p>
    </div>
    
    <button @click="useInjectedMethod">使用注入的方法</button>
  </div>
</template>

<script setup>
import { inject, computed } from 'vue'

// 注入值
const theme = inject('theme')
const themes = inject('themes')
const currentUser = inject('currentUser')
const changeTheme = inject('changeTheme')

// 计算主题样式
const themeStyles = computed(() => {
  const currentTheme = themes[theme.value]
  return {
    backgroundColor: currentTheme.background,
    color: currentTheme.text,
    padding: '15px',
    borderRadius: '8px',
    border: `2px solid ${currentTheme.primary}`
  }
})

function useInjectedMethod() {
  // 切换主题
  const themeNames = Object.keys(themes)
  const currentIndex = themeNames.indexOf(theme.value)
  const nextIndex = (currentIndex + 1) % themeNames.length
  changeTheme(themeNames[nextIndex])
}
</script>

<style scoped>
.child-component {
  border: 2px solid #17a2b8;
  padding: 20px;
  border-radius: 8px;
  margin: 20px 0;
}

button {
  background-color: #17a2b8;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #138496;
}
</style>
```

### 6.2 模板引用（Template Refs）

```vue
<template>
  <div class="template-refs-demo">
    <h2>模板引用（Template Refs）演示</h2>
    
    <div class="basic-ref">
      <h3>基本模板引用</h3>
      <div ref="basicElement" class="basic-element">
        这是一个基本的DOM元素
      </div>
      <button @click="accessBasicRef">访问基本引用</button>
      <button @click="modifyBasicRef">修改基本引用</button>
    </div>
    
    <div class="component-ref">
      <h3>组件引用</h3>
      <ref-child ref="childComponent"></ref-child>
      <button @click="accessComponentRef">访问组件引用</button>
      <button @click="callComponentMethod">调用组件方法</button>
    </div>
    
    <div class="v-for-ref">
      <h3>v-for 中的引用</h3>
      <div v-for="(item, index) in items" :key="item.id" :ref="el => setItemRef(el, index)">
        项目 {{ index + 1 }}: {{ item.name }}
      </div>
      <button @click="accessForRefs">访问v-for引用</button>
      <button @click="clearForRefs">清除v-for引用</button>
    </div>
    
    <div class="ref-info">
      <h3>引用信息</h3>
      <div v-if="refInfo" class="info-display">
        <p>类型: {{ refInfo.type }}</p>
        <p>标签名: {{ refInfo.tagName }}</p>
        <p>内容: {{ refInfo.content }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import RefChild from './RefChild.vue'

// 基本模板引用
const basicElement = ref(null)
const refInfo = ref(null)

// 组件引用
const childComponent = ref(null)

// v-for 引用
const items = ref([
  { id: 1, name: '苹果' },
  { id: 2, name: '香蕉' },
  { id: 3, name: '橙子' }
])

const itemRefs = ref([])

function setItemRef(el, index) {
  if (el) {
    itemRefs.value[index] = el
  } else {
    itemRefs.value.splice(index, 1)
  }
}

function accessBasicRef() {
  if (basicElement.value) {
    refInfo.value = {
      type: 'DOM元素',
      tagName: basicElement.value.tagName,
      content: basicElement.value.textContent
    }
    console.log('基本引用:', basicElement.value)
  }
}

function modifyBasicRef() {
  if (basicElement.value) {
    basicElement.value.style.backgroundColor = '#42b983'
    basicElement.value.style.color = 'white'
    basicElement.value.textContent = '修改后的DOM元素'
  }
}

function accessComponentRef() {
  if (childComponent.value) {
    refInfo.value = {
      type: '组件',
      tagName: '组件实例',
      content: childComponent.value.$el?.textContent || '无内容'
    }
    console.log('组件引用:', childComponent.value)
  }
}

function callComponentMethod() {
  if (childComponent.value) {
    childComponent.value.childMethod()
    refInfo.value = {
      type: '组件方法调用',
      tagName: '方法执行',
      content: '子组件方法已被调用'
    }
  }
}

function accessForRefs() {
  console.log('v-for 引用:', itemRefs.value)
  refInfo.value = {
    type: 'v-for 引用',
    tagName: '数组',
    content: `找到 ${itemRefs.value.length} 个元素`
  }
  
  // 为每个元素添加样式
  itemRefs.value.forEach((el, index) => {
    if (el) {
      el.style.backgroundColor = `hsl(${index * 60}, 70%, 80%)`
    }
  })
}

function clearForRefs() {
  itemRefs.value = []
  refInfo.value = {
    type: 'v-for 引用',
    tagName: '数组',
    content: 'v-for 引用已清除'
  }
}

// 组件挂载后自动访问引用
onMounted(() => {
  console.log('组件挂载完成，可以访问模板引用')
  accessBasicRef()
})
</script>

<style scoped>
.template-refs-demo {
  border: 2px solid #fd7e14;
  padding: 20px;
  border-radius: 8px;
}

.basic-ref, .component-ref, .v-for-ref, .ref-info {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.basic-element {
  padding: 15px;
  background-color: #e9ecef;
  border-radius: 4px;
  margin: 10px 0;
}

.v-for-ref div {
  padding: 10px;
  margin: 5px 0;
  background-color: #e9ecef;
  border-radius: 4px;
}

.info-display {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
  border-left: 4px solid #2196f3;
}

button {
  background-color: #fd7e14;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

button:hover {
  background-color: #e66a00;
}
</style>
```

## 7. 性能优化

### 7.1 响应式性能优化

```vue
<template>
  <div class="performance-optimization">
    <h2>响应式性能优化演示</h2>
    
    <div class="shallow-reactive">
      <h3>浅层响应式 (shallowReactive)</h3>
      <p>外层属性: {{ shallowState.name }}</p>
      <p>内层属性: {{ shallowState.nested.value }}</p>
      <button @click="updateShallowOuter">更新外层属性</button>
      <button @click="updateShallowInner">更新内层属性</button>
      <p>外层更新次数: {{ shallowOuterCount }}</p>
      <p>内层更新次数: {{ shallowInnerCount }}</p>
    </div>
    
    <div class="shallow-ref">
      <h3>浅层引用 (shallowRef)</h3>
      <p>值: {{ shallowRefValue.value }}</p>
      <button @click="updateShallowRef">更新引用</button>
      <button @click="updateShallowRefProperty">更新属性</button>
      <p>引用更新次数: {{ shallowRefCount }}</p>
      <p>属性更新次数: {{ shallowRefPropertyCount }}</p>
    </div>
    
    <div class="mark-raw">
      <h3>标记原始对象 (markRaw)</h3>
      <p>普通对象: {{ normalObject.name }}</p>
      <p>标记原始对象: {{ rawObject.name }}</p>
      <button @click="updateBothObjects">更新两个对象</button>
      <p>普通对象是否响应式: {{ isNormalObjectReactive }}</p>
      <p>原始对象是否响应式: {{ isRawObjectReactive }}</p>
    </div>
    
    <div class="effect-scope">
      <h3>副作用作用域 (effectScope)</h3>
      <p>作用域计数: {{ scopeCount }}</p>
      <p>普通计数: {{ normalCount }}</p>
      <button @click="incrementScope">增加作用域计数</button>
      <button @click="incrementNormal">增加普通计数</button>
      <button @click="stopScope">停止作用域</button>
    </div>
  </div>
</template>

<script setup>
import { 
  ref, 
  reactive, 
  shallowReactive, 
  shallowRef, 
  markRaw, 
  isReactive,
  effectScope,
  watch 
} from 'vue'

// 浅层响应式
const shallowState = shallowReactive({
  name: '张三',
  nested: {
    value: '嵌套值'
  }
})

const shallowOuterCount = ref(0)
const shallowInnerCount = ref(0)

watch(() => shallowState.name, () => {
  shallowOuterCount.value++
})

watch(() => shallowState.nested.value, () => {
  shallowInnerCount.value++
})

function updateShallowOuter() {
  shallowState.name = '李四'
}

function updateShallowInner() {
  shallowState.nested.value = '更新后的嵌套值'
}

// 浅层引用
const shallowRefValue = shallowRef({
  count: 0,
  nested: {
    value: '浅层引用嵌套值'
  }
})

const shallowRefCount = ref(0)
const shallowRefPropertyCount = ref(0)

watch(shallowRefValue, () => {
  shallowRefCount.value++
}, { deep: false })

watch(() => shallowRefValue.value.count, () => {
  shallowRefPropertyCount.value++
})

function updateShallowRef() {
  shallowRefValue.value = {
    count: shallowRefValue.value.count + 1,
    nested: {
      value: '更新后的浅层引用嵌套值'
    }
  }
}

function updateShallowRefProperty() {
  shallowRefValue.value.count++
  shallowRefValue.value.nested.value = '更新属性后的值'
}

// 标记原始对象
const normalObject = reactive({
  name: '普通对象'
})

const rawObject = markRaw({
  name: '原始对象'
})

const isNormalObjectReactive = computed(() => isReactive(normalObject))
const isRawObjectReactive = computed(() => isReactive(rawObject))

function updateBothObjects() {
  normalObject.name = '更新的普通对象'
  rawObject.name = '更新的原始对象'
}

// 副作用作用域
const scopeCount = ref(0)
const normalCount = ref(0)
let scope

function createScope() {
  scope = effectScope()
  
  scope.run(() => {
    const count = ref(0)
    
    watch(count, (newVal) => {
      scopeCount.value = newVal
    })
    
    function increment() {
      count.value++
    }
    
    // 暴露increment函数
    window.incrementScope = increment
  })
}

function incrementScope() {
  if (window.incrementScope) {
    window.incrementScope()
  }
}

function incrementNormal() {
  normalCount.value++
}

function stopScope() {
  if (scope) {
    scope.stop()
    scope = null
    delete window.incrementScope
  }
}

// 创建作用域
createScope()
</script>

<style scoped>
.performance-optimization {
  border: 2px solid #dc3545;
  padding: 20px;
  border-radius: 8px;
}

.shallow-reactive, .shallow-ref, .mark-raw, .effect-scope {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

button {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

button:hover {
  background-color: #c82333;
}
</style>
```

## 8. 最佳实践和总结

### 8.1 组合式API最佳实践

```vue
<template>
  <div class="best-practices">
    <h2>组合式API最佳实践</h2>
    
    <div class="code-organization">
      <h3>代码组织</h3>
      <p>计数: {{ counter.count }}</p>
      <p>主题: {{ currentTheme }}</p>
      <p>用户: {{ user.name }}</p>
      <button @click="counter.increment">增加计数</button>
      <button @click="changeTheme">切换主题</button>
    </div>
    
    <div class="type-safety">
      <h3>类型安全</h3>
      <p>类型化产品: {{ typedProduct.name }} - {{ typedProduct.price }}</p>
      <button @click="updateTypedProduct">更新类型化产品</button>
    </div>
    
    <div class="error-handling">
      <h3>错误处理</h3>
      <p>加载状态: {{ loadingStatus }}</p>
      <p>错误信息: {{ errorMessage }}</p>
      <button @click="simulateApiCall">模拟API调用</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useCounter } from '@/composables/useCounter'

// 1. 使用composables组织逻辑
const counter = useCounter(0)

// 2. 按功能组织代码
// 主题相关逻辑
const themes = ['light', 'dark', 'ocean']
const currentTheme = ref('light')

function changeTheme() {
  const currentIndex = themes.indexOf(currentTheme.value)
  const nextIndex = (currentIndex + 1) % themes.length
  currentTheme.value = themes[nextIndex]
}

// 用户相关逻辑
const user = ref({
  name: '张三',
  email: 'zhangsan@example.com'
})

// 3. 类型安全（TypeScript）
interface Product {
  id: number
  name: string
  price: number
  category: string
}

const typedProduct = ref<Product>({
  id: 1,
  name: 'Vue3书籍',
  price: 89.99,
  category: '技术'
})

function updateTypedProduct() {
  typedProduct.value = {
    ...typedProduct.value,
    price: typedProduct.value.price + 10
  }
}

// 4. 错误处理
const loadingStatus = ref('idle')
const errorMessage = ref('')

async function simulateApiCall() {
  loadingStatus.value = 'loading'
  errorMessage.value = ''
  
  try {
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // 模拟成功或失败
    const shouldFail = Math.random() > 0.5
    
    if (shouldFail) {
      throw new Error('API调用失败')
    }
    
    loadingStatus.value = 'success'
  } catch (error) {
    loadingStatus.value = 'error'
    errorMessage.value = error.message
  }
}

// 5. 性能优化
const expensiveComputed = computed(() => {
  // 这里放置昂贵的计算
  console.log('执行昂贵的计算')
  return `计算结果: ${counter.count * 100}`
})
</script>

<style scoped>
.best-practices {
  border: 2px solid #28a745;
  padding: 20px;
  border-radius: 8px;
}

.code-organization, .type-safety, .error-handling {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

button:hover {
  background-color: #218838;
}
</style>
```

### 8.2 总结

Vue3组合式API提供了强大而灵活的方式来组织组件逻辑：

#### 核心优势
- **逻辑集中**：相关逻辑可以组织在一起，提高可维护性
- **更好的复用性**：通过composables实现逻辑复用
- **更好的TypeScript支持**：提供更好的类型推断
- **更灵活的代码组织**：按功能而非选项组织代码

#### 主要API
- **响应式系统**：ref(), reactive(), computed(), watch(), watchEffect()
- **生命周期**：onMounted(), onUpdated(), onUnmounted()等
- **工具函数**：toRefs(), toRef(), readonly(), markRaw()等
- **依赖注入**：provide(), inject()
- **模板引用**：ref() 用于DOM和组件引用

#### 最佳实践
1. **使用composables**组织可复用逻辑
2. **按功能组织代码**而非按选项
3. **合理使用响应式API**选择ref或reactive
4. **注意性能优化**使用shallowRef、markRaw等
5. **完善的错误处理**和类型安全

通过掌握Vue3组合式API，你可以构建更加现代化、可维护、高性能的Vue应用。在实际项目中，组合式API特别适合复杂组件和大型应用的开发。
