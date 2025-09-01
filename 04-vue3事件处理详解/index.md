# Vue3事件处理详解


# Vue3事件处理详解

## 🎯 事件处理基础

Vue3提供了强大的事件处理系统，通过`v-on`指令（简写为`@`）来监听DOM事件，并在事件触发时执行相应的JavaScript代码。

### 基本事件绑定

```vue
<template>
  <div>
    <!-- 完整语法 -->
    <button v-on:click="handleClick">点击我</button>
    
    <!-- 简写语法 -->
    <button @click="handleClick">点击我</button>
    
    <!-- 内联语句 -->
    <button @click="count++">计数: {{ count }}</button>
    
    <!-- 内联语句调用方法 -->
    <button @click="greet('Vue3')">问候</button>
    
    <!-- 事件对象 -->
    <button @click="showEvent($event)">显示事件对象</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)

function handleClick() {
  console.log('按钮被点击')
  count.value++
}

function greet(name) {
  console.log(`Hello, ${name}!`)
  alert(`Hello, ${name}!`)
}

function showEvent(event) {
  console.log('事件对象:', event)
  console.log('事件类型:', event.type)
  console.log('目标元素:', event.target)
  console.log('当前目标:', event.currentTarget)
}
</script>
```

## 📋 事件修饰符

事件修饰符是以点开头的特殊后缀，用于指定事件处理方式。

### 1. .stop - 阻止事件冒泡

```vue
<template>
  <div>
    <!-- 无阻止冒泡 -->
    <div @click="handleOuterClick" class="outer">
      <button @click="handleInnerClick">内部按钮（会冒泡）</button>
    </div>
    
    <!-- 阻止冒泡 -->
    <div @click="handleOuterClick" class="outer">
      <button @click.stop="handleInnerClick">内部按钮（阻止冒泡）</button>
    </div>
    
    <!-- 多层嵌套演示 -->
    <div @click="handleLevel3" class="level3">
      <div @click="handleLevel2" class="level2">
        <div @click="handleLevel1" class="level1">
          <button @click.stop="handleButtonClick">最内层按钮</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
function handleOuterClick() {
  console.log('外层div被点击')
}

function handleInnerClick() {
  console.log('内层按钮被点击')
}

function handleLevel3() {
  console.log('第3层被点击')
}

function handleLevel2() {
  console.log('第2层被点击')
}

function handleLevel1() {
  console.log('第1层被点击')
}

function handleButtonClick() {
  console.log('按钮被点击，阻止冒泡')
}
</script>

<style>
.outer, .level1, .level2, .level3 {
  padding: 20px;
  margin: 10px 0;
  border: 1px solid #ddd;
}

.level1 { background-color: #f0f8ff; }
.level2 { background-color: #e6f3ff; }
.level3 { background-color: #dceeff; }
</style>
```

### 2. .prevent - 阻止默认行为

```vue
<template>
  <div>
    <!-- 表单提交 -->
    <form @submit="handleSubmit">
      <input type="text" v-model="formData.name" placeholder="输入姓名">
      <button type="submit">提交（会刷新页面）</button>
    </form>
    
    <!-- 阻止默认提交行为 -->
    <form @submit.prevent="handleSubmit">
      <input type="text" v-model="formData.name" placeholder="输入姓名">
      <button type="submit">提交（不会刷新页面）</button>
    </form>
    
    <!-- 链接点击 -->
    <a href="https://vuejs.org" @click="handleLinkClick">Vue官网（会跳转）</a>
    
    <!-- 阻止链接跳转 -->
    <a href="https://vuejs.org" @click.prevent="handleLinkClick">Vue官网（不会跳转）</a>
    
    <!-- 右键菜单 -->
    <div @contextmenu="handleContextMenu" class="context-area">
      右键点击此处（会显示浏览器菜单）
    </div>
    
    <!-- 阻止右键菜单 -->
    <div @contextmenu.prevent="handleContextMenu" class="context-area">
      右键点击此处（不会显示浏览器菜单）
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const formData = reactive({ name: '' })

function handleSubmit() {
  console.log('表单提交:', formData.name)
  alert(`提交成功: ${formData.name}`)
  formData.name = ''
}

function handleLinkClick() {
  console.log('链接被点击，但不会跳转')
  alert('链接被点击，但不会跳转')
}

function handleContextMenu(event) {
  console.log('右键菜单事件:', event)
  alert('右键菜单事件被触发')
}
</script>

<style>
form {
  margin: 20px 0;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input {
  padding: 8px;
  margin-right: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #3aa876;
}

a {
  margin: 0 10px;
  color: #42b983;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.context-area {
  width: 300px;
  height: 100px;
  border: 1px solid #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 10px 0;
  background-color: #f9f9f9;
}
</style>
```

### 3. .capture - 使用事件捕获模式

```vue
<template>
  <div>
    <!-- 冒泡模式（默认） -->
    <div @click="handleParent" class="parent">
      <div @click="handleChild" class="child">
        <button @click="handleButton">冒泡模式</button>
      </div>
    </div>
    
    <!-- 捕获模式 -->
    <div @click.capture="handleParent" class="parent">
      <div @click.capture="handleChild" class="child">
        <button @click="handleButton">捕获模式</button>
      </div>
    </div>
    
    <!-- 混合模式 -->
    <div @click.capture="handleParent" class="parent">
      <div @click="handleChild" class="child">
        <button @click.capture="handleButton">混合模式</button>
      </div>
    </div>
  </div>
</template>

<script setup>
function handleParent() {
  console.log('父元素事件处理')
}

function handleChild() {
  console.log('子元素事件处理')
}

function handleButton() {
  console.log('按钮事件处理')
}
</script>

<style>
.parent {
  padding: 20px;
  background-color: #f0f8ff;
  border: 2px solid #42b983;
  margin: 10px 0;
}

.child {
  padding: 15px;
  background-color: #e6f3ff;
  border: 2px solid #3aa876;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### 4. .self - 只当事件目标是元素本身时触发

```vue
<template>
  <div>
    <!-- 父元素监听子元素事件 -->
    <div @click="handleContainer" class="container">
      <button @click="handleButton">按钮</button>
      <span>文本内容</span>
    </div>
    
    <!-- 只有点击容器本身才触发 -->
    <div @click.self="handleContainer" class="container">
      <button @click="handleButton">按钮</button>
      <span>文本内容</span>
    </div>
    
    <!-- 复杂示例 -->
    <div @click.self="handleOuter" class="outer">
      <div @click.self="handleMiddle" class="middle">
        <div @click.self="handleInner" class="inner">
          <button @click="handleButton">最内层按钮</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
function handleContainer() {
  console.log('容器被点击')
}

function handleButton() {
  console.log('按钮被点击')
}

function handleOuter() {
  console.log('外层容器被点击')
}

function handleMiddle() {
  console.log('中层容器被点击')
}

function handleInner() {
  console.log('内层容器被点击')
}
</script>

<style>
.container {
  padding: 20px;
  background-color: #f0f8ff;
  border: 1px solid #42b983;
  margin: 10px 0;
}

.outer, .middle, .inner {
  padding: 15px;
  margin: 5px;
  border: 1px solid #ddd;
}

.outer { background-color: #fff0f0; }
.middle { background-color: #f0fff0; }
.inner { background-color: #f0f0ff; }

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}
</style>
```

### 5. .once - 事件只触发一次

```vue
<template>
  <div>
    <!-- 普通点击事件 -->
    <button @click="handleNormalClick">普通点击（可多次）</button>
    <p>普通点击次数: {{ normalCount }}</p>
    
    <!-- 只触发一次 -->
    <button @click.once="handleOnceClick">只触发一次</button>
    <p>一次性点击次数: {{ onceCount }}</p>
    
    <!-- 提交表单只触发一次 -->
    <form @submit.once="handleSubmit">
      <input type="text" v-model="formData.name" placeholder="输入姓名">
      <button type="submit">提交（只触发一次）</button>
    </form>
    
    <!-- 一次性监听多个事件 -->
    <button 
      @click.once="handleClickOnce"
      @mouseenter.once="handleMouseOnce"
    >
      多个一次性事件
    </button>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const normalCount = ref(0)
const onceCount = ref(0)
const formData = reactive({ name: '' })

function handleNormalClick() {
  normalCount.value++
  console.log('普通点击:', normalCount.value)
}

function handleOnceClick() {
  onceCount.value++
  console.log('一次性点击:', onceCount.value)
}

function handleSubmit() {
  console.log('表单提交:', formData.name)
  alert(`提交成功: ${formData.name}`)
  formData.name = ''
}

function handleClickOnce() {
  console.log('点击事件（只触发一次）')
}

function handleMouseOnce() {
  console.log('鼠标进入事件（只触发一次）')
}
</script>

<style>
button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}

button:hover {
  background-color: #3aa876;
}

form {
  margin: 20px 0;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input {
  padding: 8px;
  margin-right: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
</style>
```

### 6. .passive - 以 passive 模式添加监听器

```vue
<template>
  <div>
    <!-- 普通滚动事件 -->
    <div @scroll="handleNormalScroll" class="scroll-container">
      <div class="scroll-content">
        <p v-for="n in 50" :key="n">滚动内容 {{ n }}</p>
      </div>
    </div>
    
    <!-- passive 模式滚动事件 -->
    <div @scroll.passive="handlePassiveScroll" class="scroll-container">
      <div class="scroll-content">
        <p v-for="n in 50" :key="n">滚动内容 {{ n }}</p>
      </div>
    </div>
    
    <!-- 触摸事件 -->
    <div 
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      class="touch-area"
    >
      触摸区域（普通模式）
    </div>
    
    <div 
      @touchstart.passive="handleTouchStart"
      @touchmove.passive="handleTouchMove"
      class="touch-area"
    >
      触摸区域（passive模式）
    </div>
  </div>
</template>

<script setup>
function handleNormalScroll(event) {
  console.log('普通滚动事件:', event.target.scrollTop)
  // 这里可以执行一些耗时的操作，但可能会影响滚动性能
}

function handlePassiveScroll(event) {
  console.log('Passive滚动事件:', event.target.scrollTop)
  // Passive模式下不能调用preventDefault()
}

function handleTouchStart(event) {
  console.log('触摸开始:', event.touches)
}

function handleTouchMove(event) {
  console.log('触摸移动:', event.touches)
  // 在passive模式下调用preventDefault()会报错
  // event.preventDefault() // 这行代码在passive模式下会报错
}
</script>

<style>
.scroll-container {
  height: 200px;
  overflow-y: auto;
  border: 1px solid #ddd;
  margin: 10px 0;
  background-color: #f9f9f9;
}

.scroll-content {
  padding: 20px;
}

.touch-area {
  width: 300px;
  height: 150px;
  border: 2px solid #42b983;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 10px 0;
  background-color: #f0f8ff;
  user-select: none;
}
</style>
```

## ⌨️ 按键修饰符

按键修饰符用于监听键盘事件，只有在特定按键按下时才触发事件处理函数。

### 1. 常用按键修饰符

```vue
<template>
  <div>
    <!-- 回车键 -->
    <input 
      @keyup.enter="handleEnter" 
      placeholder="按回车触发"
      v-model="enterValue"
    >
    <p>回车输入: {{ enterValue }}</p>
    
    <!-- ESC键 -->
    <input 
      @keyup.esc="handleEscape" 
      placeholder="按ESC触发"
      v-model="escapeValue"
    >
    <p>ESC输入: {{ escapeValue }}</p>
    
    <!-- Tab键 -->
    <input 
      @keydown.tab="handleTab" 
      placeholder="按Tab触发"
    >
    
    <!-- 空格键 -->
    <input 
      @keyup.space="handleSpace" 
      placeholder="按空格触发"
    >
    
    <!-- 删除键 -->
    <input 
      @keyup.delete="handleDelete" 
      placeholder="按Delete触发"
      v-model="deleteValue"
    >
  </div>
</template>

<script setup>
import { ref } from 'vue'

const enterValue = ref('')
const escapeValue = ref('')
const deleteValue = ref('')

function handleEnter() {
  console.log('回车键被按下')
  alert(`回车键触发，输入值: ${enterValue.value}`)
}

function handleEscape() {
  console.log('ESC键被按下')
  escapeValue.value = ''
  alert('ESC键触发，输入已清空')
}

function handleTab() {
  console.log('Tab键被按下')
  event.preventDefault() // 阻止默认Tab行为
}

function handleSpace() {
  console.log('空格键被按下')
  alert('空格键触发')
}

function handleDelete() {
  console.log('Delete键被按下')
  alert('Delete键触发')
}
</script>

<style>
input {
  padding: 8px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 300px;
}

p {
  margin: 5px 0;
  color: #666;
}
</style>
```

### 2. 字母和数字键修饰符

```vue
<template>
  <div>
    <!-- 字母键 -->
    <input 
      @keyup.a="handleKeyA" 
      @keyup.b="handleKeyB" 
      @keyup.c="handleKeyC"
      placeholder="按A/B/C键触发"
    >
    
    <!-- 数字键 -->
    <input 
      @keyup.1="handleKey1" 
      @keyup.2="handleKey2" 
      @keyup.3="handleKey3"
      placeholder="按1/2/3键触发"
    >
    
    <!-- 方向键 -->
    <input 
      @keyup.up="handleKeyUp" 
      @keyup.down="handleKeyDown" 
      @keyup.left="handleKeyLeft" 
      @keyup.right="handleKeyRight"
      placeholder="按方向键触发"
    >
    
    <!-- 功能键 -->
    <input 
      @keyup.f1="handleF1" 
      @keyup.f2="handleF2" 
      @keyup.f3="handleF3"
      placeholder="按F1/F2/F3键触发"
    >
    
    <!-- 显示按键历史 -->
    <div class="key-history">
      <h3>按键历史:</h3>
      <div v-for="(key, index) in keyHistory" :key="index" class="key-item">
        {{ key }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const keyHistory = ref([])

function addKeyToHistory(keyName) {
  keyHistory.value.unshift(`${keyName} - ${new Date().toLocaleTimeString()}`)
  if (keyHistory.value.length > 10) {
    keyHistory.value.pop()
  }
}

function handleKeyA() {
  console.log('A键被按下')
  addKeyToHistory('A键')
}

function handleKeyB() {
  console.log('B键被按下')
  addKeyToHistory('B键')
}

function handleKeyC() {
  console.log('C键被按下')
  addKeyToHistory('C键')
}

function handleKey1() {
  console.log('1键被按下')
  addKeyToHistory('1键')
}

function handleKey2() {
  console.log('2键被按下')
  addKeyToHistory('2键')
}

function handleKey3() {
  console.log('3键被按下')
  addKeyToHistory('3键')
}

function handleKeyUp() {
  console.log('上方向键被按下')
  addKeyToHistory('↑')
}

function handleKeyDown() {
  console.log('下方向键被按下')
  addKeyToHistory('↓')
}

function handleKeyLeft() {
  console.log('左方向键被按下')
  addKeyToHistory('←')
}

function handleKeyRight() {
  console.log('右方向键被按下')
  addKeyToHistory('→')
}

function handleF1() {
  console.log('F1键被按下')
  addKeyToHistory('F1')
}

function handleF2() {
  console.log('F2键被按下')
  addKeyToHistory('F2')
}

function handleF3() {
  console.log('F3键被按下')
  addKeyToHistory('F3')
}
</script>

<style>
input {
  padding: 8px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 300px;
}

.key-history {
  margin-top: 20px;
  padding: 15px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
  max-height: 200px;
  overflow-y: auto;
}

.key-item {
  padding: 5px;
  margin: 2px 0;
  background-color: #f0f8ff;
  border-radius: 3px;
  font-family: monospace;
}
</style>
```

## 🖱️ 鼠标修饰符

鼠标修饰符用于监听特定的鼠标事件。

### 1. 鼠标按键修饰符

```vue
<template>
  <div>
    <!-- 左键点击 -->
    <div @click.left="handleLeftClick" class="mouse-area">
      左键点击区域
    </div>
    
    <!-- 右键点击 -->
    <div @click.right.prevent="handleRightClick" class="mouse-area">
      右键点击区域
    </div>
    
    <!-- 中键点击 -->
    <div @click.middle="handleMiddleClick" class="mouse-area">
      中键点击区域
    </div>
    
    <!-- 鼠标进入和离开 -->
    <div 
      @mouseenter="handleMouseEnter" 
      @mouseleave="handleMouseLeave"
      class="mouse-area"
    >
      鼠标进入/离开区域
      <p>状态: {{ mouseStatus }}</p>
    </div>
    
    <!-- 鼠标移动 -->
    <div 
      @mousemove="handleMouseMove" 
      class="mouse-area"
    >
      鼠标移动区域
      <p>坐标: {{ mouseX }}, {{ mouseY }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const mouseStatus = ref('等待中')
const mouseX = ref(0)
const mouseY = ref(0)

function handleLeftClick() {
  console.log('左键点击')
  alert('左键点击')
}

function handleRightClick() {
  console.log('右键点击')
  alert('右键点击')
}

function handleMiddleClick() {
  console.log('中键点击')
  alert('中键点击')
}

function handleMouseEnter() {
  mouseStatus.value = '鼠标进入'
  console.log('鼠标进入')
}

function handleMouseLeave() {
  mouseStatus.value = '鼠标离开'
  console.log('鼠标离开')
}

function handleMouseMove(event) {
  mouseX.value = event.offsetX
  mouseY.value = event.offsetY
  console.log('鼠标移动:', event.offsetX, event.offsetY)
}
</script>

<style>
.mouse-area {
  width: 300px;
  height: 100px;
  border: 2px solid #42b983;
  margin: 10px 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f0f8ff;
  cursor: pointer;
  user-select: none;
  position: relative;
}

.mouse-area:hover {
  background-color: #e6f3ff;
}
</style>
```

## ⚙️ 系统修饰符

系统修饰符用于监听组合键事件。

### 1. 基本系统修饰符

```vue
<template>
  <div>
    <!-- Ctrl + 点击 -->
    <button @click.ctrl="handleCtrlClick">Ctrl + 点击</button>
    
    <!-- Alt + 点击 -->
    <button @click.alt="handleAltClick">Alt + 点击</button>
    
    <!-- Shift + 点击 -->
    <button @click.shift="handleShiftClick">Shift + 点击</button>
    
    <!-- Meta + 点击 (Mac上的Command键) -->
    <button @click.meta="handleMetaClick">Meta + 点击</button>
    
    <!-- 组合修饰符 -->
    <button @click.ctrl.alt="handleCtrlAltClick">Ctrl + Alt + 点击</button>
    
    <!-- 系统修饰符 + 按键 -->
    <input 
      @keyup.ctrl.s="handleCtrlS" 
      placeholder="Ctrl + S 保存"
    >
    
    <input 
      @keyup.ctrl.c="handleCtrlC" 
      placeholder="Ctrl + C 复制"
    >
    
    <!-- 精确修饰符 (.exact) -->
    <button @click.ctrl.exact="handleExactCtrl">仅Ctrl（不包含其他键）</button>
    
    <button @click.exact="handleExactClick">无修饰符点击</button>
    
    <!-- 显示操作历史 -->
    <div class="history">
      <h3>操作历史:</h3>
      <div v-for="(action, index) in actionHistory" :key="index" class="action-item">
        {{ action }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const actionHistory = ref([])

function addToHistory(action) {
  actionHistory.value.unshift(`${action} - ${new Date().toLocaleTimeString()}`)
  if (actionHistory.value.length > 10) {
    actionHistory.value.pop()
  }
}

function handleCtrlClick() {
  console.log('Ctrl + 点击')
  addToHistory('Ctrl + 点击')
}

function handleAltClick() {
  console.log('Alt + 点击')
  addToHistory('Alt + 点击')
}

function handleShiftClick() {
  console.log('Shift + 点击')
  addToHistory('Shift + 点击')
}

function handleMetaClick() {
  console.log('Meta + 点击')
  addToHistory('Meta + 点击')
}

function handleCtrlAltClick() {
  console.log('Ctrl + Alt + 点击')
  addToHistory('Ctrl + Alt + 点击')
}

function handleCtrlS(event) {
  console.log('Ctrl + S')
  event.preventDefault()
  addToHistory('Ctrl + S 保存')
}

function handleCtrlC(event) {
  console.log('Ctrl + C')
  event.preventDefault()
  addToHistory('Ctrl + C 复制')
}

function handleExactCtrl() {
  console.log('仅Ctrl点击')
  addToHistory('仅Ctrl点击')
}

function handleExactClick() {
  console.log('无修饰符点击')
  addToHistory('无修饰符点击')
}
</script>

<style>
button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}

button:hover {
  background-color: #3aa876;
}

input {
  padding: 8px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 250px;
}

.history {
  margin-top: 20px;
  padding: 15px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
  max-height: 200px;
  overflow-y: auto;
}

.action-item {
  padding: 5px;
  margin: 2px 0;
  background-color: #f0f8ff;
  border-radius: 3px;
  font-family: monospace;
}
</style>
```

## 🎯 事件处理最佳实践

### 1. 事件处理函数命名

```vue
<template>
  <div>
    <!-- ✅ 好的命名 -->
    <button @click="handleSubmitForm">提交表单</button>
    <button @click="handleToggleModal">切换模态框</button>
    <button @click="handleDeleteItem">删除项目</button>
    
    <!-- ❌ 不好的命名 -->
    <button @click="click">点击</button>
    <button @click="func1">功能1</button>
    <button @click="doSomething">做某事</button>
  </div>
</template>

<script setup>
function handleSubmitForm() {
  // 提交表单逻辑
}

function handleToggleModal() {
  // 切换模态框逻辑
}

function handleDeleteItem() {
  // 删除项目逻辑
}
</script>
```

### 2. 防抖和节流

```vue
<template>
  <div>
    <!-- 搜索输入（防抖） -->
    <input 
      @input="handleSearchInput" 
      placeholder="搜索（防抖500ms）"
      v-model="searchQuery"
    >
    
    <!-- 按钮点击（防抖） -->
    <button @click="handleDebouncedClick">防抖点击</button>
    
    <!-- 滚动事件（节流） -->
    <div @scroll="handleScroll" class="scroll-container">
      <div class="scroll-content">
        <p v-for="n in 100" :key="n">滚动内容 {{ n }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const searchQuery = ref('')

// 防抖函数
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

// 节流函数
function throttle(func, limit) {
  let inThrottle
  return function executedFunction(...args) {
    if (!inThrottle) {
      func.apply(this, args)
      inThrottle = true
      setTimeout(() => inThrottle = false, limit)
    }
  }
}

// 防抖搜索
const handleSearchInput = debounce((event) => {
  console.log('搜索:', event.target.value)
  // 执行搜索逻辑
}, 500)

// 防抖点击
const handleDebouncedClick = debounce(() => {
  console.log('防抖点击')
  alert('防抖点击触发')
}, 1000)

// 节流滚动
const handleScroll = throttle((event) => {
  console.log('滚动位置:', event.target.scrollTop)
}, 100)
</script>

<style>
input {
  padding: 8px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 300px;
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}

.scroll-container {
  height: 200px;
  overflow-y: auto;
  border: 1px solid #ddd;
  margin: 10px 0;
  background-color: #f9f9f9;
}

.scroll-content {
  padding: 20px;
}
</style>
```

### 3. 事件委托

```vue
<template>
  <div>
    <!-- 事件委托示例 -->
    <div @click="handleListClick" class="item-list">
      <div v-for="item in items" :key="item.id" :data-id="item.id" class="list-item">
        {{ item.name }}
        <button class="delete-btn" data-action="delete">删除</button>
      </div>
    </div>
    
    <!-- 动态添加按钮 -->
    <button @click="addItem">添加项目</button>
    
    <!-- 显示操作结果 -->
    <div v-if="lastAction" class="action-result">
      最后操作: {{ lastAction }}
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([
  { id: 1, name: '项目1' },
  { id: 2, name: '项目2' },
  { id: 3, name: '项目3' }
])

const lastAction = ref('')

function handleListClick(event) {
  const target = event.target
  
  // 处理删除按钮点击
  if (target.classList.contains('delete-btn')) {
    const itemElement = target.closest('.list-item')
    const itemId = parseInt(itemElement.dataset.id)
    deleteItem(itemId)
    return
  }
  
  // 处理项目点击
  if (target.classList.contains('list-item')) {
    const itemId = parseInt(target.dataset.id)
    selectItem(itemId)
  }
}

function deleteItem(id) {
  items.value = items.value.filter(item => item.id !== id)
  lastAction.value = `删除项目 ${id}`
  console.log('删除项目:', id)
}

function selectItem(id) {
  const item = items.value.find(item => item.id === id)
  if (item) {
    lastAction.value = `选择项目: ${item.name}`
    console.log('选择项目:', item.name)
  }
}

function addItem() {
  const newId = Math.max(...items.value.map(item => item.id)) + 1
  const newItem = { id: newId, name: `项目${newId}` }
  items.value.push(newItem)
  lastAction.value = `添加项目: ${newItem.name}`
  console.log('添加项目:', newItem.name)
}
</script>

<style>
.item-list {
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 10px 0;
}

.list-item {
  padding: 10px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.list-item:hover {
  background-color: #f5f5f5;
}

.list-item:last-child {
  border-bottom: none;
}

.delete-btn {
  padding: 4px 8px;
  background-color: #ff4757;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.delete-btn:hover {
  background-color: #ff3838;
}

.action-result {
  margin-top: 10px;
  padding: 10px;
  background-color: #e8f5e8;
  border-radius: 4px;
  color: #2e7d32;
}
</style>
```

### 4. 事件清理

```vue
<template>
  <div>
    <!-- 全局事件监听示例 -->
    <button @click="startGlobalListen">开始全局监听</button>
    <button @click="stopGlobalListen">停止全局监听</button>
    
    <!-- 窗口大小监听 -->
    <button @click="startResizeListen">开始监听窗口大小</button>
    <button @click="stopResizeListen">停止监听窗口大小</button>
    
    <!-- 显示监听状态 -->
    <div class="status">
      <p>全局监听状态: {{ globalListenStatus }}</p>
      <p>窗口大小监听状态: {{ resizeListenStatus }}</p>
      <p v-if="windowSize">窗口大小: {{ windowSize.width }} x {{ windowSize.height }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onUnmounted } from 'vue'

const globalListenStatus = ref('未监听')
const resizeListenStatus = ref('未监听')
const windowSize = ref(null)

let globalClickHandler = null
let resizeHandler = null

function startGlobalListen() {
  if (globalClickHandler) return
  
  globalClickHandler = (event) => {
    console.log('全局点击事件:', event.target)
    globalListenStatus.value = '监听中'
  }
  
  document.addEventListener('click', globalClickHandler)
  globalListenStatus.value = '监听中'
}

function stopGlobalListen() {
  if (!globalClickHandler) return
  
  document.removeEventListener('click', globalClickHandler)
  globalClickHandler = null
  globalListenStatus.value = '已停止'
}

function startResizeListen() {
  if (resizeHandler) return
  
  resizeHandler = () => {
    windowSize.value = {
      width: window.innerWidth,
      height: window.innerHeight
    }
    console.log('窗口大小变化:', windowSize.value)
  }
  
  window.addEventListener('resize', resizeHandler)
  resizeHandler() // 初始化
  resizeListenStatus.value = '监听中'
}

function stopResizeListen() {
  if (!resizeHandler) return
  
  window.removeEventListener('resize', resizeHandler)
  resizeHandler = null
  resizeListenStatus.value = '已停止'
}

// 组件卸载时清理事件监听
onUnmounted(() => {
  console.log('组件卸载，清理事件监听')
  stopGlobalListen()
  stopResizeListen()
})
</script>

<style>
button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin: 5px;
}

button:hover {
  background-color: #3aa876;
}

.status {
  margin-top: 20px;
  padding: 15px;
  background-color: #f9f9f9;
  border-radius: 4px;
}

.status p {
  margin: 5px 0;
  color: #666;
}
</style>
```

## 🎯 总结

Vue3事件处理系统提供了强大而灵活的事件管理能力：

### 核心概念：

1. **事件绑定**：
   - `v-on` 或 `@` 指令
   - 支持内联语句和方法调用
   - 自动传递事件对象

2. **事件修饰符**：
   - `.stop`：阻止事件冒泡
   - `.prevent`：阻止默认行为
   - `.capture`：使用捕获模式
   - `.self`：只在元素本身触发
   - `.once`：只触发一次
   - `.passive`：passive模式监听

3. **按键修饰符**：
   - 常用按键：`.enter`、`.esc`、`.tab`、`.space`、`.delete`
   - 字母数字键：`.a`、`.1`等
   - 方向键：`.up`、`.down`、`.left`、`.right`
   - 功能键：`.f1`、`.f2`等

4. **鼠标修饰符**：
   - `.left`：左键
   - `.right`：右键
   - `.middle`：中键

5. **系统修饰符**：
   - `.ctrl`、`.alt`、`.shift`、`.meta`
   - `.exact`：精确匹配
   - 支持组合使用

### 最佳实践：

1. **性能优化**：
   - 使用防抖和节流处理频繁事件
   - 合理使用事件委托
   - 及时清理事件监听

2. **代码质量**：
   - 语义化的事件处理函数命名
   - 适当使用修饰符简化代码
   - 避免过度使用内联语句

3. **用户体验**：
   - 提供键盘快捷键支持
   - 合理处理鼠标事件
   - 确保事件响应的及时性

记住：**事件处理是Vue交互性的核心，掌握事件修饰符和最佳实践可以让你的应用更加高效和用户友好！** 🚀
