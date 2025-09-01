# JavaScript入门基础

# JavaScript入门基础

## 🌟 什么是JavaScript？

JavaScript是一种轻量级的编程语言，最初是为了给网页增加交互性而设计的。现在它已经成为Web开发的核心技术之一，与HTML和CSS一起构成了现代Web开发的三大支柱。

### JavaScript的特点
- **解释型语言**：不需要编译，浏览器直接执行
- **动态类型**：变量类型在运行时确定
- **弱类型**：变量可以自动转换类型
- **基于原型**：支持原型继承
- **函数式编程**：函数是一等公民

## 📝 基本语法

### 1. 注释

```javascript
// 单行注释

/*
  多行注释
  可以写多行
*/
```

### 2. 变量声明

JavaScript有三种声明变量的方式：

```javascript
// 1. var (旧方式，函数作用域)
var name = "张三";
var age = 25;

// 2. let (ES6，块级作用域)
let city = "北京";
let count = 100;

// 3. const (ES6，常量，块级作用域)
const PI = 3.14159;
const country = "中国";
```

**区别：**
- `var`：函数作用域，可以重复声明，存在变量提升
- `let`：块级作用域，不能重复声明，不存在变量提升
- `const`：块级作用域，声明时必须初始化，不能重新赋值

### 3. 数据类型

JavaScript有7种基本数据类型和1种复杂数据类型：

#### 基本数据类型

```javascript
// 1. Number (数字)
let integer = 42;
let float = 3.14;
let negative = -10;

// 2. String (字符串)
let singleQuote = 'Hello';
let doubleQuote = "World";
let template = `你好，${singleQuote}!`; // 模板字符串

// 3. Boolean (布尔值)
let isTrue = true;
let isFalse = false;

// 4. Undefined (未定义)
let undefinedVar;
console.log(undefinedVar); // undefined

// 5. Null (空值)
let nullVar = null;

// 6. Symbol (符号，ES6)
let symbol = Symbol('id');

// 7. BigInt (大整数，ES2020)
let bigNumber = 9007199254740991n;
```

#### 复杂数据类型

```javascript
// Object (对象)
let person = {
    name: "李四",
    age: 30,
    city: "上海"
};

// Array (数组)
let numbers = [1, 2, 3, 4, 5];
let fruits = ["苹果", "香蕉", "橙子"];

// Function (函数)
function greet(name) {
    return `你好，${name}！`;
}
```

### 4. 类型检查

```javascript
// typeof 操作符
console.log(typeof 42);           // "number"
console.log(typeof "hello");      // "string"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof null);         // "object" (这是个历史遗留问题)
console.log(typeof {});           // "object"
console.log(typeof []);           // "object"
console.log(typeof function(){}); // "function"
```

## 🔤 运算符

### 1. 算术运算符

```javascript
let a = 10;
let b = 3;

console.log(a + b);  // 13 (加法)
console.log(a - b);  // 7  (减法)
console.log(a * b);  // 30 (乘法)
console.log(a / b);  // 3.333... (除法)
console.log(a % b);  // 1  (取余)
console.log(a ** b); // 1000 (幂运算)

// 自增自减
let count = 5;
count++;  // 6
count--;  // 5
++count;  // 6
--count;  // 5
```

### 2. 比较运算符

```javascript
let x = 5;
let y = '5';

console.log(x == y);   // true (值相等)
console.log(x === y);  // false (值和类型都相等)
console.log(x != y);   // false (值不相等)
console.log(x !== y);  // true (值或类型不相等)
console.log(x > 3);    // true
console.log(x < 10);   // true
console.log(x >= 5);   // true
console.log(x <= 5);   // true
```

### 3. 逻辑运算符

```javascript
let p = true;
let q = false;

console.log(p && q);  // false (逻辑与)
console.log(p || q);  // true (逻辑或)
console.log(!p);      // false (逻辑非)

// 短路运算
let result = p && "Hello";  // "Hello"
let result2 = q || "World"; // "World"
```

### 4. 位运算符

```javascript
let a = 5;  // 二进制: 0101
let b = 3;  // 二进制: 0011

console.log(a & b);   // 1 (按位与)
console.log(a | b);   // 7 (按位或)
console.log(a ^ b);   // 6 (按位异或)
console.log(~a);      // -6 (按位非)
console.log(a << 1);  // 10 (左移)
console.log(a >> 1);  // 2 (右移)
```

## 📤 输入输出

### 1. 输出方式

```javascript
// 1. console.log() - 控制台输出
console.log("Hello World!");
console.log("变量值:", x);
console.table([1, 2, 3]); // 表格形式输出

// 2. alert() - 弹窗输出
alert("这是一个警告框");

// 3. document.write() - 文档输出
document.write("<h1>这是标题</h1>");

// 4. 操作DOM元素
document.getElementById("demo").innerHTML = "Hello JavaScript!";
```

### 2. 输入方式

```javascript
// 1. prompt() - 弹窗输入
let name = prompt("请输入你的名字:");
console.log("你好,", name);

// 2. confirm() - 确认框
let isConfirmed = confirm("你确定要删除吗？");
console.log("用户选择:", isConfirmed);

// 3. 表单输入
// HTML: <input type="text" id="userInput">
let inputValue = document.getElementById("userInput").value;
```

## 💡 最佳实践

### 1. 变量命名规范

```javascript
// ✅ 好的命名
let userName = "张三";
let userAge = 25;
let isLoggedIn = true;
let maxCount = 100;

// ❌ 不好的命名
let x = "张三";
let a = 25;
let flag = true;
let max = 100;
```

### 2. 常量命名

```javascript
// 常量使用大写字母和下划线
const MAX_LENGTH = 100;
const API_URL = "https://api.example.com";
const PI = 3.14159;
```

### 3. 代码风格

```javascript
// 使用分号结束语句
let name = "张三";

// 使用适当的缩进
function calculate(a, b) {
    let sum = a + b;
    return sum;
}

// 使用空格增强可读性
let result = (a + b) * (c - d);
```

## 🎯 练习

### 练习1：变量操作
```javascript
// 声明变量并计算
let price = 100;
let quantity = 5;
let total = price * quantity;
console.log("总价:", total);
```

### 练习2：类型转换
```javascript
// 字符串转数字
let str = "123";
let num = Number(str);
console.log(num + 1); // 124

// 数字转字符串
let num2 = 456;
let str2 = String(num2);
console.log(str2 + "7"); // "4567"
```

### 练习3：简单计算器
```javascript
// 简单计算器
let num1 = 10;
let num2 = 5;

console.log("加法:", num1 + num2);
console.log("减法:", num1 - num2);
console.log("乘法:", num1 * num2);
console.log("除法:", num1 / num2);
console.log("取余:", num1 % num2);
```

## 📋 总结

JavaScript的基础语法包括：
- 变量声明：`var`、`let`、`const`
- 数据类型：数字、字符串、布尔值、undefined、null、symbol、bigint、对象
- 运算符：算术、比较、逻辑、位运算
- 输入输出：console.log、alert、prompt等
- 代码规范：命名规范、代码风格

掌握这些基础知识是学习JavaScript的第一步，后续我们将学习更复杂的流程控制、函数、对象等概念。

**记住：编程是一门实践性很强的技能，多写代码、多练习才能真正掌握！** 🚀
