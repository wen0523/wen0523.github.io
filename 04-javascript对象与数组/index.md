# JavaScript对象与数组

# JavaScript对象与数组

## 🎯 对象与数组概述

对象和数组是JavaScript中最重要的数据结构，它们让我们能够组织和操作复杂的数据。对象用于存储键值对，数组用于存储有序列表。

### 对象的特点
- **键值对存储**：使用属性名和值
- **无序性**：属性没有固定顺序
- **引用类型**：存储的是对象的引用
- **动态性**：可以动态添加或删除属性

### 数组的特点
- **有序性**：元素有固定的索引顺序
- **动态大小**：可以动态添加或删除元素
- **引用类型**：存储的是数组的引用
- **丰富的API**：提供很多有用的方法

## 📦 JavaScript对象

### 1. 对象创建方式

```javascript
// 1. 对象字面量（推荐）
const person = {
    name: "张三",
    age: 30,
    city: "北京"
};

// 2. 构造函数
function Person(name, age, city) {
    this.name = name;
    this.age = age;
    this.city = city;
}

const person2 = new Person("李四", 25, "上海");

// 3. Object.create()
const person3 = Object.create(null);
person3.name = "王五";
person3.age = 28;
person3.city = "广州";

// 4. ES6 Class
class PersonClass {
    constructor(name, age, city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
}

const person4 = new PersonClass("赵六", 32, "深圳");
```

### 2. 对象属性操作

```javascript
const user = {
    name: "张三",
    age: 30,
    city: "北京"
};

// 访问属性
console.log(user.name);      // "张三"
console.log(user["age"]);    // 30

// 添加属性
user.email = "zhangsan@example.com";
user["phone"] = "13800138000";

// 修改属性
user.age = 31;
user["city"] = "上海";

// 删除属性
delete user.email;
delete user["phone"];

// 检查属性是否存在
console.log("name" in user);        // true
console.log(user.hasOwnProperty("name")); // true

// 获取所有属性名
console.log(Object.keys(user));     // ["name", "age", "city"]

// 获取所有属性值
console.log(Object.values(user));   // ["张三", 31, "上海"]

// 获取所有键值对
console.log(Object.entries(user));  // [["name", "张三"], ["age", 31], ["city", "上海"]]
```

### 3. 对象方法

```javascript
const calculator = {
    // 方法定义
    add: function(a, b) {
        return a + b;
    },
    
    // ES6简写语法
    subtract(a, b) {
        return a - b;
    },
    
    multiply: (a, b) => a * b,
    
    // 使用this
    divide(a, b) {
        return a / b;
    }
};

console.log(calculator.add(5, 3));      // 8
console.log(calculator.subtract(10, 4)); // 6
console.log(calculator.multiply(2, 3));  // 6
console.log(calculator.divide(15, 3));   // 5
```

### 4. 对象解构

```javascript
const user = {
    name: "张三",
    age: 30,
    city: "北京",
    email: "zhangsan@example.com"
};

// 基本解构
const { name, age, city } = user;
console.log(name, age, city); // "张三" 30 "北京"

// 重命名
const { name: userName, age: userAge } = user;
console.log(userName, userAge); // "张三" 30

// 默认值
const { name, country = "中国" } = user;
console.log(country); // "中国"

// 嵌套解构
const userWithAddress = {
    name: "李四",
    address: {
        street: "人民路",
        city: "上海",
        zip: "200000"
    }
};

const { name: name2, address: { city: city2, zip } } = userWithAddress;
console.log(name2, city2, zip); // "李四" "上海" "200000"
```

### 5. 对象合并与复制

```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };

// Object.assign() - 浅拷贝
const merged = Object.assign({}, obj1, obj2);
console.log(merged); // { a: 1, b: 3, c: 4 }

// 展开运算符 - 浅拷贝
const spreadMerged = { ...obj1, ...obj2 };
console.log(spreadMerged); // { a: 1, b: 3, c: 4 }

// 深拷贝
const deepCopy = JSON.parse(JSON.stringify(obj1));
console.log(deepCopy); // { a: 1, b: 2 }

// 自定义深拷贝函数
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    if (obj instanceof Date) return new Date(obj);
    if (obj instanceof Array) return obj.map(item => deepClone(item));
    
    const cloned = {};
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            cloned[key] = deepClone(obj[key]);
        }
    }
    return cloned;
}
```

### 6. 对象遍历

```javascript
const person = {
    name: "张三",
    age: 30,
    city: "北京"
};

// for...in 循环
for (let key in person) {
    if (person.hasOwnProperty(key)) {
        console.log(`${key}: ${person[key]}`);
    }
}

// Object.keys()
Object.keys(person).forEach(key => {
    console.log(`${key}: ${person[key]}`);
});

// Object.entries()
Object.entries(person).forEach(([key, value]) => {
    console.log(`${key}: ${value}`);
});
```

## 📊 JavaScript数组

### 1. 数组创建方式

```javascript
// 1. 数组字面量（推荐）
const fruits = ["苹果", "香蕉", "橙子"];

// 2. Array构造函数
const numbers = new Array(1, 2, 3, 4, 5);

// 3. 创建指定长度的空数组
const emptyArray = new Array(5); // [empty × 5]

// 4. Array.of() - 解决Array构造函数的问题
const arrayOf = Array.of(5); // [5]

// 5. Array.from() - 从类数组对象创建数组
const fromArray = Array.from("hello"); // ["h", "e", "l", "l", "o"]
const fromSet = Array.from(new Set([1, 2, 3])); // [1, 2, 3]
const fromMap = Array.from(new Map([[1, 'a'], [2, 'b']])); // [[1, 'a'], [2, 'b']]
```

### 2. 数组基本操作

```javascript
const colors = ["红色", "绿色", "蓝色"];

// 访问元素
console.log(colors[0]); // "红色"
console.log(colors[1]); // "绿色"

// 修改元素
colors[1] = "黄色";
console.log(colors); // ["红色", "黄色", "蓝色"]

// 获取数组长度
console.log(colors.length); // 3

// 添加元素到末尾
colors.push("紫色");
console.log(colors); // ["红色", "黄色", "蓝色", "紫色"]

// 删除末尾元素
colors.pop();
console.log(colors); // ["红色", "黄色", "蓝色"]

// 添加元素到开头
colors.unshift("白色");
console.log(colors); // ["白色", "红色", "黄色", "蓝色"]

// 删除开头元素
colors.shift();
console.log(colors); // ["红色", "黄色", "蓝色"]
```

### 3. 数组遍历方法

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - 遍历每个元素
numbers.forEach((num, index) => {
    console.log(`索引${index}: ${num}`);
});

// map - 创建新数组
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - 过滤数组
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4]

// reduce - 累积计算
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 15

// find - 查找第一个符合条件的元素
const found = numbers.find(num => num > 3);
console.log(found); // 4

// findIndex - 查找第一个符合条件的元素索引
const foundIndex = numbers.findIndex(num => num > 3);
console.log(foundIndex); // 3

// some - 是否有元素满足条件
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven); // true

// every - 是否所有元素都满足条件
const allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true
```

### 4. 数组修改方法

```javascript
const fruits = ["苹果", "香蕉", "橙子", "葡萄"];

// slice - 提取子数组（不改变原数组）
const citrus = fruits.slice(1, 3);
console.log(citrus); // ["香蕉", "橙子"]
console.log(fruits); // ["苹果", "香蕉", "橙子", "葡萄"]

// splice - 删除/插入元素（改变原数组）
const removed = fruits.splice(1, 2, "西瓜", "菠萝");
console.log(removed); // ["香蕉", "橙子"]
console.log(fruits); // ["苹果", "西瓜", "菠萝", "葡萄"]

// concat - 合并数组（不改变原数组）
const moreFruits = ["桃子", "梨子"];
const allFruits = fruits.concat(moreFruits);
console.log(allFruits); // ["苹果", "西瓜", "菠萝", "葡萄", "桃子", "梨子"]

// join - 连接成字符串
const fruitString = fruits.join(", ");
console.log(fruitString); // "苹果, 西瓜, 菠萝, 葡萄"

// reverse - 反转数组（改变原数组）
fruits.reverse();
console.log(fruits); // ["葡萄", "菠萝", "西瓜", "苹果"]

// sort - 排序数组（改变原数组）
fruits.sort();
console.log(fruits); // ["苹果", "菠萝", "葡萄", "西瓜"]

// 数字排序（需要自定义比较函数）
const numbers = [5, 2, 8, 1, 9];
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 2, 5, 8, 9]
```

### 5. 数组解构

```javascript
const colors = ["红色", "绿色", "蓝色", "黄色"];

// 基本解构
const [red, green, blue] = colors;
console.log(red, green, blue); // "红色" "绿色" "蓝色"

// 跳过元素
const [first, , third] = colors;
console.log(first, third); // "红色" "蓝色"

// 默认值
const [a, b, c = "默认值"] = ["第一", "第二"];
console.log(a, b, c); // "第一" "第二" "默认值"

// 剩余参数
const [primary, ...secondary] = colors;
console.log(primary);    // "红色"
console.log(secondary);  // ["绿色", "蓝色", "黄色"]

// 交换变量
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2 1
```

### 6. 数组查找与搜索

```javascript
const users = [
    { id: 1, name: "张三", age: 25 },
    { id: 2, name: "李四", age: 30 },
    { id: 3, name: "王五", age: 35 }
];

// find - 查找第一个符合条件的对象
const user = users.find(u => u.age > 28);
console.log(user); // { id: 2, name: "李四", age: 30 }

// filter - 查找所有符合条件的对象
const youngUsers = users.filter(u => u.age < 30);
console.log(youngUsers); // [{ id: 1, name: "张三", age: 25 }]

// findIndex - 查找索引
const index = users.findIndex(u => u.name === "王五");
console.log(index); // 2

// includes - 检查是否包含某个值
const numbers = [1, 2, 3, 4, 5];
console.log(numbers.includes(3)); // true
console.log(numbers.includes(6)); // false

// indexOf - 查找值的索引
console.log(numbers.indexOf(3)); // 2
console.log(numbers.indexOf(6)); // -1

// lastIndexOf - 从后往前查找
const repeated = [1, 2, 3, 2, 1];
console.log(repeated.lastIndexOf(2)); // 3
```

## 🎨 高级特性

### 1. 对象的高级特性

```javascript
// 计算属性名
const dynamicKey = "age";
const person = {
    name: "张三",
    [dynamicKey]: 30
};

console.log(person.age); // 30

// 对象方法的高级用法
const advancedObject = {
    // getter和setter
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(' ');
    },
    
    // 简写方法
    greet() {
        return `你好，${this.fullName}！`;
    }
};

advancedObject.fullName = "张 三丰";
console.log(advancedObject.fullName); // "张 三丰"
console.log(advancedObject.greet());   // "你好，张 三丰！"

// 对象的静态方法
const obj = { a: 1, b: 2 };

// 冻结对象（不可添加、删除、修改属性）
Object.freeze(obj);
// obj.c = 3; // 不会添加
// delete obj.a; // 不会删除
// obj.a = 10; // 不会修改

// 密封对象（不可添加、删除属性，但可修改）
const sealedObj = { a: 1, b: 2 };
Object.seal(sealedObj);
// sealedObj.c = 3; // 不会添加
// delete sealedObj.a; // 不会删除
sealedObj.a = 10; // 可以修改
```

### 2. 数组的高级特性

```javascript
// 扁平化数组
const nested = [1, [2, [3, [4]]]];
const flattened = nested.flat(Infinity);
console.log(flattened); // [1, 2, 3, 4]

// 数组分组
const numbers = [1, 2, 3, 4, 5, 6];
const grouped = numbers.reduce((acc, num) => {
    const key = num % 2 === 0 ? 'even' : 'odd';
    acc[key] = acc[key] || [];
    acc[key].push(num);
    return acc;
}, {});

console.log(grouped); // { odd: [1, 3, 5], even: [2, 4, 6] }

// 数组去重
const duplicates = [1, 2, 2, 3, 3, 3, 4];
const unique = [...new Set(duplicates)];
console.log(unique); // [1, 2, 3, 4]

// 数组交集、并集、差集
const arr1 = [1, 2, 3, 4];
const arr2 = [3, 4, 5, 6];

const intersection = arr1.filter(x => arr2.includes(x));
const union = [...new Set([...arr1, ...arr2])];
const difference = arr1.filter(x => !arr2.includes(x));

console.log(intersection); // [3, 4]
console.log(union);        // [1, 2, 3, 4, 5, 6]
console.log(difference);   // [1, 2]
```

### 3. 对象与数组的转换

```javascript
// 对象转数组
const person = {
    name: "张三",
    age: 30,
    city: "北京"
};

const keys = Object.keys(person);
const values = Object.values(person);
const entries = Object.entries(person);

console.log(keys);    // ["name", "age", "city"]
console.log(values);  // ["张三", 30, "北京"]
console.log(entries); // [["name", "张三"], ["age", 30], ["city", "北京"]]

// 数组转对象
const pairs = [["name", "李四"], ["age", 25], ["city", "上海"]];
const objFromArray = Object.fromEntries(pairs);
console.log(objFromArray); // { name: "李四", age: 25, city: "上海" }
```

## 🎯 实际应用示例

### 1. 购物车系统

```javascript
// 购物车类
class ShoppingCart {
    constructor() {
        this.items = [];
        this.coupons = [];
    }
    
    // 添加商品
    addItem(product, quantity = 1) {
        const existingItem = this.items.find(item => item.product.id === product.id);
        
        if (existingItem) {
            existingItem.quantity += quantity;
        } else {
            this.items.push({ product, quantity });
        }
        
        return this;
    }
    
    // 移除商品
    removeItem(productId) {
        this.items = this.items.filter(item => item.product.id !== productId);
        return this;
    }
    
    // 更新数量
    updateQuantity(productId, quantity) {
        const item = this.items.find(item => item.product.id === productId);
        if (item) {
            item.quantity = Math.max(0, quantity);
            if (item.quantity === 0) {
                this.removeItem(productId);
            }
        }
        return this;
    }
    
    // 计算小计
    getSubtotal() {
        return this.items.reduce((total, item) => {
            return total + (item.product.price * item.quantity);
        }, 0);
    }
    
    // 计算折扣
    getDiscount() {
        return this.coupons.reduce((total, coupon) => {
            return total + coupon.amount;
        }, 0);
    }
    
    // 计算总价
    getTotal() {
        return Math.max(0, this.getSubtotal() - this.getDiscount());
    }
    
    // 获取商品列表
    getItems() {
        return this.items;
    }
    
    // 清空购物车
    clear() {
        this.items = [];
        this.coupons = [];
        return this;
    }
}

// 使用示例
const cart = new ShoppingCart();

const products = [
    { id: 1, name: "苹果", price: 5 },
    { id: 2, name: "香蕉", price: 3 },
    { id: 3, name: "橙子", price: 4 }
];

cart.addItem(products[0], 2)
    .addItem(products[1], 3)
    .addItem(products[2], 1);

console.log("购物车商品:", cart.getItems());
console.log("小计:", cart.getSubtotal());
console.log("总计:", cart.getTotal());
```

### 2. 用户管理系统

```javascript
// 用户管理系统
class UserManager {
    constructor() {
        this.users = [];
        this.nextId = 1;
    }
    
    // 添加用户
    addUser(userData) {
        const user = {
            id: this.nextId++,
            ...userData,
            createdAt: new Date(),
            isActive: true
        };
        
        this.users.push(user);
        return user;
    }
    
    // 获取用户
    getUserById(id) {
        return this.users.find(user => user.id === id);
    }
    
    // 获取所有用户
    getAllUsers() {
        return this.users;
    }
    
    // 更新用户
    updateUser(id, updates) {
        const userIndex = this.users.findIndex(user => user.id === id);
        if (userIndex !== -1) {
            this.users[userIndex] = { ...this.users[userIndex], ...updates };
            return this.users[userIndex];
        }
        return null;
    }
    
    // 删除用户
    deleteUser(id) {
        const userIndex = this.users.findIndex(user => user.id === id);
        if (userIndex !== -1) {
            const deletedUser = this.users.splice(userIndex, 1)[0];
            return deletedUser;
        }
        return null;
    }
    
    // 搜索用户
    searchUsers(query) {
        const lowercaseQuery = query.toLowerCase();
        return this.users.filter(user => 
            user.name.toLowerCase().includes(lowercaseQuery) ||
            user.email.toLowerCase().includes(lowercaseQuery)
        );
    }
    
    // 按年龄分组
    groupByAge() {
        return this.users.reduce((groups, user) => {
            const ageGroup = Math.floor(user.age / 10) * 10;
            const key = `${ageGroup}-${ageGroup + 9}`;
            
            if (!groups[key]) {
                groups[key] = [];
            }
            
            groups[key].push(user);
            return groups;
        }, {});
    }
    
    // 获取活跃用户
    getActiveUsers() {
        return this.users.filter(user => user.isActive);
    }
    
    // 统计信息
    getStats() {
        return {
            total: this.users.length,
            active: this.getActiveUsers().length,
            averageAge: this.users.reduce((sum, user) => sum + user.age, 0) / this.users.length,
            ageGroups: this.groupByAge()
        };
    }
}

// 使用示例
const userManager = new UserManager();

userManager.addUser({ name: "张三", email: "zhangsan@example.com", age: 25 });
userManager.addUser({ name: "李四", email: "lisi@example.com", age: 30 });
userManager.addUser({ name: "王五", email: "wangwu@example.com", age: 35 });

console.log("所有用户:", userManager.getAllUsers());
console.log("统计信息:", userManager.getStats());
console.log("搜索结果:", userManager.searchUsers("张"));
console.log("年龄分组:", userManager.groupByAge());
```

## 📋 最佳实践

### 1. 对象的最佳实践

```javascript
// ✅ 使用有意义的属性名
const userProfile = {
    firstName: "张",
    lastName: "三",
    emailAddress: "zhangsan@example.com",
    dateOfBirth: "1990-01-01"
};

// ❌ 使用模糊的属性名
const user = {
    n: "张",
    l: "三",
    e: "zhangsan@example.com",
    dob: "1990-01-01"
};

// ✅ 使用常量定义对象属性名
const USER_PROPERTIES = {
    FIRST_NAME: 'firstName',
    LAST_NAME: 'lastName',
    EMAIL: 'email'
};

const userWithConstants = {
    [USER_PROPERTIES.FIRST_NAME]: "张",
    [USER_PROPERTIES.LAST_NAME]: "三",
    [USER_PROPERTIES.EMAIL]: "zhangsan@example.com"
};
```

### 2. 数组的最佳实践

```javascript
// ✅ 使用适当的数组方法
const numbers = [1, 2, 3, 4, 5];

// 使用map而不是forEach来创建新数组
const doubled = numbers.map(n => n * 2);

// 使用filter而不是手动过滤
const evenNumbers = numbers.filter(n => n % 2 === 0);

// 使用reduce而不是手动累积
const sum = numbers.reduce((acc, n) => acc + n, 0);

// ❌ 避免使用for循环进行简单操作
const doubledBad = [];
for (let i = 0; i < numbers.length; i++) {
    doubledBad.push(numbers[i] * 2);
}

// ✅ 使用解构赋值
const [first, second, ...rest] = numbers;

// ✅ 使用展开运算符复制数组
const numbersCopy = [...numbers];

// ❌ 避免直接修改原数组
const sorted = [...numbers].sort((a, b) => a - b);
```

### 3. 性能考虑

```javascript
// ✅ 对象属性访问缓存
function processLargeObject(obj) {
    const property = obj.some.deeply.nested.property;
    
    // 多次使用缓存的属性
    for (let i = 0; i < 1000; i++) {
        // 使用property而不是重复访问obj.some.deeply.nested.property
    }
}

// ✅ 数组长度缓存
function processLargeArray(arr) {
    const length = arr.length;
    
    for (let i = 0; i < length; i++) {
        // 使用length而不是每次都访问arr.length
    }
}

// ❌ 避免在循环中创建对象
function badPerformance(arr) {
    const result = [];
    for (let i = 0; i < arr.length; i++) {
        // 每次循环都创建新对象
        result.push({ value: arr[i], index: i });
    }
    return result;
}

// ✅ 批量处理
function goodPerformance(arr) {
    return arr.map((value, index) => ({ value, index }));
}
```

## 🎯 总结

对象和数组是JavaScript中最常用的数据结构，掌握它们的使用对于编写高效的JavaScript代码至关重要：

### 对象要点：
- **创建方式**：字面量、构造函数、Object.create、ES6 Class
- **属性操作**：访问、添加、修改、删除、检查存在性
- **方法**：定义方法、this关键字、getter/setter
- **高级特性**：计算属性名、对象静态方法、深拷贝
- **遍历方式**：for...in、Object.keys、Object.values、Object.entries

### 数组要点：
- **创建方式**：字面量、Array构造函数、Array.of、Array.from
- **基本操作**：访问、修改、添加、删除元素
- **遍历方法**：forEach、map、filter、reduce、find等
- **修改方法**：slice、splice、concat、join、reverse、sort
- **高级特性**：解构赋值、扁平化、分组、去重

### 最佳实践：
- **命名规范**：使用有意义的属性名和方法名
- **方法选择**：根据需求选择合适的数组方法
- **性能优化**：缓存属性访问、避免不必要的对象创建
- **代码可读性**：使用解构赋值、展开运算符等现代语法

记住：**对象和数组是JavaScript的基石，熟练掌握它们的使用将让你的代码更加优雅和高效！** 🚀
