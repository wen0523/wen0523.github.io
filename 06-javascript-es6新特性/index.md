# JavaScript ES6新特性

# JavaScript ES6新特性

## 🎯 ES6概述

ES6（ECMAScript 2015）是JavaScript语言的一次重大更新，引入了许多现代化的语法和特性，让JavaScript编程变得更加简洁、高效和可读。ES6的发布标志着JavaScript正式进入现代编程语言的时代。

### ES6的主要改进
- **语法简化**：更简洁的语法糖
- **模块化**：原生模块系统
- **异步处理**：Promise、async/await
- **面向对象**：类、继承、getter/setter
- **数据结构**：Map、Set、解构赋值
- **函数增强**：箭头函数、默认参数、剩余参数

## 🚀 let和const

### 1. let关键字

```javascript
// let声明的变量具有块级作用域
{
    let x = 10;
    console.log(x); // 10
}
// console.log(x); // ReferenceError: x is not defined

// 不能重复声明
let y = 20;
// let y = 30; // SyntaxError: Identifier 'y' has already been declared

// 没有变量提升
// console.log(z); // ReferenceError: Cannot access 'z' before initialization
let z = 30;

// for循环中的let
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i); // 0, 1, 2 (每个i都是独立的)
    }, 100);
}
```

### 2. const关键字

```javascript
// const声明常量，必须初始化
const PI = 3.14159;
console.log(PI); // 3.14159

// 不能重新赋值
// PI = 3.14; // TypeError: Assignment to constant variable

// 但对象和数组的内容可以修改
const person = { name: "张三" };
person.name = "李四"; // 可以修改属性
console.log(person); // { name: "李四" }

const numbers = [1, 2, 3];
numbers.push(4); // 可以添加元素
console.log(numbers); // [1, 2, 3, 4]

// 完全冻结对象
const frozen = Object.freeze({ name: "王五" });
// frozen.name = "赵六"; // 不会修改 (严格模式下会报错)
```

## 🏹 箭头函数

### 1. 基本语法

```javascript
// 基本箭头函数
const add = (a, b) => a + b;
console.log(add(2, 3)); // 5

// 无参数
const getRandom = () => Math.random();
console.log(getRandom());

// 单个参数（可以省略括号）
const square = x => x * x;
console.log(square(5)); // 25

// 多行函数体
const multiply = (a, b) => {
    const result = a * b;
    return result;
};
console.log(multiply(4, 6)); // 24
```

### 2. this绑定规则

```javascript
// 普通函数中的this
function Person() {
    this.age = 0;
    
    setInterval(function() {
        this.age++; // this指向window或undefined
        console.log(this.age);
    }, 1000);
}

const person = new Person(); // NaN (this.age是window.age)

// 箭头函数中的this
function Person2() {
    this.age = 0;
    
    setInterval(() => {
        this.age++; // this指向Person2实例
        console.log(this.age);
    }, 1000);
}

const person2 = new Person2(); // 1, 2, 3, ...

// 对象方法中的this
const calculator = {
    value: 0,
    
    // 普通方法
    add: function(n) {
        this.value += n;
        return this;
    },
    
    // 箭头函数方法
    multiply: (n) => {
        this.value *= n; // this不指向calculator
        return this;
    }
};

console.log(calculator.add(5).value); // 5
console.log(calculator.multiply(2)); // NaN (this.value是undefined)
```

## 📦 解构赋值

### 1. 数组解构

```javascript
// 基本数组解构
const numbers = [1, 2, 3, 4, 5];
const [a, b, c] = numbers;
console.log(a, b, c); // 1, 2, 3

// 跳过元素
const [first, , third] = numbers;
console.log(first, third); // 1, 3

// 默认值
const [x, y, z = 30] = [10, 20];
console.log(x, y, z); // 10, 20, 30

// 剩余参数
const [head, ...tail] = numbers;
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

// 交换变量
let m = 1, n = 2;
[m, n] = [n, m];
console.log(m, n); // 2, 1
```

### 2. 对象解构

```javascript
// 基本对象解构
const person = {
    name: "张三",
    age: 30,
    city: "北京"
};

const { name, age, city } = person;
console.log(name, age, city); // "张三" 30 "北京"

// 重命名
const { name: userName, age: userAge } = person;
console.log(userName, userAge); // "张三" 30

// 默认值
const { country = "中国" } = person;
console.log(country); // "中国"

// 嵌套解构
const user = {
    profile: {
        firstName: "李",
        lastName: "四"
    },
    contacts: {
        email: "lisi@example.com",
        phone: "13800138000"
    }
};

const { 
    profile: { firstName, lastName },
    contacts: { email }
} = user;

console.log(firstName, lastName, email); // "李" "四" "lisi@example.com"
```

### 3. 函数参数解构

```javascript
// 数组参数解构
function sum([a, b, c]) {
    return a + b + c;
}

console.log(sum([1, 2, 3])); // 6

// 对象参数解构
function greet({ name, age = 18 }) {
    return `你好，${name}！你今年${age}岁。`;
}

console.log(greet({ name: "王五", age: 25 })); // "你好，王五！你今年25岁。"
console.log(greet({ name: "赵六" })); // "你好，赵六！你今年18岁。"

// 复杂解构
function processUser({ 
    name, 
    profile: { age, city }, 
    contacts: { email }
}) {
    return `${name}(${age})来自${city}，邮箱：${email}`;
}

const userData = {
    name: "钱七",
    profile: { age: 28, city: "上海" },
    contacts: { email: "qianqi@example.com" }
};

console.log(processUser(userData)); // "钱七(28)来自上海，邮箱：qianqi@example.com"
```

## 🔄 模板字符串

### 1. 基本用法

```javascript
// 基本模板字符串
const name = "张三";
const age = 25;
const message = `你好，${name}！你今年${age}岁。`;
console.log(message); // "你好，张三！你今年25岁。"

// 多行字符串
const html = `
    <div class="user">
        <h2>${name}</h2>
        <p>年龄：${age}</p>
    </div>
`;
console.log(html);

// 表达式计算
const a = 10, b = 20;
const result = `${a} + ${b} = ${a + b}`;
console.log(result); // "10 + 20 = 30"

// 函数调用
function uppercase(str) {
    return str.toUpperCase();
}

const text = `转换为大写：${uppercase("hello world")}`;
console.log(text); // "转换为大写：HELLO WORLD"
```

### 2. 标签模板字符串

```javascript
// 标签模板字符串
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `<strong>${values[i]}</strong>` : '');
    }, '');
}

const name2 = "李四";
const age2 = 30;
const highlighted = highlight`我叫${name2}，今年${age2}岁`;
console.log(highlighted); // "我叫<strong>李四</strong>，今年<strong>30</strong>岁"

// 实际应用：SQL查询构建
function sql(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `'${values[i]}'` : '');
    }, '');
}

const tableName = "users";
const userId = 123;
const query = sql`SELECT * FROM ${tableName} WHERE id = ${userId}`;
console.log(query); // "SELECT * FROM 'users' WHERE id = '123'"
```

## 📊 扩展运算符

### 1. 数组扩展

```javascript
// 数组复制
const original = [1, 2, 3];
const copy = [...original];
console.log(copy); // [1, 2, 3]

// 数组合并
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4, 5, 6]

// 数组作为函数参数
function sum(a, b, c) {
    return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6

// Math.max应用
const max = Math.max(...numbers);
console.log(max); // 3
```

### 2. 对象扩展

```javascript
// 对象复制
const originalObj = { a: 1, b: 2 };
const copyObj = { ...originalObj };
console.log(copyObj); // { a: 1, b: 2 }

// 对象合并
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj); // { a: 1, b: 2, c: 3, d: 4 }

// 覆盖属性
const base = { a: 1, b: 2, c: 3 };
const override = { b: 20, d: 40 };
const result = { ...base, ...override };
console.log(result); // { a: 1, b: 20, c: 3, d: 40 }
```

## 🎨 新的数据结构

### 1. Map

```javascript
// 创建Map
const map = new Map();

// 设置键值对
map.set('name', '张三');
map.set('age', 30);
map.set(123, '数字键');
map.set({ id: 1 }, '对象键');

// 获取值
console.log(map.get('name')); // "张三"
console.log(map.get(123)); // "数字键"

// 检查键是否存在
console.log(map.has('name')); // true
console.log(map.has('nonexistent')); // false

// 删除键值对
map.delete('age');
console.log(map.has('age')); // false

// 获取大小
console.log(map.size); // 3

// 清空Map
map.clear();
console.log(map.size); // 0

// 遍历Map
const map2 = new Map([
    ['a', 1],
    ['b', 2],
    ['c', 3]
]);

// 使用for...of
for (const [key, value] of map2) {
    console.log(`${key}: ${value}`);
}

// 使用forEach
map2.forEach((value, key) => {
    console.log(`${key}: ${value}`);
});
```

### 2. Set

```javascript
// 创建Set
const set = new Set([1, 2, 3, 3, 4, 4, 5]);
console.log(set); // Set { 1, 2, 3, 4, 5 }

// 添加元素
set.add(6);
set.add(1); // 重复元素不会被添加
console.log(set); // Set { 1, 2, 3, 4, 5, 6 }

// 删除元素
set.delete(3);
console.log(set); // Set { 1, 2, 4, 5, 6 }

// 检查元素是否存在
console.log(set.has(2)); // true
console.log(set.has(3)); // false

// 获取大小
console.log(set.size); // 5

// 清空Set
set.clear();
console.log(set.size); // 0

// 数组去重
const duplicates = [1, 2, 2, 3, 3, 3, 4, 5, 5];
const unique = [...new Set(duplicates)];
console.log(unique); // [1, 2, 3, 4, 5]

// 交集、并集、差集
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([3, 4, 5, 6]);

const intersection = new Set([...setA].filter(x => setB.has(x)));
const union = new Set([...setA, ...setB]);
const difference = new Set([...setA].filter(x => !setB.has(x)));

console.log(intersection); // Set { 3, 4 }
console.log(union); // Set { 1, 2, 3, 4, 5, 6 }
console.log(difference); // Set { 1, 2 }
```

## 🔄 异步编程增强

### 1. Promise

```javascript
// 创建Promise
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve('操作成功！');
        } else {
            reject('操作失败！');
        }
    }, 1000);
});

// 使用Promise
promise
    .then(result => {
        console.log(result); // "操作成功！"
    })
    .catch(error => {
        console.error(error);
    })
    .finally(() => {
        console.log('Promise完成');
    });

// Promise.all
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results); // [1, 2, 3]
    });

// Promise.race
const slowPromise = new Promise(resolve => {
    setTimeout(() => resolve('慢'), 2000);
});

const fastPromise = new Promise(resolve => {
    setTimeout(() => resolve('快'), 1000);
});

Promise.race([slowPromise, fastPromise])
    .then(result => {
        console.log(result); // "快"
    });
```

### 2. Generator函数

```javascript
// 基本Generator函数
function* simpleGenerator() {
    yield 1;
    yield 2;
    yield 3;
    return 4;
}

const gen = simpleGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: 4, done: true }

// 带参数的Generator
function* counter(start = 0) {
    let count = start;
    while (true) {
        yield count++;
    }
}

const counterGen = counter(10);
console.log(counterGen.next().value); // 10
console.log(counterGen.next().value); // 11
console.log(counterGen.next().value); // 12

// 异步Generator的应用
function* fetchUsers() {
    const users = yield fetch('/api/users');
    const posts = yield fetch(`/api/posts?userId=${users[0].id}`);
    return { users, posts };
}
```

## 🎯 类和模块

### 1. 类（Class）

```javascript
// 基本类
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    // 实例方法
    greet() {
        return `你好，我是${this.name}！`;
    }

    // 静态方法
    static createAnonymous() {
        return new Person('匿名', 0);
    }
}

const person = new Person('张三', 30);
console.log(person.greet()); // "你好，我是张三！"
console.log(Person.createAnonymous()); // Person { name: '匿名', age: 0 }

// 继承
class Student extends Person {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }

    study() {
        return `${this.name}正在学习`;
    }

    // 重写父类方法
    greet() {
        return `${super.greet()} 我是一名学生。`;
    }
}

const student = new Student('李四', 20, '大三');
console.log(student.study()); // "李四正在学习"
console.log(student.greet()); // "你好，我是李四！ 我是一名学生。"

// Getter和Setter
class Circle {
    constructor(radius) {
        this.radius = radius;
    }

    get diameter() {
        return this.radius * 2;
    }

    set diameter(value) {
        this.radius = value / 2;
    }

    get area() {
        return Math.PI * this.radius ** 2;
    }
}

const circle = new Circle(5);
console.log(circle.diameter); // 10
console.log(circle.area); // 78.53981633974483

circle.diameter = 20;
console.log(circle.radius); // 10
```

### 2. 模块（Module）

```javascript
// math.js - 导出模块
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

export default class Calculator {
    constructor() {
        this.result = 0;
    }

    add(n) {
        this.result += n;
        return this;
    }

    multiply(n) {
        this.result *= n;
        return this;
    }

    getResult() {
        return this.result;
    }
}

// main.js - 导入模块
import Calculator, { PI, add, multiply } from './math.js';

console.log(PI); // 3.14159
console.log(add(2, 3)); // 5
console.log(multiply(4, 5)); // 20

const calc = new Calculator();
const result = calc.add(10).multiply(2).getResult();
console.log(result); // 20

// 动态导入
async function loadModule() {
    const module = await import('./math.js');
    console.log(module.add(1, 2)); // 3
}
```

## 🎨 其他新特性

### 1. 参数默认值

```javascript
// 基本默认值
function greet(name = '游客', age = 18) {
    return `你好，${name}！你今年${age}岁。`;
}

console.log(greet()); // "你好，游客！你今年18岁。"
console.log(greet('张三', 25)); // "你好，张三！你今年25岁。"

// 解构参数默认值
function createUser({ name = '匿名', age = 18, city = '北京' } = {}) {
    return { name, age, city };
}

console.log(createUser()); // { name: '匿名', age: 18, city: '北京' }
console.log(createUser({ name: '李四' })); // { name: '李四', age: 18, city: '北京' }

// 前面参数的默认值
function createProfile(name, options = {}) {
    const { age = 18, city = '北京' } = options;
    return { name, age, city };
}

console.log(createProfile('王五')); // { name: '王五', age: 18, city: '北京' }
console.log(createProfile('赵六', { age: 25 })); // { name: '赵六', age: 25, city: '北京' }
```

### 2. 剩余参数

```javascript
// 收集所有参数
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum(10, 20, 30)); // 60

// 与其他参数结合
function introduce(greeting, ...names) {
    names.forEach(name => {
        console.log(`${greeting}，${name}！`);
    });
}

introduce('你好', '张三', '李四', '王五');
// "你好，张三！"
// "你好，李四！"
// "你好，王五！"

// 箭头函数中的剩余参数
const multiply = (...factors) => {
    return factors.reduce((product, factor) => product * factor, 1);
};

console.log(multiply(2, 3, 4)); // 24
```

### 3. 对象字面量增强

```javascript
// 属性简写
const name = '张三';
const age = 30;

const person = {
    name,
    age,
    greet() {
        return `你好，我是${this.name}！`;
    }
};

console.log(person); // { name: '张三', age: 30, greet: [Function: greet] }

// 计算属性名
const dynamicKey = 'age';
const user = {
    ['first' + 'Name']: '李四',
    [dynamicKey]: 25
};

console.log(user); // { firstName: '李四', age: 25 }

// 方法简写
const calculator = {
    add(a, b) {
        return a + b;
    },
    multiply(a, b) {
        return a * b;
    }
};

console.log(calculator.add(5, 3)); // 8
console.log(calculator.multiply(4, 6)); // 24
```

### 4. Symbol

```javascript
// 创建Symbol
const id = Symbol('id');
const id2 = Symbol('id');

console.log(id === id2); // false (每个Symbol都是唯一的)

// 作为对象属性键
const user = {
    [id]: '12345',
    name: '张三',
    age: 30
};

console.log(user[id]); // '12345'
console.log(user.name); // '张三'

// 全局Symbol
const globalId = Symbol.for('globalId');
const globalId2 = Symbol.for('globalId');

console.log(globalId === globalId2); // true

// 获取Symbol描述
console.log(id.description); // 'id'
console.log(globalId.description); // 'globalId'

// 内置Symbol
const arr = [1, 2, 3];
arr[Symbol.iterator] = function*() {
    for (let i = this.length - 1; i >= 0; i--) {
        yield this[i];
    }
};

console.log([...arr]); // [3, 2, 1]
```

## 🎯 实际应用示例

### 1. 现代化的数据处理

```javascript
// 使用ES6特性处理数据
const users = [
    { id: 1, name: '张三', age: 25, city: '北京' },
    { id: 2, name: '李四', age: 30, city: '上海' },
    { id: 3, name: '王五', age: 28, city: '广州' }
];

// 使用解构和箭头函数
const userNames = users.map(({ name }) => name);
console.log(userNames); // ['张三', '李四', '王五']

// 使用filter和箭头函数
const youngUsers = users.filter(({ age }) => age < 30);
console.log(youngUsers); // [{ id: 1, name: '张三', age: 25, city: '北京' }, { id: 3, name: '王五', age: 28, city: '广州' }]

// 使用reduce和对象字面量增强
const usersByCity = users.reduce((acc, { city, ...user }) => {
    acc[city] = acc[city] || [];
    acc[city].push(user);
    return acc;
}, {});

console.log(usersByCity);
/*
{
  '北京': [{ id: 1, name: '张三', age: 25 }],
  '上海': [{ id: 2, name: '李四', age: 30 }],
  '广州': [{ id: 3, name: '王五', age: 28 }]
}
*/
```

### 2. 现代化的API设计

```javascript
// 使用ES6类设计API
class UserAPI {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.cache = new Map();
    }

    async getUser(id) {
        // 检查缓存
        if (this.cache.has(id)) {
            return this.cache.get(id);
        }

        try {
            const response = await fetch(`${this.baseUrl}/users/${id}`);
            const user = await response.json();
            
            // 缓存结果
            this.cache.set(id, user);
            return user;
        } catch (error) {
            console.error('获取用户失败:', error);
            throw error;
        }
    }

    async getUsers(filter = {}) {
        const params = new URLSearchParams(filter);
        const response = await fetch(`${this.baseUrl}/users?${params}`);
        return response.json();
    }

    async createUser(userData) {
        const response = await fetch(`${this.baseUrl}/users`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(userData)
        });
        return response.json();
    }

    clearCache() {
        this.cache.clear();
    }
}

// 使用API
const api = new UserAPI('https://api.example.com');

// 使用async/await
async function main() {
    try {
        const user = await api.getUser(1);
        console.log(user);
        
        const users = await api.getUsers({ age_min: 20, age_max: 30 });
        console.log(users);
        
        const newUser = await api.createUser({
            name: '新用户',
            age: 25,
            city: '深圳'
        });
        console.log(newUser);
    } catch (error) {
        console.error('API调用失败:', error);
    }
}

main();
```

## 📋 最佳实践

### 1. 代码风格

```javascript
// ✅ 使用const和let代替var
const name = '张三';
let age = 25;

// ✅ 使用箭头函数
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);

// ✅ 使用模板字符串
const message = `你好，${name}！你今年${age}岁。`;

// ✅ 使用解构赋值
const { firstName, lastName } = user;
const [first, second] = array;

// ✅ 使用对象字面量增强
const obj = {
    firstName,
    lastName,
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
};
```

### 2. 性能优化

```javascript
// ✅ 使用Map代替普通对象作为映射
const map = new Map();
map.set('key1', 'value1');
map.set('key2', 'value2');

// ✅ 使用Set进行快速查找
const set = new Set([1, 2, 3, 4, 5]);
console.log(set.has(3)); // true

// ✅ 使用展开运算符复制数组
const original = [1, 2, 3];
const copy = [...original];

// ✅ 使用Promise.all并行处理
const promises = [fetch('/api/1'), fetch('/api/2'), fetch('/api/3')];
Promise.all(promises).then(responses => {
    // 处理所有响应
});
```

### 3. 错误处理

```javascript
// ✅ 使用async/await的错误处理
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('获取数据失败:', error);
        throw error;
    }
}

// ✅ 使用Promise的错误处理
fetch('/api/data')
    .then(response => response.json())
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('请求失败:', error);
    });
```

## 🎯 总结

ES6为JavaScript带来了革命性的改进，让这门语言变得更加现代化和强大：

### 核心特性：
- **变量声明**：let、const的块级作用域
- **函数增强**：箭头函数、默认参数、剩余参数
- **解构赋值**：数组解构、对象解构、函数参数解构
- **字符串增强**：模板字符串、多行字符串、标签模板
- **数据结构**：Map、Set、Symbol
- **异步编程**：Promise、Generator函数
- **面向对象**：类、继承、getter/setter
- **模块系统**：import/export语法

### 实际应用：
- **代码简化**：更简洁的语法糖
- **性能提升**：更高效的内存管理
- **可读性增强**：更直观的代码结构
- **开发体验**：更好的开发工具支持

### 最佳实践：
- **使用现代语法**：优先使用ES6+特性
- **保持一致性**：在项目中统一代码风格
- **性能考虑**：合理使用新特性
- **向后兼容**：考虑目标环境的支持情况

记住：**ES6不仅仅是语法的改进，更是编程思想的革新。掌握ES6特性，让你的JavaScript代码更加优雅和高效！** 🚀
