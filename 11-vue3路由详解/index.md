# Vue3路由详解


# Vue3路由详解

## 概述

Vue Router是Vue.js的官方路由库，专门用于构建单页应用（SPA）。它允许我们在不重新加载页面的情况下，通过不同的URL路径来切换不同的视图组件，提供流畅的用户体验。

## 1. Vue Router基础

### 1.1 路由的作用

Vue Router的核心作用是实现前端路由，让单页应用能够：

- **URL映射**：不同的URL对应不同的组件
- **无刷新导航**：页面切换不重新加载
- **状态管理**：管理应用的路由状态
- **历史记录**：支持浏览器的前进后退功能

### 1.2 安装Vue Router

#### CDN方式
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue3路由演示</title>
    <script src="https://unpkg.com/vue@3"></script>
    <script src="https://unpkg.com/vue-router@4"></script>
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

#### NPM方式
```bash
npm install vue-router@4
# 或者
yarn add vue-router@4
```

### 1.3 基本使用流程

```vue
<template>
  <div id="app">
    <nav>
      <router-link to="/">首页</router-link>
      <router-link to="/about">关于</router-link>
      <router-link to="/contact">联系</router-link>
    </nav>
    
    <main>
      <router-view></router-view>
    </main>
  </div>
</template>

<script>
import { createApp } from 'vue'
import { createRouter, createWebHistory } from 'vue-router'

// 1. 定义路由组件
const Home = { template: '<div><h2>首页</h2><p>欢迎来到首页</p></div>' }
const About = { template: '<div><h2>关于我们</h2><p>这是关于页面</p></div>' }
const Contact = { template: '<div><h2>联系我们</h2><p>这是联系页面</p></div>' }

// 2. 定义路由配置
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
  { path: '/contact', component: Contact }
]

// 3. 创建路由实例
const router = createRouter({
  history: createWebHistory(),
  routes
})

// 4. 创建Vue应用并使用路由
const app = createApp({})
app.use(router)
app.mount('#app')
</script>

<style>
nav {
  padding: 20px;
  background-color: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
}

nav a {
  margin-right: 20px;
  text-decoration: none;
  color: #495057;
  font-weight: 500;
}

nav a.router-link-active {
  color: #007bff;
  font-weight: bold;
}

main {
  padding: 20px;
}
</style>
```

## 2. 路由配置

### 2.1 路由模式

Vue Router支持两种路由模式：

#### Hash模式
```javascript
const router = createRouter({
  history: createWebHashHistory(),
  routes
})
```
- URL格式：`http://example.com/#/about`
- 兼容性更好，不需要服务器配置
- SEO不友好

#### History模式
```javascript
const router = createRouter({
  history: createWebHistory(),
  routes
})
```
- URL格式：`http://example.com/about`
- 更美观，SEO友好
- 需要服务器配置支持

### 2.2 动态路由

```javascript
const routes = [
  // 基本动态路由
  { path: '/user/:id', component: User },
  
  // 多段动态路由
  { path: '/user/:id/posts/:postId', component: UserPost },
  
  // 可选参数
  { path: '/user/:id?', component: User },
  
  // 重复参数
  { path: '/user/:id+', component: UserList },
  
  // 自定义正则
  { path: '/user/:id(\\d+)', component: User }
]
```

### 2.3 嵌套路由

```javascript
const routes = [
  {
    path: '/user',
    component: UserLayout,
    children: [
      {
        path: '', // 默认子路由
        component: UserHome
      },
      {
        path: 'profile',
        component: UserProfile
      },
      {
        path: 'settings',
        component: UserSettings
      }
    ]
  }
]
```

## 3. 核心组件

### 3.1 `<router-link>`

`<router-link>`是Vue Router提供的导航组件，用于声明式导航。

#### 基本用法
```vue
<template>
  <nav>
    <!-- 基本导航 -->
    <router-link to="/">首页</router-link>
    <router-link to="/about">关于</router-link>
    
    <!-- 动态绑定 -->
    <router-link :to="dynamicPath">动态链接</router-link>
    
    <!-- 对象形式 -->
    <router-link :to="{ path: '/user', query: { id: 123 } }">
      用户页面
    </router-link>
    
    <!-- 命名路由 -->
    <router-link :to="{ name: 'user', params: { id: 123 } }">
      用户详情
    </router-link>
  </nav>
</template>

<script setup>
import { ref } from 'vue'

const dynamicPath = ref('/contact')
</script>
```

#### 高级属性
```vue
<template>
  <nav>
    <!-- replace模式 - 不保留历史记录 -->
    <router-link to="/about" replace>关于(替换)</router-link>
    
    <!-- append模式 - 在当前路径后追加 -->
    <router-link to="child" append>子页面</router-link>
    
    <!-- 自定义激活class -->
    <router-link to="/home" active-class="active-link">首页</router-link>
    
    <!-- 精确匹配激活class -->
    <router-link to="/" exact-active-class="exact-active">首页</router-link>
    
    <!-- 自定义触发事件 -->
    <router-link to="/about" event="mouseover">鼠标悬停触发</router-link>
    
    <!-- 自定义标签 -->
    <router-link to="/custom" tag="li">自定义标签</router-link>
  </nav>
</template>

<style>
.active-link {
  color: #007bff;
  font-weight: bold;
  border-bottom: 2px solid #007bff;
}

.exact-active {
  background-color: #007bff;
  color: white;
}
</style>
```

### 3.2 `<router-view>`

`<router-view>`是路由的出口组件，用于渲染当前匹配的路由组件。

#### 基本用法
```vue
<template>
  <div class="app">
    <header>
      <router-link to="/">首页</router-link>
      <router-link to="/about">关于</router-link>
    </header>
    
    <main>
      <!-- 基本路由视图 -->
      <router-view></router-view>
    </main>
    
    <footer>
      <p>&copy; 2024 Vue Router演示</p>
    </footer>
  </div>
</template>
```

#### 命名视图
```vue
<template>
  <div class="app">
    <header>
      <router-link to="/">首页</router-link>
      <router-link to="/complex">复杂布局</router-link>
    </header>
    
    <!-- 命名视图 -->
    <router-view class="main-content"></router-view>
    <router-view name="sidebar" class="sidebar"></router-view>
    <router-view name="footer" class="footer"></router-view>
  </div>
</template>

<script>
const routes = [
  {
    path: '/complex',
    components: {
      default: MainContent,
      sidebar: Sidebar,
      footer: CustomFooter
    }
  }
]
</script>

<style>
.app {
  display: grid;
  grid-template-columns: 1fr 300px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

.main-content {
  grid-column: 1;
  grid-row: 2;
}

.sidebar {
  grid-column: 2;
  grid-row: 2;
  background-color: #f8f9fa;
  padding: 20px;
}

.footer {
  grid-column: 1 / -1;
  grid-row: 3;
  background-color: #343a40;
  color: white;
  padding: 20px;
  text-align: center;
}
</style>
```

## 4. 编程式导航

### 4.1 基本导航方法

```vue
<template>
  <div class="navigation-demo">
    <h2>编程式导航演示</h2>
    
    <div class="navigation-controls">
      <button @click="goToHome">跳转到首页</button>
      <button @click="goToAbout">跳转到关于</button>
      <button @click="goToUser">跳转到用户页面</button>
      <button @click="goBack">返回</button>
      <button @click="goForward">前进</button>
    </div>
    
    <div class="dynamic-navigation">
      <input v-model="targetPath" placeholder="输入目标路径">
      <button @click="navigateToPath">跳转</button>
    </div>
    
    <div class="navigation-info">
      <p>当前路径: {{ currentPath }}</p>
      <p>历史记录长度: {{ historyLength }}</p>
      <p>当前位置: {{ currentPosition }}</p>
    </div>
    
    <div class="navigation-log">
      <h3>导航日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in navigationLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const targetPath = ref('/about')
const currentPath = ref('')
const historyLength = ref(0)
const currentPosition = ref(0)
const navigationLogs = ref([])

function addLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = `[${timestamp}] ${message}`
  navigationLogs.value.push(log)
  console.log(log)
}

// 基本导航方法
function goToHome() {
  router.push('/')
  addLog('跳转到首页')
}

function goToAbout() {
  router.push('/about')
  addLog('跳转到关于页面')
}

function goToUser() {
  router.push({
    path: '/user/123',
    query: { from: 'navigation' }
  })
  addLog('跳转到用户页面 (ID: 123)')
}

function goBack() {
  router.go(-1)
  addLog('返回上一页')
}

function goForward() {
  router.go(1)
  addLog('前进到下一页')
}

function navigateToPath() {
  if (targetPath.value.trim()) {
    router.push(targetPath.value)
    addLog(`跳转到路径: ${targetPath.value}`)
  }
}

// 监听路由变化
onMounted(() => {
  updateRouteInfo()
})

function updateRouteInfo() {
  currentPath.value = route.path
  historyLength.value = window.history.length
  currentPosition.value = window.history.state?.position || 0
}

// 监听路由变化
router.afterEach(() => {
  updateRouteInfo()
})
</script>

<style scoped>
.navigation-demo {
  border: 2px solid #007bff;
  padding: 20px;
  border-radius: 8px;
}

.navigation-controls, .dynamic-navigation, .navigation-info, .navigation-log {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.navigation-controls button, .dynamic-navigation button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
  margin-bottom: 10px;
}

.navigation-controls button:hover, .dynamic-navigation button:hover {
  background-color: #0056b3;
}

.dynamic-navigation input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
  width: 200px;
}

.navigation-log {
  max-height: 300px;
  overflow-y: auto;
}

.log-content {
  background-color: #e9ecef;
  padding: 15px;
  border-radius: 4px;
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
</style>
```

### 4.2 高级导航选项

```vue
<template>
  <div class="advanced-navigation">
    <h2>高级导航选项演示</h2>
    
    <div class="replace-navigation">
      <h3>Replace导航</h3>
      <button @click="navigateWithReplace">使用replace导航</button>
      <p>当前路径: {{ currentPath }}</p>
      <p>说明：replace导航不会在历史记录中添加新记录</p>
    </div>
    
    <div class="params-navigation">
      <h3>参数导航</h3>
      <input v-model="userId" placeholder="用户ID" type="number">
      <button @click="navigateWithParams">导航到用户</button>
      <button @click="navigateWithQuery">导航带查询参数</button>
    </div>
    
    <div class="hash-navigation">
      <h3>Hash导航</h3>
      <input v-model="hashValue" placeholder="Hash值">
      <button @click="navigateWithHash">导航到Hash</button>
    </div>
    
    <div class="navigation-result">
      <h3>导航结果</h3>
      <div class="result-info">
        <p>完整URL: {{ fullUrl }}</p>
        <p>路径: {{ route.path }}</p>
        <p>查询参数: {{ JSON.stringify(route.query) }}</p>
        <p>路由参数: {{ JSON.stringify(route.params) }}</p>
        <p>Hash: {{ route.hash }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const userId = ref(123)
const hashValue = ref('section1')

// 计算完整URL
const fullUrl = computed(() => {
  return window.location.href
})

const currentPath = computed(() => {
  return route.path
})

// Replace导航
function navigateWithReplace() {
  router.replace({
    path: '/replace-demo',
    query: { timestamp: Date.now() }
  })
}

// 参数导航
function navigateWithParams() {
  if (userId.value) {
    router.push({
      name: 'user',
      params: { id: userId.value.toString() }
    })
  }
}

function navigateWithQuery() {
  router.push({
    path: '/search',
    query: {
      q: 'vue',
      page: 1,
      sort: 'date'
    }
  })
}

// Hash导航
function navigateWithHash() {
  if (hashValue.value) {
    router.push({
      path: '/hash-demo',
      hash: `#${hashValue.value}`
    })
  }
}
</script>

<style scoped>
.advanced-navigation {
  border: 2px solid #28a745;
  padding: 20px;
  border-radius: 8px;
}

.replace-navigation, .params-navigation, .hash-navigation, .navigation-result {
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

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
  width: 150px;
}

.result-info {
  background-color: #e9ecef;
  padding: 15px;
  border-radius: 4px;
  font-family: monospace;
  font-size: 14px;
}

.result-info p {
  margin: 5px 0;
}
</style>
```

## 5. 路由守卫

### 5.1 全局守卫

```vue
<template>
  <div class="route-guards">
    <h2>路由守卫演示</h2>
    
    <div class="auth-status">
      <h3>认证状态</h3>
      <p>当前用户: {{ currentUser?.name || '未登录' }}</p>
      <p>是否认证: {{ isAuthenticated ? '已认证' : '未认证' }}</p>
      <button @click="toggleAuth">{{ isAuthenticated ? '退出登录' : '登录' }}</button>
    </div>
    
    <div class="navigation-links">
      <h3>导航链接</h3>
      <router-link to="/">首页</router-link>
      <router-link to="/public">公开页面</router-link>
      <router-link to="/private">私有页面</router-link>
      <router-link to="/admin">管理员页面</router-link>
    </div>
    
    <div class="route-view">
      <router-view></router-view>
    </div>
    
    <div class="guard-logs">
      <h3>守卫日志</h3>
      <div class="log-content">
        <div v-for="(log, index) in guardLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const isAuthenticated = ref(false)
const currentUser = ref(null)
const guardLogs = ref([])

function addLog(message) {
  const timestamp = new Date().toLocaleTimeString()
  const log = `[${timestamp}] ${message}`
  guardLogs.value.push(log)
  console.log(log)
}

// 切换认证状态
function toggleAuth() {
  if (isAuthenticated.value) {
    isAuthenticated.value = false
    currentUser.value = null
    addLog('用户退出登录')
  } else {
    isAuthenticated.value = true
    currentUser.value = {
      id: 1,
      name: '张三',
      role: 'user'
    }
    addLog('用户登录成功')
  }
}

// 全局前置守卫
router.beforeEach((to, from, next) => {
  addLog(`前置守卫: 从 ${from.path} 到 ${to.path}`)
  
  // 检查是否需要认证
  if (to.meta.requiresAuth && !isAuthenticated.value) {
    addLog('需要认证但用户未登录，重定向到登录页')
    next('/login')
    return
  }
  
  // 检查管理员权限
  if (to.meta.requiresAdmin && currentUser.value?.role !== 'admin') {
    addLog('需要管理员权限但用户不是管理员，重定向到首页')
    next('/')
    return
  }
  
  addLog('导航允许通过')
  next()
})

// 全局解析守卫
router.beforeResolve(async (to, from, next) => {
  addLog(`解析守卫: 解析路由 ${to.path}`)
  
  // 模拟异步数据加载
  if (to.meta.requiresData) {
    addLog('加载路由所需数据...')
    await new Promise(resolve => setTimeout(resolve, 500))
    addLog('数据加载完成')
  }
  
  next()
})

// 全局后置钩子
router.afterEach((to, from) => {
  addLog(`后置钩子: 导航完成，从 ${from.path} 到 ${to.path}`)
  
  // 更新页面标题
  if (to.meta.title) {
    document.title = to.meta.title
  }
})

onMounted(() => {
  addLog('路由守卫演示组件已挂载')
})
</script>

<style scoped>
.route-guards {
  border: 2px solid #dc3545;
  padding: 20px;
  border-radius: 8px;
}

.auth-status, .navigation-links, .route-view, .guard-logs {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.navigation-links {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.navigation-links a {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.navigation-links a:hover {
  background-color: #0056b3;
}

.navigation-links a.router-link-active {
  background-color: #28a745;
}

button {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #c82333;
}

.guard-logs {
  max-height: 400px;
  overflow-y: auto;
}

.log-content {
  background-color: #f8d7da;
  padding: 15px;
  border-radius: 4px;
  border-left: 4px solid #dc3545;
}

.log-item {
  padding: 5px 0;
  border-bottom: 1px solid #f5c6cb;
  font-family: monospace;
  font-size: 12px;
}

.log-item:last-child {
  border-bottom: none;
}
</style>
```

### 5.2 路由独享守卫

```javascript
const routes = [
  {
    path: '/admin',
    component: Admin,
    meta: { requiresAuth: true, requiresAdmin: true },
    beforeEnter: (to, from, next) => {
      // 路由独享守卫
      console.log('进入管理员页面前的守卫')
      
      // 检查管理员权限
      if (!isAdmin()) {
        next('/forbidden')
        return
      }
      
      next()
    }
  }
]
```

### 5.3 组件内守卫

```vue
<template>
  <div class="component-guard">
    <h2>组件内守卫演示</h2>
    <p>当前路由参数: {{ $route.params.id }}</p>
    <p>组件数据: {{ componentData }}</p>
  </div>
</template>

<script>
export default {
  name: 'ComponentGuard',
  data() {
    return {
      componentData: null
    }
  },
  
  // 在路由进入前调用
  beforeRouteEnter(to, from, next) {
    console.log('组件内守卫: beforeRouteEnter')
    
    // 可以访问组件实例
    next(vm => {
      // 通过vm访问组件实例
      vm.componentData = '通过守卫设置的数据'
    })
  },
  
  // 在路由更新前调用
  beforeRouteUpdate(to, from, next) {
    console.log('组件内守卫: beforeRouteUpdate')
    
    // 当路由参数变化时重新加载数据
    this.loadData(to.params.id)
    next()
  },
  
  // 在路由离开前调用
  beforeRouteLeave(to, from, next) {
    console.log('组件内守卫: beforeRouteLeave')
    
    // 可以阻止导航或显示确认对话框
    if (this.hasUnsavedChanges) {
      const answer = window.confirm('您有未保存的更改，确定要离开吗？')
      if (!answer) {
        return next(false)
      }
    }
    
    next()
  },
  
  methods: {
    loadData(id) {
      // 模拟数据加载
      console.log('加载数据:', id)
    }
  }
}
</script>
```

## 6. 路由元信息

### 6.1 基本元信息

```javascript
const routes = [
  {
    path: '/dashboard',
    component: Dashboard,
    meta: {
      title: '仪表板',
      requiresAuth: true,
      roles: ['admin', 'user'],
      layout: 'default',
      keepAlive: true
    }
  },
  {
    path: '/settings',
    component: Settings,
    meta: {
      title: '设置',
      requiresAuth: true,
      roles: ['admin'],
      layout: 'settings',
      breadcrumb: ['首页', '设置']
    }
  }
]
```

### 6.2 使用元信息

```vue
<template>
  <div class="meta-info-demo">
    <h2>路由元信息演示</h2>
    
    <div class="current-meta">
      <h3>当前路由元信息</h3>
      <div v-if="route.meta">
        <p>标题: {{ route.meta.title || '无' }}</p>
        <p>需要认证: {{ route.meta.requiresAuth ? '是' : '否' }}</p>
        <p>允许角色: {{ route.meta.roles?.join(', ') || '无限制' }}</p>
        <p>布局: {{ route.meta.layout || '默认' }}</p>
        <p>保持活跃: {{ route.meta.keepAlive ? '是' : '否' }}</p>
      </div>
    </div>
    
    <div class="navigation-menu">
      <h3>导航菜单</h3>
      <div v-for="route in filteredRoutes" :key="route.path" class="menu-item">
        <router-link :to="route.path">
          {{ route.meta?.title || route.path }}
          <span v-if="route.meta?.requiresAuth" class="auth-badge">认证</span>
          <span v-if="route.meta?.roles?.includes('admin')" class="admin-badge">管理员</span>
        </router-link>
      </div>
    </div>
    
    <div class="dynamic-layout">
      <h3>动态布局</h3>
      <component :is="currentLayout">
        <router-view></router-view>
      </component>
    </div>
  </div>
</template>

<script setup>
import { computed, markRaw } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

// 布局组件
const DefaultLayout = markRaw({
  template: '<div class="default-layout"><slot></slot></div>'
})

const SettingsLayout = markRaw({
  template: '<div class="settings-layout"><slot></slot></div>'
})

// 根据元信息选择布局
const currentLayout = computed(() => {
  switch (route.meta?.layout) {
    case 'settings':
      return SettingsLayout
    default:
      return DefaultLayout
  }
})

// 过滤路由用于菜单显示
const filteredRoutes = computed(() => {
  return router.getRoutes().filter(route => {
    return route.meta && route.meta.title
  })
})
</script>

<style scoped>
.meta-info-demo {
  border: 2px solid #6f42c1;
  padding: 20px;
  border-radius: 8px;
}

.current-meta, .navigation-menu, .dynamic-layout {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.menu-item {
  margin: 10px 0;
}

.menu-item a {
  display: inline-block;
  padding: 8px 16px;
  background-color: #6f42c1;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.menu-item a:hover {
  background-color: #5a32a3;
}

.auth-badge, .admin-badge {
  display: inline-block;
  padding: 2px 6px;
  border-radius: 10px;
  font-size: 10px;
  margin-left: 5px;
}

.auth-badge {
  background-color: #ffc107;
  color: #212529;
}

.admin-badge {
  background-color: #dc3545;
  color: white;
}

.default-layout {
  border: 2px solid #28a745;
  padding: 20px;
  border-radius: 8px;
}

.settings-layout {
  border: 2px solid #17a2b8;
  padding: 20px;
  border-radius: 8px;
  background-color: #e3f2fd;
}
</style>
```

## 7. 滚动行为

### 7.1 基本滚动行为

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, savedPosition) {
    // 返回保存的位置
    if (savedPosition) {
      return savedPosition
    }
    
    // 滚动到锚点
    if (to.hash) {
      return {
        el: to.hash,
        behavior: 'smooth'
      }
    }
    
    // 滚动到顶部
    return { top: 0 }
  }
})
```

### 7.2 高级滚动行为

```vue
<template>
  <div class="scroll-behavior-demo">
    <h2>滚动行为演示</h2>
    
    <div class="navigation">
      <router-link to="/scroll/page1">页面1</router-link>
      <router-link to="/scroll/page2#section1">页面2 - 第1节</router-link>
      <router-link to="/scroll/page2#section2">页面2 - 第2节</router-link>
      <router-link to="/scroll/page3">页面3</router-link>
    </div>
    
    <div class="scroll-content">
      <router-view></router-view>
    </div>
    
    <div class="scroll-info">
      <h3>滚动信息</h3>
      <p>当前滚动位置: {{ scrollPosition.y }}px</p>
      <p>当前路由: {{ currentRoute }}</p>
      <button @click="scrollToTop">滚动到顶部</button>
      <button @click="scrollToBottom">滚动到底部</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const scrollPosition = ref({ x: 0, y: 0 })
const currentRoute = ref('')

// 监听滚动事件
function handleScroll() {
  scrollPosition.value = {
    x: window.pageXOffset,
    y: window.pageYOffset
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  currentRoute.value = route.path
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

function scrollToBottom() {
  window.scrollTo({
    top: document.body.scrollHeight,
    behavior: 'smooth'
  })
}
</script>

<style scoped>
.scroll-behavior-demo {
  border: 2px solid #fd7e14;
  padding: 20px;
  border-radius: 8px;
}

.navigation {
  margin-bottom: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.navigation a {
  padding: 8px 16px;
  background-color: #fd7e14;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.navigation a:hover {
  background-color: #e66a00;
}

.scroll-content {
  min-height: 400px;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  background-color: white;
}

.scroll-info {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

button {
  background-color: #fd7e14;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

button:hover {
  background-color: #e66a00;
}
</style>
```

## 8. 路由懒加载

### 8.1 基本懒加载

```javascript
const routes = [
  {
    path: '/about',
    component: () => import('./views/About.vue')
  },
  {
    path: '/contact',
    component: () => import('./views/Contact.vue')
  }
]
```

### 8.2 分组懒加载

```javascript
const routes = [
  {
    path: '/user',
    component: () => import(/* webpackChunkName: "user" */ './views/User.vue')
  },
  {
    path: '/user/profile',
    component: () => import(/* webpackChunkName: "user" */ './views/UserProfile.vue')
  },
  {
    path: '/user/settings',
    component: () => import(/* webpackChunkName: "user" */ './views/UserSettings.vue')
  }
]
```

### 8.3 懒加载组件

```vue
<template>
  <div class="lazy-loading-demo">
    <h2>路由懒加载演示</h2>
    
    <div class="loading-status">
      <h3>加载状态</h3>
      <p>当前加载状态: {{ loadingStatus }}</p>
      <p>已加载的组件: {{ loadedComponents.join(', ') || '无' }}</p>
    </div>
    
    <div class="lazy-navigation">
      <h3>懒加载导航</h3>
      <router-link to="/lazy/home">首页（立即加载）</router-link>
      <router-link to="/lazy/about">关于页面（懒加载）</router-link>
      <router-link to="/lazy/contact">联系页面（懒加载）</router-link>
      <router-link to="/lazy/admin">管理员页面（懒加载）</router-link>
    </div>
    
    <div class="lazy-content">
      <div v-if="isLoading" class="loading-indicator">
        <div class="spinner"></div>
        <p>正在加载组件...</p>
      </div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const loadingStatus = ref('就绪')
const isLoading = ref(false)
const loadedComponents = ref([])

// 懒加载路由配置
const lazyRoutes = [
  {
    path: '/lazy/home',
    name: 'lazy-home',
    component: () => import('./components/LazyHome.vue'),
    meta: { immediate: true }
  },
  {
    path: '/lazy/about',
    name: 'lazy-about',
    component: () => import('./components/LazyAbout.vue')
  },
  {
    path: '/lazy/contact',
    name: 'lazy-contact',
    component: () => import('./components/LazyContact.vue')
  },
  {
    path: '/lazy/admin',
    name: 'lazy-admin',
    component: () => import('./components/LazyAdmin.vue')
  }
]

// 监听路由变化
router.beforeEach((to, from, next) => {
  if (to.path.startsWith('/lazy')) {
    loadingStatus.value = '加载中...'
    isLoading.value = true
  }
  next()
})

router.afterEach((to) => {
  if (to.path.startsWith('/lazy')) {
    loadingStatus.value = '加载完成'
    isLoading.value = false
    
    // 记录已加载的组件
    const componentName = to.name
    if (componentName && !loadedComponents.value.includes(componentName)) {
      loadedComponents.value.push(componentName)
    }
  }
})

onMounted(() => {
  console.log('懒加载演示组件已挂载')
})
</script>

<style scoped>
.lazy-loading-demo {
  border: 2px solid #20c997;
  padding: 20px;
  border-radius: 8px;
}

.loading-status, .lazy-navigation, .lazy-content {
  margin: 20px 0;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.lazy-navigation {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.lazy-navigation a {
  padding: 8px 16px;
  background-color: #20c997;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.lazy-navigation a:hover {
  background-color: #1ca088;
}

.loading-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background-color: #e3f2fd;
  border-radius: 8px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #20c997;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

## 9. 最佳实践和总结

### 9.1 路由组织最佳实践

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

// 路由模块化
const routes = [
  {
    path: '/',
    component: () => import('@/layouts/MainLayout.vue'),
    children: [
      {
        path: '',
        name: 'home',
        component: () => import('@/views/Home.vue'),
        meta: {
          title: '首页',
          keepAlive: true
        }
      }
    ]
  },
  {
    path: '/auth',
    component: () => import('@/layouts/AuthLayout.vue'),
    children: [
      {
        path: 'login',
        name: 'login',
        component: () => import('@/views/auth/Login.vue'),
        meta: {
          title: '登录',
          guestOnly: true
        }
      }
    ]
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    }
    return { top: 0 }
  }
})

// 路由守卫
router.beforeEach((to, from, next) => {
  // 设置页面标题
  if (to.meta.title) {
    document.title = `${to.meta.title} - My App`
  }
  
  // 认证检查
  const isAuthenticated = localStorage.getItem('token')
  
  if (to.meta.requiresAuth && !isAuthenticated) {
    next('/auth/login')
    return
  }
  
  if (to.meta.guestOnly && isAuthenticated) {
    next('/')
    return
  }
  
  next()
})

export default router
```

### 9.2 性能优化

```javascript
// 路由懒加载优化
const routes = [
  {
    path: '/dashboard',
    component: () => import(/* webpackChunkName: "dashboard" */ '@/views/Dashboard.vue'),
    meta: {
      requiresAuth: true,
      preload: true
    }
  }
]

// 预加载重要路由
const preloadRoutes = () => {
  const importantRoutes = ['/dashboard', '/profile']
  importantRoutes.forEach(path => {
    const route = router.resolve(path)
    if (route.matched.length) {
      route.matched.forEach(record => {
        if (record.components.default) {
          record.components.default()
        }
      })
    }
  })
}
```

### 9.3 总结

Vue Router是Vue3生态系统中不可或缺的部分，它提供了强大的路由管理功能：

#### 核心功能
- **声明式导航**：使用`<router-link>`进行导航
- **编程式导航**：使用`router.push()`、`router.replace()`等方法
- **路由守卫**：全局守卫、路由独享守卫、组件内守卫
- **动态路由**：支持动态参数和嵌套路由
- **路由元信息**：为路由添加额外信息
- **懒加载**：支持代码分割和懒加载

#### 最佳实践
1. **路由组织**：按功能模块组织路由
2. **权限控制**：使用路由守卫和元信息
3. **性能优化**：使用懒加载和预加载
4. **用户体验**：合理的滚动行为和加载状态
5. **SEO优化**：使用History模式

通过掌握Vue Router，你可以构建功能完善、用户体验良好的单页应用。在实际项目中，合理使用路由功能可以大大提高应用的可维护性和用户体验。
