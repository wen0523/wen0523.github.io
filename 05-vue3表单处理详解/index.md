# Vue3表单处理详解


# Vue3表单处理详解

## 🎯 表单绑定基础

Vue3提供了强大的表单处理能力，通过`v-model`指令实现双向数据绑定，简化了表单数据的收集和处理。

### 基本表单绑定

```vue
<template>
  <div>
    <!-- 文本输入框 -->
    <div class="form-group">
      <label for="username">用户名:</label>
      <input 
        id="username" 
        v-model="formData.username" 
        placeholder="请输入用户名"
      >
      <p>输入内容: {{ formData.username }}</p>
    </div>
    
    <!-- 密码输入框 -->
    <div class="form-group">
      <label for="password">密码:</label>
      <input 
        id="password" 
        v-model="formData.password" 
        type="password"
        placeholder="请输入密码"
      >
    </div>
    
    <!-- 多行文本 -->
    <div class="form-group">
      <label for="description">描述:</label>
      <textarea 
        id="description" 
        v-model="formData.description" 
        placeholder="请输入描述信息"
        rows="4"
      ></textarea>
      <p>字数: {{ formData.description.length }}</p>
    </div>
    
    <!-- 提交按钮 -->
    <button @click="handleSubmit" class="submit-btn">提交表单</button>
    
    <!-- 显示表单数据 -->
    <div v-if="submitted" class="result">
      <h3>提交结果:</h3>
      <pre>{{ JSON.stringify(formData, null, 2) }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const formData = reactive({
  username: '',
  password: '',
  description: ''
})

const submitted = ref(false)

function handleSubmit() {
  console.log('表单提交:', formData)
  submitted.value = true
  
  // 模拟表单提交
  setTimeout(() => {
    alert(`提交成功！用户名: ${formData.username}`)
    submitted.value = false
  }, 1000)
}
</script>

<style>
.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

.submit-btn {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.submit-btn:hover {
  background-color: #3aa876;
}

.result {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

.result pre {
  margin: 0;
  white-space: pre-wrap;
  word-break: break-all;
}
</style>
```

## 📋 单选按钮和复选框

### 1. 复选框

```vue
<template>
  <div>
    <!-- 单个复选框 -->
    <div class="form-group">
      <label>
        <input type="checkbox" v-model="singleChecked">
        我同意服务条款
      </label>
      <p>状态: {{ singleChecked ? '已同意' : '未同意' }}</p>
    </div>
    
    <!-- 多个复选框绑定到数组 -->
    <div class="form-group">
      <label>选择兴趣爱好:</label>
      <div class="checkbox-group">
        <label v-for="hobby in hobbies" :key="hobby.value">
          <input 
            type="checkbox" 
            :value="hobby.value"
            v-model="selectedHobbies"
          >
          {{ hobby.label }}
        </label>
      </div>
      <p>选择的爱好: {{ selectedHobbies.join(', ') || '无' }}</p>
    </div>
    
    <!-- 复选框的true/false值 -->
    <div class="form-group">
      <label>
        <input 
          type="checkbox" 
          v-model="toggleValue"
          true-value="yes"
          false-value="no"
        >
        自定义值的复选框
      </label>
      <p>当前值: {{ toggleValue }}</p>
    </div>
    
    <!-- 复选框对象绑定 -->
    <div class="form-group">
      <label>选择技能:</label>
      <div class="checkbox-group">
        <label v-for="skill in skills" :key="skill.id">
          <input 
            type="checkbox" 
            :value="skill"
            v-model="selectedSkills"
          >
          {{ skill.name }}
        </label>
      </div>
      <p>选择的技能:</p>
      <ul>
        <li v-for="skill in selectedSkills" :key="skill.id">
          {{ skill.name }} - 等级: {{ skill.level }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const singleChecked = ref(false)

const hobbies = ref([
  { value: 'reading', label: '阅读' },
  { value: 'music', label: '音乐' },
  { value: 'sports', label: '运动' },
  { value: 'travel', label: '旅行' }
])

const selectedHobbies = ref([])

const toggleValue = ref('no')

const skills = ref([
  { id: 1, name: 'JavaScript', level: '高级' },
  { id: 2, name: 'Vue.js', level: '中级' },
  { id: 3, name: 'Python', level: '初级' },
  { id: 4, name: 'React', level: '中级' }
])

const selectedSkills = ref([])
</script>

<style>
.checkbox-group {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  margin-top: 10px;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 5px;
  cursor: pointer;
}

.checkbox-group input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.form-group {
  margin-bottom: 25px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

ul {
  margin: 10px 0;
  padding-left: 20px;
}

li {
  margin: 5px 0;
}
</style>
```

### 2. 单选按钮

```vue
<template>
  <div>
    <!-- 基本单选按钮 -->
    <div class="form-group">
      <label>性别:</label>
      <div class="radio-group">
        <label>
          <input type="radio" v-model="gender" value="male">
          男
        </label>
        <label>
          <input type="radio" v-model="gender" value="female">
          女
        </label>
        <label>
          <input type="radio" v-model="gender" value="other">
          其他
        </label>
      </div>
      <p>选择的性别: {{ gender }}</p>
    </div>
    
    <!-- 单选按钮绑定对象 -->
    <div class="form-group">
      <label>选择城市:</label>
      <div class="radio-group">
        <label v-for="city in cities" :key="city.id">
          <input 
            type="radio" 
            :value="city"
            v-model="selectedCity"
          >
          {{ city.name }}
        </label>
      </div>
      <p v-if="selectedCity">
        选择的城市: {{ selectedCity.name }} - {{ selectedCountry }}
      </p>
    </div>
    
    <!-- 单选按钮组 -->
    <div class="form-group">
      <label>支付方式:</label>
      <div class="radio-group">
        <label v-for="payment in paymentMethods" :key="payment.value">
          <input 
            type="radio" 
            v-model="selectedPayment"
            :value="payment.value"
          >
          {{ payment.label }}
          <span class="payment-desc">{{ payment.description }}</span>
        </label>
      </div>
      <p>支付方式: {{ selectedPayment }}</p>
    </div>
    
    <!-- 禁用状态 -->
    <div class="form-group">
      <label>会员等级:</label>
      <div class="radio-group">
        <label>
          <input type="radio" v-model="memberLevel" value="normal">
          普通会员
        </label>
        <label>
          <input type="radio" v-model="memberLevel" value="premium" disabled>
          高级会员（暂不可用）
        </label>
        <label>
          <input type="radio" v-model="memberLevel" value="vip" disabled>
          VIP会员（暂不可用）
        </label>
      </div>
      <p>会员等级: {{ memberLevel }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const gender = ref('male')

const cities = ref([
  { id: 1, name: '北京', country: '中国' },
  { id: 2, name: '上海', country: '中国' },
  { id: 3, name: '纽约', country: '美国' },
  { id: 4, name: '伦敦', country: '英国' }
])

const selectedCity = ref(null)

const selectedCountry = computed(() => {
  return selectedCity.value ? selectedCity.value.country : ''
})

const paymentMethods = ref([
  { 
    value: 'alipay', 
    label: '支付宝', 
    description: '快速安全' 
  },
  { 
    value: 'wechat', 
    label: '微信支付', 
    description: '便捷支付' 
  },
  { 
    value: 'bank', 
    label: '银行卡', 
    description: '传统支付' 
  },
  { 
    value: 'cod', 
    label: '货到付款', 
    description: '现金支付' 
  }
])

const selectedPayment = ref('alipay')

const memberLevel = ref('normal')
</script>

<style>
.radio-group {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 10px;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 5px;
  cursor: pointer;
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.radio-group label:hover {
  background-color: #f0f8ff;
  border-color: #42b983;
}

.radio-group input[type="radio"] {
  width: auto;
  margin: 0;
}

.radio-group input[type="radio"]:checked + span {
  color: #42b983;
  font-weight: bold;
}

.payment-desc {
  font-size: 12px;
  color: #666;
  margin-left: 5px;
}

.radio-group label:has(input:disabled) {
  opacity: 0.5;
  cursor: not-allowed;
}

.form-group {
  margin-bottom: 25px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}
</style>
```

## 📝 选择框

### 1. 单选选择框

```vue
<template>
  <div>
    <!-- 基本选择框 -->
    <div class="form-group">
      <label for="country">国家:</label>
      <select id="country" v-model="selectedCountry">
        <option value="">请选择国家</option>
        <option value="china">中国</option>
        <option value="usa">美国</option>
        <option value="uk">英国</option>
        <option value="japan">日本</option>
      </select>
      <p>选择的国家: {{ selectedCountry }}</p>
    </div>
    
    <!-- 绑定对象到选择框 -->
    <div class="form-group">
      <label for="city">城市:</label>
      <select id="city" v-model="selectedCity">
        <option value="">请选择城市</option>
        <option 
          v-for="city in cities" 
          :key="city.id" 
          :value="city"
        >
          {{ city.name }}
        </option>
      </select>
      <p v-if="selectedCity">
        选择的城市: {{ selectedCity.name }} - {{ selectedCountry }}
      </p>
    </div>
    
    <!-- 分组选择框 -->
    <div class="form-group">
      <label for="department">部门:</label>
      <select id="department" v-model="selectedDepartment">
        <option value="">请选择部门</option>
        <optgroup label="技术部门">
          <option value="frontend">前端开发</option>
          <option value="backend">后端开发</option>
          <option value="mobile">移动开发</option>
        </optgroup>
        <optgroup label="产品部门">
          <option value="product">产品设计</option>
          <option value="operation">产品运营</option>
        </optgroup>
        <optgroup label="市场部门">
          <option value="marketing">市场营销</option>
          <option value="sales">销售</option>
        </optgroup>
      </select>
      <p>选择的部门: {{ selectedDepartment }}</p>
    </div>
    
    <!-- 禁用选项 -->
    <div class="form-group">
      <label for="status">状态:</label>
      <select id="status" v-model="selectedStatus">
        <option value="active">激活</option>
        <option value="inactive">未激活</option>
        <option value="pending" disabled>待处理</option>
        <option value="blocked" disabled>已禁用</option>
      </select>
      <p>当前状态: {{ selectedStatus }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedCountry = ref('')

const cities = ref([
  { id: 1, name: '北京', country: 'china' },
  { id: 2, name: '上海', country: 'china' },
  { id: 3, name: '纽约', country: 'usa' },
  { id: 4, name: '洛杉矶', country: 'usa' },
  { id: 5, name: '伦敦', country: 'uk' },
  { id: 6, name: '东京', country: 'japan' }
])

const selectedCity = ref(null)

const selectedDepartment = ref('')

const selectedStatus = ref('active')
</script>

<style>
.form-group {
  margin-bottom: 25px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

select {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  background-color: white;
}

select:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

optgroup {
  font-weight: bold;
  color: #666;
}

option {
  padding: 5px;
}

option:disabled {
  color: #999;
  background-color: #f5f5f5;
}
</style>
```

### 2. 多选选择框

```vue
<template>
  <div>
    <!-- 基本多选选择框 -->
    <div class="form-group">
      <label for="languages">编程语言:</label>
      <select id="languages" v-model="selectedLanguages" multiple>
        <option value="javascript">JavaScript</option>
        <option value="python">Python</option>
        <option value="java">Java</option>
        <option value="c++">C++</option>
        <option value="go">Go</option>
        <option value="rust">Rust</option>
      </select>
      <p>选择的语言: {{ selectedLanguages.join(', ') || '无' }}</p>
      <p class="hint">按住Ctrl/Cmd键可多选</p>
    </div>
    
    <!-- 多选对象 -->
    <div class="form-group">
      <label for="skills">技能:</label>
      <select id="skills" v-model="selectedSkills" multiple>
        <option 
          v-for="skill in skills" 
          :key="skill.id" 
          :value="skill"
        >
          {{ skill.name }} ({{ skill.level }})
        </option>
      </select>
      <div v-if="selectedSkills.length > 0">
        <h4>选择的技能:</h4>
        <ul>
          <li v-for="skill in selectedSkills" :key="skill.id">
            {{ skill.name }} - {{ skill.level }}
          </li>
        </ul>
      </div>
    </div>
    
    <!-- 多选分组 -->
    <div class="form-group">
      <label for="tools">开发工具:</label>
      <select id="tools" v-model="selectedTools" multiple>
        <optgroup label="前端工具">
          <option value="vscode">VS Code</option>
          <option value="webstorm">WebStorm</option>
          <option value="sublime">Sublime Text</option>
        </optgroup>
        <optgroup label="后端工具">
          <option value="intellij">IntelliJ IDEA</option>
          <option value="eclipse">Eclipse</option>
          <option value="postman">Postman</option>
        </optgroup>
        <optgroup label="数据库工具">
          <option value="mysql">MySQL Workbench</option>
          <option value="pgadmin">pgAdmin</option>
          <option value="redis">Redis Desktop</option>
        </optgroup>
      </select>
      <p>选择的工具: {{ selectedTools.join(', ') || '无' }}</p>
    </div>
    
    <!-- 带最大选择限制 -->
    <div class="form-group">
      <label for="favorites">最喜欢的颜色 (最多3个):</label>
      <select 
        id="favorites" 
        v-model="selectedColors" 
        multiple
        @change="handleColorChange"
      >
        <option value="red">红色</option>
        <option value="blue">蓝色</option>
        <option value="green">绿色</option>
        <option value="yellow">黄色</option>
        <option value="purple">紫色</option>
        <option value="orange">橙色</option>
      </select>
      <p>选择的颜色: {{ selectedColors.join(', ') || '无' }}</p>
      <p class="hint">已选择 {{ selectedColors.length }}/3</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const selectedLanguages = ref(['javascript', 'python'])

const skills = ref([
  { id: 1, name: 'Vue.js', level: '高级' },
  { id: 2, name: 'React', level: '中级' },
  { id: 3, name: 'Angular', level: '初级' },
  { id: 4, name: 'Node.js', level: '中级' },
  { id: 5, name: 'TypeScript', level: '高级' }
])

const selectedSkills = ref([])

const selectedTools = ref([])

const selectedColors = ref([])

function handleColorChange(event) {
  if (selectedColors.value.length > 3) {
    // 移除最后选择的项
    const lastSelected = event.target.options[event.target.selectedIndex]
    selectedColors.value = selectedColors.value.filter(color => color !== lastSelected.value)
    
    // 取消选择
    lastSelected.selected = false
    
    alert('最多只能选择3个颜色！')
  }
}
</script>

<style>
.form-group {
  margin-bottom: 25px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

select {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  background-color: white;
  min-height: 100px;
}

select:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

select[multiple] {
  height: auto;
}

.hint {
  font-size: 12px;
  color: #666;
  margin-top: 5px;
}

ul {
  margin: 10px 0;
  padding-left: 20px;
}

li {
  margin: 5px 0;
}

optgroup {
  font-weight: bold;
  color: #666;
}
</style>
```

## 🛠️ v-model修饰符

### 1. .lazy - 延迟更新

```vue
<template>
  <div>
    <!-- 普通 v-model -->
    <div class="form-group">
      <label for="normal">普通输入 (实时更新):</label>
      <input 
        id="normal" 
        v-model="normalInput" 
        placeholder="输入内容"
      >
      <p>实时显示: {{ normalInput }}</p>
      <p>输入次数: {{ normalCount }}</p>
    </div>
    
    <!-- .lazy 修饰符 -->
    <div class="form-group">
      <label for="lazy">延迟输入 (失去焦点时更新):</label>
      <input 
        id="lazy" 
        v-model.lazy="lazyInput" 
        placeholder="输入内容"
      >
      <p>延迟显示: {{ lazyInput }}</p>
      <p>更新次数: {{ lazyCount }}</p>
    </div>
    
    <!-- 文本域对比 -->
    <div class="form-group">
      <label for="normal-textarea">普通文本域:</label>
      <textarea 
        id="normal-textarea" 
        v-model="normalTextarea" 
        placeholder="输入多行文本"
        rows="4"
      ></textarea>
      <p>字符数: {{ normalTextarea.length }}</p>
    </div>
    
    <div class="form-group">
      <label for="lazy-textarea">延迟文本域:</label>
      <textarea 
        id="lazy-textarea" 
        v-model.lazy="lazyTextarea" 
        placeholder="输入多行文本"
        rows="4"
      ></textarea>
      <p>字符数: {{ lazyTextarea.length }}</p>
    </div>
    
    <!-- 应用场景：搜索框 -->
    <div class="form-group">
      <label for="search">搜索框 (使用.lazy减少API调用):</label>
      <input 
        id="search" 
        v-model.lazy="searchQuery" 
        placeholder="输入搜索关键词"
      >
      <button @click="performSearch">搜索</button>
      <p v-if="lastSearch">上次搜索: {{ lastSearch }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const normalInput = ref('')
const lazyInput = ref('')
const normalTextarea = ref('')
const lazyTextarea = ref('')
const searchQuery = ref('')
const lastSearch = ref('')

const normalCount = ref(0)
const lazyCount = ref(0)

// 监听普通输入
watch(normalInput, (newValue) => {
  normalCount.value++
  console.log('普通输入更新:', newValue)
})

// 监听延迟输入
watch(lazyInput, (newValue) => {
  lazyCount.value++
  console.log('延迟输入更新:', newValue)
})

// 监听搜索查询
watch(searchQuery, (newValue) => {
  if (newValue.trim()) {
    console.log('搜索查询更新:', newValue)
    // 这里可以触发搜索API
  }
})

function performSearch() {
  if (searchQuery.value.trim()) {
    lastSearch.value = searchQuery.value
    console.log('执行搜索:', searchQuery.value)
    alert(`搜索: ${searchQuery.value}`)
  }
}
</script>

<style>
.form-group {
  margin-bottom: 25px;
  padding: 15px;
  border: 1px solid #eee;
  border-radius: 4px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

input, textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

input:focus, textarea:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

button {
  margin-top: 10px;
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

p {
  margin: 5px 0;
  color: #666;
}
</style>
```

### 2. .number - 数字转换

```vue
<template>
  <div>
    <!-- 普通输入 (字符串) -->
    <div class="form-group">
      <label for="string-input">普通输入 (字符串类型):</label>
      <input 
        id="string-input" 
        v-model="stringInput" 
        type="number"
        placeholder="输入数字"
      >
      <p>值: {{ stringInput }}</p>
      <p>类型: {{ typeof stringInput }}</p>
      <p>计算结果: {{ stringInput + 10 }}</p>
    </div>
    
    <!-- .number 修饰符 -->
    <div class="form-group">
      <label for="number-input">数字输入 (数字类型):</label>
      <input 
        id="number-input" 
        v-model.number="numberInput" 
        type="number"
        placeholder="输入数字"
      >
      <p>值: {{ numberInput }}</p>
      <p>类型: {{ typeof numberInput }}</p>
      <p>计算结果: {{ numberInput + 10 }}</p>
    </div>
    
    <!-- 价格计算器 -->
    <div class="form-group">
      <label for="price">单价:</label>
      <input 
        id="price" 
        v-model.number="price" 
        type="number"
        step="0.01"
        placeholder="0.00"
      >
      
      <label for="quantity">数量:</label>
      <input 
        id="quantity" 
        v-model.number="quantity" 
        type="number"
        placeholder="1"
      >
      
      <p>总价: {{ totalPrice }}</p>
      <p>类型检查 - 价格: {{ typeof price }}, 数量: {{ typeof quantity }}</p>
    </div>
    
    <!-- 年龄验证 -->
    <div class="form-group">
      <label for="age">年龄:</label>
      <input 
        id="age" 
        v-model.number="age" 
        type="number"
        min="1"
        max="150"
        placeholder="输入年龄"
      >
      
      <p v-if="age" class="validation">
        <span v-if="isValidAge">✅ 有效年龄</span>
        <span v-else>❌ 无效年龄 (1-150)</span>
      </p>
      
      <p>当前年龄: {{ age }}, 类型: {{ typeof age }}</p>
    </div>
    
    <!-- 数值范围选择 -->
    <div class="form-group">
      <label for="range">范围值 (1-100):</label>
      <input 
        id="range" 
        v-model.number="rangeValue" 
        type="range"
        min="1"
        max="100"
      >
      <p>当前值: {{ rangeValue }} (类型: {{ typeof rangeValue }})</p>
      
      <div class="progress-bar">
        <div 
          class="progress-fill" 
          :style="{ width: rangeValue + '%' }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const stringInput = ref('')
const numberInput = ref(0)

const price = ref(0)
const quantity = ref(1)

const age = ref(null)

const rangeValue = ref(50)

const totalPrice = computed(() => {
  return (price.value * quantity.value).toFixed(2)
})

const isValidAge = computed(() => {
  return age.value >= 1 && age.value <= 150
})
</script>

<style>
.form-group {
  margin-bottom: 25px;
  padding: 15px;
  border: 1px solid #eee;
  border-radius: 4px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  margin-bottom: 10px;
}

input:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

input[type="range"] {
  margin: 10px 0;
}

p {
  margin: 5px 0;
  color: #666;
}

.validation {
  font-weight: bold;
  margin: 10px 0;
}

.validation span {
  padding: 5px 10px;
  border-radius: 4px;
}

.validation span:first-child {
  color: #28a745;
  background-color: #d4edda;
}

.validation span:last-child {
  color: #dc3545;
  background-color: #f8d7da;
}

.progress-bar {
  width: 100%;
  height: 20px;
  background-color: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  margin: 10px 0;
}

.progress-fill {
  height: 100%;
  background-color: #42b983;
  transition: width 0.3s ease;
}
</style>
```

### 3. .trim - 去除空格

```vue
<template>
  <div>
    <!-- 普通输入 -->
    <div class="form-group">
      <label for="normal-input">普通输入:</label>
      <input 
        id="normal-input" 
        v-model="normalInput" 
        placeholder="输入内容（包含空格）"
      >
      <p>输入内容: "{{ normalInput }}"</p>
      <p>长度: {{ normalInput.length }}</p>
      <p>去除空格后: "{{ normalInput.trim() }}"</p>
    </div>
    
    <!-- .trim 修饰符 -->
    <div class="form-group">
      <label for="trim-input">去除空格输入:</label>
      <input 
        id="trim-input" 
        v-model.trim="trimInput" 
        placeholder="输入内容（自动去除首尾空格）"
      >
      <p>输入内容: "{{ trimInput }}"</p>
      <p>长度: {{ trimInput.length }}</p>
    </div>
    
    <!-- 用户名验证 -->
    <div class="form-group">
      <label for="username">用户名:</label>
      <input 
        id="username" 
        v-model.trim="username" 
        placeholder="输入用户名"
      >
      
      <div class="validation">
        <p v-if="username.length === 0">请输入用户名</p>
        <p v-else-if="username.length < 3">用户名至少3个字符</p>
        <p v-else-if="username.length > 20">用户名最多20个字符</p>
        <p v-else class="success">✅ 用户名格式正确</p>
      </div>
      
      <p>用户名: "{{ username }}" (长度: {{ username.length }})</p>
    </div>
    
    <!-- 邮箱验证 -->
    <div class="form-group">
      <label for="email">邮箱:</label>
      <input 
        id="email" 
        v-model.trim="email" 
        type="email"
        placeholder="输入邮箱地址"
      >
      
      <div class="validation">
        <p v-if="email.length === 0">请输入邮箱</p>
        <p v-else-if="!isValidEmail">❌ 邮箱格式不正确</p>
        <p v-else class="success">✅ 邮箱格式正确</p>
      </div>
      
      <p>邮箱: "{{ email }}"</p>
    </div>
    
    <!-- 密码确认 -->
    <div class="form-group">
      <label for="password">密码:</label>
      <input 
        id="password" 
        v-model.trim="password" 
        type="password"
        placeholder="输入密码"
      >
      
      <label for="confirm-password">确认密码:</label>
      <input 
        id="confirm-password" 
        v-model.trim="confirmPassword" 
        type="password"
        placeholder="再次输入密码"
      >
      
      <div class="validation">
        <p v-if="password.length === 0">请输入密码</p>
        <p v-else-if="password.length < 6">密码至少6个字符</p>
        <p v-else-if="confirmPassword.length === 0">请确认密码</p>
        <p v-else-if="password !== confirmPassword">❌ 两次输入的密码不一致</p>
        <p v-else class="success">✅ 密码设置成功</p>
      </div>
    </div>
    
    <!-- 标签输入 -->
    <div class="form-group">
      <label for="tags">标签 (用逗号分隔):</label>
      <input 
        id="tags" 
        v-model.trim="tagsInput" 
        placeholder="输入标签，用逗号分隔"
        @keyup.enter="addTags"
      >
      <button @click="addTags">添加标签</button>
      
      <div v-if="tags.length > 0" class="tags-container">
        <span v-for="(tag, index) in tags" :key="index" class="tag">
          {{ tag }}
          <button @click="removeTag(index)" class="remove-tag">×</button>
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const normalInput = ref('')
const trimInput = ref('')

const username = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')

const tagsInput = ref('')
const tags = ref([])

const isValidEmail = computed(() => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email.value)
})

function addTags() {
  if (tagsInput.value.trim()) {
    const newTags = tagsInput.value
      .split(',')
      .map(tag => tag.trim())
      .filter(tag => tag && !tags.value.includes(tag))
    
    tags.value.push(...newTags)
    tagsInput.value = ''
  }
}

function removeTag(index) {
  tags.value.splice(index, 1)
}
</script>

<style>
.form-group {
  margin-bottom: 25px;
  padding: 15px;
  border: 1px solid #eee;
  border-radius: 4px;
}

.form-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  color: #333;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  margin-bottom: 10px;
}

input:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

button {
  padding: 8px 15px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #3aa876;
}

p {
  margin: 5px 0;
  color: #666;
}

.validation {
  margin: 10px 0;
}

.validation p {
  margin: 5px 0;
  padding: 5px 10px;
  border-radius: 4px;
}

.validation .success {
  color: #28a745;
  background-color: #d4edda;
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.tag {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 5px 10px;
  background-color: #e3f2fd;
  color: #1976d2;
  border-radius: 15px;
  font-size: 14px;
}

.remove-tag {
  background: none;
  border: none;
  color: #1976d2;
  cursor: pointer;
  padding: 0;
  margin: 0;
  font-size: 16px;
  font-weight: bold;
}

.remove-tag:hover {
  color: #0d47a1;
}
</style>
```

## 🎯 表单验证

### 1. 基本表单验证

```vue
<template>
  <div>
    <h2>用户注册表单</h2>
    
    <form @submit.prevent="handleSubmit" class="registration-form">
      <!-- 用户名 -->
      <div class="form-group">
        <label for="reg-username">用户名:</label>
        <input 
          id="reg-username" 
          v-model.trim="form.username" 
          type="text"
          :class="{ 'error': errors.username }"
          placeholder="3-20个字符"
        >
        <span v-if="errors.username" class="error-message">{{ errors.username }}</span>
      </div>
      
      <!-- 邮箱 -->
      <div class="form-group">
        <label for="reg-email">邮箱:</label>
        <input 
          id="reg-email" 
          v-model.trim="form.email" 
          type="email"
          :class="{ 'error': errors.email }"
          placeholder="example@email.com"
        >
        <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
      </div>
      
      <!-- 密码 -->
      <div class="form-group">
        <label for="reg-password">密码:</label>
        <input 
          id="reg-password" 
          v-model.trim="form.password" 
          type="password"
          :class="{ 'error': errors.password }"
          placeholder="至少6个字符"
        >
        <span v-if="errors.password" class="error-message">{{ errors.password }}</span>
      </div>
      
      <!-- 确认密码 -->
      <div class="form-group">
        <label for="reg-confirm-password">确认密码:</label>
        <input 
          id="reg-confirm-password" 
          v-model.trim="form.confirmPassword" 
          type="password"
          :class="{ 'error': errors.confirmPassword }"
          placeholder="再次输入密码"
        >
        <span v-if="errors.confirmPassword" class="error-message">{{ errors.confirmPassword }}</span>
      </div>
      
      <!-- 年龄 -->
      <div class="form-group">
        <label for="reg-age">年龄:</label>
        <input 
          id="reg-age" 
          v-model.number="form.age" 
          type="number"
          :class="{ 'error': errors.age }"
          placeholder="18-100"
        >
        <span v-if="errors.age" class="error-message">{{ errors.age }}</span>
      </div>
      
      <!-- 性别 -->
      <div class="form-group">
        <label>性别:</label>
        <div class="radio-group">
          <label>
            <input type="radio" v-model="form.gender" value="male">
            男
          </label>
          <label>
            <input type="radio" v-model="form.gender" value="female">
            女
          </label>
        </div>
        <span v-if="errors.gender" class="error-message">{{ errors.gender }}</span>
      </div>
      
      <!-- 兴趣爱好 -->
      <div class="form-group">
        <label>兴趣爱好:</label>
        <div class="checkbox-group">
          <label v-for="hobby in hobbies" :key="hobby.value">
            <input 
              type="checkbox" 
              v-model="form.hobbies"
              :value="hobby.value"
            >
            {{ hobby.label }}
          </label>
        </div>
        <span v-if="errors.hobbies" class="error-message">{{ errors.hobbies }}</span>
      </div>
      
      <!-- 服务条款 -->
      <div class="form-group">
        <label>
          <input type="checkbox" v-model="form.agreeTerms">
          我同意服务条款和隐私政策
        </label>
        <span v-if="errors.agreeTerms" class="error-message">{{ errors.agreeTerms }}</span>
      </div>
      
      <!-- 提交按钮 -->
      <button type="submit" :disabled="isSubmitting" class="submit-btn">
        {{ isSubmitting ? '提交中...' : '注册' }}
      </button>
    </form>
    
    <!-- 提交结果 -->
    <div v-if="submitResult" class="result">
      <h3>提交结果:</h3>
      <pre>{{ JSON.stringify(submitResult, null, 2) }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const form = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: null,
  gender: '',
  hobbies: [],
  agreeTerms: false
})

const errors = reactive({})
const isSubmitting = ref(false)
const submitResult = ref(null)

const hobbies = ref([
  { value: 'reading', label: '阅读' },
  { value: 'music', label: '音乐' },
  { value: 'sports', label: '运动' },
  { value: 'travel', label: '旅行' }
])

// 验证规则
const validationRules = {
  username: {
    required: true,
    minLength: 3,
    maxLength: 20,
    pattern: /^[a-zA-Z0-9_]+$/,
    message: '用户名必须为3-20个字符，只能包含字母、数字和下划线'
  },
  email: {
    required: true,
    pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
    message: '请输入有效的邮箱地址'
  },
  password: {
    required: true,
    minLength: 6,
    message: '密码至少6个字符'
  },
  confirmPassword: {
    required: true,
    custom: (value) => value === form.password,
    message: '两次输入的密码不一致'
  },
  age: {
    required: true,
    min: 18,
    max: 100,
    message: '年龄必须在18-100之间'
  },
  gender: {
    required: true,
    message: '请选择性别'
  },
  hobbies: {
    required: true,
    minLength: 1,
    message: '至少选择一个兴趣爱好'
  },
  agreeTerms: {
    required: true,
    custom: (value) => value === true,
    message: '必须同意服务条款'
  }
}

// 验证函数
function validateField(field, value) {
  const rule = validationRules[field]
  if (!rule) return null
  
  // 必填验证
  if (rule.required && (!value || (Array.isArray(value) && value.length === 0))) {
    return rule.message
  }
  
  // 最小长度
  if (rule.minLength && value && value.length < rule.minLength) {
    return rule.message
  }
  
  // 最大长度
  if (rule.maxLength && value && value.length > rule.maxLength) {
    return rule.message
  }
  
  // 最小值
  if (rule.min !== undefined && value < rule.min) {
    return rule.message
  }
  
  // 最大值
  if (rule.max !== undefined && value > rule.max) {
    return rule.message
  }
  
  // 正则表达式
  if (rule.pattern && value && !rule.pattern.test(value)) {
    return rule.message
  }
  
  // 自定义验证
  if (rule.custom && !rule.custom(value)) {
    return rule.message
  }
  
  return null
}

// 验证整个表单
function validateForm() {
  const newErrors = {}
  
  // 清空之前的错误
  Object.keys(errors).forEach(key => {
    delete errors[key]
  })
  
  // 验证每个字段
  Object.keys(validationRules).forEach(field => {
    const error = validateField(field, form[field])
    if (error) {
      newErrors[field] = error
    }
  })
  
  // 更新错误对象
  Object.assign(errors, newErrors)
  
  return Object.keys(newErrors).length === 0
}

// 实时验证
function validateOnInput(field) {
  const error = validateField(field, form[field])
  if (error) {
    errors[field] = error
  } else {
    delete errors[field]
  }
}

// 提交表单
async function handleSubmit() {
  if (!validateForm()) {
    alert('请修正表单错误')
    return
  }
  
  isSubmitting.value = true
  
  try {
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 2000))
    
    submitResult.value = {
      success: true,
      data: { ...form },
      timestamp: new Date().toISOString()
    }
    
    alert('注册成功！')
    
    // 重置表单
    Object.keys(form).forEach(key => {
      if (Array.isArray(form[key])) {
        form[key] = []
      } else if (typeof form[key] === 'boolean') {
        form[key] = false
      } else {
        form[key] = ''
      }
    })
    
    Object.keys(errors).forEach(key => {
      delete errors[key]
    })
    
  } catch (error) {
    submitResult.value = {
      success: false,
      error: error.message
    }
    alert('注册失败，请重试')
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style>
.registration-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
  color: #333;
}

.form-group input[type="text"],
.form-group input[type="email"],
.form-group input[type="password"],
.form-group input[type="number"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.form-group input.error {
  border-color: #dc3545;
  box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.1);
}

.form-group input:focus {
  outline: none;
  border-color: #42b983;
  box-shadow: 0 0 0 2px rgba(66, 185, 131, 0.1);
}

.radio-group,
.checkbox-group {
  display: flex;
  gap: 15px;
  margin-top: 5px;
}

.radio-group label,
.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 5px;
  font-weight: normal;
}

.error-message {
  display: block;
  margin-top: 5px;
  color: #dc3545;
  font-size: 12px;
}

.submit-btn {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-btn:hover:not(:disabled) {
  background-color: #3aa876;
}

.submit-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.result {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

.result pre {
  margin: 0;
  white-space: pre-wrap;
  word-break: break-all;
}
</style>
```

## 🎯 总结

Vue3表单处理提供了强大而灵活的数据绑定和验证能力：

### 核心概念：

1. **v-model双向绑定**：
   - 基本输入框：`<input v-model="message">`
   - 多行文本：`<textarea v-model="description">`
   - 单选按钮：`<input type="radio" v-model="gender">`
   - 复选框：`<input type="checkbox" v-model="checked">`
   - 选择框：`<select v-model="selected">`

2. **v-model修饰符**：
   - `.lazy`：延迟更新（失去焦点时）
   - `.number`：自动转换为数字类型
   - `.trim`：自动去除首尾空格

3. **表单验证**：
   - 实时验证
   - 提交验证
   - 错误提示
   - 自定义验证规则

### 最佳实践：

1. **数据结构**：
   - 使用`reactive`管理复杂表单数据
   - 合理组织表单字段结构
   - 考虑嵌套对象和数组

2. **用户体验**：
   - 提供实时验证反馈
   - 合理的错误提示
   - 防止重复提交

3. **性能优化**：
   - 使用`.lazy`减少不必要的更新
   - 合理使用计算属性
   - 避免过度验证

记住：**表单是用户交互的重要入口，良好的表单处理能大大提升用户体验和数据质量！** 🚀
