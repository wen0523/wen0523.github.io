# Vue3最佳实践详解


# Vue3最佳实践详解

## 一、Vue3最佳实践概述

### 1.1 什么是最佳实践

Vue3最佳实践是经过大量项目验证的开发模式和编码规范，能够帮助我们构建出高质量、易维护、性能优秀的Vue3应用程序。这些实践涵盖了从代码风格到架构设计的各个方面。

**最佳实践的价值：**
- **提高代码质量**：减少bug，提高可读性
- **增强可维护性**：便于后续开发和维护
- **优化性能**：提供更好的用户体验
- **团队协作**：统一开发标准，降低沟通成本
- **降低风险**：避免常见的陷阱和错误

### 1.2 最佳实践的原则

**核心原则：**
1. **KISS (Keep It Simple, Stupid)**：保持简单，避免过度设计
2. **DRY (Don't Repeat Yourself)**：避免重复代码
3. **SOLID**：面向对象设计原则
4. **YAGNI (You Ain't Gonna Need It)**：不要过度设计
5. **渐进式增强**：逐步完善，不要一开始就追求完美

## 二、项目结构最佳实践

### 2.1 目录结构设计

**推荐的项目结构：**
```
src/
├── assets/                  # 静态资源
│   ├── images/             # 图片
│   ├── styles/             # 样式文件
│   │   ├── global.scss     # 全局样式
│   │   ├── variables.scss  # 变量定义
│   │   └── mixins.scss     # 混合器
│   └── fonts/              # 字体文件
├── components/             # 组件
│   ├── base/               # 基础组件
│   ├── business/           # 业务组件
│   └── layout/             # 布局组件
├── composables/            # 组合式函数
├── stores/                 # 状态管理
├── router/                 # 路由配置
├── views/                  # 页面组件
├── utils/                  # 工具函数
├── types/                  # TypeScript类型
├── constants/              # 常量定义
├── api/                    # API接口
├── plugins/                # 插件
├── hooks/                  # 自定义hooks
└── App.vue                 # 根组件
```

### 2.2 文件命名规范

**组件文件命名：**
```typescript
// 好的做法 - 使用PascalCase
UserProfile.vue
OrderList.vue
LoginForm.vue

// 不好的做法 - 使用kebab-case或camelCase
user-profile.vue
orderList.vue
```

**工具函数文件命名：**
```typescript
// 好的做法 - 使用动词或描述性名称
formatDate.ts
debounce.ts
validateEmail.ts
http.ts

// 不好的做法 - 命名不清晰
utils.ts
helpers.ts
```

### 2.3 组件组织原则

**单一职责原则：**
```vue
<!-- 好的做法 - 每个组件只负责一个功能 -->
<template>
  <div class="user-avatar">
    <img :src="avatarUrl" :alt="username" @error="handleError" />
    <span v-if="showUsername" class="username">{{ username }}</span>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

interface Props {
  username: string
  avatarUrl?: string
  showUsername?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  showUsername: false
})

const handleError = (event: Event) => {
  const img = event.target as HTMLImageElement
  img.src = '/default-avatar.png'
}
</script>
```

## 三、组件设计最佳实践

### 3.1 组件通信

**Props和Events：**
```vue
<!-- 父组件 -->
<template>
  <UserCard
    :user="user"
    :loading="loading"
    @edit="handleEdit"
    @delete="handleDelete"
  />
</template>

<script setup lang="ts">
import UserCard from './UserCard.vue'
import { ref } from 'vue'

const user = ref({
  id: 1,
  name: 'John Doe',
  email: 'john@example.com'
})

const loading = ref(false)

const handleEdit = (userData: User) => {
  console.log('Edit user:', userData)
}

const handleDelete = (userId: number) => {
  console.log('Delete user:', userId)
}
</script>
```

**Provide/Inject：**
```typescript
// 父组件
<template>
  <div class="app">
    <ThemeProvider :theme="currentTheme">
      <RouterView />
    </ThemeProvider>
  </div>
</template>

<script setup lang="ts">
import { provide, ref } from 'vue'
import ThemeProvider from './ThemeProvider.vue'

const currentTheme = ref('light')

provide('theme', currentTheme)
provide('updateTheme', (newTheme: string) => {
  currentTheme.value = newTheme
})
</script>

// 子组件
<template>
  <div :class="themeClass">
    <button @click="toggleTheme">Toggle Theme</button>
  </div>
</template>

<script setup lang="ts">
import { inject, computed } from 'vue'

const theme = inject('theme')
const updateTheme = inject('updateTheme')

const themeClass = computed(() => `theme-${theme.value}`)

const toggleTheme = () => {
  const newTheme = theme.value === 'light' ? 'dark' : 'light'
  updateTheme?.(newTheme)
}
</script>
```

### 3.2 组件复用

**组合式函数：**
```typescript
// composables/usePagination.ts
import { ref, computed } from 'vue'

interface PaginationOptions {
  initialPage?: number
  initialPageSize?: number
  totalItems?: number
}

export function usePagination(options: PaginationOptions = {}) {
  const currentPage = ref(options.initialPage || 1)
  const pageSize = ref(options.initialPageSize || 10)
  const totalItems = ref(options.totalItems || 0)

  const totalPages = computed(() => Math.ceil(totalItems.value / pageSize.value))
  const startIndex = computed(() => (currentPage.value - 1) * pageSize.value)
  const endIndex = computed(() => Math.min(startIndex.value + pageSize.value, totalItems.value))

  const goToPage = (page: number) => {
    if (page >= 1 && page <= totalPages.value) {
      currentPage.value = page
    }
  }

  const nextPage = () => goToPage(currentPage.value + 1)
  const prevPage = () => goToPage(currentPage.value - 1)

  const setPageSize = (size: number) => {
    pageSize.value = size
    currentPage.value = 1 // 重置到第一页
  }

  return {
    currentPage,
    pageSize,
    totalItems,
    totalPages,
    startIndex,
    endIndex,
    goToPage,
    nextPage,
    prevPage,
    setPageSize
  }
}
```

**使用组合式函数：**
```vue
<template>
  <div class="data-table">
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in paginatedUsers" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
    
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
      
      <select v-model="pageSize" @change="handlePageSizeChange">
        <option value="5">5 per page</option>
        <option value="10">10 per page</option>
        <option value="20">20 per page</option>
      </select>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { usePagination } from '@/composables/usePagination'

interface User {
  id: number
  name: string
  email: string
}

const props = defineProps<{
  users: User[]
}>()

const { 
  currentPage, 
  pageSize, 
  totalPages, 
  startIndex, 
  endIndex, 
  nextPage, 
  prevPage, 
  setPageSize 
} = usePagination({
  initialPage: 1,
  initialPageSize: 10,
  totalItems: computed(() => props.users.length)
})

const paginatedUsers = computed(() => 
  props.users.slice(startIndex.value, endIndex.value)
)

const handlePageSizeChange = () => {
  setPageSize(parseInt(pageSize.value))
}
</script>
```

### 3.3 组件性能优化

**使用v-memo：**
```vue
<template>
  <div class="user-list">
    <!-- 只有当user.id或user.selected变化时才重新渲染 -->
    <div 
      v-for="user in users" 
      :key="user.id"
      v-memo="[user.id, user.selected]"
      class="user-item"
      :class="{ selected: user.selected }"
      @click="toggleUserSelection(user.id)"
    >
      <img :src="user.avatar" :alt="user.name" />
      <div class="user-info">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
      </div>
      <div v-if="user.selected" class="selected-indicator">✓</div>
    </div>
  </div>
</template>
```

**虚拟滚动：**
```vue
<template>
  <div class="virtual-list" @scroll="handleScroll">
    <div class="scroll-content" :style="{ height: `${totalHeight}px` }">
      <div 
        v-for="item in visibleItems" 
        :key="item.id"
        class="list-item"
        :style="{ transform: `translateY(${item.offset}px)` }"
      >
        {{ item.content }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

interface ListItem {
  id: number
  content: string
}

const props = defineProps<{
  items: ListItem[]
  itemHeight: number
}>()

const scrollTop = ref(0)
const containerHeight = ref(0)

const totalHeight = computed(() => props.items.length * props.itemHeight)

const startIndex = computed(() => Math.floor(scrollTop.value / props.itemHeight))
const endIndex = computed(() => {
  const visibleCount = Math.ceil(containerHeight.value / props.itemHeight)
  return Math.min(startIndex.value + visibleCount + 5, props.items.length - 1)
})

const visibleItems = computed(() => {
  return props.items.slice(startIndex.value, endIndex.value + 1).map((item, index) => ({
    ...item,
    offset: (startIndex.value + index) * props.itemHeight
  }))
})

const handleScroll = (event: Event) => {
  scrollTop.value = event.target.scrollTop
}

const updateContainerHeight = () => {
  containerHeight.value = document.querySelector('.virtual-list')?.clientHeight || 0
}

onMounted(() => {
  updateContainerHeight()
  window.addEventListener('resize', updateContainerHeight)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateContainerHeight)
})
</script>
```

## 四、状态管理最佳实践

### 4.1 Pinia状态管理

**Store设计原则：**
```typescript
// stores/user.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import { userApi } from '@/api/user'

export const useUserStore = defineStore('user', () => {
  // 状态
  const user = ref<User | null>(null)
  const token = ref<string>('')
  const loading = ref<boolean>(false)
  const error = ref<string>('')

  // 计算属性
  const isLoggedIn = computed(() => !!token.value)
  const userRole = computed(() => user.value?.role || 'guest')

  // 异步操作
  const login = async (credentials: LoginCredentials) => {
    loading.value = true
    error.value = ''
    
    try {
      const response = await userApi.login(credentials)
      token.value = response.token
      user.value = response.user
      
      // 持久化
      localStorage.setItem('token', response.token)
      
      return response
    } catch (err) {
      error.value = err.message
      throw err
    } finally {
      loading.value = false
    }
  }

  const logout = () => {
    user.value = null
    token.value = ''
    error.value = ''
    localStorage.removeItem('token')
  }

  return {
    // 状态
    user,
    token,
    loading,
    error,
    
    // 计算属性
    isLoggedIn,
    userRole,
    
    // 方法
    login,
    logout
  }
})
```

**模块化状态管理：**
```typescript
// stores/index.ts
import { createPinia } from 'pinia'
import { createPersistedState } from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(createPersistedState())

export default pinia

// 使用示例
import { useUserStore } from '@/stores/user'
import { useCartStore } from '@/stores/cart'

export function useAppStores() {
  return {
    user: useUserStore(),
    cart: useCartStore()
  }
}
```

### 4.2 状态管理最佳实践

**避免过度使用全局状态：**
```typescript
// 好的做法 - 组件内部状态
const localState = ref({
  isActive: false,
  count: 0
})

// 不好的做法 - 不必要的全局状态
const appStore = useAppStore()
const localUIState = appStore.localUIState // 这应该在组件内部管理
```

**合理使用计算属性：**
```typescript
// 好的做法 - 计算属性缓存
const filteredUsers = computed(() => 
  users.value.filter(user => 
    user.name.includes(searchTerm.value)
  )
)

// 不好的做法 - 方法重复计算
const getFilteredUsers = () => {
  return users.value.filter(user => 
    user.name.includes(searchTerm.value)
  )
}
```

## 五、路由管理最佳实践

### 5.1 路由设计

**模块化路由：**
```typescript
// router/modules/dashboard.ts
import type { RouteRecordRaw } from 'vue-router'

const routes: RouteRecordRaw[] = [
  {
    path: '/dashboard',
    name: 'Dashboard',
    component: () => import('@/views/Dashboard.vue'),
    meta: {
      title: '仪表板',
      requiresAuth: true,
      permissions: ['dashboard:read']
    }
  },
  {
    path: '/dashboard/analytics',
    name: 'DashboardAnalytics',
    component: () => import('@/views/DashboardAnalytics.vue'),
    meta: {
      title: '数据分析',
      requiresAuth: true,
      permissions: ['analytics:read']
    }
  }
]

export default routes
```

**路由守卫：**
```typescript
// router/guards/auth.ts
import type { Router } from 'vue-router'
import { useUserStore } from '@/stores/user'

export function setupAuthGuard(router: Router) {
  router.beforeEach(async (to, from, next) => {
    const userStore = useUserStore()
    
    // 检查是否需要认证
    if (to.meta.requiresAuth && !userStore.isLoggedIn) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
      return
    }
    
    // 获取用户信息
    if (userStore.isLoggedIn && !userStore.user) {
      try {
        await userStore.fetchUserProfile()
      } catch (error) {
        userStore.logout()
        next('/login')
        return
      }
    }
    
    next()
  })
}
```

### 5.2 路由性能优化

**路由懒加载：**
```typescript
// 好的做法 - 路由懒加载
const routes: RouteRecordRaw[] = [
  {
    path: '/about',
    name: 'About',
    component: () => import('@/views/About.vue')
  },
  {
    path: '/contact',
    name: 'Contact',
    component: () => import('@/views/Contact.vue')
  }
]

// 不好的做法 - 同步加载所有路由
import About from '@/views/About.vue'
import Contact from '@/views/Contact.vue'

const routes: RouteRecordRaw[] = [
  {
    path: '/about',
    name: 'About',
    component: About
  },
  {
    path: '/contact',
    name: 'Contact',
    component: Contact
  }
]
```

## 六、样式管理最佳实践

### 6.1 CSS模块化

** scoped样式：**
```vue
<template>
  <div class="user-card">
    <img :src="user.avatar" class="avatar" />
    <div class="user-info">
      <h3 class="name">{{ user.name }}</h3>
      <p class="email">{{ user.email }}</p>
    </div>
  </div>
</template>

<style scoped>
.user-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info {
  flex: 1;
}

.name {
  margin: 0 0 4px 0;
  font-size: 16px;
  font-weight: 600;
}

.email {
  margin: 0;
  color: #666;
  font-size: 14px;
}
</style>
```

**CSS变量：**
```scss
// styles/variables.scss
:root {
  // 颜色系统
  --color-primary: #1976d2;
  --color-primary-light: #42a5f5;
  --color-primary-dark: #1565c0;
  
  --color-secondary: #424242;
  --color-secondary-light: #616161;
  --color-secondary-dark: #212121;
  
  --color-success: #4caf50;
  --color-warning: #ff9800;
  --color-error: #f44336;
  --color-info: #2196f3;
  
  // 间距系统
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  
  // 字体系统
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 20px;
  
  // 边框圆角
  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-radius-lg: 12px;
  
  // 阴影
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
}
```

### 6.2 响应式设计

**响应式组件：**
```vue
<template>
  <div class="responsive-card">
    <div class="card-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.responsive-card {
  background: white;
  border-radius: var(--border-radius-md);
  box-shadow: var(--shadow-md);
  padding: var(--spacing-md);
  
  @media (max-width: 768px) {
    padding: var(--spacing-sm);
    border-radius: var(--border-radius-sm);
  }
  
  @media (max-width: 480px) {
    padding: var(--spacing-xs);
    box-shadow: var(--shadow-sm);
  }
}

.card-content {
  max-width: 100%;
  overflow: hidden;
}
</style>
```

## 七、性能优化最佳实践

### 7.1 组件性能优化

**使用key优化列表渲染：**
```vue
<template>
  <div class="user-list">
    <!-- 好的做法 - 使用唯一稳定的key -->
    <div v-for="user in users" :key="user.id" class="user-item">
      {{ user.name }}
    </div>
    
    <!-- 不好的做法 - 使用索引作为key -->
    <div v-for="(user, index) in users" :key="index" class="user-item">
      {{ user.name }}
    </div>
  </div>
</template>
```

**避免不必要的重新渲染：**
```vue
<template>
  <div class="product-list">
    <!-- 使用v-show代替v-if进行频繁切换 -->
    <div v-show="showFilters" class="filters">
      <!-- 过滤器内容 -->
    </div>
    
    <!-- 静态内容使用v-once -->
    <div v-once class="static-header">
      <h1>Product Catalog</h1>
      <p>Browse our extensive collection</p>
    </div>
    
    <!-- 懒加载组件 -->
    <Suspense v-if="shouldLoadChart">
      <LazyChart />
      <template #fallback>
        <div class="chart-placeholder">Loading chart...</div>
      </template>
    </Suspense>
  </div>
</template>
```

### 7.2 数据响应式优化

**合理使用响应式数据：**
```typescript
// 好的做法 - 只需要响应式的数据才设置为响应式
const reactiveData = ref({
  count: 0,
  user: null
})

// 非响应式数据
const staticConfig = {
  apiEndpoint: 'https://api.example.com',
  timeout: 5000
}

// 使用shallowRef处理大型对象
const largeObject = shallowRef({
  // 大型数据，内部属性变化不会触发更新
  nestedData: { /* 大量数据 */ }
})
```

**优化计算属性：**
```typescript
// 好的做法 - 计算属性缓存
const expensiveComputed = computed(() => {
  return heavyCalculation()
})

// 不好的做法 - 每次都重新计算
const getExpensiveResult = () => {
  return heavyCalculation()
}
```

## 八、错误处理最佳实践

### 8.1 全局错误处理

**Vue错误处理：**
```typescript
// main.ts
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 全局错误处理
app.config.errorHandler = (error, instance, info) => {
  console.error('Vue Error:', error)
  console.error('Component:', instance?.$options.name)
  console.error('Error Info:', info)
  
  // 发送到错误监控服务
  sendToErrorService(error, {
    component: instance?.$options.name,
    info
  })
}

// 全局警告处理
app.config.warnHandler = (msg, instance, trace) => {
  console.warn('Vue Warning:', msg)
  console.warn('Component:', instance?.$options.name)
  console.warn('Trace:', trace)
}

app.mount('#app')
```

**JavaScript错误处理：**
```typescript
// utils/errorHandler.ts
export class ErrorHandler {
  static handle(error: Error, context?: any) {
    console.error('Error:', error)
    console.error('Context:', context)
    
    // 发送到错误监控服务
    this.sendToMonitoringService(error, context)
    
    // 显示用户友好的错误信息
    this.showUserFriendlyError(error)
  }
  
  private static sendToMonitoringService(error: Error, context?: any) {
    if (process.env.NODE_ENV === 'production') {
      // 发送到Sentry或其他错误监控服务
      fetch('/api/error-log', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          message: error.message,
          stack: error.stack,
          context,
          timestamp: new Date().toISOString(),
          userAgent: navigator.userAgent
        })
      })
    }
  }
  
  private static showUserFriendlyError(error: Error) {
    // 根据错误类型显示不同的用户提示
    let message = 'An error occurred. Please try again later.'
    
    if (error.message.includes('Network Error')) {
      message = 'Network connection error. Please check your internet connection.'
    } else if (error.message.includes('401')) {
      message = 'Authentication failed. Please log in again.'
    } else if (error.message.includes('404')) {
      message = 'The requested resource was not found.'
    }
    
    // 显示错误消息（使用toast或其他UI组件）
    alert(message)
  }
}
```

### 8.2 异步操作错误处理

**API调用错误处理：**
```typescript
// utils/http.ts
import axios, { AxiosInstance, AxiosError } from 'axios'

class HttpClient {
  private instance: AxiosInstance

  constructor() {
    this.instance = axios.create({
      baseURL: import.meta.env.VITE_API_BASE_URL,
      timeout: 10000
    })
    
    this.setupInterceptors()
  }

  private setupInterceptors() {
    // 请求拦截器
    this.instance.interceptors.request.use(
      (config) => {
        const token = localStorage.getItem('token')
        if (token) {
          config.headers.Authorization = `Bearer ${token}`
        }
        return config
      },
      (error) => {
        return Promise.reject(error)
      }
    )

    // 响应拦截器
    this.instance.interceptors.response.use(
      (response) => {
        return response.data
      },
      (error: AxiosError) => {
        return this.handleResponseError(error)
      }
    )
  }

  private handleResponseError(error: AxiosError) {
    const status = error.response?.status
    const message = error.response?.data?.message || error.message

    switch (status) {
      case 401:
        // 未授权，跳转到登录页
        localStorage.removeItem('token')
        window.location.href = '/login'
        break
      case 403:
        console.error('Permission denied:', message)
        break
      case 404:
        console.error('Resource not found:', message)
        break
      case 500:
        console.error('Server error:', message)
        break
      default:
        console.error('Network error:', message)
    }

    return Promise.reject(error)
  }

  async get<T>(url: string, config?: any): Promise<T> {
    try {
      return await this.instance.get(url, config)
    } catch (error) {
      ErrorHandler.handle(error as Error, { url, method: 'GET' })
      throw error
    }
  }

  async post<T>(url: string, data?: any, config?: any): Promise<T> {
    try {
      return await this.instance.post(url, data, config)
    } catch (error) {
      ErrorHandler.handle(error as Error, { url, method: 'POST', data })
      throw error
    }
  }
}

export const http = new HttpClient()
```

## 九、测试最佳实践

### 9.1 单元测试

**组件测试：**
```typescript
// tests/components/UserCard.spec.ts
import { mount } from '@vue/test-utils'
import { describe, it, expect, vi } from 'vitest'
import UserCard from '@/components/UserCard.vue'

describe('UserCard.vue', () => {
  const mockUser = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    avatar: 'https://example.com/avatar.jpg'
  }

  it('renders user information correctly', () => {
    const wrapper = mount(UserCard, {
      props: {
        user: mockUser
      }
    })

    expect(wrapper.find('.user-name').text()).toBe(mockUser.name)
    expect(wrapper.find('.user-email').text()).toBe(mockUser.email)
    expect(wrapper.find('.user-avatar').attributes('src')).toBe(mockUser.avatar)
  })

  it('emits edit event when edit button is clicked', async () => {
    const wrapper = mount(UserCard, {
      props: {
        user: mockUser
      }
    })

    await wrapper.find('.edit-button').trigger('click')

    expect(wrapper.emitted('edit')).toBeTruthy()
    expect(wrapper.emitted('edit')[0]).toEqual([mockUser])
  })

  it('shows loading state when loading prop is true', () => {
    const wrapper = mount(UserCard, {
      props: {
        user: mockUser,
        loading: true
      }
    })

    expect(wrapper.find('.loading-spinner').exists()).toBe(true)
    expect(wrapper.find('.edit-button').exists()).toBe(false)
  })

  it('handles missing avatar gracefully', () => {
    const userWithoutAvatar = { ...mockUser, avatar: undefined }
    const wrapper = mount(UserCard, {
      props: {
        user: userWithoutAvatar
      }
    })

    expect(wrapper.find('.user-avatar').attributes('src')).toBe('/default-avatar.png')
  })
})
```

**组合式函数测试：**
```typescript
// tests/composables/usePagination.spec.ts
import { renderHook } from '@testing-library/vue'
import { describe, it, expect } from 'vitest'
import { usePagination } from '@/composables/usePagination'

describe('usePagination', () => {
  it('calculates pagination correctly', () => {
    const { result } = renderHook(() => 
      usePagination({
        initialPage: 1,
        initialPageSize: 10,
        totalItems: 25
      })
    )

    expect(result.value.totalPages).toBe(3)
    expect(result.value.startIndex).toBe(0)
    expect(result.value.endIndex).toBe(10)
  })

  it('handles page navigation correctly', () => {
    const { result } = renderHook(() => 
      usePagination({
        initialPage: 1,
        initialPageSize: 10,
        totalItems: 25
      })
    )

    result.value.nextPage()
    expect(result.value.currentPage).toBe(2)

    result.value.prevPage()
    expect(result.value.currentPage).toBe(1)
  })

  it('resets to first page when page size changes', () => {
    const { result } = renderHook(() => 
      usePagination({
        initialPage: 2,
        initialPageSize: 10,
        totalItems: 25
      })
    )

    result.value.setPageSize(20)
    expect(result.value.currentPage).toBe(1)
    expect(result.value.pageSize).toBe(20)
  })
})
```

### 9.2 集成测试

**API集成测试：**
```typescript
// tests/api/user.spec.ts
import { describe, it, expect, beforeEach } from 'vitest'
import { userApi } from '@/api/user'
import { http } from '@/api/http'

// Mock http client
vi.mock('@/api/http')

describe('User API', () => {
  beforeEach(() => {
    vi.clearAllMocks()
  })

  describe('getUserProfile', () => {
    it('fetches user profile successfully', async () => {
      const mockUser = {
        id: 1,
        name: 'John Doe',
        email: 'john@example.com'
      }

      vi.mocked(http.get).mockResolvedValue(mockUser)

      const result = await userApi.getUserProfile()

      expect(http.get).toHaveBeenCalledWith('/user/profile')
      expect(result).toEqual(mockUser)
    })

    it('handles authentication errors', async () => {
      const error = new Error('Unauthorized')
      vi.mocked(http.get).mockRejectedValue(error)

      await expect(userApi.getUserProfile()).rejects.toThrow('Unauthorized')
    })
  })

  describe('updateUserProfile', () => {
    it('updates user profile successfully', async () => {
      const updateData = { name: 'Jane Doe' }
      const updatedUser = {
        id: 1,
        name: 'Jane Doe',
        email: 'john@example.com'
      }

      vi.mocked(http.put).mockResolvedValue(updatedUser)

      const result = await userApi.updateUserProfile(updateData)

      expect(http.put).toHaveBeenCalledWith('/user/profile', updateData)
      expect(result).toEqual(updatedUser)
    })
  })
})
```

## 十、文档和代码注释

### 10.1 组件文档

**组件注释规范：**
```vue
<template>
  <!--
    @component UserCard
    @description 显示用户信息的卡片组件
    @example
    <UserCard 
      :user="user" 
      :loading="loading"
      @edit="handleEdit"
      @delete="handleDelete"
    />
  -->
  <div class="user-card">
    <!-- 用户头像 -->
    <img 
      :src="user.avatar || defaultAvatar" 
      :alt="user.name"
      class="user-avatar"
      @error="handleAvatarError"
    />
    
    <!-- 用户信息 -->
    <div class="user-info">
      <h3 class="user-name">{{ user.name }}</h3>
      <p class="user-email">{{ user.email }}</p>
      <p class="user-role">{{ user.role }}</p>
    </div>
    
    <!-- 操作按钮 -->
    <div class="user-actions" v-if="!loading">
      <button 
        @click="$emit('edit', user)"
        class="btn-edit"
        title="编辑用户"
      >
        ✏️
      </button>
      <button 
        @click="$emit('delete', user.id)"
        class="btn-delete"
        title="删除用户"
      >
        🗑️
      </button>
    </div>
    
    <!-- 加载状态 -->
    <div v-if="loading" class="loading-overlay">
      <div class="loading-spinner"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
/**
 * 用户卡片组件
 * @component UserCard
 */

// 导入定义
import { ref } from 'vue'

// 类型定义
interface User {
  id: number
  name: string
  email: string
  role: string
  avatar?: string
}

// Props定义
interface Props {
  /** 用户信息对象 */
  user: User
  /** 是否显示加载状态 */
  loading?: boolean
  /** 是否显示操作按钮 */
  showActions?: boolean
}

// 设置默认值
const props = withDefaults(defineProps<Props>(), {
  loading: false,
  showActions: true
})

// 事件定义
interface Emits {
  (e: 'edit', user: User): void
  (e: 'delete', userId: number): void
}

const emit = defineEmits<Emits>()

// 默认头像
const defaultAvatar = '/default-avatar.png'

// 处理头像加载错误
const handleAvatarError = (event: Event) => {
  const img = event.target as HTMLImageElement
  img.src = defaultAvatar
}
</script>

<style scoped>
/* 用户卡片样式 */
.user-card {
  position: relative;
  background: white;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  gap: 16px;
}

.user-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info {
  flex: 1;
}

.user-name {
  margin: 0 0 4px 0;
  font-size: 16px;
  font-weight: 600;
}

.user-email {
  margin: 0 0 4px 0;
  color: #666;
  font-size: 14px;
}

.user-role {
  margin: 0;
  color: #999;
  font-size: 12px;
  text-transform: uppercase;
}

.user-actions {
  display: flex;
  gap: 8px;
}

.btn-edit,
.btn-delete {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.btn-edit:hover {
  background-color: #e3f2fd;
}

.btn-delete:hover {
  background-color: #ffebee;
}

.loading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
}

.loading-spinner {
  width: 24px;
  height: 24px;
  border: 2px solid #f3f3f3;
  border-top: 2px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
```

### 10.2 函数文档

**工具函数文档：**
```typescript
// utils/format.ts

/**
 * 格式化日期为本地字符串
 * @param date - 要格式化的日期对象或时间戳
 * @param format - 格式模式，默认为 'YYYY-MM-DD'
 * @returns 格式化后的日期字符串
 * @example
 * formatDate(new Date(), 'YYYY-MM-DD HH:mm:ss')
 * // 返回: "2024-01-15 14:30:45"
 */
export function formatDate(
  date: Date | number | string,
  format: string = 'YYYY-MM-DD'
): string {
  const d = new Date(date)
  
  if (isNaN(d.getTime())) {
    throw new Error('Invalid date')
  }
  
  const year = d.getFullYear()
  const month = String(d.getMonth() + 1).padStart(2, '0')
  const day = String(d.getDate()).padStart(2, '0')
  const hours = String(d.getHours()).padStart(2, '0')
  const minutes = String(d.getMinutes()).padStart(2, '0')
  const seconds = String(d.getSeconds()).padStart(2, '0')
  
  return format
    .replace('YYYY', String(year))
    .replace('MM', month)
    .replace('DD', day)
    .replace('HH', hours)
    .replace('mm', minutes)
    .replace('ss', seconds)
}

/**
 * 防抖函数
 * @param func - 要防抖的函数
 * @param delay - 延迟时间（毫秒）
 * @returns 防抖后的函数
 * @example
 * const debouncedSearch = debounce(searchUsers, 300)
 * input.addEventListener('input', debouncedSearch)
 */
export function debounce<T extends (...args: any[]) => any>(
  func: T,
  delay: number
): (...args: Parameters<T>) => void {
  let timeoutId: NodeJS.Timeout | null = null
  
  return function (this: any, ...args: Parameters<T>) {
    const context = this
    
    if (timeoutId) {
      clearTimeout(timeoutId)
    }
    
    timeoutId = setTimeout(() => {
      func.apply(context, args)
    }, delay)
  }
}

/**
 * 节流函数
 * @param func - 要节流的函数
 * @param limit - 时间限制（毫秒）
 * @returns 节流后的函数
 * @example
 * const throttledScroll = throttle(handleScroll, 100)
 * window.addEventListener('scroll', throttledScroll)
 */
export function throttle<T extends (...args: any[]) => any>(
  func: T,
  limit: number
): (...args: Parameters<T>) => void {
  let inThrottle: boolean = false
  
  return function (this: any, ...args: Parameters<T>) {
    const context = this
    
    if (!inThrottle) {
      func.apply(context, args)
      inThrottle = true
      setTimeout(() => {
        inThrottle = false
      }, limit)
    }
  }
}

/**
 * 深拷贝函数
 * @param obj - 要拷贝的对象
 * @returns 深拷贝后的对象
 * @example
 * const original = { a: 1, b: { c: 2 } }
 * const copy = deepClone(original)
 * copy.b.c = 3
 * console.log(original.b.c) // 2
 */
export function deepClone<T>(obj: T): T {
  if (obj === null || typeof obj !== 'object') {
    return obj
  }
  
  if (obj instanceof Date) {
    return new Date(obj.getTime()) as T
  }
  
  if (obj instanceof Array) {
    return obj.map(item => deepClone(item)) as T
  }
  
  if (typeof obj === 'object') {
    const clonedObj = {} as T
    for (const key in obj) {
      if (obj.hasOwnProperty(key)) {
        clonedObj[key] = deepClone(obj[key])
      }
    }
    return clonedObj
  }
  
  return obj
}
```

## 十一、团队协作最佳实践

### 11.1 代码审查

**代码审查清单：**
```markdown
## 代码审查检查清单

### 代码质量
- [ ] 代码是否符合项目的编码规范
- [ ] 变量和函数命名是否清晰易懂
- [ ] 是否有重复代码可以提取
- [ ] 代码复杂度是否过高
- [ ] 是否有足够的注释和文档

### 功能实现
- [ ] 功能实现是否完整
- [ ] 是否考虑了边界情况
- [ ] 错误处理是否完善
- [ ] 是否有性能问题
- [ ] 用户体验是否良好

### 测试覆盖
- [ ] 是否有对应的单元测试
- [ ] 测试覆盖率是否足够
- [ ] 是否有集成测试
- [ ] 测试用例是否覆盖了各种场景

### 安全性
- [ ] 是否有安全漏洞
- [ ] 输入验证是否充分
- [ ] 敏感信息是否正确处理
- [ ] 权限控制是否合理

### 性能考虑
- [ ] 是否有性能瓶颈
- [ ] 资源使用是否合理
- [ ] 是否有内存泄漏
- [ ] 数据库查询是否优化
```

### 11.2 Git工作流

**分支策略：**
```bash
# 功能分支命名规范
feature/user-authentication
feature/payment-integration
feature/dashboard-analytics

# 修复分支命名规范
bugfix/fix-login-validation
bugfix/fix-memory-leak
bugfix/fix-api-timeout

# 发布分支命名规范
release/v1.0.0
release/v1.1.0

# 热修复分支命名规范
hotfix/critical-security-patch
hotfix/production-bug-fix
```

**提交信息规范：**
```bash
# 提交信息格式
<type>(<scope>): <subject>

# 类型说明
feat: 新功能
fix: 修复bug
docs: 文档更新
style: 代码格式化
refactor: 重构
test: 测试相关
chore: 构建过程或辅助工具的变动

# 示例
feat(auth): add user login functionality
fix(api): handle timeout errors properly
docs: update API documentation
style: format code according to style guide
refactor: improve user service architecture
test: add unit tests for user validation
chore: upgrade dependencies to latest versions
```

## 十二、总结

### 12.1 最佳实践总结

**核心原则：**
1. **保持简单**：避免过度设计，追求简洁明了的代码
2. **一致性**：保持代码风格和架构的一致性
3. **可维护性**：编写易于理解和维护的代码
4. **性能优先**：时刻关注性能影响
5. **测试驱动**：确保代码质量和稳定性

**关键实践：**
- 使用组合式API替代Options API
- 合理使用TypeScript增强类型安全
- 实施模块化架构和组件化开发
- 建立完善的错误处理机制
- 编写全面的测试用例
- 保持良好的文档和注释

### 12.2 持续改进

**定期回顾：**
- 每周进行代码审查会议
- 分析项目中的问题和改进点
- 更新和优化开发流程
- 学习和引入新的最佳实践

**技术债务管理：**
- 定期重构和优化代码
- 更新依赖和工具链
- 修复已知的问题和bug
- 改进性能和用户体验

### 12.3 未来展望

**技术趋势：**
- Vue 3.4+的新特性应用
- Composition API的深入使用
- TypeScript的全面采用
- 现代构建工具的优化

**架构演进：**
- 微前端架构的探索
- 服务端渲染的应用
- 移动端适配的优化
- 性能监控和优化

---

**Vue3最佳实践是一个不断发展的领域，随着Vue生态系统的成熟和团队经验的积累，我们需要不断学习和适应新的实践方式。最重要的是建立适合自己团队和项目的开发规范，并在实践中不断完善和优化。记住，最佳实践的目的是提高开发效率和代码质量，而不是为了遵循规则而遵循规则。**
