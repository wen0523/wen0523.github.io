# Vue3工程化详解


# Vue3工程化详解

## 一、Vue3工程化概述

### 1.1 什么是工程化

Vue3工程化是指使用现代化的工具链和最佳实践来构建、管理和维护Vue3应用程序的过程。工程化不仅仅是技术选择，更是一种开发理念和团队协作方式。

**工程化的核心要素：**
- **开发工具**：Vite、Vue CLI、TypeScript
- **代码规范**：ESLint、Prettier、代码风格统一
- **构建优化**：Webpack、Rollup、代码分割
- **测试体系**：单元测试、集成测试、E2E测试
- **部署流程**：CI/CD、自动化部署、监控

### 1.2 工程化的优势

**开发效率提升：**
- 自动化工具链减少重复工作
- 代码规范统一，降低沟通成本
- 热重载、快速开发体验

**代码质量保障：**
- 代码检查和格式化
- 自动化测试覆盖
- 代码审查流程

**项目维护性：**
- 清晰的项目结构
- 完善的文档体系
- 标准化的开发流程

## 二、项目初始化和配置

### 2.1 使用Vite创建项目

**基础项目创建：**
```bash
# 使用npm
npm create vite@latest my-vue-app -- --template vue

# 使用yarn
yarn create vite my-vue-app --template vue

# 使用pnpm
pnpm create vite my-vue-app --template vue
```

**完整的项目配置：**
```bash
# 创建Vue3 + TypeScript项目
npm create vite@latest my-vue-app -- --template vue-ts

# 创建Vue3 + TypeScript + Router项目
npm create vite@latest my-vue-app -- --template vue-ts
cd my-vue-app
npm install vue-router@4
npm install pinia
npm install axios
```

### 2.2 项目结构设计

**推荐的项目结构：**
```
my-vue-app/
├── public/                    # 静态资源
├── src/
│   ├── assets/               # 项目资源
│   │   ├── images/           # 图片
│   │   ├── styles/           # 样式文件
│   │   │   ├── global.scss   # 全局样式
│   │   │   └── variables.scss # 样式变量
│   │   └── fonts/            # 字体文件
│   ├── components/           # 公共组件
│   │   ├── common/           # 通用组件
│   │   ├── layout/           # 布局组件
│   │   └── business/         # 业务组件
│   ├── composables/          # 组合式函数
│   ├── hooks/                # 自定义hooks
│   ├── stores/               # Pinia状态管理
│   ├── router/               # 路由配置
│   ├── views/                # 页面组件
│   ├── utils/                # 工具函数
│   │   ├── http/             # HTTP请求
│   │   ├── validation/      # 验证工具
│   │   └── helpers/          # 辅助函数
│   ├── types/                # TypeScript类型定义
│   ├── constants/            # 常量定义
│   ├── api/                  # API接口
│   ├── App.vue               # 根组件
│   └── main.ts               # 入口文件
├── tests/                    # 测试文件
├── docs/                     # 项目文档
├── .env                      # 环境变量
├── .env.development          # 开发环境变量
├── .env.production           # 生产环境变量
├── vite.config.ts            # Vite配置
├── tsconfig.json             # TypeScript配置
├── package.json              # 项目依赖
└── README.md                 # 项目说明
```

### 2.3 环境配置

**环境变量配置：**
```env
# .env
VITE_APP_TITLE=My Vue App
VITE_APP_VERSION=1.0.0
VITE_APP_API_BASE_URL=https://api.example.com

# .env.development
VITE_APP_API_BASE_URL=http://localhost:3000/api
VITE_APP_DEBUG=true

# .env.production
VITE_APP_API_BASE_URL=https://api.example.com
VITE_APP_DEBUG=false
```

**Vite配置文件：**
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'
import { viteMockServe } from 'vite-plugin-mock'

export default defineConfig({
  plugins: [
    vue(),
    createSvgIconsPlugin({
      iconDirs: [resolve(process.cwd(), 'src/assets/icons')],
      symbolId: 'icon-[name]'
    }),
    viteMockServe({
      mockPath: 'mock',
      localEnabled: true,
      prodEnabled: false,
      injectCode: `
        import { setupProdMockServer } from './mock/_createProductionServer';
        setupProdMockServer();
      `,
      logger: true,
      supportTs: false
    })
  ],
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
      '@components': resolve(__dirname, 'src/components'),
      '@utils': resolve(__dirname, 'src/utils'),
      '@stores': resolve(__dirname, 'src/stores'),
      '@views': resolve(__dirname, 'src/views'),
      '@assets': resolve(__dirname, 'src/assets'),
      '@composables': resolve(__dirname, 'src/composables')
    }
  },
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `@import "@/assets/styles/variables.scss";`
      }
    }
  },
  server: {
    port: 3000,
    open: true,
    cors: true,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  },
  build: {
    outDir: 'dist',
    assetsDir: 'assets',
    sourcemap: false,
    chunkSizeWarningLimit: 1000,
    rollupOptions: {
      output: {
        manualChunks: {
          vue: ['vue', 'vue-router', 'pinia'],
          echarts: ['echarts'],
          utils: ['axios', 'dayjs', 'lodash-es']
        }
      }
    }
  }
})
```

## 三、TypeScript集成

### 3.1 TypeScript配置

**tsconfig.json配置：**
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "moduleResolution": "node",
    "strict": true,
    "jsx": "preserve",
    "sourceMap": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "lib": ["ESNext", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"],
      "@stores/*": ["src/stores/*"],
      "@views/*": ["src/views/*"],
      "@assets/*": ["src/assets/*"],
      "@composables/*": ["src/composables/*"]
    },
    "types": [
      "vite/client",
      "node",
      "jest"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.d.ts",
    "src/**/*.tsx",
    "src/**/*.vue"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "**/*.spec.ts"
  ]
}
```

### 3.2 类型定义

**全局类型定义：**
```typescript
// src/types/global.d.ts
export interface User {
  id: number
  username: string
  email: string
  avatar?: string
  role: 'admin' | 'user' | 'guest'
  createdAt: string
  updatedAt: string
}

export interface ApiResponse<T = any> {
  code: number
  message: string
  data: T
  timestamp: number
}

export interface PaginatedResponse<T> {
  items: T[]
  total: number
  page: number
  pageSize: number
  totalPages: number
}

export interface RouteConfig {
  path: string
  name: string
  component: () => Promise<any>
  meta?: {
    title?: string
    requiresAuth?: boolean
    permissions?: string[]
    keepAlive?: boolean
  }
}

// 扩展Vue的ComponentCustomProperties
declare module '@vue/runtime-core' {
  export interface ComponentCustomProperties {
    $message: (message: string, type?: 'success' | 'error' | 'warning' | 'info') => void
    $loading: {
      show: () => void
      hide: () => void
    }
    $auth: {
      isAuthenticated: () => boolean
      hasPermission: (permission: string) => boolean
      login: (token: string) => void
      logout: () => void
    }
  }
}
```

**组件类型定义：**
```typescript
// src/types/components.d.ts
import { PropType } from 'vue'

export interface ButtonProps {
  type?: 'primary' | 'secondary' | 'danger' | 'warning'
  size?: 'small' | 'medium' | 'large'
  disabled?: boolean
  loading?: boolean
  onClick?: () => void
}

export interface TableProps<T = any> {
  data: T[]
  columns: TableColumn[]
  loading?: boolean
  pagination?: {
    current: number
    pageSize: number
    total: number
    onChange: (page: number, pageSize: number) => void
  }
}

export interface TableColumn {
  key: string
  title: string
  dataIndex?: string
  width?: number
  align?: 'left' | 'center' | 'right'
  render?: (value: any, record: any, index: number) => any
}
```

### 3.3 工具函数类型化

```typescript
// src/utils/http.ts
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios'
import type { ApiResponse } from '@/types'

class HttpClient {
  private instance: AxiosInstance

  constructor(config: AxiosRequestConfig) {
    this.instance = axios.create(config)
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
      (response: AxiosResponse<ApiResponse>) => {
        const { code, message, data } = response.data
        
        if (code === 200) {
          return data
        } else {
          this.handleError(message, code)
          return Promise.reject(new Error(message))
        }
      },
      (error) => {
        this.handleError(error.message, error.response?.status)
        return Promise.reject(error)
      }
    )
  }

  private handleError(message: string, code: number) {
    console.error(`API Error [${code}]: ${message}`)
    
    switch (code) {
      case 401:
        // 跳转到登录页
        window.location.href = '/login'
        break
      case 403:
        console.error('没有权限访问')
        break
      case 404:
        console.error('资源不存在')
        break
      case 500:
        console.error('服务器错误')
        break
    }
  }

  async get<T = any>(url: string, config?: AxiosRequestConfig): Promise<T> {
    return this.instance.get(url, config)
  }

  async post<T = any>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    return this.instance.post(url, data, config)
  }

  async put<T = any>(url: string, data?: any, config?: AxiosRequestConfig): Promise<T> {
    return this.instance.put(url, data, config)
  }

  async delete<T = any>(url: string, config?: AxiosRequestConfig): Promise<T> {
    return this.instance.delete(url, config)
  }
}

export const http = new HttpClient({
  baseURL: import.meta.env.VITE_APP_API_BASE_URL,
  timeout: 10000
})
```

## 四、代码规范和工具配置

### 4.1 ESLint配置

**ESLint配置文件：**
```javascript
// .eslintrc.js
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es2021: true
  },
  extends: [
    'plugin:vue/vue3-recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended'
  ],
  parser: 'vue-eslint-parser',
  parserOptions: {
    parser: '@typescript-eslint/parser',
    ecmaVersion: 2021,
    sourceType: 'module'
  },
  plugins: ['vue', '@typescript-eslint'],
  rules: {
    // Vue相关规则
    'vue/multi-word-component-names': 'off',
    'vue/no-v-html': 'off',
    'vue/require-default-prop': 'off',
    'vue/require-explicit-emits': 'error',
    'vue/no-multiple-template-root': 'off',
    
    // TypeScript相关规则
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/no-non-null-assertion': 'off',
    
    // 通用规则
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'prefer-const': 'error',
    'no-var': 'error',
    'object-shorthand': 'error',
    'prefer-arrow-callback': 'error'
  },
  globals: {
    defineProps: 'readonly',
    defineEmits: 'readonly',
    defineExpose: 'readonly',
    withDefaults: 'readonly'
  }
}
```

### 4.2 Prettier配置

**Prettier配置文件：**
```json
// .prettierrc
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "printWidth": 100,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf",
  "vueIndentScriptAndStyle": true
}
```

**忽略文件：**
```
# .prettierignore
dist/
node_modules/
public/
*.min.js
*.min.css
*.json
*.md
```

### 4.3 Git hooks配置

**husky配置：**
```json
// package.json
{
  "scripts": {
    "prepare": "husky install",
    "lint": "eslint --ext .js,.jsx,.ts,.tsx,.vue src",
    "lint:fix": "eslint --ext .js,.jsx,.ts,.tsx,.vue src --fix",
    "format": "prettier --write src/**/*.{js,jsx,ts,tsx,vue,css,scss,md}",
    "type-check": "vue-tsc --noEmit"
  }
}
```

**lint-staged配置：**
```json
// package.json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx,vue}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{css,scss,md}": [
      "prettier --write"
    ]
  }
}
```

## 五、状态管理工程化

### 5.1 Pinia状态管理结构

**store结构设计：**
```
src/stores/
├── index.ts                 # Pinia实例配置
├── modules/                 # 模块化store
│   ├── user/
│   │   ├── index.ts         # user store
│   │   ├── types.ts         # user类型定义
│   │   └── actions.ts       # user异步操作
│   ├── app/
│   │   ├── index.ts         # app store
│   │   ├── types.ts         # app类型定义
│   │   └── actions.ts       # app异步操作
│   └── cart/
│       ├── index.ts         # cart store
│       ├── types.ts         # cart类型定义
│       └── actions.ts       # cart异步操作
└── plugins/                 # store插件
    ├── persist.ts           # 持久化插件
    └── logger.ts            # 日志插件
```

**用户store示例：**
```typescript
// src/stores/modules/user/index.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { User } from '@/types'
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
  const userName = computed(() => user.value?.username || '')

  // 异步操作
  const login = async (username: string, password: string) => {
    loading.value = true
    error.value = ''
    
    try {
      const response = await userApi.login(username, password)
      token.value = response.token
      user.value = response.user
      
      // 保存到本地存储
      localStorage.setItem('token', response.token)
      localStorage.setItem('user', JSON.stringify(response.user))
      
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
    
    // 清除本地存储
    localStorage.removeItem('token')
    localStorage.removeItem('user')
  }

  const fetchUserProfile = async () => {
    if (!token.value) return
    
    loading.value = true
    try {
      const profile = await userApi.getProfile()
      user.value = profile
    } catch (err) {
      error.value = err.message
      if (err.response?.status === 401) {
        logout()
      }
    } finally {
      loading.value = false
    }
  }

  const updateUser = async (userData: Partial<User>) => {
    loading.value = true
    try {
      const updatedUser = await userApi.updateUser(userData)
      user.value = updatedUser
    } catch (err) {
      error.value = err.message
      throw err
    } finally {
      loading.value = false
    }
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
    userName,
    
    // 方法
    login,
    logout,
    fetchUserProfile,
    updateUser
  }
})
```

### 5.2 持久化插件

**本地存储持久化：**
```typescript
// src/stores/plugins/persist.ts
import { PiniaPluginContext } from 'pinia'

interface PersistOptions {
  key?: string
  storage?: Storage
  paths?: string[]
}

export const persistPlugin = (options: PersistOptions = {}) => {
  return ({ store }: PiniaPluginContext) => {
    const {
      key = store.$id,
      storage = localStorage,
      paths = null
    } = options

    // 从存储中恢复状态
    try {
      const storedState = storage.getItem(key)
      if (storedState) {
        const parsedState = JSON.parse(storedState)
        
        if (paths) {
          // 只恢复指定的路径
          paths.forEach(path => {
            const value = path.split('.').reduce((obj, key) => obj?.[key], parsedState)
            if (value !== undefined) {
              path.split('.').reduce((obj, key, index, arr) => {
                if (index === arr.length - 1) {
                  obj[key] = value
                }
                return obj[key]
              }, store.$state)
            }
          })
        } else {
          // 恢复整个状态
          store.$patch(parsedState)
        }
      }
    } catch (err) {
      console.error(`Failed to restore state for ${key}:`, err)
    }

    // 订阅状态变化
    store.$subscribe(
      (mutation, state) => {
        try {
          let stateToSave = state
          
          if (paths) {
            // 只保存指定的路径
            stateToSave = {}
            paths.forEach(path => {
              const value = path.split('.').reduce((obj, key) => obj?.[key], state)
              if (value !== undefined) {
                path.split('.').reduce((obj, key, index, arr) => {
                  if (index === arr.length - 1) {
                    obj[key] = value
                  }
                  return obj[key]
                }, stateToSave)
              }
            })
          }
          
          storage.setItem(key, JSON.stringify(stateToSave))
        } catch (err) {
          console.error(`Failed to persist state for ${key}:`, err)
        }
      },
      { detached: true }
    )
  }
}
```

## 六、路由工程化

### 6.1 路由结构设计

**模块化路由配置：**
```
src/router/
├── index.ts                 # 路由主配置
├── modules/                 # 模块化路由
│   ├── home.ts              # 首页路由
│   ├── user.ts              # 用户相关路由
│   ├── dashboard.ts         # 仪表板路由
│   └── error.ts             # 错误页面路由
├── guards/                  # 路由守卫
│   ├── auth.ts              # 认证守卫
│   ├── permission.ts        # 权限守卫
│   └── loading.ts           # 加载守卫
├── types.ts                 # 路由类型定义
└── utils.ts                 # 路由工具函数
```

**路由主配置：**
```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router'
import type { App } from 'vue'
import { setupAuthGuard } from './guards/auth'
import { setupPermissionGuard } from './guards/permission'
import { setupLoadingGuard } from './guards/loading'

// 路由模块
const modules = import.meta.glob('./modules/*.ts', { eager: true })

const routes = Object.values(modules).reduce((acc: any[], module) => {
  const routeConfig = (module as any).default
  if (routeConfig && routeConfig.routes) {
    acc.push(...routeConfig.routes)
  }
  return acc
}, [])

const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  }
})

// 设置路由守卫
export function setupRouter(app: App) {
  setupAuthGuard(router)
  setupPermissionGuard(router)
  setupLoadingGuard(router)
  
  app.use(router)
}

export default router
```

**路由模块示例：**
```typescript
// src/router/modules/user.ts
import type { RouteConfig } from '@/types'

const routes: RouteConfig[] = [
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/user/Login.vue'),
    meta: {
      title: '登录',
      requiresAuth: false
    }
  },
  {
    path: '/register',
    name: 'Register',
    component: () => import('@/views/user/Register.vue'),
    meta: {
      title: '注册',
      requiresAuth: false
    }
  },
  {
    path: '/profile',
    name: 'Profile',
    component: () => import('@/views/user/Profile.vue'),
    meta: {
      title: '个人资料',
      requiresAuth: true,
      permissions: ['user:read']
    }
  },
  {
    path: '/settings',
    name: 'Settings',
    component: () => import('@/views/user/Settings.vue'),
    meta: {
      title: '设置',
      requiresAuth: true,
      permissions: ['user:update']
    }
  }
]

export default {
  routes
}
```

### 6.2 路由守卫

**认证守卫：**
```typescript
// src/router/guards/auth.ts
import type { Router } from 'vue-router'
import { useUserStore } from '@/stores/user'

export function setupAuthGuard(router: Router) {
  router.beforeEach(async (to, from, next) => {
    const userStore = useUserStore()
    const requiresAuth = to.meta.requiresAuth !== false
    
    // 如果需要认证但用户未登录
    if (requiresAuth && !userStore.isLoggedIn) {
      // 如果目标路由不是登录页，重定向到登录页
      if (to.path !== '/login') {
        next({
          path: '/login',
          query: { redirect: to.fullPath }
        })
      } else {
        next()
      }
      return
    }
    
    // 如果用户已登录但访问登录页，重定向到首页
    if (userStore.isLoggedIn && to.path === '/login') {
      next('/')
      return
    }
    
    // 获取用户信息
    if (userStore.isLoggedIn && !userStore.user) {
      try {
        await userStore.fetchUserProfile()
      } catch (error) {
        // 获取用户信息失败，可能是token过期
        userStore.logout()
        next({
          path: '/login',
          query: { redirect: to.fullPath }
        })
        return
      }
    }
    
    next()
  })
}
```

**权限守卫：**
```typescript
// src/router/guards/permission.ts
import type { Router } from 'vue-router'
import { useUserStore } from '@/stores/user'

export function setupPermissionGuard(router: Router) {
  router.beforeEach((to, from, next) => {
    const userStore = useUserStore()
    const requiredPermissions = to.meta.permissions as string[] || []
    
    // 如果没有权限要求，直接放行
    if (requiredPermissions.length === 0) {
      next()
      return
    }
    
    // 检查用户权限
    const hasPermission = requiredPermissions.every(permission => 
      userStore.hasPermission(permission)
    )
    
    if (hasPermission) {
      next()
    } else {
      // 权限不足，跳转到403页面
      next('/403')
    }
  })
}
```

## 七、API工程化

### 7.1 API结构设计

**模块化API结构：**
```
src/api/
├── index.ts                 # API主配置
├── http.ts                  # HTTP客户端
├── modules/                 # 模块化API
│   ├── user/
│   │   ├── index.ts         # 用户API
│   │   ├── types.ts         # 用户API类型
│   │   └── validators.ts    # 用户API验证
│   ├── product/
│   │   ├── index.ts         # 产品API
│   │   ├── types.ts         # 产品API类型
│   │   └── validators.ts    # 产品API验证
│   └── order/
│       ├── index.ts         # 订单API
│       ├── types.ts         # 订单API类型
│       └── validators.ts    # 订单API验证
├── interceptors/            # 拦截器
│   ├── auth.ts              # 认证拦截器
│   ├── error.ts             # 错误拦截器
│   └── loading.ts           # 加载拦截器
└── utils/                   # API工具
    ├── cache.ts             # 缓存工具
    ├── retry.ts             # 重试工具
    └── upload.ts            # 上传工具
```

**用户API示例：**
```typescript
// src/api/modules/user/index.ts
import { http } from '@/api/http'
import type { User, LoginRequest, RegisterRequest } from './types'
import { validateLogin, validateRegister } from './validators'

export const userApi = {
  // 用户登录
  async login(username: string, password: string) {
    const requestData: LoginRequest = { username, password }
    validateLogin(requestData)
    
    return http.post<{ token: string; user: User }>('/auth/login', requestData)
  },
  
  // 用户注册
  async register(userData: RegisterRequest) {
    validateRegister(userData)
    
    return http.post<User>('/auth/register', userData)
  },
  
  // 获取用户信息
  async getProfile() {
    return http.get<User>('/user/profile')
  },
  
  // 更新用户信息
  async updateUser(userData: Partial<User>) {
    return http.put<User>('/user/profile', userData)
  },
  
  // 修改密码
  async changePassword(oldPassword: string, newPassword: string) {
    return http.post<void>('/user/change-password', {
      oldPassword,
      newPassword
    })
  },
  
  // 获取用户列表
  async getUsers(params?: {
    page?: number
    pageSize?: number
    role?: string
    search?: string
  }) {
    return http.get<ApiResponse<User[]>>('/user/users', { params })
  },
  
  // 删除用户
  async deleteUser(userId: number) {
    return http.delete<void>(`/user/users/${userId}`)
  }
}
```

### 7.2 API缓存机制

**请求缓存工具：**
```typescript
// src/api/utils/cache.ts
interface CacheOptions {
  ttl?: number // 缓存时间（毫秒）
  maxSize?: number // 最大缓存数量
}

export class ApiCache {
  private cache = new Map<string, { data: any; timestamp: number }>()
  private options: CacheOptions

  constructor(options: CacheOptions = {}) {
    this.options = {
      ttl: 5 * 60 * 1000, // 5分钟
      maxSize: 100,
      ...options
    }
  }

  private generateKey(url: string, params?: any): string {
    return `${url}${params ? JSON.stringify(params) : ''}`
  }

  private isExpired(timestamp: number): boolean {
    return Date.now() - timestamp > this.options.ttl!
  }

  private cleanup() {
    if (this.cache.size > this.options.maxSize!) {
      // 删除最旧的缓存项
      const oldestKey = this.cache.keys().next().value
      this.cache.delete(oldestKey)
    }
  }

  get(url: string, params?: any): any {
    const key = this.generateKey(url, params)
    const cached = this.cache.get(key)
    
    if (cached && !this.isExpired(cached.timestamp)) {
      return cached.data
    }
    
    return null
  }

  set(url: string, data: any, params?: any): void {
    const key = this.generateKey(url, params)
    this.cache.set(key, {
      data,
      timestamp: Date.now()
    })
    
    this.cleanup()
  }

  clear(): void {
    this.cache.clear()
  }

  clearPattern(pattern: RegExp): void {
    for (const key of this.cache.keys()) {
      if (pattern.test(key)) {
        this.cache.delete(key)
      }
    }
  }
}

// 全局缓存实例
export const apiCache = new ApiCache()
```

## 八、测试工程化

### 8.1 单元测试配置

**Jest配置：**
```javascript
// jest.config.js
module.exports = {
  preset: '@vue/cli-plugin-unit-jest',
  testEnvironment: 'jsdom',
  transform: {
    '^.+\\.vue$': '@vue/vue3-jest',
    '^.+\\.(js|jsx|ts|tsx)$': 'babel-jest'
  },
  moduleNameMapping: {
    '^@/(.*)$': '<rootDir>/src/$1',
    '^@components/(.*)$': '<rootDir>/src/components/$1',
    '^@utils/(.*)$': '<rootDir>/src/utils/$1',
    '^@stores/(.*)$': '<rootDir>/src/stores/$1',
    '^@views/(.*)$': '<rootDir>/src/views/$1',
    '^@assets/(.*)$': '<rootDir>/src/assets/$1',
    '^@composables/(.*)$': '<rootDir>/src/composables/$1'
  },
  setupFilesAfterEnv: ['<rootDir>/tests/setup.js'],
  collectCoverage: true,
  collectCoverageFrom: [
    'src/**/*.{js,ts,vue}',
    '!src/main.ts',
    '!src/router/**/*',
    '!src/stores/**/*',
    '!**/node_modules/**'
  ],
  coverageReporters: ['text', 'lcov', 'clover']
}
```

**测试设置文件：**
```javascript
// tests/setup.js
import { config } from '@vue/test-utils'
import { createPinia, setActivePinia } from 'pinia'

// 全局mock
vi.mock('axios', () => ({
  default: {
    create: () => ({
      get: vi.fn(),
      post: vi.fn(),
      put: vi.fn(),
      delete: vi.fn()
    })
  }
}))

// 全局组件注册
config.global.components = {
  // 注册全局组件
}

// 全局指令注册
config.global.directives = {
  // 注册全局指令
}

// 设置Pinia
config.global.plugins = [
  {
    install(app) {
      const pinia = createPinia()
      app.use(pinia)
      setActivePinia(pinia)
    }
  }
]
```

### 8.2 组件测试示例

**简单组件测试：**
```typescript
// tests/components/Button.spec.ts
import { mount } from '@vue/test-utils'
import { describe, it, expect } from 'vitest'
import Button from '@/components/common/Button.vue'

describe('Button.vue', () => {
  it('renders correctly', () => {
    const wrapper = mount(Button, {
      props: {
        type: 'primary'
      }
    })
    
    expect(wrapper.text()).toContain('Button')
    expect(wrapper.classes()).toContain('btn-primary')
  })

  it('emits click event when clicked', async () => {
    const wrapper = mount(Button)
    
    await wrapper.trigger('click')
    
    expect(wrapper.emitted('click')).toBeTruthy()
  })

  it('shows loading state', () => {
    const wrapper = mount(Button, {
      props: {
        loading: true
      }
    })
    
    expect(wrapper.find('.loading-spinner').exists()).toBe(true)
    expect(wrapper.find('button').attributes('disabled')).toBe('')
  })

  it('disables button when disabled prop is true', () => {
    const wrapper = mount(Button, {
      props: {
        disabled: true
      }
    })
    
    expect(wrapper.find('button').attributes('disabled')).toBe('')
  })
})
```

**复杂组件测试：**
```typescript
// tests/components/UserTable.spec.ts
import { mount } from '@vue/test-utils'
import { describe, it, expect, vi } from 'vitest'
import UserTable from '@/components/business/UserTable.vue'
import { useUserStore } from '@/stores/user'

// Mock user store
vi.mock('@/stores/user')
const mockUserStore = {
  users: [
    { id: 1, username: 'user1', email: 'user1@example.com' },
    { id: 2, username: 'user2', email: 'user2@example.com' }
  ],
  loading: false,
  fetchUsers: vi.fn(),
  deleteUser: vi.fn()
}

describe('UserTable.vue', () => {
  beforeEach(() => {
    vi.clearAllMocks()
    ;(useUserStore as any).mockReturnValue(mockUserStore)
  })

  it('renders user table correctly', () => {
    const wrapper = mount(UserTable)
    
    expect(wrapper.find('table').exists()).toBe(true)
    expect(wrapper.findAll('tbody tr')).toHaveLength(2)
  })

  it('shows loading spinner when loading', async () => {
    mockUserStore.loading = true
    const wrapper = mount(UserTable)
    
    expect(wrapper.find('.loading-spinner').exists()).toBe(true)
    expect(wrapper.find('table').exists()).toBe(false)
  })

  it('calls fetchUsers on mount', () => {
    mount(UserTable)
    
    expect(mockUserStore.fetchUsers).toHaveBeenCalled()
  })

  it('deletes user when delete button is clicked', async () => {
    const wrapper = mount(UserTable)
    
    await wrapper.findAll('.delete-btn')[0].trigger('click')
    
    expect(mockUserStore.deleteUser).toHaveBeenCalledWith(1)
  })

  it('shows empty state when no users', async () => {
    mockUserStore.users = []
    const wrapper = mount(UserTable)
    
    expect(wrapper.find('.empty-state').exists()).toBe(true)
    expect(wrapper.find('table').exists()).toBe(false)
  })
})
```

### 8.3 API测试

**API测试示例：**
```typescript
// tests/api/user.spec.ts
import { describe, it, expect, vi, beforeEach } from 'vitest'
import { userApi } from '@/api/modules/user'
import { http } from '@/api/http'

vi.mock('@/api/http')

describe('User API', () => {
  beforeEach(() => {
    vi.clearAllMocks()
  })

  describe('login', () => {
    it('should call login API with correct parameters', async () => {
      const mockResponse = {
        token: 'fake-token',
        user: { id: 1, username: 'testuser' }
      }
      
      vi.mocked(http.post).mockResolvedValue(mockResponse)
      
      const result = await userApi.login('testuser', 'password')
      
      expect(http.post).toHaveBeenCalledWith('/auth/login', {
        username: 'testuser',
        password: 'password'
      })
      expect(result).toEqual(mockResponse)
    })

    it('should handle login errors', async () => {
      const mockError = new Error('Invalid credentials')
      vi.mocked(http.post).mockRejectedValue(mockError)
      
      await expect(userApi.login('testuser', 'wrongpassword'))
        .rejects.toThrow('Invalid credentials')
    })
  })

  describe('getProfile', () => {
    it('should return user profile', async () => {
      const mockUser = {
        id: 1,
        username: 'testuser',
        email: 'test@example.com'
      }
      
      vi.mocked(http.get).mockResolvedValue(mockUser)
      
      const result = await userApi.getProfile()
      
      expect(http.get).toHaveBeenCalledWith('/user/profile')
      expect(result).toEqual(mockUser)
    })
  })
})
```

## 九、CI/CD工程化

### 9.1 GitHub Actions配置

**CI/CD工作流：**
```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linter
      run: npm run lint
    
    - name: Run type check
      run: npm run type-check
    
    - name: Run tests
      run: npm run test:unit
    
    - name: Build project
      run: npm run build
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage/lcov.info
        flags: unittests
        name: codecov-umbrella

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18.x'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build project
      run: npm run build
    
    - name: Deploy to production
      run: |
        # 部署脚本
        echo "Deploying to production..."
        # 添加实际的部署命令
```

### 9.2 Docker配置

**Dockerfile：**
```dockerfile
# Dockerfile
# 构建阶段
FROM node:18-alpine AS builder

WORKDIR /app

# 复制package文件
COPY package*.json ./

# 安装依赖
RUN npm ci --only=production

# 复制源代码
COPY . .

# 构建应用
RUN npm run build

# 生产阶段
FROM nginx:alpine

# 复制构建结果
COPY --from=builder /app/dist /usr/share/nginx/html

# 复制nginx配置
COPY nginx.conf /etc/nginx/nginx.conf

# 暴露端口
EXPOSE 80

# 启动nginx
CMD ["nginx", "-g", "daemon off;"]
```

**nginx配置：**
```nginx
# nginx.conf
events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    server {
        listen 80;
        server_name localhost;
        root /usr/share/nginx/html;
        index index.html;

        # 启用gzip压缩
        gzip on;
        gzip_vary on;
        gzip_min_length 1024;
        gzip_types text/plain text/css text/xml text/javascript application/javascript application/xml+rss application/json;

        # 处理Vue Router的history模式
        location / {
            try_files $uri $uri/ /index.html;
        }

        # 静态资源缓存
        location /assets/ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }

        # API代理
        location /api/ {
            proxy_pass http://backend:3000/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

## 十、总结

### 10.1 工程化检查清单

**项目结构：**
- [ ] 清晰的目录结构
- [ ] 模块化组织
- [ ] 统一的命名规范
- [ ] 完善的文档体系

**开发工具：**
- [ ] TypeScript集成
- [ ] ESLint + Prettier配置
- [ ] Git hooks设置
- [ ] 开发环境配置

**代码质量：**
- [ ] 单元测试覆盖
- [ ] 代码审查流程
- [ ] 性能监控
- [ ] 错误监控

**部署流程：**
- [ ] CI/CD配置
- [ ] 环境配置管理
- [ ] 自动化部署
- [ ] 回滚机制

### 10.2 工程化最佳实践

1. **渐进式实施**：不要一次性实现所有工程化功能
2. **团队协作**：建立团队规范和流程
3. **文档先行**：完善的文档是工程化成功的关键
4. **持续改进**：定期review和优化工程化流程
5. **工具选择**：选择合适的工具，不要过度追求新技术

### 10.3 未来趋势

- **AI辅助开发**：智能代码生成和优化
- **微前端架构**：更灵活的项目组织方式
- **低代码平台**：提高开发效率的新工具
- **DevOps整合**：更紧密的开发运维协作

---

**Vue3工程化是一个系统工程，需要从项目结构、开发工具、代码质量、部署流程等多个维度进行规划和实施。通过合理的工程化实践，我们可以构建出高质量、易维护、可扩展的Vue3应用。记住，工程化的目标是提高开发效率和代码质量，而不是为了工程化而工程化。**
