# Vue3指令系统详解


# Vue3指令系统详解

## 🎯 什么是Vue指令？

Vue指令 (Directives) 是带有 `v-` 前缀的特殊属性，用于在模板中提供响应式功能。指令的职责是当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM。

### 指令的基本语法

```vue
<template>
  <!-- 完整语法 -->
  <div v-if="isVisible">显示内容</div>
  
  <!-- 动态参数 -->
  <div v-bind:[attributeName]="value">动态属性</div>
  
  <!-- 动态事件 -->
  <button v-on:[eventName]="handleEvent">动态事件</button>
</template>
```

## 📋 内置指令详解

### 1. v-text - 更新文本内容

`v-text` 指令用于更新元素的文本内容，类似于 `{{ }}` 插值，但会覆盖元素内的所有内容。

```vue
<template>
  <div>
    <!-- 使用插值 -->
    <div>{{ message }}</div>
    
    <!-- 使用v-text -->
    <div v-text="message"></div>
    
    <!-- v-text会覆盖原有内容 -->
    <div v-text="message">这部分内容会被覆盖</div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue3!')
</script>
```

### 2. v-html - 更新HTML内容

`v-html` 指令用于更新元素的HTML内容，但要注意XSS安全风险。

```vue
<template>
  <div>
    <!-- 普通文本插值 -->
    <div>{{ rawHtml }}</div>
    
    <!-- HTML渲染 -->
    <div v-html="rawHtml"></div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const rawHtml = ref('<span style="color: red;">红色文字</span>')
</script>
```

⚠️ **安全警告**：使用`v-html`时要注意XSS攻击风险，确保内容是可信的。

### 3. v-show - 显示/隐藏元素

`v-show` 指令通过切换CSS的 `display` 属性来控制元素的显示和隐藏。

```vue
<template>
  <div>
    <!-- 基本用法 -->
    <div v-show="isVisible">这个元素会显示/隐藏</div>
    
    <!-- 控制按钮 -->
    <button @click="toggleVisibility">切换显示</button>
    
    <!-- 复杂示例 -->
    <div class="user-profile" v-show="isLoggedIn">
      <h2>用户资料</h2>
      <p>用户名: {{ username }}</p>
      <button @click="logout">退出登录</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isVisible = ref(true)
const isLoggedIn = ref(false)
const username = ref('张三')

function toggleVisibility() {
  isVisible.value = !isVisible.value
}

function logout() {
  isLoggedIn.value = false
}
</script>
```

### 4. v-if / v-else-if / v-else - 条件渲染

条件渲染指令根据表达式的值来决定是否渲染元素。

```vue
<template>
  <div>
    <!-- 基本条件渲染 -->
    <div v-if="userRole === 'admin'">管理员界面</div>
    <div v-else-if="userRole === 'editor'">编辑者界面</div>
    <div v-else-if="userRole === 'user'">普通用户界面</div>
    <div v-else>游客界面</div>
    
    <!-- 使用template包裹多个元素 -->
    <template v-if="isLoggedIn">
      <header>欢迎回来</header>
      <main>主要内容</main>
      <footer>页脚信息</footer>
    </template>
    
    <!-- 控制按钮 -->
    <button @click="changeRole('admin')">管理员</button>
    <button @click="changeRole('editor')">编辑者</button>
    <button @click="changeRole('user')">普通用户</button>
    <button @click="changeRole('guest')">游客</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const userRole = ref('guest')
const isLoggedIn = ref(true)

function changeRole(role) {
  userRole.value = role
  isLoggedIn.value = role !== 'guest'
}
</script>
```

### 5. v-for - 列表渲染

`v-for` 指令用于基于数组或对象进行列表渲染。

#### 基本数组渲染

```vue
<template>
  <div>
    <!-- 基本数组渲染 -->
    <ul>
      <li v-for="item in items" :key="item.id">
        {{ item.name }} - {{ item.price }}
      </li>
    </ul>
    
    <!-- 带索引的渲染 -->
    <ul>
      <li v-for="(item, index) in items" :key="item.id">
        {{ index + 1 }}. {{ item.name }}
      </li>
    </ul>
    
    <!-- 多维数组渲染 -->
    <div v-for="(category, categoryIndex) in categories" :key="categoryIndex">
      <h3>{{ category.name }}</h3>
      <ul>
        <li v-for="(item, itemIndex) in category.items" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([
  { id: 1, name: '苹果', price: 5 },
  { id: 2, name: '香蕉', price: 3 },
  { id: 3, name: '橙子', price: 4 }
])

const categories = ref([
  {
    name: '水果',
    items: [
      { id: 1, name: '苹果' },
      { id: 2, name: '香蕉' }
    ]
  },
  {
    name: '蔬菜',
    items: [
      { id: 3, name: '西红柿' },
      { id: 4, name: '黄瓜' }
    ]
  }
])
</script>
```

#### 对象渲染

```vue
<template>
  <div>
    <!-- 对象渲染 -->
    <ul>
      <li v-for="(value, key) in user" :key="key">
        {{ key }}: {{ value }}
      </li>
    </ul>
    
    <!-- 带索引的对象渲染 -->
    <ul>
      <li v-for="(value, key, index) in user" :key="key">
        {{ index }}. {{ key }}: {{ value }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const user = ref({
  name: '张三',
  age: 25,
  city: '北京',
  email: 'zhangsan@example.com'
})
</script>
```

#### 数字范围渲染

```vue
<template>
  <div>
    <!-- 数字范围渲染 -->
    <ul>
      <li v-for="n in 5" :key="n">
        数字: {{ n }}
      </li>
    </ul>
    
    <!-- 自定义范围 -->
    <div v-for="n in (start, end)" :key="n">
      {{ n }}
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const start = ref(1)
const end = ref(10)
</script>
```

### 6. v-on - 事件绑定

`v-on` 指令用于绑定事件监听器，简写为 `@`。

#### 基本事件绑定

```vue
<template>
  <div>
    <!-- 完整语法 -->
    <button v-on:click="handleClick">点击我</button>
    
    <!-- 简写语法 -->
    <button @click="handleClick">点击我</button>
    
    <!-- 内联语句 -->
    <button @click="count++">计数: {{ count }}</button>
    
    <!-- 事件对象 -->
    <button @click="handleEvent">获取事件对象</button>
    
    <!-- 多个事件处理 -->
    <button @click="handleClick" @mouseenter="handleMouseEnter">多个事件</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)

function handleClick() {
  console.log('按钮被点击')
  count.value++
}

function handleEvent(event) {
  console.log('事件对象:', event)
  console.log('目标元素:', event.target)
}

function handleMouseEnter() {
  console.log('鼠标进入按钮')
}
</script>
```

#### 事件修饰符

```vue
<template>
  <div>
    <!-- 阻止事件冒泡 -->
    <div @click="handleDivClick" class="outer">
      <button @click.stop="handleButtonClick">阻止冒泡</button>
    </div>
    
    <!-- 阻止默认行为 -->
    <a href="https://example.com" @click.prevent="handleLinkClick">阻止默认行为</a>
    
    <!-- 事件只触发一次 -->
    <button @click.once="handleOnceClick">只触发一次</button>
    
    <!-- 按键修饰符 -->
    <input @keyup.enter="handleEnter" placeholder="按回车">
    <input @keyup.esc="handleEscape" placeholder="按ESC">
    
    <!-- 系统修饰符 -->
    <button @click.ctrl="handleCtrlClick">Ctrl+点击</button>
    <button @click.alt="handleAltClick">Alt+点击</button>
    <button @click.shift="handleShiftClick">Shift+点击</button>
    
    <!-- 鼠标修饰符 -->
    <div @click.left="handleLeftClick">左键点击</div>
    <div @click.right.prevent="handleRightClick">右键点击</div>
    <div @click.middle="handleMiddleClick">中键点击</div>
  </div>
</template>

<script setup>
function handleDivClick() {
  console.log('外层div被点击')
}

function handleButtonClick() {
  console.log('按钮被点击，但不会冒泡到div')
}

function handleLinkClick() {
  console.log('链接被点击，但不会跳转')
}

function handleOnceClick() {
  console.log('这个按钮只触发一次')
}

function handleEnter() {
  console.log('回车键被按下')
}

function handleEscape() {
  console.log('ESC键被按下')
}

function handleCtrlClick() {
  console.log('Ctrl+点击')
}

function handleAltClick() {
  console.log('Alt+点击')
}

function handleShiftClick() {
  console.log('Shift+点击')
}

function handleLeftClick() {
  console.log('左键点击')
}

function handleRightClick() {
  console.log('右键点击')
}

function handleMiddleClick() {
  console.log('中键点击')
}
</script>

<style>
.outer {
  padding: 20px;
  background-color: #f0f0f0;
  margin: 10px 0;
}
</style>
```

### 7. v-bind - 属性绑定

`v-bind` 指令用于动态绑定HTML属性，简写为 `:`。

#### 基本属性绑定

```vue
<template>
  <div>
    <!-- 完整语法 -->
    <img v-bind:src="imageUrl" v-bind:alt="imageAlt">
    
    <!-- 简写语法 -->
    <img :src="imageUrl" :alt="imageAlt">
    
    <!-- 布尔属性 -->
    <button :disabled="isDisabled">禁用按钮</button>
    <input :readonly="isReadonly" :value="inputValue">
    
    <!-- 动态class绑定 -->
    <div :class="{ active: isActive, 'text-danger': hasError }">
      动态class示例
    </div>
    
    <!-- 数组语法 -->
    <div :class="[activeClass, errorClass]">
      数组class示例
    </div>
    
    <!-- 三元运算符 -->
    <div :class="isActive ? 'active' : 'inactive'">
      三元运算符class
    </div>
    
    <!-- 动态style绑定 -->
    <div :style="{ color: textColor, fontSize: fontSize + 'px' }">
      动态style示例
    </div>
    
    <!-- 数组语法style -->
    <div :style="[baseStyle, overrideStyle]">
      数组style示例
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const imageUrl = ref('https://example.com/image.jpg')
const imageAlt = ref('示例图片')
const isDisabled = ref(false)
const isReadonly = ref(false)
const inputValue = ref('只读输入')
const isActive = ref(true)
const hasError = ref(false)
const textColor = ref('blue')
const fontSize = ref(16)
const activeClass = ref('active')
const errorClass = ref('error')
const baseStyle = ref({ color: 'red', fontSize: '16px' })
const overrideStyle = ref({ fontWeight: 'bold' })
</script>
```

#### 动态属性名

```vue
<template>
  <div>
    <!-- 动态属性名 -->
    <img :[attributeName]="imageUrl">
    
    <!-- 动态事件名 -->
    <button @[eventName]="handleClick">动态事件</button>
    
    <!-- 动态class名 -->
    <div :[dynamicClass]="true">动态class名</div>
    
    <!-- 控制按钮 -->
    <button @click="changeAttribute('src')">改为src</button>
    <button @click="changeAttribute('alt')">改为alt</button>
    <button @click="changeEvent('click')">改为click</button>
    <button @click="changeEvent('mouseenter')">改为mouseenter</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const attributeName = ref('src')
const eventName = ref('click')
const imageUrl = ref('https://example.com/image.jpg')
const dynamicClass = ref('active')

function handleClick() {
  console.log('按钮被点击')
}

function handleMouseEnter() {
  console.log('鼠标进入按钮')
}

function changeAttribute(attr) {
  attributeName.value = attr
}

function changeEvent(event) {
  eventName.value = event
}
</script>
```

### 8. v-model - 双向数据绑定

`v-model` 指令用于在表单元素上创建双向数据绑定。

#### 基本用法

```vue
<template>
  <div>
    <!-- 文本输入 -->
    <input v-model="text" placeholder="输入文本">
    <p>输入的内容: {{ text }}</p>
    
    <!-- 多行文本 -->
    <textarea v-model="textarea" placeholder="多行文本"></textarea>
    <p>多行内容: {{ textarea }}</p>
    
    <!-- 复选框 -->
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
    
    <!-- 多个复选框 -->
    <div>
      <input type="checkbox" id="apple" value="苹果" v-model="fruits">
      <label for="apple">苹果</label>
      
      <input type="checkbox" id="banana" value="香蕉" v-model="fruits">
      <label for="banana">香蕉</label>
      
      <input type="checkbox" id="orange" value="橙子" v-model="fruits">
      <label for="orange">橙子</label>
    </div>
    <p>选择的水果: {{ fruits }}</p>
    
    <!-- 单选按钮 -->
    <div>
      <input type="radio" id="male" value="男" v-model="gender">
      <label for="male">男</label>
      
      <input type="radio" id="female" value="女" v-model="gender">
      <label for="female">女</label>
    </div>
    <p>性别: {{ gender }}</p>
    
    <!-- 选择框 -->
    <select v-model="selectedCity">
      <option value="">请选择城市</option>
      <option value="北京">北京</option>
      <option value="上海">上海</option>
      <option value="广州">广州</option>
    </select>
    <p>选择的城市: {{ selectedCity }}</p>
    
    <!-- 多选选择框 -->
    <select v-model="selectedCities" multiple>
      <option value="北京">北京</option>
      <option value="上海">上海</option>
      <option value="广州">广州</option>
      <option value="深圳">深圳</option>
    </select>
    <p>选择的城市: {{ selectedCities }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const text = ref('')
const textarea = ref('')
const checked = ref(false)
const fruits = ref([])
const gender = ref('男')
const selectedCity = ref('')
const selectedCities = ref([])
</script>
```

#### v-model修饰符

```vue
<template>
  <div>
    <!-- .lazy：在change事件后同步 -->
    <input v-model.lazy="lazyMessage" placeholder="延迟更新">
    <p>延迟更新: {{ lazyMessage }}</p>
    
    <!-- .number：自动转换为数字 -->
    <input v-model.number="age" type="number" placeholder="年龄">
    <p>年龄: {{ age }} (类型: {{ typeof age }})</p>
    
    <!-- .trim：自动去除首尾空格 -->
    <input v-model.trim="trimmedMessage" placeholder="去除空格">
    <p>去除空格: "{{ trimmedMessage }}"</p>
    
    <!-- 组合使用修饰符 -->
    <input v-model.lazy.trim.number="complexValue" placeholder="组合修饰符">
    <p>组合修饰符: {{ complexValue }} (类型: {{ typeof complexValue }})</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const lazyMessage = ref('')
const age = ref(0)
const trimmedMessage = ref('')
const complexValue = ref('')
</script>
```

### 9. v-slot - 插槽

`v-slot` 指令用于具名插槽，简写为 `#`。

#### 基本插槽

```vue
<template>
  <div>
    <!-- 基本插槽 -->
    <BasicSlot>
      <p>这是插槽内容</p>
      <button>插槽按钮</button>
    </BasicSlot>
    
    <!-- 具名插槽 -->
    <NamedSlot>
      <template #header>
        <h2>页面标题</h2>
      </template>
      
      <template #default>
        <p>主要内容区域</p>
      </template>
      
      <template #footer>
        <button>提交</button>
        <button>取消</button>
      </template>
    </NamedSlot>
    
    <!-- 作用域插槽 -->
    <ScopedSlot :items="items">
      <template #default="{ item, index }">
        <div class="item">
          {{ index + 1 }}. {{ item.name }} - {{ item.price }}
        </div>
      </template>
    </ScopedSlot>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([
  { name: '苹果', price: 5 },
  { name: '香蕉', price: 3 },
  { name: '橙子', price: 4 }
])
</script>

<!-- 基本插槽组件 -->
<script>
const BasicSlot = {
  template: `
    <div class="slot-container">
      <h3>基本插槽组件</h3>
      <slot></slot>
    </div>
  `
}
</script>

<!-- 具名插槽组件 -->
<script>
const NamedSlot = {
  template: `
    <div class="named-slot">
      <header>
        <slot name="header"></slot>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>
  `
}
</script>

<!-- 作用域插槽组件 -->
<script>
const ScopedSlot = {
  props: ['items'],
  template: `
    <div class="scoped-slot">
      <h3>作用域插槽</h3>
      <slot v-for="(item, index) in items" :item="item" :index="index"></slot>
    </div>
  `
}
</script>

<style>
.slot-container, .named-slot, .scoped-slot {
  border: 1px solid #ddd;
  padding: 20px;
  margin: 10px 0;
}

.item {
  padding: 8px;
  margin: 4px 0;
  background-color: #f5f5f5;
  border-radius: 4px;
}
</style>
```

### 10. v-pre - 跳过编译

`v-pre` 指令跳过元素及其子元素的编译过程。

```vue
<template>
  <div>
    <!-- 正常编译 -->
    <div>{{ message }}</div>
    
    <!-- 跳过编译 -->
    <div v-pre>{{ message }}</div>
    
    <!-- 跳过编译的子元素 -->
    <div v-pre>
      <span>{{ message }}</span>
      <button @click="handleClick">{{ buttonText }}</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue3')
const buttonText = ref('点击我')

function handleClick() {
  console.log('这个按钮不会被Vue编译')
}
</script>
```

### 11. v-cloak - 隐藏未编译的模板

`v-cloak` 指令用于隐藏未编译的模板，直到编译完成。

```vue
<template>
  <div>
    <!-- 使用v-cloak -->
    <div v-cloak>
      {{ message }}
      <button @click="handleClick">点击我</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue3')

function handleClick() {
  console.log('按钮被点击')
}
</script>

<style>
[v-cloak] {
  display: none;
}
</style>
```

### 12. v-once - 只渲染一次

`v-once` 指令让元素或组件只渲染一次，后续数据变化不会重新渲染。

```vue
<template>
  <div>
    <!-- 普通响应式 -->
    <div>
      <p>普通响应式: {{ message }}</p>
      <button @click="updateMessage">更新消息</button>
    </div>
    
    <!-- 只渲染一次 -->
    <div v-once>
      <p>只渲染一次: {{ message }}</p>
      <p>当前时间: {{ currentTime }}</p>
    </div>
    
    <!-- 整个组件只渲染一次 -->
    <OnceComponent v-once :data="staticData" />
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue3')
const currentTime = ref(new Date().toLocaleString())
const staticData = ref({ name: '静态数据' })

function updateMessage() {
  message.value = 'Updated: ' + new Date().toLocaleString()
  currentTime.value = new Date().toLocaleString()
}
</script>

<script>
const OnceComponent = {
  props: ['data'],
  template: `
    <div class="once-component">
      <h3>只渲染一次的组件</h3>
      <p>{{ data.name }}</p>
      <p>渲染时间: {{ new Date().toLocaleString() }}</p>
    </div>
  `
}
</script>

<style>
.once-component {
  background-color: #f0f8ff;
  padding: 15px;
  margin: 10px 0;
  border-radius: 4px;
}
</style>
```

### 13. v-memo - 记忆化模板

`v-memo` 指令用于记忆化模板片段，只有在依赖项变化时才重新渲染。

```vue
<template>
  <div>
    <!-- 使用v-memo优化列表渲染 -->
    <div v-for="item in items" :key="item.id" v-memo="[item.id, item.isActive]">
      <h3>{{ item.name }}</h3>
      <p>价格: {{ item.price }}</p>
      <p>状态: {{ item.isActive ? '激活' : '未激活' }}</p>
      <button @click="toggleActive(item.id)">切换状态</button>
    </div>
    
    <!-- 控制按钮 -->
    <button @click="updatePrice">更新价格</button>
    <button @click="addItem">添加项目</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([
  { id: 1, name: '苹果', price: 5, isActive: true },
  { id: 2, name: '香蕉', price: 3, isActive: false },
  { id: 3, name: '橙子', price: 4, isActive: true }
])

function toggleActive(id) {
  const item = items.value.find(item => item.id === id)
  if (item) {
    item.isActive = !item.isActive
  }
}

function updatePrice() {
  items.value.forEach(item => {
    item.price += 1
  })
}

function addItem() {
  const newItem = {
    id: Date.now(),
    name: '新水果',
    price: 6,
    isActive: true
  }
  items.value.push(newItem)
}
</script>

<style>
div[v-for] {
  border: 1px solid #ddd;
  padding: 15px;
  margin: 10px 0;
  border-radius: 4px;
}

button {
  margin: 5px;
  padding: 5px 10px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #3aa876;
}
</style>
```

## 🛠️ 自定义指令

### 1. 注册自定义指令

#### 全局注册

```javascript
// main.js
import { createApp } from 'vue'

const app = createApp({})

// 全局自定义指令
app.directive('focus', {
  mounted(el) {
    el.focus()
  }
})

app.directive('color', {
  mounted(el, binding) {
    el.style.color = binding.value
  },
  updated(el, binding) {
    el.style.color = binding.value
  }
})

app.mount('#app')
```

#### 局部注册

```vue
<template>
  <div>
    <input v-focus placeholder="自动聚焦">
    <p v-color="'red'">红色文字</p>
    <p v-color="textColor">动态颜色</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const textColor = ref('blue')

// 局部自定义指令
const vFocus = {
  mounted(el) {
    el.focus()
  }
}

const vColor = {
  mounted(el, binding) {
    el.style.color = binding.value
  },
  updated(el, binding) {
    el.style.color = binding.value
  }
}
</script>
```

### 2. 自定义指令钩子函数

```vue
<template>
  <div>
    <input v-demo="message" placeholder="自定义指令示例">
    <div v-position="{ x: 100, y: 200 }">定位元素</div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const message = ref('Hello Vue3')

// 完整的自定义指令
const vDemo = {
  // 元素挂载前
  beforeMount(el, binding) {
    console.log('beforeMount', el, binding)
    el.value = binding.value
  },
  
  // 元素挂载后
  mounted(el, binding) {
    console.log('mounted', el, binding)
    el.addEventListener('input', (e) => {
      console.log('输入:', e.target.value)
    })
  },
  
  // 更新前
  beforeUpdate(el, binding) {
    console.log('beforeUpdate', el, binding)
  },
  
  // 更新后
  updated(el, binding) {
    console.log('updated', el, binding)
  },
  
  // 卸载前
  beforeUnmount(el) {
    console.log('beforeUnmount', el)
  },
  
  // 卸载后
  unmounted(el) {
    console.log('unmounted', el)
  }
}

// 位置指令
const vPosition = {
  mounted(el, binding) {
    el.style.position = 'absolute'
    el.style.left = binding.value.x + 'px'
    el.style.top = binding.value.y + 'px'
    el.style.backgroundColor = '#f0f0f0'
    el.style.padding = '10px'
  }
}
</script>
```

### 3. 实用自定义指令示例

#### 防抖指令

```vue
<template>
  <div>
    <input v-debounce="handleSearch" placeholder="搜索（防抖）">
    <button v-debounce:500ms="handleClick" @click="handleClick">点击（防抖）</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const vDebounce = {
  mounted(el, binding) {
    const { value, arg = '300ms' } = binding
    const delay = parseInt(arg) || 300
    
    let timeout
    
    const debouncedFn = function(...args) {
      clearTimeout(timeout)
      timeout = setTimeout(() => {
        value.apply(this, args)
      }, delay)
    }
    
    el._debouncedFn = debouncedFn
    
    if (el.tagName === 'INPUT') {
      el.addEventListener('input', debouncedFn)
    } else {
      el.addEventListener('click', debouncedFn)
    }
  },
  
  beforeUnmount(el) {
    if (el._debouncedFn) {
      el.removeEventListener('input', el._debouncedFn)
      el.removeEventListener('click', el._debouncedFn)
    }
  }
}

function handleSearch(event) {
  console.log('搜索:', event.target.value)
}

function handleClick() {
  console.log('按钮被点击（防抖）')
}
</script>
```

#### 拖拽指令

```vue
<template>
  <div>
    <div v-drag class="draggable">
      拖拽我
    </div>
    
    <div v-drag="{ x: 200, y: 100 }" class="draggable">
      初始位置
    </div>
  </div>
</template>

<script setup>
const vDrag = {
  mounted(el, binding) {
    const { value = { x: 0, y: 0 } } = binding
    
    el.style.position = 'absolute'
    el.style.left = value.x + 'px'
    el.style.top = value.y + 'px'
    el.style.cursor = 'move'
    
    let isDragging = false
    let startX, startY, initialX, initialY
    
    const startDrag = (e) => {
      isDragging = true
      startX = e.clientX
      startY = e.clientY
      initialX = parseInt(el.style.left)
      initialY = parseInt(el.style.top)
      
      document.addEventListener('mousemove', drag)
      document.addEventListener('mouseup', stopDrag)
      
      e.preventDefault()
    }
    
    const drag = (e) => {
      if (!isDragging) return
      
      const dx = e.clientX - startX
      const dy = e.clientY - startY
      
      el.style.left = (initialX + dx) + 'px'
      el.style.top = (initialY + dy) + 'px'
    }
    
    const stopDrag = () => {
      isDragging = false
      document.removeEventListener('mousemove', drag)
      document.removeEventListener('mouseup', stopDrag)
    }
    
    el.addEventListener('mousedown', startDrag)
  },
  
  beforeUnmount(el) {
    el.removeEventListener('mousedown', el._startDrag)
  }
}
</script>

<style>
.draggable {
  width: 100px;
  height: 100px;
  background-color: #42b983;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  user-select: none;
}
</style>
```

#### 权限指令

```vue
<template>
  <div>
    <button v-permission="'admin'">管理员按钮</button>
    <button v-permission="'editor'">编辑者按钮</button>
    <button v-permission="'user'">用户按钮</button>
    
    <div v-permission="['admin', 'editor']">
      管理员和编辑者可见
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 模拟用户权限
const userPermissions = ref(['admin', 'user'])

const vPermission = {
  mounted(el, binding) {
    const requiredPermissions = Array.isArray(binding.value) 
      ? binding.value 
      : [binding.value]
    
    const hasPermission = requiredPermissions.some(permission => 
      userPermissions.value.includes(permission)
    )
    
    if (!hasPermission) {
      el.style.display = 'none'
      el.setAttribute('hidden', 'true')
    }
  },
  
  updated(el, binding) {
    const requiredPermissions = Array.isArray(binding.value) 
      ? binding.value 
      : [binding.value]
    
    const hasPermission = requiredPermissions.some(permission => 
      userPermissions.value.includes(permission)
    )
    
    el.style.display = hasPermission ? '' : 'none'
    el.setAttribute('hidden', !hasPermission)
  }
}
</script>
```

## 📋 指令最佳实践

### 1. 性能优化

- 合理使用`v-if`和`v-show`：
  - `v-if`：条件很少改变时使用
  - `v-show`：频繁切换显示/隐藏时使用

- 为`v-for`添加唯一的key：
  ```vue
  <!-- ✅ 正确 -->
  <div v-for="item in items" :key="item.id"></div>
  
  <!-- ❌ 错误 -->
  <div v-for="item in items" :key="index"></div>
  ```

- 使用`v-memo`优化大列表渲染：
  ```vue
  <div v-for="item in largeList" :key="item.id" v-memo="[item.id, item.active]">
    <!-- 复杂内容 -->
  </div>
  ```

### 2. 可读性维护

- 优先使用简写语法：
  ```vue
  <!-- ✅ 推荐 -->
  <img :src="imageUrl" @click="handleClick" v-model="message">
  
  <!-- ❌ 不推荐 -->
  <img v-bind:src="imageUrl" v-on:click="handleClick" v-model="message">
  ```

- 合理使用事件修饰符：
  ```vue
  <!-- ✅ 推荐 -->
  <form @submit.prevent="handleSubmit">
    <input @keyup.enter="handleEnter">
  </form>
  
  <!-- ❌ 不推荐 -->
  <form @submit="handleSubmit($event)">
    <input @keyup="handleEnter($event)">
  </form>
  ```

### 3. 安全性

- 谨慎使用`v-html`，确保内容可信：
  ```vue
  <!-- ✅ 安全：可信内容 -->
  <div v-html="trustedContent"></div>
  
  <!-- ❌ 危险：用户输入内容 -->
  <div v-html="userInput"></div>
  ```

- 使用`v-pre`处理大量静态内容：
  ```vue
  <div v-pre>
    <!-- 大量静态HTML内容 -->
    <h1>静态标题</h1>
    <p>静态段落</p>
  </div>
  ```

## 🎯 总结

Vue3指令系统是框架的核心特性之一，提供了丰富的DOM操作和响应式功能：

### 核心指令分类：

1. **内容渲染指令**：
   - `v-text`：更新文本内容
   - `v-html`：更新HTML内容
   - `v-pre`：跳过编译
   - `v-cloak`：隐藏未编译模板
   - `v-once`：只渲染一次

2. **条件渲染指令**：
   - `v-if` / `v-else-if` / `v-else`：条件渲染
   - `v-show`：显示/隐藏元素

3. **列表渲染指令**：
   - `v-for`：基于数组/对象进行列表渲染
   - `v-memo`：记忆化模板优化

4. **事件处理指令**：
   - `v-on` / `@`：事件绑定
   - 支持多种修饰符：`.stop`、`.prevent`、`.once`等

5. **属性绑定指令**：
   - `v-bind` / `:`：动态属性绑定
   - 支持动态属性名和复杂表达式

6. **双向绑定指令**：
   - `v-model`：表单双向数据绑定
   - 支持多种修饰符：`.lazy`、`.number`、`.trim`

7. **插槽指令**：
   - `v-slot` / `#`：插槽内容分发
   - 支持具名插槽和作用域插槽

### 自定义指令能力：

- **钩子函数**：`beforeMount`、`mounted`、`beforeUpdate`、`updated`、`beforeUnmount`、`unmounted`
- **参数处理**：支持动态参数和表达式
- **实用场景**：防抖、拖拽、权限控制、动画等

记住：**指令是Vue声明式渲染的核心，合理使用指令可以大大提高开发效率和代码可维护性！** 🚀
