# Vue3生命周期详解


# Vue3生命周期详解

## 概述

Vue3生命周期是组件从创建到销毁的整个过程，包括初始化、数据绑定、DOM操作、更新和销毁等阶段。理解生命周期对于开发高质量的Vue应用至关重要，它允许我们在合适的时机执行相应的逻辑。

## 1. 生命周期基础概念

### 1.1 什么是生命周期

生命周期是指Vue组件从创建到销毁的整个过程，在这个过程中，Vue提供了一系列的"钩子函数"（Hook Functions），允许我们在特定的时机执行代码。

```vue
<template>
  <div class="lifecycle-intro">
    <h2>Vue3生命周期概述</h2>
    
    <div class="lifecycle-diagram">
      <h3>组件生命周期流程</h3>
      <div class="flow-chart">
        <div class="stage">创建阶段</div>
        <div class="arrow">→</div>
        <div class="stage">挂载阶段</div>
        <div class="arrow">→</div>
        <div class="stage">更新阶段</div>
        <div class="arrow">→</div>
        <div class="stage">销毁阶段</div>
      </div>
    </div>
    
    <div class="lifecycle-info">
      <h3>生命周期的作用</h3>
      <ul>
        <li>🎯 <strong>初始化数据</strong>：在组件创建时设置初始状态</li>
        <li>🌐 <strong>DOM操作</strong>：在DOM挂载后操作DOM元素</li>
        <li>🔄 <strong>数据更新</strong>：在数据变化时执行相应逻辑</li>
        <li>🧹 <strong>资源清理</strong>：在组件销毁时清理资源</li>
        <li>📡 <strong>事件监听</strong>：添加和移除事件监听器</li>
        <li>🔗 <strong>API调用</strong>：在合适的时机调用API</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.lifecycle-intro {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
}

.lifecycle-diagram, .lifecycle-info {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.flow-chart {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 15px;
}

.stage {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border-radius: 25px;
  font-weight: bold;
  text-align: center;
  min-width: 120px;
}

.arrow {
  font-size: 24px;
  color: #007bff;
}

.lifecycle-info ul {
  list-style: none;
  padding: 0;
}

.lifecycle-info li {
  margin: 10px 0;
  padding: 10px;
  background-color: white;
  border-radius: 8px;
  border-left: 4px solid #007bff;
}

.lifecycle-info li strong {
  color: #007bff;
}
</style>
```

## 2. Vue3生命周期钩子

### 2.1 Options API生命周期钩子

在Vue3中，Options API的生命周期钩子与Vue2基本相同，但有一些变化。

```vue
<template>
  <div class="options-lifecycle">
    <h2>Options API生命周期钩子</h2>
    
    <div class="demo-component">
      <h3>演示组件</h3>
      <p>计数: {{ count }}</p>
      <p>消息: {{ message }}</p>
      <button @click="increment">增加计数</button>
      <button @click="updateMessage">更新消息</button>
      <button @click="destroyComponent">销毁组件</button>
    </div>
    
    <div class="lifecycle-logs">
      <h3>生命周期日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in lifecycleLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OptionsLifecycle',
  
  data() {
    return {
      count: 0,
      message: 'Hello Vue3!',
      lifecycleLogs: [],
      componentDestroyed: false
    }
  },
  
  // beforeCreate - 组件实例创建前
  beforeCreate() {
    this.addLog('beforeCreate: 组件实例创建前')
    console.log('beforeCreate: 此时data和methods都还未初始化')
  },
  
  // created - 组件实例创建后
  created() {
    this.addLog('created: 组件实例创建后')
    console.log('created: 此时data和methods已初始化，但DOM还未创建')
    
    // 可以在这里进行API调用
    this.fetchInitialData()
  },
  
  // beforeMount - 组件挂载前
  beforeMount() {
    this.addLog('beforeMount: 组件挂载前')
    console.log('beforeMount: 此时模板已编译，但还未挂载到DOM')
  },
  
  // mounted - 组件挂载后
  mounted() {
    this.addLog('mounted: 组件挂载后')
    console.log('mounted: 此时组件已挂载到DOM，可以操作DOM元素')
    
    // 可以在这里操作DOM或添加事件监听器
    this.addDOMEventListeners()
  },
  
  // beforeUpdate - 组件更新前
  beforeUpdate() {
    this.addLog('beforeUpdate: 组件更新前')
    console.log('beforeUpdate: 数据已变化，但DOM还未更新')
  },
  
  // updated - 组件更新后
  updated() {
    this.addLog('updated: 组件更新后')
    console.log('updated: DOM已更新，可以操作更新后的DOM')
  },
  
  // beforeUnmount - 组件销毁前
  beforeUnmount() {
    this.addLog('beforeUnmount: 组件销毁前')
    console.log('beforeUnmount: 组件还未销毁，可以清理资源')
    
    // 可以在这里移除事件监听器或清理定时器
    this.cleanupResources()
  },
  
  // unmounted - 组件销毁后
  unmounted() {
    this.addLog('unmounted: 组件销毁后')
    console.log('unmounted: 组件已销毁，所有资源已被清理')
  },
  
  // errorCaptured - 捕获子组件错误
  errorCaptured(err, vm, info) {
    this.addLog(`errorCaptured: 捕获到错误 - ${err.message}`)
    console.error('errorCaptured:', err, vm, info)
    
    // 返回false阻止错误继续传播
    return false
  },
  
  methods: {
    addLog(message) {
      if (!this.componentDestroyed) {
        const timestamp = new Date().toLocaleTimeString()
        const log = `[${timestamp}] ${message}`
        this.lifecycleLogs.push(log)
        console.log(log)
      }
    },
    
    increment() {
      this.count++
    },
    
    updateMessage() {
      this.message = `更新时间: ${new Date().toLocaleTimeString()}`
    },
    
    destroyComponent() {
      this.componentDestroyed = true
      this.$destroy()
    },
    
    fetchInitialData() {
      this.addLog('fetchInitialData: 模拟API调用')
      // 模拟API调用
      setTimeout(() => {
        this.addLog('fetchInitialData: API调用完成')
      }, 1000)
    },
    
    addDOMEventListeners() {
      this.addLog('addDOMEventListeners: 添加DOM事件监听器')
      // 添加全局事件监听器
      window.addEventListener('resize', this.handleResize)
    },
    
    cleanupResources() {
      this.addLog('cleanupResources: 清理资源')
      // 移除事件监听器
      window.removeEventListener('resize', this.handleResize)
    },
    
    handleResize() {
      this.addLog('handleResize: 窗口大小变化')
    }
  }
}
</script>

<style scoped>
.options-lifecycle {
  border: 2px solid #28a745;
  padding: 20px;
  border-radius: 8px;
}

.demo-component {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.demo-component button {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

.demo-component button:hover {
  background-color: #218838;
}

.demo-component button:last-child {
  background-color: #dc3545;
}

.demo-component button:last-child:hover {
  background-color: #c82333;
}

.lifecycle-logs {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.log-content {
  max-height: 400px;
  overflow-y: auto;
  background-color: #e9ecef;
  padding: 15px;
  border-radius: 4px;
}

.log-item {
  padding: 8px 0;
  border-bottom: 1px solid #dee2e6;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}
</style>
```

### 2.2 Composition API生命周期钩子

Composition API使用不同的生命周期钩子函数，更加函数式和灵活。

```vue
<template>
  <div class="composition-lifecycle">
    <h2>Composition API生命周期钩子</h2>
    
    <div class="demo-component">
      <h3>Composition API演示组件</h3>
      <p>计数: {{ count }}</p>
      <p>用户: {{ user.name }}</p>
      <p>主题: {{ theme }}</p>
      <button @click="increment">增加计数</button>
      <button @click="updateUser">更新用户</button>
      <button @click="toggleTheme">切换主题</button>
      <button @click="destroyComponent">销毁组件</button>
    </div>
    
    <div class="lifecycle-info">
      <h3>生命周期信息</h3>
      <div class="info-grid">
        <div class="info-item">
          <strong>组件状态:</strong> {{ componentStatus }}
        </div>
        <div class="info-item">
          <strong>创建时间:</strong> {{ creationTime }}
        </div>
        <div class="info-item">
          <strong>更新次数:</strong> {{ updateCount }}
        </div>
        <div class="info-item">
          <strong>DOM访问:</strong> {{ domAccessible ? '可访问' : '不可访问' }}
        </div>
      </div>
    </div>
    
    <div class="lifecycle-logs">
      <h3>生命周期日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in lifecycleLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted, onErrorCaptured, onRenderTracked, onRenderTriggered } from 'vue'

// 响应式数据
const count = ref(0)
const user = reactive({ name: '张三', age: 25 })
const theme = ref('light')
const componentStatus = ref('初始化')
const creationTime = ref('')
const updateCount = ref(0)
const domAccessible = ref(false)
const lifecycleLogs = ref([])
const componentDestroyed = ref(false)

// 工具函数
function addLog(message) {
  if (!componentDestroyed.value) {
    const timestamp = new Date().toLocaleTimeString()
    const log = `[${timestamp}] ${message}`
    lifecycleLogs.value.push(log)
    console.log(log)
  }
}

// 生命周期钩子
// setup() 函数本身在beforeCreate和created之间执行
addLog('setup: Composition API setup函数执行')

// onBeforeMount - 组件挂载前
onBeforeMount(() => {
  addLog('onBeforeMount: 组件挂载前')
  componentStatus.value = '准备挂载'
  console.log('onBeforeMount: 此时模板已编译，但还未挂载到DOM')
})

// onMounted - 组件挂载后
onMounted(() => {
  addLog('onMounted: 组件挂载后')
  componentStatus.value = '已挂载'
  creationTime.value = new Date().toLocaleString()
  domAccessible.value = true
  console.log('onMounted: 此时组件已挂载到DOM，可以操作DOM元素')
  
  // 可以在这里操作DOM或添加事件监听器
  setupDOMOperations()
})

// onBeforeUpdate - 组件更新前
onBeforeUpdate(() => {
  addLog('onBeforeUpdate: 组件更新前')
  componentStatus.value = '准备更新'
  console.log('onBeforeUpdate: 数据已变化，但DOM还未更新')
})

// onUpdated - 组件更新后
onUpdated(() => {
  addLog('onUpdated: 组件更新后')
  componentStatus.value = '已更新'
  updateCount.value++
  console.log('onUpdated: DOM已更新，可以操作更新后的DOM')
})

// onBeforeUnmount - 组件销毁前
onBeforeUnmount(() => {
  addLog('onBeforeUnmount: 组件销毁前')
  componentStatus.value = '准备销毁'
  console.log('onBeforeUnmount: 组件还未销毁，可以清理资源')
  
  // 可以在这里移除事件监听器或清理定时器
  cleanupResources()
})

// onUnmounted - 组件销毁后
onUnmounted(() => {
  addLog('onUnmounted: 组件销毁后')
  componentStatus.value = '已销毁'
  componentDestroyed.value = true
  console.log('onUnmounted: 组件已销毁，所有资源已被清理')
})

// onErrorCaptured - 捕获子组件错误
onErrorCaptured((err, vm, info) => {
  addLog(`onErrorCaptured: 捕获到错误 - ${err.message}`)
  console.error('onErrorCaptured:', err, vm, info)
  
  // 返回false阻止错误继续传播
  return false
})

// onRenderTracked - 渲染追踪
onRenderTracked((e) => {
  addLog(`onRenderTracked: ${e.type} - ${e.key || 'anonymous'}`)
  console.log('onRenderTracked:', e)
})

// onRenderTriggered - 渲染触发
onRenderTriggered((e) => {
  addLog(`onRenderTriggered: ${e.type} - ${e.key || 'anonymous'}`)
  console.log('onRenderTriggered:', e)
})

// 方法
function increment() {
  count.value++
}

function updateUser() {
  user.name = '李四'
  user.age = 30
}

function toggleTheme() {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}

function destroyComponent() {
  // 在Composition API中，组件销毁由父组件控制
  // 这里我们模拟销毁过程
  componentDestroyed.value = true
  addLog('destroyComponent: 组件销毁请求已发送')
}

function setupDOMOperations() {
  addLog('setupDOMOperations: 设置DOM操作')
  
  // 添加事件监听器
  window.addEventListener('resize', handleResize)
  
  // 模拟API调用
  setTimeout(() => {
    addLog('setupDOMOperations: API调用完成')
  }, 1000)
}

function cleanupResources() {
  addLog('cleanupResources: 清理资源')
  
  // 移除事件监听器
  window.removeEventListener('resize', handleResize)
}

function handleResize() {
  addLog('handleResize: 窗口大小变化')
}
</script>

<style scoped>
.composition-lifecycle {
  border: 2px solid #17a2b8;
  padding: 20px;
  border-radius: 8px;
}

.demo-component {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.demo-component button {
  background-color: #17a2b8;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

.demo-component button:hover {
  background-color: #138496;
}

.demo-component button:last-child {
  background-color: #dc3545;
}

.demo-component button:last-child:hover {
  background-color: #c82333;
}

.lifecycle-info {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-top: 15px;
}

.info-item {
  background-color: white;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

.lifecycle-logs {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.log-content {
  max-height: 400px;
  overflow-y: auto;
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
}

.log-item {
  padding: 8px 0;
  border-bottom: 1px solid #dee2e6;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}
</style>
```

## 3. 生命周期钩子对比

### 3.1 Options API vs Composition API

```vue
<template>
  <div class="lifecycle-comparison">
    <h2>生命周期钩子对比</h2>
    
    <div class="comparison-table">
      <h3>Options API vs Composition API</h3>
      <table>
        <thead>
          <tr>
            <th>Options API</th>
            <th>Composition API</th>
            <th>执行时机</th>
            <th>主要用途</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>beforeCreate / created</td>
            <td>setup()</td>
            <td>组件实例创建前后</td>
            <td>初始化数据、设置响应式</td>
          </tr>
          <tr>
            <td>beforeMount</td>
            <td>onBeforeMount</td>
            <td>组件挂载前</td>
            <td>最后的准备工作</td>
          </tr>
          <tr>
            <td>mounted</td>
            <td>onMounted</td>
            <td>组件挂载后</td>
            <td>DOM操作、API调用、事件监听</td>
          </tr>
          <tr>
            <td>beforeUpdate</td>
            <td>onBeforeUpdate</td>
            <td>组件更新前</td>
            <td>更新前的准备工作</td>
          </tr>
          <tr>
            <td>updated</td>
            <td>onUpdated</td>
            <td>组件更新后</td>
            <td>DOM更新后的操作</td>
          </tr>
          <tr>
            <td>beforeDestroy</td>
            <td>onBeforeUnmount</td>
            <td>组件销毁前</td>
            <td>资源清理、事件移除</td>
          </tr>
          <tr>
            <td>destroyed</td>
            <td>onUnmounted</td>
            <td>组件销毁后</td>
            <td>最终清理工作</td>
          </tr>
          <tr>
            <td>errorCaptured</td>
            <td>onErrorCaptured</td>
            <td>捕获错误时</td>
            <td>错误处理、日志记录</td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <div class="migration-guide">
      <h3>迁移指南</h3>
      <div class="migration-steps">
        <div class="step">
          <h4>1. setup()函数</h4>
          <p>setup()函数替代了beforeCreate和created，在组件创建时执行。</p>
          <pre><code>// Options API
export default {
  beforeCreate() {
    console.log('beforeCreate')
  },
  created() {
    console.log('created')
  }
}

// Composition API
import { onMounted } from 'vue'

export default {
  setup() {
    console.log('setup - 相当于created')
    
    onMounted(() => {
      console.log('mounted')
    })
    
    return {}
  }
}</code></pre>
        </div>
        
        <div class="step">
          <h4>2. 生命周期钩子导入</h4>
          <p>Composition API需要从vue中导入相应的生命周期钩子函数。</p>
          <pre><code>import { 
  onBeforeMount, 
  onMounted, 
  onBeforeUpdate, 
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured
} from 'vue'</code></pre>
        </div>
        
        <div class="step">
          <h4>3. 钩子函数使用</h4>
          <p>在setup()中直接调用钩子函数，传入回调函数。</p>
          <pre><code>onMounted(() => {
  // 组件挂载后的逻辑
  console.log('组件已挂载')
})

onUnmounted(() => {
  // 组件销毁前的清理逻辑
  console.log('组件即将销毁')
})</code></pre>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.lifecycle-comparison {
  border: 2px solid #6f42c1;
  padding: 20px;
  border-radius: 8px;
}

.comparison-table, .migration-guide {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #dee2e6;
}

th {
  background-color: #6f42c1;
  color: white;
  font-weight: bold;
}

tr:nth-child(even) {
  background-color: #f8f9fa;
}

tr:hover {
  background-color: #e9ecef;
}

.migration-steps {
  display: grid;
  gap: 20px;
  margin-top: 15px;
}

.step {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.step h4 {
  color: #6f42c1;
  margin-top: 0;
}

pre {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  overflow-x: auto;
  border: 1px solid #dee2e6;
}

code {
  font-family: monospace;
  font-size: 14px;
  color: #333;
}
</style>
```

## 4. 父子组件生命周期

### 4.1 父子组件生命周期顺序

```vue
<template>
  <div class="parent-child-lifecycle">
    <h2>父子组件生命周期顺序</h2>
    
    <div class="lifecycle-sequence">
      <h3>生命周期执行顺序</h3>
      <div class="sequence-diagram">
        <div class="parent-phase">
          <h4>父组件</h4>
          <div class="phase-item">beforeCreate</div>
          <div class="phase-item">created</div>
          <div class="phase-item">beforeMount</div>
        </div>
        
        <div class="child-phase">
          <h4>子组件</h4>
          <div class="phase-item">beforeCreate</div>
          <div class="phase-item">created</div>
          <div class="phase-item">beforeMount</div>
          <div class="phase-item">mounted</div>
        </div>
        
        <div class="parent-phase">
          <h4>父组件</h4>
          <div class="phase-item">mounted</div>
        </div>
      </div>
    </div>
    
    <div class="demo-container">
      <h3>实际演示</h3>
      <div class="controls">
        <button @click="showChild = !showChild">
          {{ showChild ? '隐藏子组件' : '显示子组件' }}
        </button>
        <button @click="updateParentData">更新父组件数据</button>
        <button @click="clearLogs">清除日志</button>
      </div>
      
      <div class="parent-component">
        <h4>父组件</h4>
        <p>父组件计数: {{ parentCount }}</p>
        <p>父组件消息: {{ parentMessage }}</p>
        
        <lifecycle-child v-if="showChild" :message="parentMessage" @child-updated="handleChildUpdated"></lifecycle-child>
      </div>
    </div>
    
    <div class="lifecycle-logs">
      <h3>完整生命周期日志</h3>
      <div class="log-controls">
        <label>
          <input type="checkbox" v-model="showParentLogs" checked>
          显示父组件日志
        </label>
        <label>
          <input type="checkbox" v-model="showChildLogs" checked>
          显示子组件日志
        </label>
      </div>
      <div class="log-content">
        <div v-for="(log, index) in filteredLogs" :key="index" :class="['log-item', log.type]">
          {{ log.message }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'
import LifecycleChild from './LifecycleChild.vue'

// 父组件状态
const parentCount = ref(0)
const parentMessage = ref('Hello from parent!')
const showChild = ref(true)
const lifecycleLogs = ref([])
const showParentLogs = ref(true)
const showChildLogs = ref(true)

// 过滤后的日志
const filteredLogs = computed(() => {
  return lifecycleLogs.value.filter(log => {
    if (log.type === 'parent' && !showParentLogs.value) return false
    if (log.type === 'child' && !showChildLogs.value) return false
    return true
  })
})

// 添加日志
function addLog(type, message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = {
    type,
    message: `[${timestamp}] ${type}: ${message}`
  }
  lifecycleLogs.value.push(log)
  console.log(log.message)
}

// 父组件生命周期
onBeforeMount(() => {
  addLog('parent', 'beforeMount - 父组件挂载前')
})

onMounted(() => {
  addLog('parent', 'mounted - 父组件挂载后')
})

onBeforeUpdate(() => {
  addLog('parent', 'beforeUpdate - 父组件更新前')
})

onUpdated(() => {
  addLog('parent', 'updated - 父组件更新后')
})

onBeforeUnmount(() => {
  addLog('parent', 'beforeUnmount - 父组件销毁前')
})

onUnmounted(() => {
  addLog('parent', 'unmounted - 父组件销毁后')
})

// 方法
function updateParentData() {
  parentCount.value++
  parentMessage.value = `更新时间: ${new Date().toLocaleTimeString()}`
}

function handleChildUpdated(message) {
  addLog('parent', `接收到子组件更新: ${message}`)
}

function clearLogs() {
  lifecycleLogs.value = []
}

// 提供给子组件的方法
const childLogger = {
  addLog: (message) => addLog('child', message)
}

// 使用provide/inject传递日志函数
import { provide } from 'vue'
provide('childLogger', childLogger)
</script>

<style scoped>
.parent-child-lifecycle {
  border: 2px solid #fd7e14;
  padding: 20px;
  border-radius: 8px;
}

.lifecycle-sequence, .demo-container, .lifecycle-logs {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.sequence-diagram {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-top: 15px;
}

.parent-phase, .child-phase {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 2px solid #fd7e14;
}

.parent-phase h4, .child-phase h4 {
  margin-top: 0;
  color: #fd7e14;
}

.phase-item {
  padding: 8px 12px;
  margin: 5px 0;
  background-color: #fff3cd;
  border-radius: 4px;
  border-left: 4px solid #ffc107;
  font-family: monospace;
  font-size: 14px;
}

.controls {
  margin-bottom: 20px;
}

.controls button {
  background-color: #fd7e14;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

.controls button:hover {
  background-color: #e66a00;
}

.parent-component {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #fd7e14;
  margin-bottom: 20px;
}

.parent-component h4 {
  margin-top: 0;
  color: #fd7e14;
}

.log-controls {
  margin-bottom: 15px;
}

.log-controls label {
  margin-right: 20px;
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

.log-content {
  max-height: 500px;
  overflow-y: auto;
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 4px;
}

.log-item {
  padding: 8px 0;
  border-bottom: 1px solid #ffeaa7;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}

.log-item.parent {
  border-left: 4px solid #fd7e14;
  padding-left: 10px;
}

.log-item.child {
  border-left: 4px solid #17a2b8;
  padding-left: 10px;
}
</style>
```

## 5. 特殊生命周期钩子

### 5.1 Keep-alive组件生命周期

```vue
<template>
  <div class="keep-alive-lifecycle">
    <h2>Keep-alive组件生命周期</h2>
    
    <div class="keep-alive-info">
      <h3>Keep-alive说明</h3>
      <p>Keep-alive是Vue的内置组件，用于缓存不活动的组件实例，而不是销毁它们。</p>
      <div class="features">
        <div class="feature">
          <h4>🔄 组件缓存</h4>
          <p>组件被切换时不会被销毁，而是被缓存</p>
        </div>
        <div class="feature">
          <h4>⚡ 快速切换</h4>
          <p>再次访问时直接从缓存中恢复，无需重新创建</p>
        </div>
        <div class="feature">
          <h4>🎯 状态保持</h4>
          <p>组件的状态会被保持，包括表单输入、滚动位置等</p>
        </div>
      </div>
    </div>
    
    <div class="demo-container">
      <h3>Keep-alive演示</h3>
      <div class="tab-navigation">
        <button 
          v-for="tab in tabs" 
          :key="tab.name"
          @click="currentTab = tab.name"
          :class="['tab-btn', { active: currentTab === tab.name }]"
        >
          {{ tab.label }}
        </button>
      </div>
      
      <div class="tab-content">
        <keep-alive>
          <component :is="currentTabComponent" @tab-data="handleTabData"></component>
        </keep-alive>
      </div>
      
      <div class="tab-info">
        <h4>当前标签信息</h4>
        <p>活跃标签: {{ currentTab }}</p>
        <p>标签数据: {{ JSON.stringify(tabData) }}</p>
        <p>缓存状态: {{ cacheStatus }}</p>
      </div>
    </div>
    
    <div class="lifecycle-logs">
      <h3>Keep-alive生命周期日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in lifecycleLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TabA from './TabA.vue'
import TabB from './TabB.vue'
import TabC from './TabC.vue'

// 标签配置
const tabs = [
  { name: 'tabA', label: '标签 A', component: TabA },
  { name: 'tabB', label: '标签 B', component: TabB },
  { name: 'tabC', label: '标签 C', component: TabC }
]

const currentTab = ref('tabA')
const tabData = ref({})
const cacheStatus = ref('正常')
const lifecycleLogs = ref([])

// 计算当前组件
const currentTabComponent = computed(() => {
  const tab = tabs.find(t => t.name === currentTab.value)
  return tab ? tab.component : null
})

// 添加日志
function addLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = `[${timestamp}] ${message}`
  lifecycleLogs.value.push(log)
  console.log(log)
}

// 处理标签数据
function handleTabData(data) {
  tabData.value = data
  addLog(`接收到标签数据: ${JSON.stringify(data)}`)
}

// 监听标签切换
import { watch } from 'vue'
watch(currentTab, (newTab, oldTab) => {
  addLog(`标签切换: ${oldTab} → ${newTab}`)
  cacheStatus.value = `${oldTab} 已缓存`
})
</script>

<style scoped>
.keep-alive-lifecycle {
  border: 2px solid #20c997;
  padding: 20px;
  border-radius: 8px;
}

.keep-alive-info, .demo-container, .lifecycle-logs {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-top: 15px;
}

.feature {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
  text-align: center;
}

.feature h4 {
  color: #20c997;
  margin-top: 0;
}

.tab-navigation {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.tab-btn {
  padding: 10px 20px;
  border: 2px solid #20c997;
  background-color: white;
  color: #20c997;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  background-color: #20c997;
  color: white;
}

.tab-btn.active {
  background-color: #20c997;
  color: white;
}

.tab-content {
  background-color: white;
  border: 2px solid #20c997;
  border-radius: 8px;
  padding: 20px;
  min-height: 300px;
}

.tab-info {
  margin-top: 15px;
  padding: 15px;
  background-color: #e3f2fd;
  border-radius: 8px;
}

.tab-info h4 {
  margin-top: 0;
  color: #20c997;
}

.log-content {
  max-height: 300px;
  overflow-y: auto;
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
}

.log-item {
  padding: 8px 0;
  border-bottom: 1px solid #dee2e6;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}
</style>
```

## 6. 生命周期最佳实践

### 6.1 生命周期使用建议

```vue
<template>
  <div class="lifecycle-best-practices">
    <h2>生命周期最佳实践</h2>
    
    <div class="practices-grid">
      <div class="practice-category">
        <h3>🚀 推荐做法</h3>
        <div class="practice-item">
          <h4>✅ 在created中初始化数据</h4>
          <pre><code>created() {
  this.fetchData()
  this.setupEventListeners()
}</code></pre>
        </div>
        
        <div class="practice-item">
          <h4>✅ 在mounted中操作DOM</h4>
          <pre><code>mounted() {
  this.$refs.input.focus()
  this.initChart()
}</code></pre>
        </div>
        
        <div class="practice-item">
          <h4>✅ 在beforeUnmount中清理资源</h4>
          <pre><code>beforeUnmount() {
  this.clearTimers()
  this.removeEventListeners()
}</code></pre>
        </div>
      </div>
      
      <div class="practice-category">
        <h3>⚠️ 避免做法</h3>
        <div class="practice-item">
          <h4>❌ 在created中访问DOM</h4>
          <pre><code>// 错误：DOM还未创建
created() {
  this.$refs.element.focus() // undefined
}</code></pre>
        </div>
        
        <div class="practice-item">
          <h4>❌ 在mounted中进行耗时操作</h4>
          <pre><code>// 错误：阻塞UI渲染
mounted() {
  this.longRunningSyncTask()
}</code></pre>
        </div>
        
        <div class="practice-item">
          <h4>❌ 忘记清理事件监听器</h4>
          <pre><code>// 错误：内存泄漏
mounted() {
  window.addEventListener('resize', handler)
  // 忘记在beforeUnmount中移除
}</code></pre>
        </div>
      </div>
    </div>
    
    <div class="performance-tips">
      <h3>🔧 性能优化技巧</h3>
      <div class="tips-grid">
        <div class="tip">
          <h4>懒加载资源</h4>
          <p>在mounted中延迟加载非关键资源</p>
          <pre><code>mounted() {
  setTimeout(() => {
    this.loadHeavyResources()
  }, 100)
}</code></pre>
        </div>
        
        <div class="tip">
          <h4>防抖处理</h4>
          <p>对频繁触发的事件进行防抖处理</p>
          <pre><code>mounted() {
  this.debouncedHandler = debounce(this.handler, 300)
  window.addEventListener('resize', this.debouncedHandler)
}</code></pre>
        </div>
        
        <div class="tip">
          <h4>条件渲染</h4>
          <p>合理使用v-if减少不必要的组件创建</p>
          <pre><code>&lt;!-- 只在需要时渲染 --&gt;
&lt;heavy-component v-if="showHeavy" /&gt;</code></pre>
        </div>
      </div>
    </div>
    
    <div class="debugging-tips">
      <h3>🐛 调试技巧</h3>
      <div class="debug-methods">
        <div class="method">
          <h4>生命周期日志</h4>
          <p>在每个生命周期钩子中添加日志，便于追踪问题</p>
        </div>
        
        <div class="method">
          <h4>Vue DevTools</h4>
          <p>使用Vue DevTools查看组件生命周期状态</p>
        </div>
        
        <div class="method">
          <h4>性能分析</h4>
          <p>使用performance API分析生命周期执行时间</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.lifecycle-best-practices {
  border: 2px solid #6610f2;
  padding: 20px;
  border-radius: 8px;
}

.practices-grid, .performance-tips, .debugging-tips {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.practice-category {
  margin-bottom: 30px;
}

.practice-category h3 {
  color: #6610f2;
  margin-top: 0;
}

.practice-item {
  background-color: white;
  padding: 15px;
  margin: 10px 0;
  border-radius: 8px;
  border-left: 4px solid #6610f2;
}

.practice-item h4 {
  margin-top: 0;
  color: #6610f2;
}

.practice-item pre {
  margin: 10px 0;
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 4px;
  overflow-x: auto;
}

.tips-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 15px;
}

.tip {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.tip h4 {
  color: #6610f2;
  margin-top: 0;
}

.tip pre {
  margin: 10px 0;
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 4px;
  overflow-x: auto;
}

.debug-methods {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-top: 15px;
}

.method {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
  text-align: center;
}

.method h4 {
  color: #6610f2;
  margin-top: 0;
}

code {
  font-family: monospace;
  font-size: 14px;
  color: #333;
}
</style>
```

## 7. 总结

### 7.1 生命周期要点总结

#### 核心概念
- **生命周期**：组件从创建到销毁的整个过程
- **钩子函数**：在特定时机执行的回调函数
- **执行顺序**：父子组件的生命周期执行顺序很重要

#### Options API钩子
- **beforeCreate/created**：组件创建前后
- **beforeMount/mounted**：组件挂载前后
- **beforeUpdate/updated**：组件更新前后
- **beforeUnmount/unmounted**：组件销毁前后
- **errorCaptured**：错误捕获

#### Composition API钩子
- **setup()**：替代beforeCreate/created
- **onBeforeMount/onMounted**：挂载前后
- **onBeforeUpdate/onUpdated**：更新前后
- **onBeforeUnmount/onUnmounted**：销毁前后
- **onErrorCaptured**：错误捕获

#### 特殊场景
- **Keep-alive**：onActivated/onDeactivated
- **父子组件**：父组件挂载→子组件挂载→父组件mounted
- **调试钩子**：onRenderTracked/onRenderTriggered

### 7.2 最佳实践

#### 推荐做法
1. **在created中初始化数据**：进行API调用、设置初始状态
2. **在mounted中操作DOM**：访问DOM元素、添加事件监听器
3. **在beforeUnmount中清理资源**：移除事件监听器、清理定时器
4. **合理使用异步操作**：避免阻塞UI渲染

#### 避免做法
1. **在created中访问DOM**：DOM还未创建
2. **在mounted中进行耗时操作**：影响首屏渲染
3. **忘记清理资源**：导致内存泄漏
4. **在更新钩子中修改状态**：可能导致无限循环

#### 性能优化
1. **懒加载资源**：延迟加载非关键资源
2. **防抖处理**：对频繁事件进行防抖
3. **合理使用缓存**：Keep-alive缓存组件
4. **条件渲染**：减少不必要的组件创建

通过深入理解Vue3的生命周期，你可以更好地控制组件的行为，提高应用的性能和用户体验。在实际开发中，合理使用生命周期钩子是构建高质量Vue应用的关键。
