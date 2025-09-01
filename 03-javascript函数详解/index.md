# JavaScript函数详解

# JavaScript函数详解

## 🎯 什么是函数？

函数是JavaScript中的一等公民，它是可重复使用的代码块，可以接收输入参数，执行特定任务，并返回结果。函数是模块化编程的基础，让代码更加清晰、可维护。

### 函数的特点
- **可重用性**：一次定义，多次调用
- **模块化**：将复杂问题分解为小任务
- **封装性**：隐藏实现细节，只暴露接口
- **参数化**：通过参数接收不同的输入

## 📝 函数定义方式

### 1. 函数声明（Function Declaration）

```javascript
// 基本语法
function functionName(parameters) {
    // 函数体
    return result;
}

// 示例
function greet(name) {
    return `你好，${name}！`;
}

console.log(greet("张三")); // "你好，张三！"

// 函数声明提升（Hoisting）
sayHello(); // 可以在声明前调用

function sayHello() {
    console.log("Hello!");
}
```

### 2. 函数表达式（Function Expression）

```javascript
// 基本语法
const functionName = function(parameters) {
    // 函数体
    return result;
};

// 示例
const add = function(a, b) {
    return a + b;
};

console.log(add(3, 5)); // 8

// 函数表达式没有提升
// subtract(5, 3); // 报错：subtract is not defined

const subtract = function(a, b) {
    return a - b;
};
```

### 3. 箭头函数（Arrow Function，ES6）

```javascript
// 基本语法
const functionName = (parameters) => {
    // 函数体
    return result;
};

// 示例
const multiply = (a, b) => {
    return a * b;
};

console.log(multiply(4, 6)); // 24

// 简化语法
// 如果只有一个参数，可以省略括号
const square = x => x * x;

// 如果函数体只有一行返回语句，可以省略大括号和return
const divide = (a, b) => a / b;

// 无参数时使用空括号
const getRandom = () => Math.random();
```

### 4. 匿名函数

```javascript
// 作为回调函数
setTimeout(function() {
    console.log("1秒后执行");
}, 1000);

// 立即执行函数（IIFE）
(function() {
    console.log("立即执行");
})();

// 带参数的IIFE
(function(name) {
    console.log(`你好，${name}！`);
})("李四");
```

### 5. 构造函数（不推荐）

```javascript
// 使用Function构造函数（不推荐，有安全风险）
const add = new Function('a', 'b', 'return a + b');
console.log(add(2, 3)); // 5
```

## 🔧 函数参数

### 1. 参数默认值（ES6）

```javascript
// 传统方式
function greet(name) {
    name = name || "游客";
    return `你好，${name}！`;
}

// ES6默认值
function greetES6(name = "游客") {
    return `你好，${name}！`;
}

console.log(greetES6());           // "你好，游客！"
console.log(greetES6("王五"));     // "你好，王五！"

// 复杂默认值
function createUser(options = {}) {
    const defaults = {
        name: "匿名用户",
        age: 18,
        role: "user"
    };
    
    return { ...defaults, ...options };
}

console.log(createUser({ name: "赵六", age: 25 }));
// { name: "赵六", age: 25, role: "user" }
```

### 2. 剩余参数（Rest Parameters，ES6）

```javascript
// 收集所有参数
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum(10, 20, 30));    // 60

// 结合其他参数
function introduce(greeting, ...names) {
    names.forEach(name => {
        console.log(`${greeting}，${name}！`);
    });
}

introduce("你好", "张三", "李四", "王五");
```

### 3. 展开运算符（Spread Operator，ES6）

```javascript
// 展开数组作为参数
const numbers = [1, 2, 3, 4, 5];
console.log(Math.max(...numbers)); // 5

// 合并数组
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// 复制数组
const original = [1, 2, 3];
const copy = [...original];
console.log(copy); // [1, 2, 3]
```

### 4. 参数解构（ES6）

```javascript
// 对象参数解构
function createUser({ name, age = 18, city = "北京" }) {
    return `用户：${name}，年龄：${age}，城市：${city}`;
}

const user = { name: "张三", age: 25, city: "上海" };
console.log(createUser(user)); // "用户：张三，年龄：25，城市：上海"

// 数组参数解构
function getFirstAndSecond([first, second]) {
    return [first, second];
}

const numbers2 = [10, 20, 30, 40];
console.log(getFirstAndSecond(numbers2)); // [10, 20]
```

## 🔄 函数调用方式

### 1. 普通函数调用

```javascript
function sayHello(name) {
    console.log(`你好，${name}！`);
}

sayHello("世界"); // "你好，世界！"
```

### 2. 方法调用

```javascript
const person = {
    name: "张三",
    greet: function() {
        console.log(`你好，我是${this.name}！`);
    }
};

person.greet(); // "你好，我是张三！"
```

### 3. 构造函数调用

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    console.log(`你好，我是${this.name}，今年${this.age}岁。`);
};

const person1 = new Person("李四", 30);
person1.greet(); // "你好，我是李四，今年30岁。"
```

### 4. 间接调用（call、apply、bind）

```javascript
function introduce(greeting, punctuation) {
    console.log(`${greeting}，我是${this.name}${punctuation}`);
}

const person2 = { name: "王五" };

// call方法
introduce.call(person2, "大家好", "！"); // "大家好，我是王五！"

// apply方法
introduce.apply(person2, ["各位朋友", "。"]); // "各位朋友，我是王五。"

// bind方法
const boundIntroduce = introduce.bind(person2, "欢迎光临");
boundIntroduce("～"); // "欢迎光临，我是王五～"
```

## 📤 返回值

### 1. 基本返回值

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(3, 5)); // 8
```

### 2. 多个返回值

```javascript
// 返回数组
function getMinMax(numbers) {
    return [Math.min(...numbers), Math.max(...numbers)];
}

const [min, max] = getMinMax([1, 2, 3, 4, 5]);
console.log(min, max); // 1 5

// 返回对象
function getUserInfo() {
    return {
        name: "张三",
        age: 25,
        city: "北京"
    };
}

const { name, age } = getUserInfo();
console.log(name, age); // "张三" 25
```

### 3. 提前返回

```javascript
function validateUser(user) {
    // 参数验证
    if (!user) return "用户信息不能为空";
    if (!user.name) return "用户名不能为空";
    if (user.age < 0) return "年龄不能为负数";
    if (user.age > 150) return "年龄不能超过150";
    
    // 业务逻辑
    return "验证通过";
}

console.log(validateUser(null));                // "用户信息不能为空"
console.log(validateUser({ name: "" }));        // "用户名不能为空"
console.log(validateUser({ name: "张三", age: -5 })); // "年龄不能为负数"
console.log(validateUser({ name: "张三", age: 25 })); // "验证通过"
```

### 4. 隐式返回

```javascript
// 没有return语句的函数返回undefined
function noReturn() {
    console.log("这个函数没有return");
}

console.log(noReturn()); // undefined

// return后面没有值也返回undefined
function returnNothing() {
    return;
}

console.log(returnNothing()); // undefined
```

## 🏠 函数作用域

### 1. 全局作用域

```javascript
// 全局变量
let globalVar = "我是全局变量";

function showGlobal() {
    console.log(globalVar); // 可以访问全局变量
}

showGlobal(); // "我是全局变量"
console.log(globalVar); // "我是全局变量"
```

### 2. 函数作用域

```javascript
function testScope() {
    // 局部变量
    let localVar = "我是局部变量";
    console.log(localVar); // 可以访问
}

testScope(); // "我是局部变量"
// console.log(localVar); // 报错：localVar is not defined
```

### 3. 块级作用域（ES6）

```javascript
function testBlockScope() {
    if (true) {
        let blockVar = "我是块级变量";
        const blockConst = "我是块级常量";
        console.log(blockVar); // 可以访问
    }
    
    // console.log(blockVar);   // 报错：blockVar is not defined
    // console.log(blockConst); // 报错：blockConst is not defined
}

testBlockScope();
```

### 4. 闭包作用域

```javascript
function createCounter() {
    let count = 0;
    
    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

## 🎨 高级函数特性

### 1. 高阶函数

```javascript
// 函数作为参数
function operate(a, b, operation) {
    return operation(a, b);
}

function add(a, b) { return a + b; }
function multiply(a, b) { return a * b; }

console.log(operate(5, 3, add));      // 8
console.log(operate(5, 3, multiply)); // 15

// 函数作为返回值
function createMultiplier(factor) {
    return function(x) {
        return x * factor;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5));  // 10
console.log(triple(5));  // 15
```

### 2. 纯函数

```javascript
// 纯函数：相同输入总是产生相同输出，没有副作用
function pureAdd(a, b) {
    return a + b;
}

console.log(pureAdd(2, 3)); // 5
console.log(pureAdd(2, 3)); // 5

// 非纯函数：有副作用
let counter = 0;
function impureAdd(a, b) {
    counter++;
    return a + b;
}

console.log(impureAdd(2, 3)); // 5
console.log(counter);        // 1
console.log(impureAdd(2, 3)); // 5
console.log(counter);        // 2
```

### 3. 递归函数

```javascript
// 阶乘
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

console.log(factorial(5)); // 120

// 斐波那契数列
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(10)); // 55
```

### 4. 柯里化（Currying）

```javascript
// 柯里化：将多参数函数转换为单参数函数链
function curryAdd(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

console.log(curryAdd(1)(2)(3)); // 6

// 通用柯里化函数
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        }
        return function(...args2) {
            return curried.apply(this, args.concat(args2));
        };
    };
}

const add3 = (a, b, c) => a + b + c;
const curriedAdd = curry(add3);
console.log(curriedAdd(1)(2)(3)); // 6
console.log(curriedAdd(1, 2)(3)); // 6
```

## 🛠️ 内置函数方法

### 1. Function.prototype方法

```javascript
// call、apply、bind
function greet(greeting, punctuation) {
    console.log(`${greeting}，我是${this.name}${punctuation}`);
}

const person = { name: "张三" };

greet.call(person, "你好", "！");           // "你好，我是张三！"
greet.apply(person, ["大家好", "。"]);       // "大家好，我是张三。"
const boundGreet = greet.bind(person, "欢迎");
boundGreet("～");                            // "欢迎，我是张三～"
```

### 2. 函数属性

```javascript
function example() {
    console.log("示例函数");
}

// 函数名
console.log(example.name); // "example"

// 函数长度（参数个数）
console.log(example.length); // 0

// 函数原型
console.log(example.prototype); // {}
```

## 🎯 实际应用示例

### 1. 工具函数库

```javascript
// 字符串工具函数
const stringUtils = {
    // 首字母大写
    capitalize: function(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
    },
    
    // 反转字符串
    reverse: function(str) {
        return str.split('').reverse().join('');
    },
    
    // 驼峰转下划线
    camelToSnake: function(str) {
        return str.replace(/[A-Z]/g, letter => `_${letter.toLowerCase()}`);
    }
};

console.log(stringUtils.capitalize("hello")); // "Hello"
console.log(stringUtils.reverse("hello"));    // "olleh"
console.log(stringUtils.camelToSnake("helloWorld")); // "hello_world"
```

### 2. 数组处理函数

```javascript
// 数组工具函数
const arrayUtils = {
    // 去重
    unique: function(arr) {
        return [...new Set(arr)];
    },
    
    // 扁平化
    flatten: function(arr) {
        return arr.reduce((acc, val) => 
            Array.isArray(val) ? acc.concat(arrayUtils.flatten(val)) : acc.concat(val), 
        []);
    },
    
    // 分组
    groupBy: function(arr, key) {
        return arr.reduce((acc, obj) => {
            const group = obj[key];
            acc[group] = acc[group] || [];
            acc[group].push(obj);
            return acc;
        }, {});
    }
};

const numbers = [1, 2, 2, 3, 3, 3];
console.log(arrayUtils.unique(numbers)); // [1, 2, 3]

const nested = [1, [2, [3, [4]]]];
console.log(arrayUtils.flatten(nested)); // [1, 2, 3, 4]

const users = [
    { name: "张三", age: 25 },
    { name: "李四", age: 25 },
    { name: "王五", age: 30 }
];
console.log(arrayUtils.groupBy(users, 'age'));
/*
{
  25: [{ name: "张三", age: 25 }, { name: "李四", age: 25 }],
  30: [{ name: "王五", age: 30 }]
}
*/
```

### 3. 防抖和节流函数

```javascript
// 防抖函数
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// 节流函数
function throttle(func, limit) {
    let inThrottle;
    return function() {
        const args = arguments;
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// 使用示例
const searchInput = document.getElementById('search');
const searchDebounced = debounce((value) => {
    console.log('搜索:', value);
}, 300);

searchInput.addEventListener('input', (e) => {
    searchDebounced(e.target.value);
});
```

## 📋 最佳实践

### 1. 函数命名规范

```javascript
// ✅ 好的命名
function calculateTotal(price, quantity) { }
function getUserById(id) { }
function isValidEmail(email) { }
function convertToUpperCase(str) { }

// ❌ 不好的命名
function calc(p, q) { }
function getuser(id) { }
function check(email) { }
function upper(str) { }
```

### 2. 单一职责原则

```javascript
// ❌ 一个函数做太多事情
function processUserData(userData) {
    // 验证数据
    if (!userData.name) throw new Error('姓名不能为空');
    if (!userData.email) throw new Error('邮箱不能为空');
    
    // 格式化数据
    userData.name = userData.name.trim();
    userData.email = userData.email.toLowerCase();
    
    // 保存到数据库
    database.save(userData);
    
    // 发送邮件
    emailService.sendWelcomeEmail(userData.email);
    
    // 返回结果
    return { success: true, message: '用户创建成功' };
}

// ✅ 拆分为多个函数
function validateUserData(userData) {
    if (!userData.name) throw new Error('姓名不能为空');
    if (!userData.email) throw new Error('邮箱不能为空');
}

function formatUserData(userData) {
    userData.name = userData.name.trim();
    userData.email = userData.email.toLowerCase();
    return userData;
}

function saveUserData(userData) {
    return database.save(userData);
}

function sendWelcomeEmail(email) {
    emailService.sendWelcomeEmail(email);
}

function processUserDataImproved(userData) {
    validateUserData(userData);
    const formattedData = formatUserData(userData);
    saveUserData(formattedData);
    sendWelcomeEmail(formattedData.email);
    return { success: true, message: '用户创建成功' };
}
```

### 3. 使用纯函数

```javascript
// ✅ 纯函数
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

// ❌ 非纯函数（有副作用）
let total = 0;
function addToTotal(value) {
    total += value;
    return total;
}
```

## 🎯 总结

JavaScript函数是编程的核心概念，掌握函数的各种特性对于编写高质量的代码至关重要：

- **函数定义**：函数声明、函数表达式、箭头函数
- **参数处理**：默认值、剩余参数、展开运算符、解构
- **调用方式**：普通调用、方法调用、构造函数调用、间接调用
- **返回值**：基本返回、多返回值、提前返回
- **作用域**：全局作用域、函数作用域、块级作用域、闭包
- **高级特性**：高阶函数、纯函数、递归、柯里化
- **最佳实践**：命名规范、单一职责、使用纯函数

记住：**好的函数应该像一首诗——简洁、优雅、易于理解**！🚀
