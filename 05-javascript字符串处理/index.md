# JavaScript字符串处理

# JavaScript字符串处理

## 🎯 字符串概述

字符串是JavaScript中最常用的数据类型之一，用于表示文本数据。JavaScript中的字符串是不可变的，这意味着一旦创建，就不能修改字符串的内容，任何操作都会返回新的字符串。

### 字符串的特点
- **不可变性**：字符串创建后不能修改
- **索引访问**：可以通过索引访问单个字符
- **Unicode支持**：支持各种语言的字符
- **丰富的API**：提供很多实用的字符串方法

## 📝 字符串创建方式

### 1. 字符串字面量

```javascript
// 单引号
const singleQuote = 'Hello World';

// 双引号
const doubleQuote = "Hello World";

// 模板字符串（ES6）
const name = "张三";
const greeting = `你好，${name}！`;

// 多行字符串
const multiLine = `这是一个
多行字符串
可以包含换行符`;
```

### 2. String构造函数

```javascript
// 使用String构造函数
const str1 = new String("Hello");
const str2 = String("World");

// 注意：new String()返回的是对象，不是字符串
console.log(typeof str1); // "object"
console.log(typeof str2); // "string"
```

### 3. 转义字符

```javascript
const escapeExamples = {
    newline: "第一行\n第二行",
    tab: "列1\t列2\t列3",
    quote: "他说：\"你好！\"",
    backslash: "路径：C:\\Users\\Admin",
    unicode: "Unicode: \u03A9", // Ω
    hex: "十六进制: \x41"     // A
};

console.log(escapeExamples.newline);
console.log(escapeExamples.tab);
console.log(escapeExamples.quote);
console.log(escapeExamples.backslash);
console.log(escapeExamples.unicode);
console.log(escapeExamples.hex);
```

## 🔤 字符串基本操作

### 1. 访问字符

```javascript
const str = "Hello World";

// 使用charAt()
console.log(str.charAt(0));  // "H"
console.log(str.charAt(6));  // "W"

// 使用索引访问（ES5+）
console.log(str[0]);        // "H"
console.log(str[6]);        // "W"

// 访问不存在的索引
console.log(str.charAt(20)); // ""
console.log(str[20]);       // undefined
```

### 2. 获取字符串长度

```javascript
const text = "Hello 世界";
console.log(text.length); // 8 (包括空格和汉字)

// 注意：JavaScript中的字符串长度是按UTF-16编码计算的
const emoji = "👋";
console.log(emoji.length); // 2 (因为emoji需要两个UTF-16编码单元)
```

### 3. 字符串连接

```javascript
// 使用+运算符
const firstName = "张";
const lastName = "三";
const fullName = firstName + lastName;
console.log(fullName); // "张三"

// 使用+=运算符
let greeting = "Hello";
greeting += " World";
console.log(greeting); // "Hello World"

// 使用concat()方法
const str1 = "Hello";
const str2 = "World";
const result = str1.concat(" ", str2, "!");
console.log(result); // "Hello World!"

// 模板字符串（推荐）
const name = "李四";
const age = 25;
const intro = `我叫${name}，今年${age}岁`;
console.log(intro); // "我叫李四，今年25岁"
```

## 🔍 字符串搜索与匹配

### 1. 搜索子字符串

```javascript
const text = "Hello World, Hello JavaScript";

// indexOf() - 查找子字符串位置
console.log(text.indexOf("Hello"));     // 0
console.log(text.indexOf("World"));     // 6
console.log(text.indexOf("hello"));     // -1 (区分大小写)
console.log(text.indexOf("Hello", 6));  // 13 (从索引6开始查找)

// lastIndexOf() - 从后往前查找
console.log(text.lastIndexOf("Hello")); // 13

// includes() - 检查是否包含子字符串
console.log(text.includes("World"));    // true
console.log(text.includes("world"));    // false
console.log(text.includes("Hello", 6)); // true

// startsWith() - 检查是否以子字符串开头
console.log(text.startsWith("Hello"));  // true
console.log(text.startsWith("World"));  // false

// endsWith() - 检查是否以子字符串结尾
console.log(text.endsWith("JavaScript")); // true
console.log(text.endsWith("Hello"));     // false
```

### 2. 正则表达式匹配

```javascript
const text = "The quick brown fox jumps over the lazy dog";

// match() - 匹配正则表达式
const matches = text.match(/the/gi);
console.log(matches); // ["The", "the", "the"]

// search() - 搜索正则表达式位置
console.log(text.search(/fox/));      // 16
console.log(text.search(/cat/));      // -1

// test() - 测试是否匹配
const pattern = /fox/;
console.log(pattern.test(text));      // true
```

## ✂️ 字符串提取与修改

### 1. 提取子字符串

```javascript
const text = "Hello World";

// slice() - 提取子字符串
console.log(text.slice(0, 5));     // "Hello"
console.log(text.slice(6));        // "World"
console.log(text.slice(-5));       // "World"
console.log(text.slice(0, -6));    // "Hello"

// substring() - 提取子字符串（不支持负数）
console.log(text.substring(0, 5)); // "Hello"
console.log(text.substring(6));    // "World"

// substr() - 提取子字符串（已废弃）
console.log(text.substr(0, 5));    // "Hello"
console.log(text.substr(6, 5));   // "World"
```

### 2. 字符串修改

```javascript
// replace() - 替换子字符串
const text = "Hello World";
console.log(text.replace("World", "JavaScript")); // "Hello JavaScript"

// replace() 使用正则表达式（全局替换）
const text2 = "Hello World, Hello Universe";
console.log(text2.replace(/Hello/g, "Hi")); // "Hi World, Hi Universe"

// replaceAll() - 全局替换（ES2021）
console.log(text2.replaceAll("Hello", "Hi")); // "Hi World, Hi Universe"

// 大小写转换
const mixed = "Hello World";
console.log(mixed.toLowerCase()); // "hello world"
console.log(mixed.toUpperCase()); // "HELLO WORLD"

// 首字母大写
function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}
console.log(capitalize("hello")); // "Hello"

// 驼峰转换
function toCamelCase(str) {
    return str.replace(/[-_\s]+(.)?/g, (match, char) => char ? char.toUpperCase() : '');
}
console.log(toCamelCase("hello-world")); // "helloWorld"
console.log(toCamelCase("hello_world")); // "helloWorld"
```

### 3. 字符串填充

```javascript
// padStart() - 在开头填充字符
console.log("5".padStart(3, "0"));      // "005"
console.log("abc".padStart(6, "*"));    // "***abc"

// padEnd() - 在结尾填充字符
console.log("5".padEnd(3, "0"));        // "500"
console.log("abc".padEnd(6, "*"));      // "abc***"

// 格式化数字
function formatNumber(num, length = 2) {
    return num.toString().padStart(length, "0");
}
console.log(formatNumber(7));     // "07"
console.log(formatNumber(7, 4));  // "0007"
```

## 🧹 字符串清理与验证

### 1. 去除空白字符

```javascript
const messy = "  Hello World  \n\t";

// trim() - 去除两端空白
console.log(messy.trim()); // "Hello World"

// trimStart() / trimLeft() - 去除开头空白
console.log(messy.trimStart()); // "Hello World  \n\t"

// trimEnd() / trimRight() - 去除结尾空白
console.log(messy.trimEnd()); // "  Hello World"
```

### 2. 字符串分割

```javascript
// split() - 分割字符串为数组
const csv = "apple,banana,orange";
console.log(csv.split(",")); // ["apple", "banana", "orange"]

// 限制分割次数
const text = "a,b,c,d,e";
console.log(text.split(",", 3)); // ["a", "b", "c"]

// 使用正则表达式分割
const messyData = "apple, banana; orange|grape";
console.log(messyData.split(/[,;|]\s*/)); // ["apple", "banana", "orange", "grape"]

// 分割字符为单个字符数组
const str = "Hello";
console.log(str.split("")); // ["H", "e", "l", "l", "o"]
```

### 3. 字符串验证

```javascript
// 检查是否为空字符串
function isEmpty(str) {
    return str.trim().length === 0;
}

console.log(isEmpty(""));     // true
console.log(isEmpty("   "));  // true
console.log(isEmpty("Hello")); // false

// 检查是否为数字
function isNumeric(str) {
    return !isNaN(str) && !isNaN(parseFloat(str));
}

console.log(isNumeric("123"));    // true
console.log(isNumeric("12.3"));   // true
console.log(isNumeric("abc"));   // false
console.log(isNumeric("12a"));   // false

// 检查是否为有效邮箱
function isValidEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
}

console.log(isValidEmail("test@example.com"));    // true
console.log(isValidEmail("invalid-email"));        // false

// 检查是否为有效手机号
function isValidPhone(phone) {
    const phoneRegex = /^1[3-9]\d{9}$/;
    return phoneRegex.test(phone);
}

console.log(isValidPhone("13800138000"));   // true
console.log(isValidPhone("12345678901"));   // false
```

## 🎨 高级字符串操作

### 1. 字符串重复

```javascript
// repeat() - 重复字符串
console.log("Hello".repeat(3)); // "HelloHelloHello"

// 创建分隔线
function createLine(length = 20, char = "-") {
    return char.repeat(length);
}
console.log(createLine()); // "--------------------"
console.log(createLine(10, "=")); // "=========="

// 创建缩进
function createIndent(level = 1, spaces = 4) {
    return " ".repeat(level * spaces);
}
console.log(createIndent(2) + "Hello"); // "        Hello"
```

### 2. 字符串插值

```javascript
// 模板字符串的高级用法
const user = {
    name: "张三",
    age: 25,
    city: "北京"
};

// 多行模板字符串
const profile = `
用户信息：
姓名：${user.name}
年龄：${user.age}
城市：${user.city}
`;
console.log(profile);

// 带表达式的模板字符串
const price = 100;
const discount = 0.2;
const finalPrice = price * (1 - discount);

const message = `原价：¥${price}
折扣：${discount * 100}%
折后价：¥${finalPrice.toFixed(2)}`;
console.log(message);

// 标签模板字符串
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `<strong>${values[i]}</strong>` : '');
    }, '');
}

const name = "李四";
const age = 30;
const highlighted = highlight`我叫${name}，今年${age}岁`;
console.log(highlighted);
```

### 3. 字符串编码

```javascript
// 字符编码
const char = "A";
console.log(char.charCodeAt(0)); // 65

// 字符解码
console.log(String.fromCharCode(65)); // "A"

// Unicode转义
console.log("\u0041"); // "A"
console.log("\u4E2D\u6587"); // "中文"

// Base64编码/解码
const original = "Hello World";
const encoded = btoa(original);
const decoded = atob(encoded);

console.log(encoded); // "SGVsbG8gV29ybGQ="
console.log(decoded); // "Hello World"

// URL编码/解码
const url = "https://example.com/search?q=hello world";
const encoded = encodeURIComponent(url);
const decoded = decodeURIComponent(encoded);

console.log(encoded); // "https%3A%2F%2Fexample.com%2Fsearch%3Fq%3Dhello%20world"
console.log(decoded); // "https://example.com/search?q=hello world"
```

## 🛠️ 字符串工具函数

### 1. 格式化工具

```javascript
// 货币格式化
function formatCurrency(amount, currency = "CNY", locale = "zh-CN") {
    return new Intl.NumberFormat(locale, {
        style: "currency",
        currency: currency
    }).format(amount);
}

console.log(formatCurrency(1234.56)); // "¥1,234.56"
console.log(formatCurrency(1234.56, "USD", "en-US")); // "$1,234.56"

// 数字格式化
function formatNumber(number, locale = "zh-CN") {
    return new Intl.NumberFormat(locale).format(number);
}

console.log(formatNumber(1234567.89)); // "1,234,567.89"

// 百分比格式化
function formatPercentage(number, decimals = 2) {
    return (number * 100).toFixed(decimals) + "%";
}

console.log(formatPercentage(0.1234)); // "12.34%"

// 日期格式化
function formatDate(date, locale = "zh-CN") {
    return new Intl.DateTimeFormat(locale, {
        year: "numeric",
        month: "long",
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit"
    }).format(date);
}

console.log(formatDate(new Date())); // "2024年1月15日 下午3:30"
```

### 2. 文本处理工具

```javascript
// 截断文本
function truncateText(text, maxLength = 100, suffix = "...") {
    if (text.length <= maxLength) return text;
    return text.slice(0, maxLength - suffix.length) + suffix;
}

console.log(truncateText("这是一个很长的文本", 10)); // "这是一个很..."

// 提取文本摘要
function extractSummary(text, maxLength = 200) {
    // 移除HTML标签
    const plainText = text.replace(/<[^>]*>/g, "");
    // 截断文本
    return truncateText(plainText, maxLength);
}

const htmlText = "<p>这是一个<strong>HTML</strong>文本。</p>";
console.log(extractSummary(htmlText, 20)); // "这是一个HTML文本。"

// 文本高亮
function highlightText(text, keyword, highlightClass = "highlight") {
    if (!keyword) return text;
    const regex = new RegExp(`(${keyword})`, "gi");
    return text.replace(regex, `<span class="${highlightClass}">$1</span>`);
}

console.log(highlightText("Hello World", "world")); // "Hello <span class=\"highlight\">World</span>"

// 文本反转
function reverseText(text) {
    return text.split("").reverse().join("");
}

console.log(reverseText("Hello")); // "olleH"
console.log(reverseText("你好")); // "好你"

// 检查回文
function isPalindrome(text) {
    const cleaned = text.toLowerCase().replace(/[^a-z0-9]/g, "");
    return cleaned === cleaned.split("").reverse().join("");
}

console.log(isPalindrome("A man, a plan, a canal: Panama")); // true
console.log(isPalindrome("Hello")); // false
```

### 3. 字符串生成器

```javascript
// 生成随机字符串
function generateRandomString(length = 10) {
    const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    let result = "";
    for (let i = 0; i < length; i++) {
        result += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return result;
}

console.log(generateRandomString(8)); // 例如："xY7zK9pL"

// 生成UUID
function generateUUID() {
    return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0;
        const v = c === "x" ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}

console.log(generateUUID()); // 例如："f47ac10b-58cc-4372-a567-0e02b2c3d479"

// 生成唯一ID
function generateUniqueId(prefix = "") {
    const timestamp = Date.now().toString(36);
    const random = Math.random().toString(36).substr(2, 5);
    return prefix + timestamp + random;
}

console.log(generateUniqueId("user_")); // 例如："user_k1j2x3y4z5"
```

## 🎯 实际应用示例

### 1. URL参数处理

```javascript
// URL参数解析
function parseUrlParams(url) {
    const params = {};
    const queryString = url.split("?")[1];
    
    if (!queryString) return params;
    
    const pairs = queryString.split("&");
    for (const pair of pairs) {
        const [key, value] = pair.split("=");
        params[decodeURIComponent(key)] = decodeURIComponent(value || "");
    }
    
    return params;
}

const url = "https://example.com?name=张三&age=25&city=北京";
console.log(parseUrlParams(url));
// { name: "张三", age: "25", city: "北京" }

// 构建URL参数
function buildUrlParams(params) {
    const pairs = Object.entries(params)
        .map(([key, value]) => `${encodeURIComponent(key)}=${encodeURIComponent(value)}`)
        .join("&");
    return pairs;
}

const params = { name: "李四", age: 30, city: "上海" };
console.log(buildUrlParams(params)); // "name=%E6%9D%8E%E5%9B%9B&age=30&city=%E4%B8%8A%E6%B5%B7"
```

### 2. 文件路径处理

```javascript
// 获取文件扩展名
function getFileExtension(filename) {
    return filename.slice((filename.lastIndexOf(".") - 1 >>> 0) + 2);
}

console.log(getFileExtension("document.pdf")); // "pdf"
console.log(getFileExtension("archive.tar.gz")); // "gz"
console.log(getFileExtension("noextension")); // ""

// 获取文件名（不含扩展名）
function getFileName(filename) {
    const lastDot = filename.lastIndexOf(".");
    return lastDot === -1 ? filename : filename.slice(0, lastDot);
}

console.log(getFileName("document.pdf")); // "document"
console.log(getFileName("noextension")); // "noextension"

// 规范化路径
function normalizePath(path) {
    return path.replace(/\\/g, "/")
                .replace(/\/+/g, "/")
                .replace(/\/\.\//g, "/");
}

console.log(normalizePath("C:\\Users\\Admin//Documents/./file.txt")); // "C:/Users/Admin/Documents/file.txt"
```

### 3. 文本统计工具

```javascript
// 文本统计
function analyzeText(text) {
    const words = text.trim().split(/\s+/).filter(word => word.length > 0);
    const characters = text.replace(/\s/g, "");
    const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 0);
    const paragraphs = text.split(/\n\s*\n/).filter(p => p.trim().length > 0);
    
    return {
        wordCount: words.length,
        characterCount: characters.length,
        sentenceCount: sentences.length,
        paragraphCount: paragraphs.length,
        averageWordsPerSentence: words.length / sentences.length || 0,
        averageCharactersPerWord: characters.length / words.length || 0
    };
}

const sampleText = `这是一个示例文本。
它包含多个句子。每个句子都有不同的内容。

还有更多的段落可以分析。`;

console.log(analyzeText(sampleText));
/*
{
  wordCount: 15,
  characterCount: 45,
  sentenceCount: 3,
  paragraphCount: 2,
  averageWordsPerSentence: 5,
  averageCharactersPerWord: 3
}
*/
```

## 📋 最佳实践

### 1. 性能优化

```javascript
// ✅ 避免在循环中创建字符串
let result = "";
for (let i = 0; i < 1000; i++) {
    result += "x"; // 每次都创建新字符串
}

// ✅ 使用数组join()方法
const parts = [];
for (let i = 0; i < 1000; i++) {
    parts.push("x");
}
const result2 = parts.join("");

// ✅ 使用模板字符串而不是字符串连接
const name = "张三";
const age = 25;
// ❌ const message = "姓名：" + name + "，年龄：" + age;
// ✅ const message = `姓名：${name}，年龄：${age}`;
```

### 2. 国际化考虑

```javascript
// ✅ 使用Unicode字符而不是ASCII
const greeting = "你好，世界！"; // 而不是 "Hello World!"

// ✅ 考虑字符串长度计算的复杂性
function getRealLength(str) {
    return [...str].length; // 处理多字节字符
}

console.log(getRealLength("👋")); // 1
console.log("👋".length); // 2

// ✅ 使用Intl API进行本地化
const date = new Date();
console.log(date.toLocaleDateString("zh-CN")); // "2024/1/15"
console.log(date.toLocaleDateString("en-US")); // "1/15/2024"
```

### 3. 安全性考虑

```javascript
// ✅ 防止XSS攻击
function escapeHtml(text) {
    const div = document.createElement("div");
    div.textContent = text;
    return div.innerHTML;
}

console.log(escapeHtml("<script>alert('xss')</script>")); // "&lt;script&gt;alert('xss')&lt;/script&gt;"

// ✅ 验证用户输入
function sanitizeInput(input) {
    return input.trim()
                .replace(/[<>]/g, "") // 移除HTML标签字符
                .replace(/["']/g, ""); // 移除引号
}

console.log(sanitizeInput('  <script>alert("xss")</script>  ')); // "scriptalertxssscript"
```

## 🎯 总结

字符串处理是JavaScript编程中的基础技能，掌握各种字符串操作方法对于开发高质量的Web应用至关重要：

### 基础操作：
- **创建方式**：字面量、构造函数、模板字符串
- **基本操作**：访问字符、获取长度、连接字符串
- **搜索匹配**：indexOf、includes、正则表达式
- **提取修改**：slice、replace、大小写转换

### 高级特性：
- **模板字符串**：多行字符串、插值、标签模板
- **编码处理**：Unicode、Base64、URL编码
- **国际化**：Intl API、本地化格式化
- **性能优化**：避免频繁创建字符串、使用数组join

### 实用工具：
- **格式化工具**：货币、数字、百分比、日期格式化
- **文本处理**：截断、高亮、回文检查、文本统计
- **生成器**：随机字符串、UUID、唯一ID
- **验证工具**：邮箱、手机号、数字验证

### 最佳实践：
- **性能考虑**：使用模板字符串、避免循环中创建字符串
- **安全性**：HTML转义、输入验证、防止XSS攻击
- **国际化**：使用Unicode、考虑多字节字符、本地化

记住：**字符串处理看似简单，但细节决定成败。良好的字符串处理习惯能让你的代码更加健壮和高效！** 🚀
