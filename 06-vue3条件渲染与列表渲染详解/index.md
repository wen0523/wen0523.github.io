# Vue3条件渲染与列表渲染详解


# Vue3条件渲染与列表渲染详解

## 🎯 条件渲染基础

Vue3提供了强大的条件渲染能力，通过`v-if`、`v-else-if`、`v-else`和`v-show`指令来控制元素的显示和隐藏。

### 1. v-if / v-else-if / v-else

这些指令根据表达式的值来条件性地渲染元素或元素块。

```vue
<template>
  <div>
    <!-- 基本条件渲染 -->
    <div class="condition-demo">
      <h3>用户状态</h3>
      
      <div v-if="userStatus === 'admin'">
        <p>👑 管理员权限</p>
        <p>可以管理所有用户和系统设置</p>
      </div>
      
      <div v-else-if="userStatus === 'editor'">
        <p>✏️ 编辑者权限</p>
        <p>可以编辑内容和评论</p>
      </div>
      
      <div v-else-if="userStatus === 'user'">
        <p>👤 普通用户权限</p>
        <p>可以浏览内容和发表评论</p>
      </div>
      
      <div v-else>
        <p>🔒 访客权限</p>
        <p>只能浏览公开内容</p>
      </div>
      
      <!-- 状态切换按钮 -->
      <div class="button-group">
        <button @click="userStatus = 'admin'">管理员</button>
        <button @click="userStatus = 'editor'">编辑者</button>
        <button @click="userStatus = 'user'">普通用户</button>
        <button @click="userStatus = 'guest'">访客</button>
      </div>
    </div>
    
    <!-- 复杂条件判断 -->
    <div class="score-demo">
      <h3>成绩评级</h3>
      
      <div v-if="score >= 90">
        <p class="grade excellent">🌟 优秀 (90-100分)</p>
      </div>
      <div v-else-if="score >= 80">
        <p class="grade good">👍 良好 (80-89分)</p>
      </div>
      <div v-else-if="score >= 70">
        <p class="grade average">👌 中等 (70-79分)</p>
      </div>
      <div v-else-if="score >= 60">
        <p class="grade pass">✅ 及格 (60-69分)</p>
      </div>
      <div v-else>
        <p class="grade fail">❌ 不及格 (0-59分)</p>
      </div>
      
      <!-- 分数调整 -->
      <div class="score-controls">
        <button @click="score = Math.max(0, score - 10)">-10</button>
        <span>当前分数: {{ score }}</span>
        <button @click="score = Math.min(100, score + 10)">+10</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const userStatus = ref('guest')
const score = ref(75)
</script>

<style>
.condition-demo, .score-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.button-group, .score-controls {
  margin-top: 15px;
  display: flex;
  gap: 10px;
  align-items: center;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

.grade {
  padding: 10px;
  border-radius: 4px;
  font-weight: bold;
  text-align: center;
}

.excellent {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.good {
  background-color: #d1ecf1;
  color: #0c5460;
  border: 1px solid #bee5eb;
}

.average {
  background-color: #fff3cd;
  color: #856404;
  border: 1px solid #ffeaa7;
}

.pass {
  background-color: #e2e3e5;
  color: #383d41;
  border: 1px solid #d6d8db;
}

.fail {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

h3 {
  margin-top: 0;
  color: #333;
}
</style>
```

### 2. v-show 指令

`v-show`通过切换元素的`display`CSS属性来控制显示和隐藏。

```vue
<template>
  <div>
    <!-- v-show vs v-if 对比 -->
    <div class="comparison-demo">
      <h3>v-show vs v-if 对比</h3>
      
      <div class="controls">
        <button @click="toggleVisibility">切换显示/隐藏</button>
        <span>当前状态: {{ isVisible ? '显示' : '隐藏' }}</span>
      </div>
      
      <div class="demo-container">
        <!-- v-show 示例 -->
        <div class="demo-item">
          <h4>v-show (CSS切换)</h4>
          <div v-show="isVisible" class="content-box show-box">
            <p>这个元素使用 v-show 控制</p>
            <p>元素始终存在于DOM中，只是通过CSS隐藏</p>
            <p>适合频繁切换的场景</p>
          </div>
        </div>
        
        <!-- v-if 示例 -->
        <div class="demo-item">
          <h4>v-if (DOM操作)</h4>
          <div v-if="isVisible" class="content-box if-box">
            <p>这个元素使用 v-if 控制</p>
            <p>元素在DOM中动态添加/删除</p>
            <p>适合条件很少改变的场景</p>
          </div>
        </div>
      </div>
      
      <!-- 性能对比 -->
      <div class="performance-info">
        <h4>性能特点:</h4>
        <ul>
          <li><strong>v-show</strong>: 初始渲染开销小，切换开销小，但始终占用DOM</li>
          <li><strong>v-if</strong>: 初始渲染开销大，切换开销大，但条件为假时不占用DOM</li>
        </ul>
      </div>
    </div>
    
    <!-- 实际应用场景 -->
    <div class="use-cases">
      <h3>实际应用场景</h3>
      
      <!-- 频繁切换的示例：模态框 -->
      <div class="use-case">
        <h4>模态框 (适合 v-show)</h4>
        <button @click="showModal = !showModal">
          {{ showModal ? '关闭模态框' : '打开模态框' }}
        </button>
        
        <div v-show="showModal" class="modal">
          <div class="modal-content">
            <h3>这是一个模态框</h3>
            <p>使用 v-show 控制，适合频繁打开/关闭</p>
            <button @click="showModal = false">关闭</button>
          </div>
        </div>
      </div>
      
      <!-- 条件很少改变的示例：用户权限 -->
      <div class="use-case">
        <h4>权限控制 (适合 v-if)</h4>
        <button @click="isLoggedIn = !isLoggedIn">
          {{ isLoggedIn ? '退出登录' : '登录' }}
        </button>
        
        <div v-if="isLoggedIn" class="user-panel">
          <h3>用户控制面板</h3>
          <p>只有登录用户才能看到这个面板</p>
          <p>使用 v-if 控制，因为登录状态很少改变</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isVisible = ref(true)
const showModal = ref(false)
const isLoggedIn = ref(false)

function toggleVisibility() {
  isVisible.value = !isVisible.value
}
</script>

<style>
.comparison-demo, .use-cases {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.controls {
  margin-bottom: 20px;
  display: flex;
  gap: 15px;
  align-items: center;
}

.demo-container {
  display: flex;
  gap: 20px;
  margin: 20px 0;
}

.demo-item {
  flex: 1;
}

.content-box {
  padding: 15px;
  border-radius: 4px;
  margin-top: 10px;
}

.show-box {
  background-color: #e3f2fd;
  border: 1px solid #2196f3;
}

.if-box {
  background-color: #e8f5e8;
  border: 1px solid #4caf50;
}

.performance-info {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #ffeaa7;
}

.performance-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.use-case {
  margin: 20px 0;
  padding: 15px;
  background-color: #f5f5f5;
  border-radius: 4px;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 30px;
  border-radius: 8px;
  max-width: 500px;
  width: 90%;
}

.user-panel {
  background-color: #e8f5e8;
  padding: 20px;
  border-radius: 4px;
  margin-top: 10px;
}

h3, h4 {
  margin-top: 0;
  color: #333;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}
</style>
```

### 3. template 条件组

当需要条件渲染多个元素时，可以使用`<template>`标签作为包装元素。

```vue
<template>
  <div>
    <!-- 使用template包裹多个元素 -->
    <div class="template-demo">
      <h3>用户资料 (使用template)</h3>
      
      <template v-if="isLoggedIn">
        <div class="user-info">
          <img :src="user.avatar" :alt="user.name" class="avatar">
          <div class="user-details">
            <h4>{{ user.name }}</h4>
            <p>{{ user.email }}</p>
            <p>加入时间: {{ formatDate(user.joinDate) }}</p>
          </div>
        </div>
        
        <div class="user-stats">
          <div class="stat-item">
            <strong>{{ user.posts }}</strong>
            <span>文章</span>
          </div>
          <div class="stat-item">
            <strong>{{ user.followers }}</strong>
            <span>粉丝</span>
          </div>
          <div class="stat-item">
            <strong>{{ user.following }}</strong>
            <span>关注</span>
          </div>
        </div>
        
        <div class="user-actions">
          <button @click="editProfile">编辑资料</button>
          <button @click="viewPosts">查看文章</button>
          <button @click="logout">退出登录</button>
        </div>
      </template>
      
      <template v-else>
        <div class="login-prompt">
          <h4>请先登录</h4>
          <p>登录后可以查看用户资料和更多信息</p>
          <button @click="login">立即登录</button>
        </div>
      </template>
      
      <!-- 登录状态切换 -->
      <div class="status-toggle">
        <button @click="toggleLogin">
          {{ isLoggedIn ? '模拟退出登录' : '模拟登录' }}
        </button>
      </div>
    </div>
    
    <!-- 复杂的条件渲染示例 -->
    <div class="complex-demo">
      <h3>内容审核状态</h3>
      
      <template v-if="contentStatus === 'published'">
        <div class="status-indicator published">
          <span>✅ 已发布</span>
        </div>
        <div class="content-actions">
          <button @click="editContent">编辑</button>
          <button @click="unpublishContent">下架</button>
          <button @click="viewStats">查看统计</button>
        </div>
      </template>
      
      <template v-else-if="contentStatus === 'pending'">
        <div class="status-indicator pending">
          <span>⏳ 待审核</span>
        </div>
        <div class="content-actions">
          <button @click="approveContent">通过</button>
          <button @click="rejectContent">拒绝</button>
          <button @click="editContent">编辑</button>
        </div>
      </template>
      
      <template v-else-if="contentStatus === 'draft'">
        <div class="status-indicator draft">
          <span>📝 草稿</span>
        </div>
        <div class="content-actions">
          <button @click="editContent">继续编辑</button>
          <button @click="submitForReview">提交审核</button>
          <button @click="deleteDraft">删除草稿</button>
        </div>
      </template>
      
      <template v-else-if="contentStatus === 'rejected'">
        <div class="status-indicator rejected">
          <span>❌ 已拒绝</span>
        </div>
        <div class="content-actions">
          <button @click="editContent">修改后重审</button>
          <button @click="viewRejectionReason">查看拒绝原因</button>
          <button @click="deleteContent">删除内容</button>
        </div>
      </template>
      
      <template v-else>
        <div class="status-indicator archived">
          <span>📦 已归档</span>
        </div>
        <div class="content-actions">
          <button @click="republishContent">重新发布</button>
          <button @click="viewArchive">查看归档</button>
        </div>
      </template>
      
      <!-- 状态切换 -->
      <div class="status-controls">
        <label>切换状态:</label>
        <select v-model="contentStatus">
          <option value="draft">草稿</option>
          <option value="pending">待审核</option>
          <option value="published">已发布</option>
          <option value="rejected">已拒绝</option>
          <option value="archived">已归档</option>
        </select>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const isLoggedIn = ref(false)

const user = reactive({
  name: '张三',
  email: 'zhangsan@example.com',
  avatar: 'https://i.pravatar.cc/150?img=1',
  joinDate: new Date('2023-01-15'),
  posts: 42,
  followers: 128,
  following: 56
})

const contentStatus = ref('draft')

function toggleLogin() {
  isLoggedIn.value = !isLoggedIn.value
}

function formatDate(date) {
  return date.toLocaleDateString('zh-CN')
}

function login() {
  isLoggedIn.value = true
}

function logout() {
  isLoggedIn.value = false
}

function editProfile() {
  console.log('编辑资料')
}

function viewPosts() {
  console.log('查看文章')
}

function editContent() {
  console.log('编辑内容')
}

function unpublishContent() {
  contentStatus.value = 'draft'
}

function viewStats() {
  console.log('查看统计')
}

function approveContent() {
  contentStatus.value = 'published'
}

function rejectContent() {
  contentStatus.value = 'rejected'
}

function submitForReview() {
  contentStatus.value = 'pending'
}

function deleteDraft() {
  console.log('删除草稿')
}

function viewRejectionReason() {
  console.log('查看拒绝原因')
}

function deleteContent() {
  console.log('删除内容')
}

function republishContent() {
  contentStatus.value = 'published'
}

function viewArchive() {
  console.log('查看归档')
}
</script>

<style>
.template-demo, .complex-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 20px;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.user-details h4 {
  margin: 0 0 5px 0;
  color: #333;
}

.user-details p {
  margin: 2px 0;
  color: #666;
}

.user-stats {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.stat-item {
  text-align: center;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 4px;
  min-width: 80px;
}

.stat-item strong {
  display: block;
  font-size: 18px;
  color: #42b983;
}

.stat-item span {
  font-size: 12px;
  color: #666;
}

.user-actions, .content-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.login-prompt {
  text-align: center;
  padding: 40px;
  background-color: #f0f0f0;
  border-radius: 4px;
}

.status-toggle, .status-controls {
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #eee;
}

.status-indicator {
  padding: 10px 15px;
  border-radius: 4px;
  margin-bottom: 15px;
  font-weight: bold;
}

.status-indicator.published {
  background-color: #d4edda;
  color: #155724;
}

.status-indicator.pending {
  background-color: #fff3cd;
  color: #856404;
}

.status-indicator.draft {
  background-color: #e2e3e5;
  color: #383d41;
}

.status-indicator.rejected {
  background-color: #f8d7da;
  color: #721c24;
}

.status-indicator.archived {
  background-color: #e2e3e5;
  color: #383d41;
}

select {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-left: 10px;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

h3 {
  margin-top: 0;
  color: #333;
}
</style>
```

## 📋 列表渲染基础

Vue3的`v-for`指令用于基于数组或对象进行列表渲染。

### 1. 基本数组渲染

```vue
<template>
  <div>
    <!-- 基本数组渲染 -->
    <div class="basic-array">
      <h3>基本数组渲染</h3>
      
      <ul class="item-list">
        <li v-for="item in fruits" :key="item" class="list-item">
          {{ item }}
        </li>
      </ul>
      
      <!-- 添加新水果 -->
      <div class="add-item">
        <input 
          v-model="newFruit" 
          @keyup.enter="addFruit"
          placeholder="输入新水果名称"
        >
        <button @click="addFruit">添加</button>
      </div>
    </div>
    
    <!-- 带索引的数组渲染 -->
    <div class="indexed-array">
      <h3>带索引的数组渲染</h3>
      
      <ol class="numbered-list">
        <li v-for="(fruit, index) in fruits" :key="index" class="numbered-item">
          {{ index + 1 }}. {{ fruit }}
          <button @click="removeFruit(index)" class="remove-btn">删除</button>
        </li>
      </ol>
    </div>
    
    <!-- 对象数组渲染 -->
    <div class="object-array">
      <h3>对象数组渲染</h3>
      
      <div class="user-list">
        <div v-for="user in users" :key="user.id" class="user-card">
          <img :src="user.avatar" :alt="user.name" class="user-avatar">
          <div class="user-info">
            <h4>{{ user.name }}</h4>
            <p>{{ user.email }}</p>
            <p>年龄: {{ user.age }}</p>
          </div>
          <div class="user-actions">
            <button @click="editUser(user.id)">编辑</button>
            <button @click="deleteUser(user.id)">删除</button>
          </div>
        </div>
      </div>
      
      <!-- 添加新用户 -->
      <div class="add-user">
        <h4>添加新用户</h4>
        <input 
          v-model="newUser.name" 
          placeholder="姓名"
        >
        <input 
          v-model="newUser.email" 
          type="email"
          placeholder="邮箱"
        >
        <input 
          v-model.number="newUser.age" 
          type="number"
          placeholder="年龄"
        >
        <button @click="addUser">添加用户</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const fruits = ref(['苹果', '香蕉', '橙子', '葡萄', '草莓'])
const newFruit = ref('')

const users = ref([
  { 
    id: 1, 
    name: '张三', 
    email: 'zhangsan@example.com', 
    age: 25,
    avatar: 'https://i.pravatar.cc/150?img=1'
  },
  { 
    id: 2, 
    name: '李四', 
    email: 'lisi@example.com', 
    age: 30,
    avatar: 'https://i.pravatar.cc/150?img=2'
  },
  { 
    id: 3, 
    name: '王五', 
    email: 'wangwu@example.com', 
    age: 28,
    avatar: 'https://i.pravatar.cc/150?img=3'
  }
])

const newUser = reactive({
  name: '',
  email: '',
  age: null
})

function addFruit() {
  if (newFruit.value.trim()) {
    fruits.value.push(newFruit.value.trim())
    newFruit.value = ''
  }
}

function removeFruit(index) {
  fruits.value.splice(index, 1)
}

function addUser() {
  if (newUser.name && newUser.email && newUser.age) {
    users.value.push({
      id: Date.now(),
      name: newUser.name,
      email: newUser.email,
      age: newUser.age,
      avatar: `https://i.pravatar.cc/150?img=${Math.floor(Math.random() * 70)}`
    })
    
    // 重置表单
    newUser.name = ''
    newUser.email = ''
    newUser.age = null
  }
}

function editUser(id) {
  console.log('编辑用户:', id)
}

function deleteUser(id) {
  users.value = users.value.filter(user => user.id !== id)
}
</script>

<style>
.basic-array, .indexed-array, .object-array {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.item-list, .numbered-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list-item, .numbered-item {
  padding: 10px 15px;
  margin: 5px 0;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.numbered-list {
  counter-reset: item-counter;
}

.numbered-item {
  counter-increment: item-counter;
}

.numbered-item::before {
  content: counter(item-counter) ". ";
  font-weight: bold;
  color: #42b983;
}

.add-item, .add-user {
  margin-top: 15px;
  padding-top: 15px;
  border-top: 1px solid #eee;
}

.add-item input, .add-user input {
  padding: 8px;
  margin-right: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.user-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.user-card {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 8px;
}

.user-avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info {
  flex: 1;
}

.user-info h4 {
  margin: 0 0 5px 0;
  color: #333;
}

.user-info p {
  margin: 2px 0;
  color: #666;
  font-size: 14px;
}

.user-actions {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.remove-btn {
  padding: 4px 8px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.remove-btn:hover {
  background-color: #c82333;
}

button {
  padding: 6px 12px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

button:hover {
  background-color: #3aa876;
}

h3 {
  margin-top: 0;
  color: #333;
}

h4 {
  margin: 15px 0 10px 0;
  color: #333;
}
</style>
```

### 2. 对象渲染

`v-for`也可以用来遍历对象的属性。

```vue
<template>
  <div>
    <!-- 基本对象渲染 -->
    <div class="basic-object">
      <h3>基本对象渲染</h3>
      
      <div class="user-profile">
        <div v-for="(value, key) in userProfile" :key="key" class="profile-item">
          <strong>{{ formatKey(key) }}:</strong> {{ formatValue(key, value) }}
        </div>
      </div>
    </div>
    
    <!-- 带索引的对象渲染 -->
    <div class="indexed-object">
      <h3>带索引的对象渲染</h3>
      
      <ul class="object-list">
        <li v-for="(value, key, index) in userProfile" :key="key" class="object-item">
          <span class="index">{{ index + 1 }}.</span>
          <span class="key">{{ formatKey(key) }}:</span>
          <span class="value">{{ formatValue(key, value) }}</span>
        </li>
      </ul>
    </div>
    
    <!-- 嵌套对象渲染 -->
    <div class="nested-object">
      <h3>嵌套对象渲染</h3>
      
      <div class="nested-data">
        <div v-for="(section, sectionKey) in nestedData" :key="sectionKey" class="section">
          <h4>{{ formatKey(sectionKey) }}</h4>
          
          <div v-if="typeof section === 'object' && section !== null">
            <div v-for="(item, itemKey) in section" :key="itemKey" class="item">
              <strong>{{ formatKey(itemKey) }}:</strong> {{ item }}
            </div>
          </div>
          <div v-else>
            <p>{{ section }}</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 动态对象编辑 -->
    <div class="object-editor">
      <h3>动态对象编辑</h3>
      
      <div class="editor-form">
        <div v-for="(value, key) in editableObject" :key="key" class="form-group">
          <label :for="'field-' + key">{{ formatKey(key) }}:</label>
          <input 
            :id="'field-' + key"
            v-model="editableObject[key]"
            :type="getInputType(key, value)"
          >
        </div>
        
        <div class="form-actions">
          <button @click="addNewField">添加新字段</button>
          <button @click="resetObject">重置</button>
        </div>
      </div>
      
      <div class="object-preview">
        <h4>对象预览:</h4>
        <pre>{{ JSON.stringify(editableObject, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const userProfile = reactive({
  name: '张三',
  age: 25,
  email: 'zhangsan@example.com',
  city: '北京',
  occupation: '前端开发',
  joinDate: new Date('2023-01-15'),
  isActive: true,
  salary: 15000
})

const nestedData = reactive({
  personal: {
    name: '李四',
    age: 30,
    email: 'lisi@example.com'
  },
  professional: {
    occupation: '后端开发',
    experience: '5年',
    skills: ['JavaScript', 'Python', 'Java']
  },
  contact: {
    phone: '13800138000',
    address: '上海市浦东新区',
    emergency: '王五 - 13900139000'
  }
})

const editableObject = reactive({
  title: '前端开发工程师',
  department: '技术部',
  level: '中级',
  salary: 12000,
  remote: true,
  startDate: '2023-01-01'
})

function formatKey(key) {
  const keyMap = {
    name: '姓名',
    age: '年龄',
    email: '邮箱',
    city: '城市',
    occupation: '职业',
    joinDate: '入职日期',
    isActive: '状态',
    salary: '薪资',
    phone: '电话',
    address: '地址',
    emergency: '紧急联系人',
    experience: '工作经验',
    skills: '技能',
    title: '职位',
    department: '部门',
    level: '级别',
    remote: '远程工作',
    startDate: '开始日期'
  }
  
  return keyMap[key] || key
}

function formatValue(key, value) {
  if (key === 'joinDate' || key === 'startDate') {
    return new Date(value).toLocaleDateString('zh-CN')
  }
  
  if (key === 'isActive' || key === 'remote') {
    return value ? '是' : '否'
  }
  
  if (key === 'salary') {
    return `¥${value.toLocaleString()}`
  }
  
  if (Array.isArray(value)) {
    return value.join(', ')
  }
  
  return value
}

function getInputType(key, value) {
  if (key === 'salary') return 'number'
  if (key === 'startDate') return 'date'
  if (key === 'isActive' || key === 'remote') return 'checkbox'
  return 'text'
}

function addNewField() {
  const fieldName = prompt('请输入新字段名:')
  if (fieldName && !editableObject.hasOwnProperty(fieldName)) {
    editableObject[fieldName] = ''
  }
}

function resetObject() {
  Object.assign(editableObject, {
    title: '前端开发工程师',
    department: '技术部',
    level: '中级',
    salary: 12000,
    remote: true,
    startDate: '2023-01-01'
  })
}
</script>

<style>
.basic-object, .indexed-object, .nested-object, .object-editor {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.user-profile {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.profile-item {
  padding: 15px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.profile-item strong {
  color: #42b983;
  display: block;
  margin-bottom: 5px;
}

.object-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.object-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
  margin: 5px 0;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.object-item .index {
  font-weight: bold;
  color: #42b983;
  margin-right: 10px;
  min-width: 30px;
}

.object-item .key {
  font-weight: bold;
  color: #333;
  margin-right: 10px;
  min-width: 100px;
}

.object-item .value {
  color: #666;
  flex: 1;
}

.nested-data {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.section {
  padding: 15px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.section h4 {
  margin-top: 0;
  color: #42b983;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
}

.section .item {
  padding: 5px 0;
  border-bottom: 1px solid #f5f5f5;
}

.section .item:last-child {
  border-bottom: none;
}

.editor-form {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.form-group input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.form-actions {
  grid-column: 1 / -1;
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.object-preview {
  background-color: #f5f5f5;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #ddd;
}

.object-preview h4 {
  margin-top: 0;
  color: #333;
}

.object-preview pre {
  margin: 10px 0 0 0;
  white-space: pre-wrap;
  word-break: break-all;
  font-size: 12px;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

h3 {
  margin-top: 0;
  color: #333;
}

h4 {
  margin: 0 0 10px 0;
  color: #333;
}
</style>
```

### 3. 数字范围渲染

`v-for`可以接受数字，用于渲染指定次数的元素。

```vue
<template>
  <div>
    <!-- 基本数字范围渲染 -->
    <div class="basic-range">
      <h3>基本数字范围渲染</h3>
      
      <div class="range-container">
        <div v-for="n in 5" :key="n" class="range-item">
          项目 {{ n }}
        </div>
      </div>
      
      <!-- 自定义范围 -->
      <div class="custom-range">
        <h4>自定义范围</h4>
        <div class="range-controls">
          <label>开始:</label>
          <input v-model.number="startRange" type="number" min="1">
          <label>结束:</label>
          <input v-model.number="endRange" type="number" min="1">
        </div>
        
        <div class="range-display">
          <div v-for="n in (endRange - startRange + 1)" :key="n" class="range-number">
            {{ startRange + n - 1 }}
          </div>
        </div>
      </div>
    </div>
    
    <!-- 分页器示例 -->
    <div class="pagination-demo">
      <h3>分页器示例</h3>
      
      <div class="pagination-info">
        <p>总项目数: {{ totalItems }}</p>
        <p>每页显示: {{ itemsPerPage }}</p>
        <p>总页数: {{ totalPages }}</p>
      </div>
      
      <div class="pagination">
        <button 
          @click="currentPage = 1" 
          :disabled="currentPage === 1"
        >
          首页
        </button>
        
        <button 
          @click="currentPage--" 
          :disabled="currentPage === 1"
        >
          上一页
        </button>
        
        <div class="page-numbers">
          <button 
            v-for="page in displayedPages" 
            :key="page"
            @click="currentPage = page"
            :class="{ active: currentPage === page }"
          >
            {{ page }}
          </button>
        </div>
        
        <button 
          @click="currentPage++" 
          :disabled="currentPage === totalPages"
        >
          下一页
        </button>
        
        <button 
          @click="currentPage = totalPages" 
          :disabled="currentPage === totalPages"
        >
          末页
        </button>
      </div>
      
      <div class="page-info">
        第 {{ currentPage }} 页，共 {{ totalPages }} 页
      </div>
    </div>
    
    <!-- 进度条示例 -->
    <div class="progress-demo">
      <h3>进度条示例</h3>
      
      <div class="progress-controls">
        <label>当前进度: {{ currentProgress }}%</label>
        <input 
          v-model.number="currentProgress" 
          type="range" 
          min="0" 
          max="100" 
          step="1"
        >
        <button @click="currentProgress = 0">重置</button>
        <button @click="startProgress">开始进度</button>
      </div>
      
      <div class="progress-bar">
        <div 
          v-for="n in 10" 
          :key="n"
          class="progress-segment"
          :class="{ filled: (n * 10) <= currentProgress }"
        ></div>
      </div>
      
      <div class="progress-steps">
        <div 
          v-for="step in progressSteps" 
          :key="step.id"
          class="step"
          :class="{ 
            completed: step.completed, 
            current: step.current,
            upcoming: !step.completed && !step.current
          }"
        >
          <div class="step-number">{{ step.id }}</div>
          <div class="step-label">{{ step.label }}</div>
        </div>
      </div>
    </div>
    
    <!-- 网格布局示例 -->
    <div class="grid-demo">
      <h3>网格布局示例</h3>
      
      <div class="grid-controls">
        <label>列数:</label>
        <input v-model.number="gridColumns" type="number" min="1" max="12">
        <label>行数:</label>
        <input v-model.number="gridRows" type="number" min="1" max="12">
      </div>
      
      <div 
        class="grid" 
        :style="{ 
          gridTemplateColumns: `repeat(${gridColumns}, 1fr)`,
          gridTemplateRows: `repeat(${gridRows}, 1fr)`
        }"
      >
        <div 
          v-for="cell in (gridColumns * gridRows)" 
          :key="cell"
          class="grid-cell"
        >
          {{ cell }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 基本范围
const startRange = ref(1)
const endRange = ref(10)

// 分页器
const totalItems = ref(100)
const itemsPerPage = ref(10)
const currentPage = ref(1)

const totalPages = computed(() => Math.ceil(totalItems.value / itemsPerPage.value))

const displayedPages = computed(() => {
  const pages = []
  const maxVisible = 5
  const halfVisible = Math.floor(maxVisible / 2)
  
  let start = Math.max(1, currentPage.value - halfVisible)
  let end = Math.min(totalPages.value, start + maxVisible - 1)
  
  if (end - start + 1 < maxVisible) {
    start = Math.max(1, end - maxVisible + 1)
  }
  
  for (let i = start; i <= end; i++) {
    pages.push(i)
  }
  
  return pages
})

// 进度条
const currentProgress = ref(0)
let progressInterval = null

const progressSteps = computed(() => {
  return [
    { id: 1, label: '开始', completed: currentProgress.value >= 25, current: currentProgress.value >= 0 && currentProgress.value < 25 },
    { id: 2, label: '进行中', completed: currentProgress.value >= 50, current: currentProgress.value >= 25 && currentProgress.value < 50 },
    { id: 3, label: '即将完成', completed: currentProgress.value >= 75, current: currentProgress.value >= 50 && currentProgress.value < 75 },
    { id: 4, label: '完成', completed: currentProgress.value >= 100, current: currentProgress.value >= 75 && currentProgress.value < 100 }
  ]
})

function startProgress() {
  if (progressInterval) clearInterval(progressInterval)
  
  progressInterval = setInterval(() => {
    if (currentProgress.value >= 100) {
      clearInterval(progressInterval)
      return
    }
    currentProgress.value += 1
  }, 50)
}

// 网格布局
const gridColumns = ref(4)
const gridRows = ref(3)
</script>

<style>
.basic-range, .pagination-demo, .progress-demo, .grid-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.range-container {
  display: flex;
  gap: 10px;
  margin: 15px 0;
}

.range-item {
  padding: 15px;
  background-color: #42b983;
  color: white;
  border-radius: 4px;
  text-align: center;
  flex: 1;
}

.custom-range {
  margin-top: 20px;
}

.range-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.range-controls input {
  width: 80px;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.range-display {
  display: flex;
  gap: 5px;
  flex-wrap: wrap;
}

.range-number {
  padding: 8px 12px;
  background-color: #e3f2fd;
  color: #1976d2;
  border-radius: 4px;
  min-width: 40px;
  text-align: center;
  font-weight: bold;
}

.pagination-info {
  margin-bottom: 15px;
  color: #666;
}

.pagination {
  display: flex;
  align-items: center;
  gap: 5px;
  flex-wrap: wrap;
}

.page-numbers {
  display: flex;
  gap: 2px;
}

.pagination button {
  padding: 8px 12px;
  border: 1px solid #ddd;
  background-color: white;
  color: #333;
  border-radius: 4px;
  cursor: pointer;
  min-width: 40px;
}

.pagination button:hover:not(:disabled) {
  background-color: #f0f0f0;
}

.pagination button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.pagination button.active {
  background-color: #42b983;
  color: white;
  border-color: #42b983;
}

.page-info {
  margin-top: 10px;
  text-align: center;
  color: #666;
}

.progress-controls {
  margin-bottom: 15px;
}

.progress-controls input {
  margin: 0 10px;
}

.progress-bar {
  display: flex;
  gap: 2px;
  height: 20px;
  background-color: #e0e0e0;
  border-radius: 10px;
  overflow: hidden;
  margin: 15px 0;
}

.progress-segment {
  flex: 1;
  background-color: #e0e0e0;
  transition: background-color 0.3s;
}

.progress-segment.filled {
  background-color: #42b983;
}

.progress-steps {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.step {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  flex: 1;
}

.step-number {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  transition: all 0.3s;
}

.step.completed .step-number {
  background-color: #42b983;
  color: white;
}

.step.current .step-number {
  background-color: #ff9800;
  color: white;
}

.step.upcoming .step-number {
  background-color: #e0e0e0;
  color: #666;
}

.step-label {
  font-size: 12px;
  color: #666;
  text-align: center;
}

.grid-controls {
  margin-bottom: 15px;
  display: flex;
  gap: 15px;
  align-items: center;
}

.grid-controls input {
  width: 60px;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.grid {
  display: grid;
  gap: 2px;
  background-color: #ddd;
  padding: 2px;
  border-radius: 4px;
}

.grid-cell {
  background-color: #42b983;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  min-height: 40px;
  border-radius: 2px;
}

h3 {
  margin-top: 0;
  color: #333;
}

h4 {
  margin: 15px 0 10px 0;
  color: #333;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover:not(:disabled) {
  background-color: #3aa876;
}

label {
  color: #333;
  font-weight: bold;
}
</style>
```

## 🎯 高级列表渲染

### 1. key的重要性

在列表渲染中，`key`属性对于Vue的虚拟DOM算法至关重要。

```vue
<template>
  <div>
    <!-- key的重要性演示 -->
    <div class="key-demo">
      <h3>key的重要性演示</h3>
      
      <div class="demo-section">
        <h4>❌ 错误：使用索引作为key</h4>
        <div class="user-list">
          <div v-for="(user, index) in usersWithIndexKey" :key="index" class="user-item">
            <input type="text" v-model="user.name" :placeholder="'用户' + (index + 1)">
            <span>{{ user.name }}</span>
            <button @click="removeUserWithIndexKey(index)">删除</button>
          </div>
        </div>
        <button @click="addUserWithIndexKey">添加用户</button>
      </div>
      
      <div class="demo-section">
        <h4>✅ 正确：使用唯一ID作为key</h4>
        <div class="user-list">
          <div v-for="user in usersWithIdKey" :key="user.id" class="user-item">
            <input type="text" v-model="user.name" :placeholder="'用户' + user.id">
            <span>{{ user.name }}</span>
            <button @click="removeUserWithIdKey(user.id)">删除</button>
          </div>
        </div>
        <button @click="addUserWithIdKey">添加用户</button>
      </div>
      
      <div class="explanation">
        <h4>为什么key很重要？</h4>
        <ul>
          <li><strong>使用索引作为key</strong>：当数组顺序改变时，Vue会错误地复用DOM元素</li>
          <li><strong>使用唯一ID作为key</strong>：Vue可以正确地跟踪每个元素的身份</li>
          <li><strong>性能优化</strong>：正确的key可以减少不必要的DOM操作</li>
          <li><strong>状态保持</strong>：输入框等组件的状态能正确保持</li>
        </ul>
      </div>
    </div>
    
    <!-- 复杂场景的key使用 -->
    <div class="complex-key-demo">
      <h3>复杂场景的key使用</h3>
      
      <div class="scenario">
        <h4>嵌套列表</h4>
        <div v-for="category in categories" :key="category.id" class="category">
          <h5>{{ category.name }}</h5>
          <div v-for="item in category.items" :key="item.id" class="category-item">
            <input type="checkbox" v-model="item.selected">
            <span>{{ item.name }}</span>
          </div>
        </div>
      </div>
      
      <div class="scenario">
        <h4>动态组件列表</h4>
        <div v-for="component in componentList" :key="component.id" class="component-item">
          <component :is="component.type" v-bind="component.props">
            {{ component.content }}
          </component>
        </div>
      </div>
    </div>
    
    <!-- 性能对比 -->
    <div class="performance-demo">
      <h3>性能对比演示</h3>
      
      <div class="performance-controls">
        <label>列表大小:</label>
        <input v-model.number="listSize" type="number" min="10" max="1000">
        <button @click="generateLists">生成列表</button>
        <button @click="shuffleLists">随机排序</button>
        <button @click="measurePerformance">测量性能</button>
      </div>
      
      <div class="performance-results">
        <div v-if="performanceResults.indexKey" class="result">
          <h4>索引key性能:</h4>
          <p>渲染时间: {{ performanceResults.indexKey }}ms</p>
        </div>
        <div v-if="performanceResults.idKey" class="result">
          <h4>ID key性能:</h4>
          <p>渲染时间: {{ performanceResults.idKey }}ms</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

// key重要性演示
const usersWithIndexKey = ref([
  { id: 1, name: '张三' },
  { id: 2, name: '李四' },
  { id: 3, name: '王五' }
])

const usersWithIdKey = ref([
  { id: 1, name: '张三' },
  { id: 2, name: '李四' },
  { id: 3, name: '王五' }
])

function addUserWithIndexKey() {
  const newId = Math.max(...usersWithIndexKey.value.map(u => u.id)) + 1
  usersWithIndexKey.value.push({ id: newId, name: `用户${newId}` })
}

function removeUserWithIndexKey(index) {
  usersWithIndexKey.value.splice(index, 1)
}

function addUserWithIdKey() {
  const newId = Math.max(...usersWithIdKey.value.map(u => u.id)) + 1
  usersWithIdKey.value.push({ id: newId, name: `用户${newId}` })
}

function removeUserWithIdKey(id) {
  usersWithIndexKey.value = usersWithIdKey.value.filter(user => user.id !== id)
}

// 复杂场景
const categories = ref([
  {
    id: 1,
    name: '水果',
    items: [
      { id: 101, name: '苹果', selected: false },
      { id: 102, name: '香蕉', selected: false },
      { id: 103, name: '橙子', selected: false }
    ]
  },
  {
    id: 2,
    name: '蔬菜',
    items: [
      { id: 201, name: '西红柿', selected: false },
      { id: 202, name: '黄瓜', selected: false },
      { id: 203, name: '胡萝卜', selected: false }
    ]
  }
])

const componentList = ref([
  { id: 1, type: 'button', props: { class: 'btn-primary' }, content: '按钮1' },
  { id: 2, type: 'div', props: { class: 'alert' }, content: '警告信息' },
  { id: 3, type: 'span', props: { class: 'badge' }, content: '标签' }
])

// 性能对比
const listSize = ref(100)
const performanceResults = reactive({})

const largeListWithIndexKey = ref([])
const largeListWithIdKey = ref([])

function generateLists() {
  largeListWithIndexKey.value = Array.from({ length: listSize.value }, (_, index) => ({
    id: index + 1,
    name: `项目${index + 1}`,
    value: Math.random()
  }))
  
  largeListWithIdKey.value = JSON.parse(JSON.stringify(largeListWithIndexKey.value))
}

function shuffleLists() {
  // Fisher-Yates shuffle
  function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]]
    }
  }
  
  shuffle(largeListWithIndexKey.value)
  shuffle(largeListWithIdKey.value)
}

function measurePerformance() {
  // 清空之前的结果
  performanceResults.indexKey = null
  performanceResults.idKey = null
  
  // 测量索引key性能
  setTimeout(() => {
    const start = performance.now()
    // 触发重新渲染
    largeListWithIndexKey.value = [...largeListWithIndexKey.value]
    const end = performance.now()
    performanceResults.indexKey = (end - start).toFixed(2)
  }, 100)
  
  // 测量ID key性能
  setTimeout(() => {
    const start = performance.now()
    // 触发重新渲染
    largeListWithIdKey.value = [...largeListWithIdKey.value]
    const end = performance.now()
    performanceResults.idKey = (end - start).toFixed(2)
  }, 200)
}

// 初始化
generateLists()
</script>

<style>
.key-demo, .complex-key-demo, .performance-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.demo-section {
  margin-bottom: 30px;
  padding: 15px;
  background-color: #fff;
  border-radius: 4px;
  border: 1px solid #eee;
}

.user-list {
  margin: 15px 0;
}

.user-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  margin: 5px 0;
  background-color: #f8f9fa;
  border-radius: 4px;
}

.user-item input {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
  width: 120px;
}

.user-item span {
  flex: 1;
}

.user-item button {
  padding: 4px 8px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.explanation {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #2196f3;
}

.explanation h4 {
  margin-top: 0;
  color: #1976d2;
}

.explanation ul {
  margin: 10px 0;
  padding-left: 20px;
}

.category {
  margin: 15px 0;
  padding: 15px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.category h5 {
  margin-top: 0;
  color: #42b983;
}

.category-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px;
  margin: 5px 0;
  background-color: #f8f9fa;
  border-radius: 3px;
}

.component-item {
  margin: 10px 0;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.btn-primary {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
}

.alert {
  padding: 10px;
  background-color: #fff3cd;
  color: #856404;
  border: 1px solid #ffeaa7;
  border-radius: 4px;
}

.badge {
  display: inline-block;
  padding: 4px 8px;
  background-color: #17a2b8;
  color: white;
  border-radius: 12px;
  font-size: 12px;
}

.performance-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.performance-controls input {
  width: 80px;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.performance-results {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

.result {
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #ddd;
}

.result h4 {
  margin-top: 0;
  color: #333;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

h3, h4, h5 {
  margin-top: 0;
  color: #333;
}
</style>
```

### 2. 列表过滤和排序

```vue
<template>
  <div>
    <!-- 基本过滤和排序 -->
    <div class="filter-sort-demo">
      <h3>基本过滤和排序</h3>
      
      <div class="controls">
        <div class="filter-group">
          <label>搜索:</label>
          <input 
            v-model="searchQuery" 
            placeholder="搜索用户..."
          >
        </div>
        
        <div class="sort-group">
          <label>排序:</label>
          <select v-model="sortBy">
            <option value="name">按姓名</option>
            <option value="age">按年龄</option>
            <option value="joinDate">按入职时间</option>
            <option value="salary">按薪资</option>
          </select>
          
          <select v-model="sortOrder">
            <option value="asc">升序</option>
            <option value="desc">降序</option>
          </select>
        </div>
        
        <div class="filter-group">
          <label>部门:</label>
          <select v-model="departmentFilter">
            <option value="">所有部门</option>
            <option value="技术部">技术部</option>
            <option value="市场部">市场部</option>
            <option value="人事部">人事部</option>
          </select>
        </div>
      </div>
      
      <div class="results-info">
        显示 {{ filteredUsers.length }} / {{ users.length }} 个用户
      </div>
      
      <div class="user-grid">
        <div 
          v-for="user in filteredUsers" 
          :key="user.id" 
          class="user-card"
        >
          <img :src="user.avatar" :alt="user.name" class="user-avatar">
          <div class="user-info">
            <h4>{{ user.name }}</h4>
            <p>{{ user.email }}</p>
            <p>年龄: {{ user.age }}</p>
            <p>部门: {{ user.department }}</p>
            <p>薪资: ¥{{ user.salary.toLocaleString() }}</p>
            <p>入职: {{ formatDate(user.joinDate) }}</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 高级过滤 -->
    <div class="advanced-filter-demo">
      <h3>高级过滤</h3>
      
      <div class="advanced-controls">
        <div class="filter-section">
          <h4>薪资范围</h4>
          <div class="range-filter">
            <label>最低:</label>
            <input 
              v-model.number="salaryRange.min" 
              type="number"
              placeholder="0"
            >
            <label>最高:</label>
            <input 
              v-model.number="salaryRange.max" 
              type="number"
              placeholder="无限制"
            >
          </div>
        </div>
        
        <div class="filter-section">
          <h4>年龄范围</h4>
          <div class="range-filter">
            <label>最低:</label>
            <input 
              v-model.number="ageRange.min" 
              type="number"
              placeholder="0"
            >
            <label>最高:</label>
            <input 
              v-model.number="ageRange.max" 
              type="number"
              placeholder="无限制"
            >
          </div>
        </div>
        
        <div class="filter-section">
          <h4>技能标签</h4>
          <div class="skill-tags">
            <label 
              v-for="skill in availableSkills" 
              :key="skill"
              class="skill-tag"
            >
              <input 
                type="checkbox" 
                v-model="selectedSkills" 
                :value="skill"
              >
              {{ skill }}
            </label>
          </div>
        </div>
        
        <div class="filter-section">
          <h4>其他筛选</h4>
          <div class="checkbox-filters">
            <label>
              <input type="checkbox" v-model="showActiveOnly">
              仅显示在职员工
            </label>
            <label>
              <input type="checkbox" v-model="showManagersOnly">
              仅显示管理者
            </label>
          </div>
        </div>
      </div>
      
      <div class="filter-actions">
        <button @click="applyAdvancedFilters">应用筛选</button>
        <button @click="resetAdvancedFilters">重置筛选</button>
      </div>
      
      <div class="advanced-results">
        <div 
          v-for="user in advancedFilteredUsers" 
          :key="user.id" 
          class="advanced-user-card"
        >
          <div class="card-header">
            <h4>{{ user.name }}</h4>
            <span :class="['status-badge', user.isActive ? 'active' : 'inactive']">
              {{ user.isActive ? '在职' : '离职' }}
            </span>
          </div>
          <div class="card-content">
            <p><strong>邮箱:</strong> {{ user.email }}</p>
            <p><strong>部门:</strong> {{ user.department }}</p>
            <p><strong>职位:</strong> {{ user.position }}</p>
            <p><strong>薪资:</strong> ¥{{ user.salary.toLocaleString() }}</p>
            <div class="skills">
              <strong>技能:</strong>
              <span 
                v-for="skill in user.skills" 
                :key="skill"
                class="skill-badge"
              >
                {{ skill }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

// 基本过滤和排序
const users = ref([
  { 
    id: 1, 
    name: '张三', 
    email: 'zhangsan@example.com', 
    age: 25, 
    department: '技术部',
    position: '前端开发',
    salary: 12000,
    joinDate: new Date('2023-01-15'),
    isActive: true,
    skills: ['JavaScript', 'Vue.js', 'CSS'],
    avatar: 'https://i.pravatar.cc/150?img=1'
  },
  { 
    id: 2, 
    name: '李四', 
    email: 'lisi@example.com', 
    age: 30, 
    department: '市场部',
    position: '市场经理',
    salary: 15000,
    joinDate: new Date('2022-03-20'),
    isActive: true,
    skills: ['市场营销', '数据分析', 'PPT'],
    avatar: 'https://i.pravatar.cc/150?img=2'
  },
  { 
    id: 3, 
    name: '王五', 
    email: 'wangwu@example.com', 
    age: 28, 
    department: '技术部',
    position: '后端开发',
    salary: 14000,
    joinDate: new Date('2022-08-10'),
    isActive: true,
    skills: ['Java', 'Spring', 'MySQL'],
    avatar: 'https://i.pravatar.cc/150?img=3'
  },
  { 
    id: 4, 
    name: '赵六', 
    email: 'zhaoliu@example.com', 
    age: 35, 
    department: '人事部',
    position: '人事经理',
    salary: 16000,
    joinDate: new Date('2021-05-15'),
    isActive: true,
    skills: ['人力资源管理', '招聘', '培训'],
    avatar: 'https://i.pravatar.cc/150?img=4'
  },
  { 
    id: 5, 
    name: '钱七', 
    email: 'qianqi@example.com', 
    age: 26, 
    department: '技术部',
    position: '测试工程师',
    salary: 11000,
    joinDate: new Date('2023-02-01'),
    isActive: false,
    skills: ['测试', '自动化', 'Selenium'],
    avatar: 'https://i.pravatar.cc/150?img=5'
  }
])

const searchQuery = ref('')
const sortBy = ref('name')
const sortOrder = ref('asc')
const departmentFilter = ref('')

const filteredUsers = computed(() => {
  let result = [...users.value]
  
  // 搜索过滤
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    result = result.filter(user => 
      user.name.toLowerCase().includes(query) ||
      user.email.toLowerCase().includes(query) ||
      user.department.toLowerCase().includes(query)
    )
  }
  
  // 部门过滤
  if (departmentFilter.value) {
    result = result.filter(user => user.department === departmentFilter.value)
  }
  
  // 排序
  result.sort((a, b) => {
    let aValue = a[sortBy.value]
    let bValue = b[sortBy.value]
    
    // 处理日期排序
    if (sortBy.value === 'joinDate') {
      aValue = aValue.getTime()
      bValue = bValue.getTime()
    }
    
    if (sortOrder.value === 'asc') {
      return aValue > bValue ? 1 : -1
    } else {
      return aValue < bValue ? 1 : -1
    }
  })
  
  return result
})

// 高级过滤
const salaryRange = reactive({ min: null, max: null })
const ageRange = reactive({ min: null, max: null })
const selectedSkills = ref([])
const showActiveOnly = ref(false)
const showManagersOnly = ref(false)

const availableSkills = ref([
  'JavaScript', 'Vue.js', 'React', 'Java', 'Python', 
  '市场营销', '数据分析', '人力资源管理', '测试', '自动化'
])

const advancedFilteredUsers = computed(() => {
  let result = [...users.value]
  
  // 薪资范围过滤
  if (salaryRange.min !== null) {
    result = result.filter(user => user.salary >= salaryRange.min)
  }
  if (salaryRange.max !== null) {
    result = result.filter(user => user.salary <= salaryRange.max)
  }
  
  // 年龄范围过滤
  if (ageRange.min !== null) {
    result = result.filter(user => user.age >= ageRange.min)
  }
  if (ageRange.max !== null) {
    result = result.filter(user => user.age <= ageRange.max)
  }
  
  // 技能过滤
  if (selectedSkills.value.length > 0) {
    result = result.filter(user => 
      selectedSkills.value.some(skill => user.skills.includes(skill))
    )
  }
  
  // 在职状态过滤
  if (showActiveOnly.value) {
    result = result.filter(user => user.isActive)
  }
  
  // 管理者过滤
  if (showManagersOnly.value) {
    result = result.filter(user => user.position.includes('经理'))
  }
  
  return result
})

function formatDate(date) {
  return date.toLocaleDateString('zh-CN')
}

function applyAdvancedFilters() {
  // 触发计算属性重新计算
  console.log('应用高级筛选')
}

function resetAdvancedFilters() {
  salaryRange.min = null
  salaryRange.max = null
  ageRange.min = null
  ageRange.max = null
  selectedSkills.value = []
  showActiveOnly.value = false
  showManagersOnly.value = false
}
</script>

<style>
.filter-sort-demo, .advanced-filter-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.filter-group, .sort-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.filter-group label, .sort-group label {
  font-weight: bold;
  color: #333;
}

.filter-group input, .sort-group select {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.results-info {
  margin-bottom: 15px;
  color: #666;
  font-weight: bold;
}

.user-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 15px;
}

.user-card {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 8px;
  transition: transform 0.2s, box-shadow 0.2s;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
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

.user-info h4 {
  margin: 0 0 5px 0;
  color: #333;
}

.user-info p {
  margin: 2px 0;
  color: #666;
  font-size: 14px;
}

.advanced-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.filter-section {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.filter-section h4 {
  margin-top: 0;
  color: #42b983;
  margin-bottom: 10px;
}

.range-filter {
  display: grid;
  grid-template-columns: auto 1fr auto 1fr;
  gap: 10px;
  align-items: center;
}

.range-filter input {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.skill-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.skill-tag {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 5px 10px;
  background-color: #f0f8ff;
  border: 1px solid #b3d9ff;
  border-radius: 15px;
  font-size: 14px;
}

.checkbox-filters {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.filter-actions {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.advanced-results {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 15px;
}

.advanced-user-card {
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #f8f9fa;
  border-bottom: 1px solid #eee;
}

.card-header h4 {
  margin: 0;
  color: #333;
}

.status-badge {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: bold;
}

.status-badge.active {
  background-color: #d4edda;
  color: #155724;
}

.status-badge.inactive {
  background-color: #f8d7da;
  color: #721c24;
}

.card-content {
  padding: 15px;
}

.card-content p {
  margin: 8px 0;
  color: #666;
}

.skills {
  margin-top: 10px;
}

.skill-badge {
  display: inline-block;
  padding: 2px 8px;
  background-color: #e3f2fd;
  color: #1976d2;
  border-radius: 12px;
  font-size: 12px;
  margin: 2px;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

h3 {
  margin-top: 0;
  color: #333;
}
</style>
```

## 🎯 总结

Vue3的条件渲染和列表渲染提供了强大的DOM操作能力：

### 条件渲染：

1. **v-if / v-else-if / v-else**：
   - 真正的条件渲染，元素会被添加/删除
   - 适合条件很少改变的场景
   - 支持复杂的条件判断

2. **v-show**：
   - 通过CSS的`display`属性控制显示/隐藏
   - 元素始终存在于DOM中
   - 适合频繁切换的场景

3. **template条件组**：
   - 使用`<template>`包装多个元素
   - 不会产生额外的DOM节点
   - 适合条件渲染多个元素

### 列表渲染：

1. **基本数组渲染**：
   - `v-for="item in items"`
   - `v-for="(item, index) in items"`
   - 支持动态添加/删除元素

2. **对象渲染**：
   - `v-for="(value, key) in object"`
   - `v-for="(value, key, index) in object"`
   - 支持嵌套对象渲染

3. **数字范围渲染**：
   - `v-for="n in 10"`
   - 用于渲染指定次数的元素
   - 支持自定义范围

4. **key的重要性**：
   - 使用唯一ID而不是索引
   - 帮助Vue跟踪元素身份
   - 提高性能和保持状态

### 最佳实践：

1. **性能优化**：
   - 为`v-for`提供唯一的key
   - 合理使用`v-if`和`v-show`
   - 避免在模板中进行复杂计算

2. **代码可读性**：
   - 使用语义化的变量名
   - 合理组织过滤和排序逻辑
   - 使用计算属性处理复杂逻辑

3. **用户体验**：
   - 提供搜索和过滤功能
   - 支持多种排序方式
   - 显示结果数量和状态

记住：**条件渲染和列表渲染是Vue的核心特性，合理使用可以构建出功能强大且用户友好的界面！** 🚀
