# Vue3计算属性与监听属性详解


# Vue3计算属性与监听属性详解

## 🎯 计算属性基础

计算属性（Computed Properties）是Vue3中用于处理复杂逻辑的响应式特性，它们会基于依赖的响应式数据进行缓存计算。

### 1. 基本计算属性

```vue
<template>
  <div>
    <!-- 基本计算属性示例 -->
    <div class="computed-demo">
      <h3>基本计算属性</h3>
      
      <div class="input-group">
        <label>姓:</label>
        <input v-model="lastName" placeholder="输入姓氏">
      </div>
      
      <div class="input-group">
        <label>名:</label>
        <input v-model="firstName" placeholder="输入名字">
      </div>
      
      <div class="result-group">
        <p>全名: {{ fullName }}</p>
        <p>反转全名: {{ reversedFullName }}</p>
        <p>字符数: {{ fullNameLength }}</p>
      </div>
    </div>
    
    <!-- 购物车计算示例 -->
    <div class="cart-demo">
      <h3>购物车计算</h3>
      
      <div class="cart-items">
        <div v-for="item in cartItems" :key="item.id" class="cart-item">
          <div class="item-info">
            <h4>{{ item.name }}</h4>
            <p>单价: ¥{{ item.price }}</p>
          </div>
          <div class="item-controls">
            <button @click="item.quantity > 1 && item.quantity--">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="item.quantity++">+</button>
          </div>
          <div class="item-total">
            <p>小计: ¥{{ itemTotal(item) }}</p>
          </div>
        </div>
      </div>
      
      <div class="cart-summary">
        <div class="summary-row">
          <span>商品总数:</span>
          <span>{{ totalItems }} 件</span>
        </div>
        <div class="summary-row">
          <span>商品总价:</span>
          <span>¥{{ subtotal }}</span>
        </div>
        <div class="summary-row">
          <span>运费:</span>
          <span>¥{{ shippingFee }}</span>
        </div>
        <div class="summary-row discount" v-if="discount > 0">
          <span>优惠:</span>
          <span>-¥{{ discount }}</span>
        </div>
        <div class="summary-row total">
          <span>应付总额:</span>
          <span>¥{{ totalAmount }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 基本计算属性
const firstName = ref('三')
const lastName = ref('张')

const fullName = computed(() => {
  return `${lastName.value}${firstName.value}`
})

const reversedFullName = computed(() => {
  return fullName.value.split('').reverse().join('')
})

const fullNameLength = computed(() => {
  return fullName.value.length
})

// 购物车计算
const cartItems = ref([
  { id: 1, name: 'iPhone 14', price: 5999, quantity: 1 },
  { id: 2, name: 'MacBook Pro', price: 12999, quantity: 1 },
  { id: 3, name: 'AirPods Pro', price: 1999, quantity: 2 }
])

// 计算单个商品小计（方法）
function itemTotal(item) {
  return item.price * item.quantity
}

// 计算商品总数
const totalItems = computed(() => {
  return cartItems.value.reduce((sum, item) => sum + item.quantity, 0)
})

// 计算商品总价
const subtotal = computed(() => {
  return cartItems.value.reduce((sum, item) => sum + (item.price * item.quantity), 0)
})

// 计算运费
const shippingFee = computed(() => {
  return subtotal.value > 10000 ? 0 : subtotal.value > 0 ? 99 : 0
})

// 计算优惠
const discount = computed(() => {
  const total = subtotal.value
  if (total >= 20000) return 2000
  if (total >= 10000) return 800
  if (total >= 5000) return 300
  return 0
})

// 计算应付总额
const totalAmount = computed(() => {
  return subtotal.value + shippingFee.value - discount.value
})
</script>

<style>
.computed-demo, .cart-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.input-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.result-group {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #2196f3;
}

.result-group p {
  margin: 5px 0;
  color: #1976d2;
  font-weight: bold;
}

.cart-items {
  margin-bottom: 20px;
}

.cart-item {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 15px;
  align-items: center;
  padding: 15px;
  margin-bottom: 10px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
}

.item-info h4 {
  margin: 0 0 5px 0;
  color: #333;
}

.item-info p {
  margin: 0;
  color: #666;
  font-size: 14px;
}

.item-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.item-controls button {
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.item-controls button:hover {
  background-color: #f0f0f0;
}

.item-controls span {
  min-width: 30px;
  text-align: center;
  font-weight: bold;
}

.item-total p {
  margin: 0;
  color: #42b983;
  font-weight: bold;
}

.cart-summary {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 5px 0;
}

.summary-row.discount {
  color: #dc3545;
}

.summary-row.total {
  font-size: 18px;
  font-weight: bold;
  color: #42b983;
  border-top: 2px solid #eee;
  padding-top: 15px;
  margin-top: 15px;
}

h3 {
  margin-top: 0;
  color: #333;
}
</style>
```

### 2. 计算属性vs方法

```vue
<template>
  <div>
    <!-- 计算属性vs方法对比 -->
    <div class="comparison-demo">
      <h3>计算属性 vs 方法对比</h3>
      
      <div class="input-section">
        <label>输入文本:</label>
        <input v-model="inputText" placeholder="输入一些文本">
        <p>输入次数: {{ inputCount }}</p>
      </div>
      
      <div class="results-grid">
        <div class="result-column">
          <h4>计算属性</h4>
          <div class="result-item">
            <p>原始文本: {{ computedOriginal }}</p>
            <p>大写转换: {{ computedUppercase }}</p>
            <p>字符长度: {{ computedLength }}</p>
            <p>反转文本: {{ computedReversed }}</p>
            <p>计算次数: {{ computedCallCount }}</p>
          </div>
        </div>
        
        <div class="result-column">
          <h4>方法调用</h4>
          <div class="result-item">
            <p>原始文本: {{ methodOriginal() }}</p>
            <p>大写转换: {{ methodUppercase() }}</p>
            <p>字符长度: {{ methodLength() }}</p>
            <p>反转文本: {{ methodReversed() }}</p>
            <p>调用次数: {{ methodCallCount }}</p>
          </div>
        </div>
      </div>
      
      <div class="explanation">
        <h4>性能对比说明:</h4>
        <ul>
          <li><strong>计算属性</strong>: 基于响应式依赖缓存，只有依赖数据变化时才重新计算</li>
          <li><strong>方法</strong>: 每次重新渲染都会调用，不进行缓存</li>
          <li><strong>计算次数</strong>: 在相同输入下，计算属性的调用次数明显少于方法</li>
        </ul>
      </div>
    </div>
    
    <!-- 复杂计算场景 -->
    <div class="complex-demo">
      <h3>复杂计算场景</h3>
      
      <div class="scenario-controls">
        <button @click="addDataPoint">添加数据点</button>
        <button @click="clearData">清空数据</button>
        <button @click="triggerRerender">触发重新渲染</button>
      </div>
      
      <div class="results-section">
        <div class="metric-card">
          <h4>平均值 (计算属性)</h4>
          <p class="metric-value">{{ averageValue }}</p>
          <p class="metric-info">计算次数: {{ averageComputedCount }}</p>
        </div>
        
        <div class="metric-card">
          <h4>平均值 (方法)</h4>
          <p class="metric-value">{{ averageValueMethod() }}</p>
          <p class="metric-info">调用次数: {{ averageMethodCount }}</p>
        </div>
        
        <div class="metric-card">
          <h4>最大值 (计算属性)</h4>
          <p class="metric-value">{{ maxValue }}</p>
          <p class="metric-info">计算次数: {{ maxComputedCount }}</p>
        </div>
        
        <div class="metric-card">
          <h4>最大值 (方法)</h4>
          <p class="metric-value">{{ maxValueMethod() }}</p>
          <p class="metric-info">调用次数: {{ maxMethodCount }}</p>
        </div>
      </div>
      
      <div class="data-display">
        <h4>当前数据:</h4>
        <div class="data-list">
          <span v-for="(value, index) in dataPoints" :key="index" class="data-point">
            {{ value }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 基础对比数据
const inputText = ref('')
const inputCount = ref(0)

// 计算属性调用计数
const computedCallCount = ref(0)
const methodCallCount = ref(0)

// 计算属性
const computedOriginal = computed(() => {
  computedCallCount.value++
  return inputText.value
})

const computedUppercase = computed(() => {
  computedCallCount.value++
  return inputText.value.toUpperCase()
})

const computedLength = computed(() => {
  computedCallCount.value++
  return inputText.value.length
})

const computedReversed = computed(() => {
  computedCallCount.value++
  return inputText.value.split('').reverse().join('')
})

// 方法
function methodOriginal() {
  methodCallCount.value++
  return inputText.value
}

function methodUppercase() {
  methodCallCount.value++
  return inputText.value.toUpperCase()
}

function methodLength() {
  methodCallCount.value++
  return inputText.value.length
}

function methodReversed() {
  methodCallCount.value++
  return inputText.value.split('').reverse().join('')
}

// 复杂计算场景
const dataPoints = ref([10, 25, 30, 45, 20, 35, 40])
const rerenderTrigger = ref(0)

// 计算属性计数器
const averageComputedCount = ref(0)
const maxComputedCount = ref(0)

// 方法计数器
const averageMethodCount = ref(0)
const maxMethodCount = ref(0)

// 计算属性
const averageValue = computed(() => {
  averageComputedCount.value++
  if (dataPoints.value.length === 0) return 0
  const sum = dataPoints.value.reduce((acc, val) => acc + val, 0)
  return (sum / dataPoints.value.length).toFixed(2)
})

const maxValue = computed(() => {
  maxComputedCount.value++
  if (dataPoints.value.length === 0) return 0
  return Math.max(...dataPoints.value)
})

// 方法
function averageValueMethod() {
  averageMethodCount.value++
  if (dataPoints.value.length === 0) return 0
  const sum = dataPoints.value.reduce((acc, val) => acc + val, 0)
  return (sum / dataPoints.value.length).toFixed(2)
}

function maxValueMethod() {
  maxMethodCount.value++
  if (dataPoints.value.length === 0) return 0
  return Math.max(...dataPoints.value)
}

// 操作函数
function addDataPoint() {
  const newValue = Math.floor(Math.random() * 100) + 1
  dataPoints.value.push(newValue)
}

function clearData() {
  dataPoints.value = []
}

function triggerRerender() {
  rerenderTrigger.value++
  console.log('触发重新渲染')
}

// 监听输入变化
const watchInputText = (newValue) => {
  inputCount.value++
}
</script>

<style>
.comparison-demo, .complex-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.input-section {
  margin-bottom: 20px;
}

.input-section label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.input-section input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.results-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.result-column {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.result-column h4 {
  margin-top: 0;
  color: #42b983;
  text-align: center;
}

.result-item {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
}

.result-item p {
  margin: 5px 0;
  color: #666;
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

.scenario-controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.results-section {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.metric-card {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
  text-align: center;
}

.metric-card h4 {
  margin-top: 0;
  color: #333;
}

.metric-value {
  font-size: 24px;
  font-weight: bold;
  color: #42b983;
  margin: 10px 0;
}

.metric-info {
  font-size: 12px;
  color: #666;
}

.data-display {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.data-display h4 {
  margin-top: 0;
  color: #333;
}

.data-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 10px;
}

.data-point {
  background-color: #e3f2fd;
  color: #1976d2;
  padding: 5px 10px;
  border-radius: 15px;
  font-size: 14px;
  font-weight: bold;
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

### 3. 计算属性的setter

计算属性默认只有getter，但也可以提供setter来实现双向绑定。

```vue
<template>
  <div>
    <!-- 基本setter示例 -->
    <div class="setter-demo">
      <h3>计算属性setter示例</h3>
      
      <div class="form-group">
        <label>全名:</label>
        <input v-model="fullName" placeholder="输入全名">
      </div>
      
      <div class="form-group">
        <label>姓:</label>
        <input v-model="lastName" placeholder="姓氏">
      </div>
      
      <div class="form-group">
        <label>名:</label>
        <input v-model="firstName" placeholder="名字">
      </div>
      
      <div class="info-panel">
        <h4>当前状态:</h4>
        <p>全名: {{ fullName }}</p>
        <p>分解: {{ lastName }} + {{ firstName }}</p>
      </div>
    </div>
    
    <!-- 坐标转换示例 -->
    <div class="coordinate-demo">
      <h3>坐标转换示例</h3>
      
      <div class="coordinate-controls">
        <div class="coord-group">
          <h4>直角坐标</h4>
          <div class="input-row">
            <label>X:</label>
            <input v-model.number="cartesianX" type="number" step="0.1">
          </div>
          <div class="input-row">
            <label>Y:</label>
            <input v-model.number="cartesianY" type="number" step="0.1">
          </div>
        </div>
        
        <div class="coord-group">
          <h4>极坐标</h4>
          <div class="input-row">
            <label>半径:</label>
            <input v-model.number="polarRadius" type="number" step="0.1" min="0">
          </div>
          <div class="input-row">
            <label>角度:</label>
            <input v-model.number="polarAngle" type="number" step="1">
          </div>
        </div>
      </div>
      
      <div class="coordinate-display">
        <div class="coord-display-item">
          <strong>直角坐标:</strong> ({{ cartesianX.toFixed(2) }}, {{ cartesianY.toFixed(2) }})
        </div>
        <div class="coord-display-item">
          <strong>极坐标:</strong> r={{ polarRadius.toFixed(2) }}, θ={{ polarAngle.toFixed(2) }}°
        </div>
      </div>
      
      <div class="coordinate-visual">
        <div class="coordinate-system">
          <div class="axes">
            <div class="x-axis"></div>
            <div class="y-axis"></div>
          </div>
          <div 
            class="point"
            :style="{
              left: `calc(50% + ${cartesianX * 20}px)`,
              top: `calc(50% - ${cartesianY * 20}px)`
            }"
          ></div>
        </div>
      </div>
    </div>
    
    <!-- 温度转换示例 -->
    <div class="temperature-demo">
      <h3>温度转换示例</h3>
      
      <div class="temp-converter">
        <div class="temp-input">
          <label>摄氏度 (°C):</label>
          <input v-model.number="celsius" type="number" step="0.1">
        </div>
        
        <div class="temp-input">
          <label>华氏度 (°F):</label>
          <input v-model.number="fahrenheit" type="number" step="0.1">
        </div>
        
        <div class="temp-input">
          <label>开尔文 (K):</label>
          <input v-model.number="kelvin" type="number" step="0.1">
        </div>
      </div>
      
      <div class="temp-info">
        <div class="temp-card">
          <h4>摄氏度</h4>
          <p class="temp-value">{{ celsius.toFixed(1) }}°C</p>
        </div>
        <div class="temp-card">
          <h4>华氏度</h4>
          <p class="temp-value">{{ fahrenheit.toFixed(1) }}°F</p>
        </div>
        <div class="temp-card">
          <h4>开尔文</h4>
          <p class="temp-value">{{ kelvin.toFixed(1) }}K</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 基本setter示例
const firstName = ref('三')
const lastName = ref('张')

const fullName = computed({
  get() {
    return `${lastName.value}${firstName.value}`
  },
  set(newValue) {
    const names = newValue.trim().split(/\s+/)
    if (names.length >= 2) {
      lastName.value = names[0]
      firstName.value = names.slice(1).join(' ')
    } else if (names.length === 1) {
      lastName.value = names[0]
      firstName.value = ''
    } else {
      lastName.value = ''
      firstName.value = ''
    }
  }
})

// 坐标转换示例
const cartesianX = ref(3)
const cartesianY = ref(4)

const polarRadius = computed({
  get() {
    return Math.sqrt(cartesianX.value ** 2 + cartesianY.value ** 2)
  },
  set(newValue) {
    const angle = Math.atan2(cartesianY.value, cartesianX.value)
    cartesianX.value = newValue * Math.cos(angle)
    cartesianY.value = newValue * Math.sin(angle)
  }
})

const polarAngle = computed({
  get() {
    return Math.atan2(cartesianY.value, cartesianX.value) * 180 / Math.PI
  },
  set(newValue) {
    const radius = polarRadius.value
    const angleRad = newValue * Math.PI / 180
    cartesianX.value = radius * Math.cos(angleRad)
    cartesianY.value = radius * Math.sin(angleRad)
  }
})

// 温度转换示例
const celsius = ref(20)

const fahrenheit = computed({
  get() {
    return celsius.value * 9/5 + 32
  },
  set(newValue) {
    celsius.value = (newValue - 32) * 5/9
  }
})

const kelvin = computed({
  get() {
    return celsius.value + 273.15
  },
  set(newValue) {
    celsius.value = newValue - 273.15
  }
})
</script>

<style>
.setter-demo, .coordinate-demo, .temperature-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.info-panel {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #2196f3;
}

.info-panel h4 {
  margin-top: 0;
  color: #1976d2;
}

.info-panel p {
  margin: 5px 0;
  color: #1976d2;
}

.coordinate-controls {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.coord-group {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.coord-group h4 {
  margin-top: 0;
  color: #42b983;
  text-align: center;
}

.input-row {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.input-row label {
  min-width: 60px;
  font-weight: bold;
  color: #333;
}

.input-row input {
  flex: 1;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.coordinate-display {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
  margin-bottom: 20px;
}

.coord-display-item {
  margin: 5px 0;
  color: #666;
}

.coordinate-visual {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.coordinate-system {
  position: relative;
  width: 200px;
  height: 200px;
  margin: 0 auto;
}

.axes {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.x-axis {
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 1px;
  background-color: #ddd;
}

.y-axis {
  position: absolute;
  top: 0;
  left: 50%;
  width: 1px;
  height: 100%;
  background-color: #ddd;
}

.point {
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: #42b983;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}

.temp-converter {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.temp-input {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.temp-input label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.temp-input input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.temp-info {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
}

.temp-card {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
  text-align: center;
}

.temp-card h4 {
  margin-top: 0;
  color: #333;
}

.temp-value {
  font-size: 24px;
  font-weight: bold;
  color: #42b983;
  margin: 10px 0;
}

h3 {
  margin-top: 0;
  color: #333;
}
</style>
```

## 📋 监听属性基础

监听属性（Watchers）用于观察和响应Vue实例上的数据变动。

### 1. 基本监听

```vue
<template>
  <div>
    <!-- 基本监听示例 -->
    <div class="basic-watch-demo">
      <h3>基本监听示例</h3>
      
      <div class="input-group">
        <label>监听消息:</label>
        <input v-model="message" placeholder="输入消息">
        <p>当前消息: {{ message }}</p>
        <p>消息长度: {{ messageLength }}</p>
        <p>历史记录:</p>
        <ul class="history-list">
          <li v-for="(item, index) in messageHistory" :key="index">
            {{ item }}
          </li>
        </ul>
      </div>
      
      <div class="input-group">
        <label>计数器:</label>
        <div class="counter-controls">
          <button @click="counter--">-</button>
          <span>{{ counter }}</span>
          <button @click="counter++">+</button>
        </div>
        <p>计数历史:</p>
        <ul class="history-list">
          <li v-for="(item, index) in counterHistory" :key="index">
            {{ item }}
          </li>
        </ul>
      </div>
    </div>
    
    <!-- 深度监听示例 -->
    <div class="deep-watch-demo">
      <h3>深度监听示例</h3>
      
      <div class="user-form">
        <div class="form-group">
          <label>用户名:</label>
          <input v-model="user.name" placeholder="输入用户名">
        </div>
        
        <div class="form-group">
          <label>年龄:</label>
          <input v-model.number="user.age" type="number" placeholder="输入年龄">
        </div>
        
        <div class="form-group">
          <label>邮箱:</label>
          <input v-model="user.contact.email" placeholder="输入邮箱">
        </div>
        
        <div class="form-group">
          <label>电话:</label>
          <input v-model="user.contact.phone" placeholder="输入电话">
        </div>
        
        <div class="form-group">
          <label>地址:</label>
          <input v-model="user.contact.address.city" placeholder="输入城市">
          <input v-model="user.contact.address.street" placeholder="输入街道">
        </div>
      </div>
      
      <div class="watch-results">
        <h4>监听结果:</h4>
        <div class="result-panel">
          <h5>用户对象变化:</h5>
          <pre>{{ JSON.stringify(user, null, 2) }}</pre>
        </div>
        
        <div class="result-panel">
          <h5>监听日志:</h5>
          <ul class="watch-log">
            <li v-for="(log, index) in watchLogs" :key="index">
              {{ log }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch } from 'vue'

// 基本监听
const message = ref('')
const messageLength = ref(0)
const messageHistory = ref([])

const counter = ref(0)
const counterHistory = ref([])

// 监听消息变化
watch(message, (newValue, oldValue) => {
  messageLength.value = newValue.length
  messageHistory.value.unshift(`消息从 "${oldValue}" 变为 "${newValue}"`)
  
  // 限制历史记录数量
  if (messageHistory.value.length > 10) {
    messageHistory.value.pop()
  }
  
  console.log('消息变化:', { oldValue, newValue })
})

// 监听计数器变化
watch(counter, (newValue, oldValue) => {
  counterHistory.value.unshift(`计数器从 ${oldValue} 变为 ${newValue}`)
  
  // 限制历史记录数量
  if (counterHistory.value.length > 10) {
    counterHistory.value.pop()
  }
  
  console.log('计数器变化:', { oldValue, newValue })
})

// 深度监听
const user = reactive({
  name: '',
  age: null,
  contact: {
    email: '',
    phone: '',
    address: {
      city: '',
      street: ''
    }
  }
})

const watchLogs = ref([])

function addWatchLog(message) {
  watchLogs.value.unshift(`${new Date().toLocaleTimeString()}: ${message}`)
  
  // 限制日志数量
  if (watchLogs.value.length > 20) {
    watchLogs.value.pop()
  }
}

// 监听整个用户对象（深度监听）
watch(user, (newValue, oldValue) => {
  addWatchLog('用户对象发生变化')
  console.log('用户对象变化:', { oldValue, newValue })
}, { deep: true })

// 监听用户名变化
watch(() => user.name, (newValue, oldValue) => {
  addWatchLog(`用户名从 "${oldValue}" 变为 "${newValue}"`)
})

// 监听年龄变化
watch(() => user.age, (newValue, oldValue) => {
  addWatchLog(`年龄从 ${oldValue} 变为 ${newValue}`)
})

// 监听邮箱变化
watch(() => user.contact.email, (newValue, oldValue) => {
  addWatchLog(`邮箱从 "${oldValue}" 变为 "${newValue}"`)
})

// 监听电话变化
watch(() => user.contact.phone, (newValue, oldValue) => {
  addWatchLog(`电话从 "${oldValue}" 变为 "${newValue}"`)
})

// 监听城市变化
watch(() => user.contact.address.city, (newValue, oldValue) => {
  addWatchLog(`城市从 "${oldValue}" 变为 "${newValue}"`)
})

// 监听街道变化
watch(() => user.contact.address.street, (newValue, oldValue) => {
  addWatchLog(`街道从 "${oldValue}" 变为 "${newValue}"`)
})
</script>

<style>
.basic-watch-demo, .deep-watch-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.input-group {
  margin-bottom: 25px;
  padding: 15px;
  background-color: #fff;
  border-radius: 4px;
  border: 1px solid #eee;
}

.input-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

.input-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 10px;
}

.counter-controls {
  display: flex;
  align-items: center;
  gap: 15px;
  margin: 10px 0;
}

.counter-controls button {
  width: 40px;
  height: 40px;
  border: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
}

.counter-controls button:hover {
  background-color: #f0f0f0;
}

.counter-controls span {
  font-size: 18px;
  font-weight: bold;
  min-width: 50px;
  text-align: center;
}

.history-list {
  list-style: none;
  padding: 0;
  margin: 10px 0;
  max-height: 150px;
  overflow-y: auto;
}

.history-list li {
  padding: 5px 10px;
  margin: 2px 0;
  background-color: #f0f8ff;
  border-radius: 3px;
  font-size: 12px;
  color: #1976d2;
}

.user-form {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.form-group {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 5px;
}

.watch-results {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

.result-panel {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.result-panel h5 {
  margin-top: 0;
  color: #42b983;
  margin-bottom: 10px;
}

.result-panel pre {
  margin: 0;
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 4px;
  font-size: 12px;
  max-height: 200px;
  overflow-y: auto;
}

.watch-log {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 200px;
  overflow-y: auto;
}

.watch-log li {
  padding: 5px 10px;
  margin: 2px 0;
  background-color: #f0f8ff;
  border-radius: 3px;
  font-size: 12px;
  color: #1976d2;
  border-left: 3px solid #2196f3;
}

h3 {
  margin-top: 0;
  color: #333;
}

h4, h5 {
  color: #333;
}
</style>
```

### 2. 监听选项和高级用法

```vue
<template>
  <div>
    <!-- 立即执行监听 -->
    <div class="immediate-demo">
      <h3>立即执行监听</h3>
      
      <div class="scenario">
        <h4>用户配置初始化</h4>
        <div class="config-form">
          <div class="form-group">
            <label>主题:</label>
            <select v-model="userConfig.theme">
              <option value="light">浅色主题</option>
              <option value="dark">深色主题</option>
              <option value="auto">自动主题</option>
            </select>
          </div>
          
          <div class="form-group">
            <label>语言:</label>
            <select v-model="userConfig.language">
              <option value="zh-CN">中文</option>
              <option value="en-US">English</option>
              <option value="ja-JP">日本語</option>
            </select>
          </div>
          
          <div class="form-group">
            <label>时区:</label>
            <select v-model="userConfig.timezone">
              <option value="UTC+8">北京时间</option>
              <option value="UTC+0">格林威治时间</option>
              <option value="UTC-5">纽约时间</option>
            </select>
          </div>
        </div>
        
        <div class="config-display">
          <h4>当前配置:</h4>
          <div class="config-item">
            <span>主题:</span>
            <span class="config-value">{{ userConfig.theme }}</span>
          </div>
          <div class="config-item">
            <span>语言:</span>
            <span class="config-value">{{ userConfig.language }}</span>
          </div>
          <div class="config-item">
            <span>时区:</span>
            <span class="config-value">{{ userConfig.timezone }}</span>
          </div>
        </div>
        
        <div class="initialization-log">
          <h4>初始化日志:</h4>
          <ul class="log-list">
            <li v-for="(log, index) in initLogs" :key="index">
              {{ log }}
            </li>
          </ul>
        </div>
      </div>
    </div>
    
    <!-- 异步监听 -->
    <div class="async-demo">
      <h3>异步监听</h3>
      
      <div class="search-scenario">
        <h4>搜索防抖</h4>
        <div class="search-box">
          <input 
            v-model="searchQuery" 
            placeholder="输入搜索关键词..."
            @keyup.enter="performSearch"
          >
          <button @click="performSearch">搜索</button>
        </div>
        
        <div class="search-status">
          <span v-if="isSearching" class="searching">搜索中...</span>
          <span v-else-if="searchResults.length > 0" class="results-found">
            找到 {{ searchResults.length }} 个结果
          </span>
          <span v-else-if="searchQuery" class="no-results">无搜索结果</span>
        </div>
        
        <div class="search-results">
          <div v-for="result in searchResults" :key="result.id" class="result-item">
            <h5>{{ result.title }}</h5>
            <p>{{ result.description }}</p>
            <span class="result-category">{{ result.category }}</span>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 多个监听源 -->
    <div class="multiple-demo">
      <h3>多个监听源</h3>
      
      <div class="form-layout">
        <div class="form-section">
          <h4>个人信息</h4>
          <div class="form-group">
            <label>姓名:</label>
            <input v-model="personInfo.name" placeholder="输入姓名">
          </div>
          <div class="form-group">
            <label>年龄:</label>
            <input v-model.number="personInfo.age" type="number" placeholder="输入年龄">
          </div>
        </div>
        
        <div class="form-section">
          <h4>工作信息</h4>
          <div class="form-group">
            <label>公司:</label>
            <input v-model="workInfo.company" placeholder="输入公司名称">
          </div>
          <div class="form-group">
            <label>职位:</label>
            <input v-model="workInfo.position" placeholder="输入职位">
          </div>
        </div>
      </div>
      
      <div class="combined-display">
        <h4>完整信息:</h4>
        <div class="info-card">
          <p><strong>姓名:</strong> {{ personInfo.name }}</p>
          <p><strong>年龄:</strong> {{ personInfo.age }}</p>
          <p><strong>公司:</strong> {{ workInfo.company }}</p>
          <p><strong>职位:</strong> {{ workInfo.position }}</p>
          <p><strong>最后更新:</strong> {{ lastUpdate }}</p>
        </div>
      </div>
      
      <div class="change-log">
        <h4>变更日志:</h4>
        <ul class="log-list">
          <li v-for="(log, index) in changeLogs" :key="index">
            {{ log }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watch } from 'vue'

// 立即执行监听
const userConfig = reactive({
  theme: 'light',
  language: 'zh-CN',
  timezone: 'UTC+8'
})

const initLogs = ref([])

function addInitLog(message) {
  initLogs.value.unshift(`${new Date().toLocaleTimeString()}: ${message}`)
  
  if (initLogs.value.length > 10) {
    initLogs.value.pop()
  }
}

// 立即执行监听用户配置
watch(userConfig, (newValue, oldValue) => {
  addInitLog(`配置变化: ${JSON.stringify(oldValue)} → ${JSON.stringify(newValue)}`)
  
  // 模拟应用配置
  console.log('应用新配置:', newValue)
}, { immediate: true, deep: true })

// 异步监听
const searchQuery = ref('')
const isSearching = ref(false)
const searchResults = ref([])

// 模拟搜索数据
const mockData = [
  { id: 1, title: 'Vue3入门教程', description: '学习Vue3的基础知识和核心概念', category: '教程' },
  { id: 2, title: 'JavaScript高级编程', description: '深入理解JavaScript的高级特性', category: '编程' },
  { id: 3, title: 'CSS Grid布局', description: '掌握CSS Grid布局系统', category: '前端' },
  { id: 4, title: 'Node.js后端开发', description: '使用Node.js构建后端应用', category: '后端' },
  { id: 5, title: 'TypeScript实战', description: '在实际项目中使用TypeScript', category: '编程' }
]

// 异步监听搜索查询
watch(searchQuery, async (newValue, oldValue) => {
  if (!newValue.trim()) {
    searchResults.value = []
    return
  }
  
  isSearching.value = true
  
  try {
    // 模拟API延迟
    await new Promise(resolve => setTimeout(resolve, 500))
    
    // 执行搜索
    const query = newValue.toLowerCase()
    searchResults.value = mockData.filter(item => 
      item.title.toLowerCase().includes(query) ||
      item.description.toLowerCase().includes(query)
    )
    
    console.log('搜索结果:', searchResults.value)
  } catch (error) {
    console.error('搜索失败:', error)
  } finally {
    isSearching.value = false
  }
}, { immediate: false })

function performSearch() {
  // 手动触发搜索
  console.log('手动搜索:', searchQuery.value)
}

// 多个监听源
const personInfo = reactive({
  name: '',
  age: null
})

const workInfo = reactive({
  company: '',
  position: ''
})

const lastUpdate = ref('')
const changeLogs = ref([])

function addChangeLog(message) {
  changeLogs.value.unshift(`${new Date().toLocaleTimeString()}: ${message}`)
  
  if (changeLogs.value.length > 15) {
    changeLogs.value.pop()
  }
}

// 监听多个源
watch(
  [() => personInfo.name, () => personInfo.age, () => workInfo.company, () => workInfo.position],
  ([newName, newAge, newCompany, newPosition], [oldName, oldAge, oldCompany, oldPosition]) => {
    lastUpdate.value = new Date().toLocaleString()
    
    // 检测具体的变化
    if (newName !== oldName) {
      addChangeLog(`姓名从 "${oldName}" 变为 "${newName}"`)
    }
    if (newAge !== oldAge) {
      addChangeLog(`年龄从 ${oldAge} 变为 ${newAge}`)
    }
    if (newCompany !== oldCompany) {
      addChangeLog(`公司从 "${oldCompany}" 变为 "${newCompany}"`)
    }
    if (newPosition !== oldPosition) {
      addChangeLog(`职位从 "${oldPosition}" 变为 "${newPosition}"`)
    }
    
    console.log('个人信息变化:', { personInfo, workInfo })
  }
)
</script>

<style>
.immediate-demo, .async-demo, .multiple-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.scenario {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  border: 1px solid #eee;
  margin-bottom: 15px;
}

.config-form {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
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

.form-group select, .form-group input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.config-display {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 15px;
}

.config-display h4 {
  margin-top: 0;
  color: #42b983;
}

.config-item {
  display: flex;
  justify-content: space-between;
  margin: 8px 0;
  padding: 5px 0;
  border-bottom: 1px solid #eee;
}

.config-value {
  font-weight: bold;
  color: #42b983;
}

.initialization-log {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #ffeaa7;
}

.initialization-log h4 {
  margin-top: 0;
  color: #856404;
}

.search-box {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.search-box input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.search-box button {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.search-box button:hover {
  background-color: #3aa876;
}

.search-status {
  margin-bottom: 15px;
  font-weight: bold;
}

.searching {
  color: #ffc107;
}

.results-found {
  color: #28a745;
}

.no-results {
  color: #dc3545;
}

.search-results {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 15px;
}

.result-item {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.result-item h5 {
  margin-top: 0;
  color: #333;
}

.result-item p {
  margin: 8px 0;
  color: #666;
  font-size: 14px;
}

.result-category {
  display: inline-block;
  padding: 2px 8px;
  background-color: #e3f2fd;
  color: #1976d2;
  border-radius: 12px;
  font-size: 12px;
}

.form-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.form-section {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.form-section h4 {
  margin-top: 0;
  color: #42b983;
}

.combined-display {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 15px;
}

.combined-display h4 {
  margin-top: 0;
  color: #333;
}

.info-card {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #ddd;
}

.info-card p {
  margin: 8px 0;
  color: #666;
}

.change-log {
  background-color: #e8f5e8;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #4caf50;
}

.change-log h4 {
  margin-top: 0;
  color: #2e7d32;
}

.log-list {
  list-style: none;
  padding: 0;
  margin: 10px 0;
  max-height: 150px;
  overflow-y: auto;
}

.log-list li {
  padding: 5px 10px;
  margin: 2px 0;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 3px;
  font-size: 12px;
  border-left: 3px solid #42b983;
}

h3, h4 {
  margin-top: 0;
  color: #333;
}
</style>
```

### 3. watchEffect

`watchEffect`会自动追踪回调中使用的响应式数据，并在数据变化时重新执行。

```vue
<template>
  <div>
    <!-- 基本watchEffect示例 -->
    <div class="watcheffect-demo">
      <h3>watchEffect 基础示例</h3>
      
      <div class="demo-section">
        <h4>自动追踪依赖</h4>
        <div class="controls">
          <div class="input-group">
            <label>消息:</label>
            <input v-model="message" placeholder="输入消息">
          </div>
          
          <div class="input-group">
            <label>计数:</label>
            <div class="counter-controls">
              <button @click="count--">-</button>
              <span>{{ count }}</span>
              <button @click="count++">+</button>
            </div>
          </div>
          
          <div class="input-group">
            <label>用户名:</label>
            <input v-model="username" placeholder="输入用户名">
          </div>
        </div>
        
        <div class="effect-results">
          <h5>Effect执行结果:</h5>
          <div class="result-panel">
            <p>组合结果: {{ combinedResult }}</p>
            <p>执行次数: {{ effectExecutionCount }}</p>
          </div>
        </div>
        
        <div class="effect-log">
          <h5>执行日志:</h5>
          <ul class="log-list">
            <li v-for="(log, index) in effectLogs" :key="index">
              {{ log }}
            </li>
          </ul>
        </div>
      </div>
    </div>
    
    <!-- watchEffect vs watch 对比 -->
    <div class="comparison-demo">
      <h3>watchEffect vs watch 对比</h3>
      
      <div class="comparison-grid">
        <div class="comparison-column">
          <h4>watchEffect</h4>
          <div class="demo-area">
            <div class="input-group">
              <label>文本A:</label>
              <input v-model="textA" placeholder="输入文本A">
            </div>
            <div class="input-group">
              <label>文本B:</label>
              <input v-model="textB" placeholder="输入文本B">
            </div>
            <div class="input-group">
              <label>数字:</label>
              <input v-model.number="number" type="number" placeholder="输入数字">
            </div>
          </div>
          
          <div class="result-display">
            <p>自动组合: {{ autoCombined }}</p>
            <p>执行次数: {{ watchEffectCount }}</p>
          </div>
        </div>
        
        <div class="comparison-column">
          <h4>watch</h4>
          <div class="demo-area">
            <div class="input-group">
              <label>文本A:</label>
              <input v-model="watchTextA" placeholder="输入文本A">
            </div>
            <div class="input-group">
              <label>文本B:</label>
              <input v-model="watchTextB" placeholder="输入文本B">
            </div>
            <div class="input-group">
              <label>数字:</label>
              <input v-model.number="watchNumber" type="number" placeholder="输入数字">
            </div>
          </div>
          
          <div class="result-display">
            <p>手动组合: {{ manualCombined }}</p>
            <p>执行次数: {{ watchCount }}</p>
          </div>
        </div>
      </div>
      
      <div class="comparison-explanation">
        <h4>区别说明:</h4>
        <ul>
          <li><strong>watchEffect</strong>: 自动追踪依赖，不需要明确指定监听源</li>
          <li><strong>watch</strong>: 需要明确指定监听源，更精确控制</li>
          <li><strong>watchEffect</strong>: 立即执行一次，watch需要设置immediate</li>
          <li><strong>watch</strong>: 可以访问oldValue，watchEffect不能</li>
        </ul>
      </div>
    </div>
    
    <!-- 实际应用场景 -->
    <div class="application-demo">
      <h3>实际应用场景</h3>
      
      <div class="app-scenarios">
        <!-- DOM操作场景 -->
        <div class="scenario-card">
          <h4>DOM操作</h4>
          <div class="scenario-content">
            <div class="color-controls">
              <label>背景色:</label>
              <input v-model="backgroundColor" type="color">
              <label>文字色:</label>
              <input v-model="textColor" type="color">
              <label>字体大小:</label>
              <input v-model.number="fontSize" type="range" min="12" max="48">
            </div>
            
            <div 
              class="styled-content"
              :style="dynamicStyles"
            >
              这是一段动态样式的文本内容
            </div>
          </div>
        </div>
        
        <!-- API调用场景 -->
        <div class="scenario-card">
          <h4>API调用</h4>
          <div class="scenario-content">
            <div class="api-controls">
              <label>用户ID:</label>
              <input v-model.number="userId" type="number" min="1" max="10">
              <button @click="refreshUserData">刷新</button>
            </div>
            
            <div class="user-data">
              <div v-if="isLoading" class="loading">加载中...</div>
              <div v-else-if="userData" class="user-info">
                <img :src="userData.avatar" :alt="userData.name" class="user-avatar">
                <div class="user-details">
                  <h5>{{ userData.name }}</h5>
                  <p>{{ userData.email }}</p>
                  <p>{{ userData.company }}</p>
                </div>
              </div>
              <div v-else class="no-data">无用户数据</div>
            </div>
          </div>
        </div>
        
        <!-- 缓存计算场景 -->
        <div class="scenario-card">
          <h4>缓存计算</h4>
          <div class="scenario-content">
            <div class="cache-controls">
              <label>计算表达式:</label>
              <input v-model="calcExpression" placeholder="如: 2+3*4">
              <button @click="performCalculation">计算</button>
            </div>
            
            <div class="calc-result">
              <div v-if="calcResult !== null" class="result-display">
                <p>结果: {{ calcResult }}</p>
                <p>计算次数: {{ calcCount }}</p>
                <p>缓存命中: {{ cacheHits }}/{{ calcCount }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, watchEffect, watch, computed } from 'vue'

// 基本watchEffect示例
const message = ref('')
const count = ref(0)
const username = ref('')

const combinedResult = ref('')
const effectExecutionCount = ref(0)
const effectLogs = ref([])

function addEffectLog(message) {
  effectLogs.value.unshift(`${new Date().toLocaleTimeString()}: ${message}`)
  
  if (effectLogs.value.length > 10) {
    effectLogs.value.pop()
  }
}

// 自动追踪依赖的watchEffect
watchEffect(() => {
  effectExecutionCount.value++
  
  // 自动追踪这些响应式数据
  const result = `${message.value} - ${count.value} - ${username.value}`
  combinedResult.value = result
  
  addEffectLog(`Effect执行: ${result}`)
  console.log('watchEffect执行:', result)
})

// watchEffect vs watch 对比
const textA = ref('')
const textB = ref('')
const number = ref(0)

const watchTextA = ref('')
const watchTextB = ref('')
const watchNumber = ref(0)

const watchEffectCount = ref(0)
const watchCount = ref(0)

// watchEffect - 自动追踪
const autoCombined = ref('')
watchEffect(() => {
  watchEffectCount.value++
  autoCombined.value = `${textA.value} ${textB.value} ${number.value}`
})

// watch - 手动指定
const manualCombined = ref('')
watch(
  [watchTextA, watchTextB, watchNumber],
  ([newA, newB, newNum]) => {
    watchCount.value++
    manualCombined.value = `${newA} ${newB} ${newNum}`
  }
)

// 实际应用场景
const backgroundColor = ref('#ffffff')
const textColor = ref('#000000')
const fontSize = ref(16)

const dynamicStyles = computed(() => ({
  backgroundColor: backgroundColor.value,
  color: textColor.value,
  fontSize: fontSize.value + 'px'
}))

// DOM操作的watchEffect
watchEffect(() => {
  const element = document.querySelector('.styled-content')
  if (element) {
    console.log('DOM样式更新:', dynamicStyles.value)
  }
})

// API调用场景
const userId = ref(1)
const userData = ref(null)
const isLoading = ref(false)

// 模拟用户数据
const mockUsers = [
  { id: 1, name: '张三', email: 'zhangsan@example.com', company: 'Tech Corp', avatar: 'https://i.pravatar.cc/150?img=1' },
  { id: 2, name: '李四', email: 'lisi@example.com', company: 'Design Studio', avatar: 'https://i.pravatar.cc/150?img=2' },
  { id: 3, name: '王五', email: 'wangwu@example.com', company: 'Data Systems', avatar: 'https://i.pravatar.cc/150?img=3' }
]

// API调用的watchEffect
watchEffect(async () => {
  if (userId.value) {
    isLoading.value = true
    
    try {
      // 模拟API调用
      await new Promise(resolve => setTimeout(resolve, 500))
      
      userData.value = mockUsers.find(user => user.id === userId.value) || null
      console.log('用户数据加载:', userData.value)
    } catch (error) {
      console.error('加载用户数据失败:', error)
    } finally {
      isLoading.value = false
    }
  }
})

function refreshUserData() {
  console.log('手动刷新用户数据')
}

// 缓存计算场景
const calcExpression = ref('')
const calcResult = ref(null)
const calcCount = ref(0)
const cacheHits = ref(0)
const calculationCache = new Map()

function performCalculation() {
  if (!calcExpression.value.trim()) return
  
  calcCount.value++
  
  // 检查缓存
  if (calculationCache.has(calcExpression.value)) {
    cacheHits.value++
    calcResult.value = calculationCache.get(calcExpression.value)
    console.log('缓存命中:', calcExpression.value)
    return
  }
  
  try {
    // 简单的表达式计算
    const result = eval(calcExpression.value)
    calcResult.value = result
    
    // 缓存结果
    calculationCache.set(calcExpression.value, result)
    console.log('计算并缓存:', calcExpression.value, '=', result)
  } catch (error) {
    calcResult.value = '错误: ' + error.message
  }
}

// 使用watchEffect处理缓存
watchEffect(() => {
  if (calcExpression.value) {
    // 可以在这里添加自动计算逻辑
    console.log('表达式变化:', calcExpression.value)
  }
})
</script>

<style>
.watcheffect-demo, .comparison-demo, .application-demo {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background-color: #f9f9f9;
}

.demo-section {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  border: 1px solid #eee;
  margin-bottom: 15px;
}

.controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
}

.input-group label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.input-group input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.counter-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 5px;
}

.counter-controls button {
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  cursor: pointer;
}

.counter-controls span {
  min-width: 40px;
  text-align: center;
  font-weight: bold;
}

.effect-results {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #2196f3;
  margin-bottom: 15px;
}

.effect-results h5 {
  margin-top: 0;
  color: #1976d2;
}

.result-panel {
  background-color: #fff;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ddd;
}

.result-panel p {
  margin: 5px 0;
  color: #666;
}

.effect-log {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #ffeaa7;
}

.effect-log h5 {
  margin-top: 0;
  color: #856404;
}

.comparison-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.comparison-column {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.comparison-column h4 {
  margin-top: 0;
  color: #42b983;
  text-align: center;
}

.result-display {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin-top: 10px;
}

.result-display p {
  margin: 5px 0;
  color: #666;
}

.comparison-explanation {
  background-color: #e8f5e8;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #4caf50;
}

.comparison-explanation h4 {
  margin-top: 0;
  color: #2e7d32;
}

.comparison-explanation ul {
  margin: 10px 0;
  padding-left: 20px;
}

.app-scenarios {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.scenario-card {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  border: 1px solid #eee;
}

.scenario-card h4 {
  margin-top: 0;
  color: #42b983;
}

.scenario-content {
  margin-top: 10px;
}

.color-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 10px;
  margin-bottom: 15px;
}

.color-controls label {
  font-size: 12px;
  color: #666;
}

.color-controls input {
  width: 100%;
}

.styled-content {
  padding: 20px;
  border-radius: 4px;
  text-align: center;
  font-weight: bold;
  transition: all 0.3s ease;
}

.api-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.api-controls input {
  width: 100px;
}

.user-data {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  text-align: center;
}

.loading {
  color: #ffc107;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 15px;
}

.user-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.user-details h5 {
  margin: 0 0 5px 0;
  color: #333;
}

.user-details p {
  margin: 2px 0;
  color: #666;
  font-size: 14px;
}

.cache-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.cache-controls input {
  flex: 1;
}

.calc-result {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 4px;
  text-align: center;
}

.log-list {
  list-style: none;
  padding: 0;
  margin: 10px 0;
  max-height: 120px;
  overflow-y: auto;
}

.log-list li {
  padding: 4px 8px;
  margin: 2px 0;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 3px;
  font-size: 12px;
  border-left: 3px solid #42b983;
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

h3, h4, h5 {
  margin-top: 0;
  color: #333;
}
</style>
```

## 🎯 总结

Vue3的计算属性和监听属性是响应式系统的核心特性，提供了强大的数据处理和监听能力：

### 计算属性 (Computed Properties)：

1. **基本特性**：
   - 基于依赖进行缓存，只有依赖变化时才重新计算
   - 支持getter和setter，实现双向绑定
   - 性能优于方法，适合复杂计算

2. **vs方法对比**：
   - 计算属性：缓存结果，依赖变化时重新计算
   - 方法：每次调用都执行，不进行缓存
   - 计算属性适合复杂计算，方法适合简单操作

3. **setter功能**：
   - 可以设置计算属性的值
   - 常用于数据格式化和转换
   - 支持坐标转换、温度转换等场景

### 监听属性 (Watchers)：

1. **基本监听**：
   - 监听单个响应式数据变化
   - 提供newValue和oldValue
   - 支持深度监听对象内部变化

2. **高级选项**：
   - `immediate`: 立即执行监听器
   - `deep`: 深度监听对象内部变化
   - 异步操作和防抖处理

3. **多个监听源**：
   - 可以同时监听多个数据源
   - 便于处理关联数据的变化
   - 提供完整的变更信息

### watchEffect：

1. **自动依赖追踪**：
   - 自动追踪回调中使用的响应式数据
   - 不需要明确指定监听源
   - 立即执行一次

2. **vs watch对比**：
   - watchEffect：自动追踪，立即执行，无oldValue
   - watch：明确指定，需要immediate，有oldValue
   - watchEffect适合副作用，watch适合精确控制

### 最佳实践：

1. **性能优化**：
   - 优先使用计算属性处理复杂逻辑
   - 合理使用缓存减少重复计算
   - 避免在模板中进行复杂计算

2. **代码组织**：
   - 计算属性用于派生数据
   - 监听属性用于副作用和异步操作
   - watchEffect用于自动追踪的副作用

3. **实际应用**：
   - 表单验证和数据处理
   - API调用和数据加载
   - DOM操作和样式管理
   - 缓存和状态管理

记住：**计算属性和监听属性是Vue响应式系统的核心，合理使用可以大大提升应用性能和代码可维护性！** 🚀
