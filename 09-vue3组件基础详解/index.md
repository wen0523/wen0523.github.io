# Vue3组件基础详解


# Vue3组件基础详解

## 概述

Vue3组件是构建现代Web应用的核心概念。组件是独立的、可复用的Vue实例，每个组件都包含自己的模板、逻辑和样式。通过组件系统，我们可以构建复杂而可维护的用户界面。

## 1. 组件基础概念

### 1.1 什么是组件

组件是Vue应用的基本构建块，它是一个自包含的、可复用的UI单元。每个组件都有：

- **模板（Template）**：定义组件的HTML结构
- **脚本（Script）**：包含组件的逻辑、数据和方法
- **样式（Style）**：定义组件的外观和样式

### 1.2 组件的特点

| 特性 | 描述 |
|------|------|
| **复用性** | 可以在不同地方多次使用，提高代码复用性 |
| **封装性** | 拥有独立的作用域，避免全局污染 |
| **组合性** | 可以组合多个小组件构建复杂界面 |
| **响应式** | 数据与视图双向绑定，自动更新视图 |
| **模块化** | 支持模块化开发，便于管理和维护 |

## 2. 组件的创建和注册

### 2.1 全局组件注册

全局组件在整个应用中都可以使用。

```vue
<template>
  <div id="app">
    <global-component></global-component>
    <another-global></another-global>
  </div>
</template>

<script>
import { createApp } from 'vue'

// 全局组件定义
const GlobalComponent = {
  template: '<div>这是全局组件</div>'
}

const app = createApp({})

// 注册全局组件
app.component('global-component', GlobalComponent)

// 直接定义并注册全局组件
app.component('another-global', {
  template: '<div>另一个全局组件</div>'
})

app.mount('#app')
</script>
```

### 2.2 局部组件注册

局部组件只在注册它们的组件中可用。

```vue
<template>
  <div class="parent-component">
    <h2>父组件</h2>
    <local-child></local-child>
    <another-child></another-child>
  </div>
</template>

<script>
import { ref } from 'vue'

// 局部组件定义
const LocalChild = {
  template: '<div>这是局部子组件</div>'
}

const AnotherChild = {
  template: '<div>另一个局部子组件</div>'
}

export default {
  name: 'ParentComponent',
  components: {
    LocalChild,
    AnotherChild
  },
  setup() {
    const message = ref('Hello from parent')
    return { message }
  }
}
</script>

<style scoped>
.parent-component {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}
</style>
```

### 2.3 单文件组件（.vue）

单文件组件是Vue推荐的开发方式，将模板、脚本和样式组织在一个文件中。

```vue
<!-- MyComponent.vue -->
<template>
  <div class="my-component">
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
    <button @click="handleClick">{{ buttonText }}</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 响应式数据
const title = ref('我的组件')
const description = ref('这是一个单文件组件示例')
const buttonText = ref('点击我')

// 方法
function handleClick() {
  buttonText.value = '按钮被点击了！'
  setTimeout(() => {
    buttonText.value = '点击我'
  }, 2000)
}
</script>

<style scoped>
.my-component {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  background-color: #f9f9f9;
}

.my-component h3 {
  color: #42b983;
  margin-top: 0;
}

.my-component button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.my-component button:hover {
  background-color: #35495e;
}
</style>
```

## 3. 组件通信

### 3.1 Props（父组件向子组件传递数据）

Props是父组件向子组件传递数据的主要方式。

#### 基本Props使用

```vue
<!-- 父组件 -->
<template>
  <div class="parent">
    <h2>父组件</h2>
    <child-component 
      title="从父组件传递的标题"
      :count="parentCount"
      :user="userData"
      :items="itemList"
      @update-count="handleUpdateCount"
    ></child-component>
    
    <div class="controls">
      <p>父组件计数: {{ parentCount }}</p>
      <button @click="parentCount++">增加计数</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const parentCount = ref(10)
const userData = ref({
  name: '张三',
  age: 25
})

const itemList = ref([
  { id: 1, name: '项目1' },
  { id: 2, name: '项目2' },
  { id: 3, name: '项目3' }
])

function handleUpdateCount(newCount) {
  parentCount.value = newCount
}
</script>

<style scoped>
.parent {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

.controls {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}
</style>
```

```vue
<!-- 子组件 ChildComponent.vue -->
<template>
  <div class="child">
    <h3>子组件</h3>
    <p>标题: {{ title }}</p>
    <p>计数: {{ count }}</p>
    <p>用户: {{ user.name }} ({{ user.age }}岁)</p>
    
    <div class="items">
      <h4>项目列表:</h4>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </div>
    
    <div class="controls">
      <p>子组件计数: {{ localCount }}</p>
      <button @click="incrementCount">增加计数</button>
      <button @click="sendToParent">发送到父组件</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 定义props
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  },
  user: {
    type: Object,
    required: true
  },
  items: {
    type: Array,
    default: () => []
  }
})

// 定义emits
const emit = defineEmits(['update-count'])

const localCount = ref(props.count)

function incrementCount() {
  localCount.value++
}

function sendToParent() {
  emit('update-count', localCount.value)
}
</script>

<style scoped>
.child {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
  margin: 20px 0;
}

.items {
  margin: 15px 0;
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 4px;
}

.controls {
  margin-top: 15px;
  padding: 10px;
  background-color: #e9ecef;
  border-radius: 4px;
}

button {
  margin-right: 10px;
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:first-child {
  background-color: #28a745;
  color: white;
}

button:last-child {
  background-color: #007bff;
  color: white;
}
</style>
```

#### Props验证和默认值

```vue
<template>
  <div class="props-demo">
    <h3>Props验证演示</h3>
    <validated-component
      :title="componentTitle"
      :count="componentCount"
      :is-active="isActive"
      :config="componentConfig"
      :callback="handleCallback"
    ></validated-component>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ValidatedComponent from './ValidatedComponent.vue'

const componentTitle = ref('验证演示')
const componentCount = ref(5)
const isActive = ref(true)
const componentConfig = ref({
  theme: 'light',
  language: 'zh-CN'
})

function handleCallback(message) {
  console.log('回调函数被调用:', message)
}
</script>
```

```vue
<!-- ValidatedComponent.vue -->
<template>
  <div class="validated-component">
    <p>标题: {{ title }}</p>
    <p>计数: {{ count }}</p>
    <p>状态: {{ isActive ? '激活' : '未激活' }}</p>
    <p>配置: {{ JSON.stringify(config) }}</p>
    <button @click="triggerCallback">触发回调</button>
  </div>
</template>

<script setup>
const props = defineProps({
  // 基础类型检查
  title: String,
  
  // 多个可能的类型
  count: [Number, String],
  
  // 必填字段
  isActive: {
    type: Boolean,
    required: true
  },
  
  // 带有默认值的对象
  config: {
    type: Object,
    default: () => ({
      theme: 'dark',
      language: 'en-US'
    })
  },
  
  // 自定义验证函数
  callback: {
    type: Function,
    validator: (value) => {
      return typeof value === 'function'
    }
  },
  
  // 默认值
  size: {
    type: String,
    default: 'medium',
    validator: (value) => {
      return ['small', 'medium', 'large'].includes(value)
    }
  }
})

const emit = defineEmits(['callback-triggered'])

function triggerCallback() {
  if (props.callback) {
    props.callback('从子组件发出的消息')
    emit('callback-triggered', '回调已触发')
  }
}
</script>

<style scoped>
.validated-component {
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 8px;
  background-color: #f9f9f9;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #35495e;
}
</style>
```

### 3.2 自定义事件（子组件向父组件传递数据）

自定义事件是子组件向父组件传递数据的主要方式。

```vue
<!-- 父组件 -->
<template>
  <div class="event-parent">
    <h2>自定义事件演示</h2>
    
    <event-child
      :initial-value="initialValue"
      @value-changed="handleValueChanged"
      @submit="handleSubmit"
      @cancel="handleCancel"
    ></event-child>
    
    <div class="result">
      <h3>事件处理结果:</h3>
      <p>当前值: {{ currentValue }}</p>
      <p>提交数据: {{ JSON.stringify(submittedData) }}</p>
      <p>取消次数: {{ cancelCount }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import EventChild from './EventChild.vue'

const initialValue = ref('初始值')
const currentValue = ref('初始值')
const submittedData = ref(null)
const cancelCount = ref(0)

function handleValueChanged(newValue) {
  currentValue.value = newValue
  console.log('值变化:', newValue)
}

function handleSubmit(data) {
  submittedData.value = data
  console.log('提交数据:', data)
}

function handleCancel() {
  cancelCount.value++
  console.log('取消操作')
}
</script>

<style scoped>
.event-parent {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

.result {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}
</style>
```

```vue
<!-- 子组件 EventChild.vue -->
<template>
  <div class="event-child">
    <h3>子组件</h3>
    
    <div class="input-group">
      <label>输入值:</label>
      <input 
        v-model="inputValue" 
        @input="handleInput"
        placeholder="请输入内容"
      />
    </div>
    
    <div class="actions">
      <button @click="submitForm">提交</button>
      <button @click="cancelOperation">取消</button>
    </div>
    
    <div class="status">
      <p>输入值: {{ inputValue }}</p>
      <p>状态: {{ status }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  initialValue: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['value-changed', 'submit', 'cancel'])

const inputValue = ref(props.initialValue)
const status = ref('就绪')

// 监听输入值变化
watch(inputValue, (newValue) => {
  emit('value-changed', newValue)
})

function handleInput() {
  status.value = '输入中...'
  setTimeout(() => {
    status.value = '就绪'
  }, 500)
}

function submitForm() {
  const formData = {
    value: inputValue.value,
    timestamp: new Date().toISOString(),
    length: inputValue.value.length
  }
  
  emit('submit', formData)
  status.value = '已提交'
  
  setTimeout(() => {
    status.value = '就绪'
  }, 2000)
}

function cancelOperation() {
  emit('cancel')
  status.value = '已取消'
  
  setTimeout(() => {
    status.value = '就绪'
  }, 1500)
}
</script>

<style scoped>
.event-child {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
  margin: 20px 0;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.input-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.actions {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.actions button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.actions button:first-child {
  background-color: #28a745;
  color: white;
}

.actions button:last-child {
  background-color: #dc3545;
  color: white;
}

.status {
  padding: 10px;
  background-color: #e9ecef;
  border-radius: 4px;
  font-size: 14px;
}
</style>
```

### 3.3 插槽（Slot）

插槽是Vue组件内容分发的重要机制，允许父组件向子组件传递模板内容。

#### 基本插槽

```vue
<!-- 父组件 -->
<template>
  <div class="slot-parent">
    <h2>插槽演示</h2>
    
    <basic-card>
      <h3>这是插入的标题</h3>
      <p>这是插入的段落内容</p>
      <button>点击我</button>
    </basic-card>
    
    <basic-card>
      <div class="special-content">
        <h4>特殊内容</h4>
        <p>这个卡片包含特殊样式的内容</p>
      </div>
    </basic-card>
  </div>
</template>

<script setup>
import BasicCard from './BasicCard.vue'
</script>

<style scoped>
.slot-parent {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

.special-content {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  border-radius: 8px;
}

.special-content h4 {
  margin-top: 0;
}
</style>
```

```vue
<!-- 子组件 BasicCard.vue -->
<template>
  <div class="basic-card">
    <div class="card-header">
      <slot name="header">
        <h3>默认标题</h3>
      </slot>
    </div>
    
    <div class="card-content">
      <slot>
        <p>这是默认内容</p>
      </slot>
    </div>
    
    <div class="card-footer">
      <slot name="footer">
        <p>默认底部</p>
      </slot>
    </div>
  </div>
</template>

<script setup>
</script>

<style scoped>
.basic-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  margin: 15px 0;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card-header {
  background-color: #f8f9fa;
  padding: 15px;
  border-bottom: 1px solid #ddd;
}

.card-content {
  padding: 20px;
}

.card-footer {
  background-color: #f8f9fa;
  padding: 15px;
  border-top: 1px solid #ddd;
}
</style>
```

#### 作用域插槽

作用域插槽允许子组件向父组件传递数据。

```vue
<!-- 父组件 -->
<template>
  <div class="scoped-slot-parent">
    <h2>作用域插槽演示</h2>
    
    <user-list :users="users">
      <template #default="{ user, index }">
        <div class="user-item">
          <span class="user-index">{{ index + 1 }}.</span>
          <span class="user-name">{{ user.name }}</span>
          <span class="user-age">{{ user.age }}岁</span>
          <span class="user-status" :class="user.status">
            {{ user.status === 'active' ? '在线' : '离线' }}
          </span>
        </div>
      </template>
    </user-list>
    
    <user-list :users="users">
      <template #default="{ user }">
        <div class="user-card">
          <h4>{{ user.name }}</h4>
          <p>年龄: {{ user.age }}</p>
          <p>状态: {{ user.status === 'active' ? '在线' : '离线' }}</p>
        </div>
      </template>
    </user-list>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UserList from './UserList.vue'

const users = ref([
  { id: 1, name: '张三', age: 25, status: 'active' },
  { id: 2, name: '李四', age: 30, status: 'inactive' },
  { id: 3, name: '王五', age: 28, status: 'active' },
  { id: 4, name: '赵六', age: 35, status: 'inactive' }
])
</script>

<style scoped>
.scoped-slot-parent {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

.user-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.user-index {
  font-weight: bold;
  margin-right: 10px;
}

.user-name {
  flex: 1;
  font-weight: 500;
}

.user-age {
  margin-right: 15px;
  color: #666;
}

.user-status {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
}

.user-status.active {
  background-color: #d4edda;
  color: #155724;
}

.user-status.inactive {
  background-color: #f8d7da;
  color: #721c24;
}

.user-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  margin: 10px 0;
  background-color: #f9f9f9;
}

.user-card h4 {
  margin-top: 0;
  color: #42b983;
}
</style>
```

```vue
<!-- 子组件 UserList.vue -->
<template>
  <div class="user-list">
    <h3>用户列表</h3>
    <div class="list-container">
      <slot 
        v-for="(user, index) in users" 
        :key="user.id"
        :user="user"
        :index="index"
      >
        <!-- 默认插槽内容 -->
        <div class="default-user-item">
          {{ user.name }} ({{ user.age }}岁)
        </div>
      </slot>
    </div>
  </div>
</template>

<script setup>
defineProps({
  users: {
    type: Array,
    required: true
  }
})
</script>

<style scoped>
.user-list {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
}

.list-container {
  margin-top: 15px;
}

.default-user-item {
  padding: 10px;
  border-bottom: 1px solid #eee;
  background-color: #f8f9fa;
}
</style>
```

## 4. 组件生命周期

### 4.1 生命周期钩子

Vue3组件有一系列生命周期钩子，可以在组件的不同阶段执行代码。

```vue
<template>
  <div class="lifecycle-demo">
    <h2>生命周期演示</h2>
    
    <div class="status">
      <p>当前状态: {{ currentStatus }}</p>
      <p>计数器: {{ counter }}</p>
    </div>
    
    <div class="controls">
      <button @click="increment">增加计数</button>
      <button @click="forceUpdate">强制更新</button>
      <button @click="toggleVisibility">切换显示</button>
    </div>
    
    <div v-if="isVisible" class="content">
      <p>这是组件内容</p>
      <p>创建时间: {{ createdAt }}</p>
      <p>更新次数: {{ updateCount }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

const counter = ref(0)
const currentStatus = ref('初始化')
const createdAt = ref('')
const updateCount = ref(0)
const isVisible = ref(true)

// beforeMount - 组件挂载前
onBeforeMount(() => {
  console.log('beforeMount: 组件挂载前')
  currentStatus.value = '准备挂载'
})

// mounted - 组件挂载后
onMounted(() => {
  console.log('mounted: 组件挂载完成')
  currentStatus.value = '已挂载'
  createdAt.value = new Date().toLocaleString()
  
  // 模拟API调用
  setTimeout(() => {
    console.log('API调用完成')
    currentStatus.value = '数据加载完成'
  }, 1000)
})

// beforeUpdate - 组件更新前
onBeforeUpdate(() => {
  console.log('beforeUpdate: 组件更新前')
  currentStatus.value = '准备更新'
})

// updated - 组件更新后
onUpdated(() => {
  console.log('updated: 组件更新完成')
  currentStatus.value = '已更新'
  updateCount.value++
})

// beforeUnmount - 组件卸载前
onBeforeUnmount(() => {
  console.log('beforeUnmount: 组件卸载前')
  currentStatus.value = '准备卸载'
})

// unmounted - 组件卸载后
onUnmounted(() => {
  console.log('unmounted: 组件卸载完成')
  currentStatus.value = '已卸载'
})

function increment() {
  counter.value++
}

function forceUpdate() {
  // 触发强制更新
  const temp = counter.value
  counter.value = 0
  setTimeout(() => {
    counter.value = temp
  }, 0)
}

function toggleVisibility() {
  isVisible.value = !isVisible.value
}
</script>

<style scoped>
.lifecycle-demo {
  border: 2px solid #42b983;
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
  background-color: #42b983;
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

.content {
  padding: 15px;
  background-color: #e9ecef;
  border-radius: 4px;
}
</style>
```

### 4.2 父子组件生命周期顺序

```vue
<!-- 父组件 -->
<template>
  <div class="lifecycle-parent">
    <h2>父子组件生命周期顺序</h2>
    
    <div class="controls">
      <button @click="showChild = !showChild">
        {{ showChild ? '隐藏子组件' : '显示子组件' }}
      </button>
      <button @click="updateData">更新数据</button>
    </div>
    
    <lifecycle-child v-if="showChild" :message="parentMessage"></lifecycle-child>
    
    <div class="log">
      <h3>生命周期日志:</h3>
      <div class="log-content">
        <div v-for="(log, index) in lifecycleLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'
import LifecycleChild from './LifecycleChild.vue'

const showChild = ref(false)
const parentMessage = ref('初始消息')
const lifecycleLogs = ref([])

function addLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  lifecycleLogs.value.push(`[${timestamp}] ${message}`)
  console.log(message)
}

// 父组件生命周期
onBeforeMount(() => {
  addLog('父组件 beforeMount')
})

onMounted(() => {
  addLog('父组件 mounted')
})

onBeforeUpdate(() => {
  addLog('父组件 beforeUpdate')
})

onUpdated(() => {
  addLog('父组件 updated')
})

onBeforeUnmount(() => {
  addLog('父组件 beforeUnmount')
})

onUnmounted(() => {
  addLog('父组件 unmounted')
})

function updateData() {
  parentMessage.value = `更新时间: ${new Date().toLocaleTimeString()}`
}
</script>

<style scoped>
.lifecycle-parent {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
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
  background-color: #42b983;
  color: white;
}

.controls button:last-child {
  background-color: #007bff;
  color: white;
}

.log {
  margin-top: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
}

.log h3 {
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

```vue
<!-- 子组件 LifecycleChild.vue -->
<template>
  <div class="lifecycle-child">
    <h3>子组件</h3>
    <p>父组件消息: {{ message }}</p>
    <p>子组件计数: {{ childCounter }}</p>
    <button @click="childCounter++">增加子组件计数</button>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

const props = defineProps({
  message: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['lifecycle-event'])

const childCounter = ref(0)

function notifyParent(event) {
  emit('lifecycle-event', `子组件 ${event}`)
}

// 子组件生命周期
onBeforeMount(() => {
  console.log('子组件 beforeMount')
  notifyParent('beforeMount')
})

onMounted(() => {
  console.log('子组件 mounted')
  notifyParent('mounted')
})

onBeforeUpdate(() => {
  console.log('子组件 beforeUpdate')
  notifyParent('beforeUpdate')
})

onUpdated(() => {
  console.log('子组件 updated')
  notifyParent('updated')
})

onBeforeUnmount(() => {
  console.log('子组件 beforeUnmount')
  notifyParent('beforeUnmount')
})

onUnmounted(() => {
  console.log('子组件 unmounted')
  notifyParent('unmounted')
})
</script>

<style scoped>
.lifecycle-child {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
  margin: 20px 0;
  background-color: #f8f9fa;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #0056b3;
}
</style>
```

## 5. 组件最佳实践

### 5.1 组件设计原则

```vue
<template>
  <div class="best-practices">
    <h2>组件最佳实践演示</h2>
    
    <!-- 单一职责原则 -->
    <user-avatar 
      :user="currentUser" 
      :size="avatarSize"
      @click="handleAvatarClick"
    ></user-avatar>
    
    <!-- 可复用性 -->
    <base-button
      variant="primary"
      size="large"
      :disabled="isSubmitting"
      @click="submitForm"
    >
      提交表单
    </base-button>
    
    <!-- 可组合性 -->
    <data-table
      :items="tableData"
      :columns="tableColumns"
      :loading="isLoading"
      @sort="handleSort"
      @page-change="handlePageChange"
    >
      <template #actions="{ item }">
        <action-button 
          variant="edit"
          @click="editItem(item)"
        >
          编辑
        </action-button>
        <action-button 
          variant="delete"
          @click="deleteItem(item)"
        >
          删除
        </action-button>
      </template>
    </data-table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import UserAvatar from './UserAvatar.vue'
import BaseButton from './BaseButton.vue'
import DataTable from './DataTable.vue'
import ActionButton from './ActionButton.vue'

// 单一职责：每个组件只负责一个功能
const currentUser = ref({
  id: 1,
  name: '张三',
  avatar: '/avatar.jpg'
})

const avatarSize = ref('medium')

function handleAvatarClick() {
  console.log('头像被点击')
}

// 可复用性：通过props配置组件行为
const isSubmitting = ref(false)

async function submitForm() {
  isSubmitting.value = true
  try {
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 1000))
    console.log('表单提交成功')
  } finally {
    isSubmitting.value = false
  }
}

// 可组合性：组件可以嵌套和组合
const isLoading = ref(false)
const tableData = ref([
  { id: 1, name: '项目1', status: 'active', created: '2024-01-01' },
  { id: 2, name: '项目2', status: 'inactive', created: '2024-01-02' },
  { id: 3, name: '项目3', status: 'active', created: '2024-01-03' }
])

const tableColumns = computed(() => [
  { key: 'name', label: '名称', sortable: true },
  { key: 'status', label: '状态', sortable: true },
  { key: 'created', label: '创建时间', sortable: true }
])

function handleSort(column, direction) {
  console.log('排序:', column, direction)
}

function handlePageChange(page) {
  console.log('页面变化:', page)
}

function editItem(item) {
  console.log('编辑项目:', item)
}

function deleteItem(item) {
  console.log('删除项目:', item)
}
</script>

<style scoped>
.best-practices {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}
</style>
```

### 5.2 性能优化

```vue
<template>
  <div class="performance-optimization">
    <h2>性能优化演示</h2>
    
    <!-- 使用v-memo进行缓存 -->
    <div v-for="item in expensiveList" :key="item.id" v-memo="[item.id, item.value]">
      <expensive-component :item="item"></expensive-component>
    </div>
    
    <!-- 懒加载组件 -->
    <lazy-component v-if="showLazyComponent"></lazy-component>
    <button @click="showLazyComponent = true">加载懒组件</button>
    
    <!-- 使用keep-alive缓存组件 -->
    <keep-alive>
      <cached-component v-if="showCachedComponent"></cached-component>
    </keep-alive>
    
    <!-- 异步组件 -->
    <async-component></async-component>
  </div>
</template>

<script setup>
import { ref, defineAsyncComponent } from 'vue'
import ExpensiveComponent from './ExpensiveComponent.vue'
import CachedComponent from './CachedComponent.vue'

// 懒加载组件
const LazyComponent = defineAsyncComponent(() =>
  import('./LazyComponent.vue')
)

// 异步组件
const AsyncComponent = defineAsyncComponent({
  loader: () => import('./AsyncComponent.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
  delay: 200,
  timeout: 3000
})

const showLazyComponent = ref(false)
const showCachedComponent = ref(true)

// 计算属性缓存
const expensiveList = computed(() => {
  console.log('计算昂贵的列表')
  return Array.from({ length: 100 }, (_, i) => ({
    id: i + 1,
    value: Math.random()
  }))
})

// 防抖处理
const debouncedSearch = debounce((query) => {
  console.log('搜索:', query)
}, 300)

function debounce(func, wait) {
  let timeout
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout)
      func(...args)
    }
    clearTimeout(timeout)
    timeout = setTimeout(later, wait)
  }
}
</script>

<style scoped>
.performance-optimization {
  border: 2px solid #42b983;
  padding: 20px;
  border-radius: 8px;
}

button {
  margin: 10px 0;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #42b983;
  color: white;
}

button:hover {
  background-color: #35495e;
}
</style>
```

## 6. 总结

Vue3组件系统提供了强大而灵活的方式来构建现代Web应用：

### 6.1 核心概念

- **组件定义**：Vue应用的基本构建块，包含模板、脚本和样式
- **组件注册**：全局注册和局部注册两种方式
- **单文件组件**：推荐的开发方式，将组件的三部分组织在一个文件中

### 6.2 组件通信

- **Props**：父组件向子组件传递数据
- **自定义事件**：子组件向父组件传递数据
- **插槽**：父组件向子组件传递模板内容
- **作用域插槽**：子组件向父组件传递数据

### 6.3 生命周期

- **生命周期钩子**：在组件的不同阶段执行代码
- **父子组件生命周期顺序**：了解组件挂载和更新的顺序
- **最佳实践**：合理使用生命周期钩子

### 6.4 最佳实践

- **单一职责**：每个组件只负责一个功能
- **可复用性**：通过props配置组件行为
- **可组合性**：组件可以嵌套和组合
- **性能优化**：使用v-memo、懒加载、keep-alive等技术

通过掌握Vue3组件系统，你可以构建出结构清晰、可维护、高性能的现代Web应用。在实际项目中，合理使用组件系统可以大大提高开发效率和代码质量。
