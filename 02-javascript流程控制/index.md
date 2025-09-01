# JavaScript流程控制

# JavaScript流程控制

## 🔄 什么是流程控制？

流程控制是编程中的核心概念，它决定了代码的执行顺序。通过流程控制，我们可以让程序根据不同的条件执行不同的代码块，或者重复执行某些操作。

JavaScript中的流程控制主要包括：
- **条件语句**：根据条件执行不同的代码
- **循环语句**：重复执行代码块
- **跳转语句**：改变正常的执行流程

## 🎯 条件语句

### 1. if 语句

```javascript
// 基本if语句
let age = 18;

if (age >= 18) {
    console.log("你已经成年了！");
}

// if-else语句
if (age >= 18) {
    console.log("成年人");
} else {
    console.log("未成年人");
}

// if-else if-else语句
let score = 85;

if (score >= 90) {
    console.log("优秀");
} else if (score >= 80) {
    console.log("良好");
} else if (score >= 60) {
    console.log("及格");
} else {
    console.log("不及格");
}
```

### 2. switch 语句

```javascript
// switch语句示例
let day = "Monday";

switch (day) {
    case "Monday":
        console.log("星期一");
        break;
    case "Tuesday":
        console.log("星期二");
        break;
    case "Wednesday":
        console.log("星期三");
        break;
    case "Thursday":
        console.log("星期四");
        break;
    case "Friday":
        console.log("星期五");
        break;
    case "Saturday":
    case "Sunday":
        console.log("周末");
        break;
    default:
        console.log("无效的日期");
}

// switch的case穿透（多个case执行相同代码）
let month = 1;
let season;

switch (month) {
    case 12:
    case 1:
    case 2:
        season = "冬季";
        break;
    case 3:
    case 4:
    case 5:
        season = "春季";
        break;
    case 6:
    case 7:
    case 8:
        season = "夏季";
        break;
    case 9:
    case 10:
    case 11:
        season = "秋季";
        break;
    default:
        season = "无效月份";
}

console.log(season);
```

### 3. 三元运算符

```javascript
// 三元运算符（条件表达式）
let age = 20;
let status = age >= 18 ? "成年人" : "未成年人";
console.log(status);

// 嵌套三元运算符
let score = 75;
let grade = score >= 90 ? "优秀" : 
            score >= 80 ? "良好" : 
            score >= 60 ? "及格" : "不及格";
console.log(grade);
```

### 4. 短路求值

```javascript
// 逻辑与短路 (&&)
let isLoggedIn = true;
let user = { name: "张三" };
let userName = isLoggedIn && user.name; // "张三"

let isLoggedOut = false;
let userName2 = isLoggedOut && user.name; // false

// 逻辑或短路 (||)
let username = "张三";
let displayName = username || "游客"; // "张三"

let username2 = null;
let displayName2 = username2 || "游客"; // "游客"

// 空值合并运算符 (??)
let value1 = null;
let result1 = value1 ?? "默认值"; // "默认值"

let value2 = undefined;
let result2 = value2 ?? "默认值"; // "默认值"

let value3 = 0;
let result3 = value3 ?? "默认值"; // 0 (与||的区别)
```

## 🔁 循环语句

### 1. for 循环

```javascript
// 基本for循环
for (let i = 1; i <= 5; i++) {
    console.log("第", i, "次循环");
}

// 计算1-100的和
let sum = 0;
for (let i = 1; i <= 100; i++) {
    sum += i;
}
console.log("1-100的和:", sum);

// 遍历数组
let fruits = ["苹果", "香蕉", "橙子"];
for (let i = 0; i < fruits.length; i++) {
    console.log("水果:", fruits[i]);
}

// 多重循环（九九乘法表）
for (let i = 1; i <= 9; i++) {
    let row = "";
    for (let j = 1; j <= i; j++) {
        row += `${j}×${i}=${i*j} `;
    }
    console.log(row);
}
```

### 2. while 循环

```javascript
// 基本while循环
let count = 1;
while (count <= 5) {
    console.log("Count:", count);
    count++;
}

// 猜数字游戏
let targetNumber = Math.floor(Math.random() * 100) + 1;
let guess = 0;
let attempts = 0;

while (guess !== targetNumber) {
    guess = Math.floor(Math.random() * 100) + 1;
    attempts++;
    console.log(`第${attempts}次猜测: ${guess}`);
}

console.log(`猜对了！答案是${targetNumber}，共猜了${attempts}次`);
```

### 3. do-while 循环

```javascript
// do-while循环（至少执行一次）
let i = 1;
do {
    console.log("Do-while循环:", i);
    i++;
} while (i <= 3);

// 用户输入验证
let userInput;
do {
    userInput = prompt("请输入一个正数:");
} while (userInput === null || userInput === "" || isNaN(userInput) || Number(userInput) <= 0);

console.log("你输入的正数是:", userInput);
```

### 4. for-in 循环

```javascript
// 遍历对象属性
let person = {
    name: "张三",
    age: 30,
    city: "北京"
};

for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}

// 遍历数组（不推荐，会遍历原型链上的属性）
let arr = [1, 2, 3];
for (let index in arr) {
    console.log(`索引${index}: ${arr[index]}`);
}
```

### 5. for-of 循环

```javascript
// 遍历数组
let numbers = [1, 2, 3, 4, 5];
for (let num of numbers) {
    console.log("数字:", num);
}

// 遍历字符串
let text = "Hello";
for (let char of text) {
    console.log("字符:", char);
}

// 遍历Map
let map = new Map([
    ["name", "张三"],
    ["age", 30]
]);

for (let [key, value] of map) {
    console.log(`${key}: ${value}`);
}

// 遍历Set
let set = new Set([1, 2, 3, 3, 4]);
for (let item of set) {
    console.log("Set项:", item);
}
```

### 6. 数组方法遍历

```javascript
let numbers = [1, 2, 3, 4, 5];

// forEach
numbers.forEach((num, index) => {
    console.log(`索引${index}: ${num}`);
});

// map
let doubled = numbers.map(num => num * 2);
console.log("翻倍后:", doubled);

// filter
let evenNumbers = numbers.filter(num => num % 2 === 0);
console.log("偶数:", evenNumbers);

// reduce
let sum = numbers.reduce((acc, num) => acc + num, 0);
console.log("总和:", sum);

// some 和 every
let hasEven = numbers.some(num => num % 2 === 0);
let allPositive = numbers.every(num => num > 0);
console.log("有偶数:", hasEven);
console.log("都是正数:", allPositive);
```

## 🚀 跳转语句

### 1. break 语句

```javascript
// 在循环中使用break
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        break; // 跳出循环
    }
    console.log(i);
}
// 输出: 1, 2, 3, 4

// 在switch中使用break
let day = "Monday";
switch (day) {
    case "Monday":
        console.log("星期一");
        break;
    case "Tuesday":
        console.log("星期二");
        break;
    default:
        console.log("其他");
}

// 嵌套循环中的break（跳出内层循环）
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 5; j++) {
        if (j === 3) {
            break; // 只跳出内层循环
        }
        console.log(`i=${i}, j=${j}`);
    }
}

// 使用标签跳出外层循环
outerLoop: for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 5; j++) {
        if (j === 3) {
            break outerLoop; // 跳出外层循环
        }
        console.log(`i=${i}, j=${j}`);
    }
}
```

### 2. continue 语句

```javascript
// 跳过偶数
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        continue; // 跳过本次循环
    }
    console.log(i);
}
// 输出: 1, 3, 5, 7, 9

// 跳过特定元素
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
for (let num of numbers) {
    if (num === 3 || num === 7) {
        continue;
    }
    console.log("数字:", num);
}
```

### 3. return 语句

```javascript
// 在函数中使用return
function checkAge(age) {
    if (age < 0) {
        return "年龄不能为负数";
    }
    if (age < 18) {
        return "未成年";
    }
    return "成年";
}

console.log(checkAge(-5));  // "年龄不能为负数"
console.log(checkAge(15));  // "未成年"
console.log(checkAge(25));  // "成年"

// 提前返回模式
function calculateDiscount(price, isMember) {
    // 参数验证
    if (price <= 0) {
        return 0;
    }
    
    // 会员折扣
    if (isMember) {
        return price * 0.8;
    }
    
    // 普通折扣
    if (price > 100) {
        return price * 0.9;
    }
    
    return price;
}
```

## 🎨 实际应用示例

### 1. 购物车计算

```javascript
let cart = [
    { name: "苹果", price: 5, quantity: 3 },
    { name: "香蕉", price: 3, quantity: 5 },
    { name: "橙子", price: 4, quantity: 2 }
];

// 计算总价
let total = 0;
for (let item of cart) {
    total += item.price * item.quantity;
}
console.log("购物车总价:", total);

// 应用折扣
let finalTotal = total >= 50 ? total * 0.9 : total;
console.log("折后价格:", finalTotal);
```

### 2. 用户登录验证

```javascript
function validateLogin(username, password) {
    // 用户名验证
    if (!username || username.length < 3) {
        return "用户名至少3个字符";
    }
    
    // 密码验证
    if (!password || password.length < 6) {
        return "密码至少6个字符";
    }
    
    // 模拟数据库验证
    const validUsers = {
        "admin": "123456",
        "user": "password"
    };
    
    if (validUsers[username] && validUsers[username] === password) {
        return "登录成功";
    } else {
        return "用户名或密码错误";
    }
}

console.log(validateLogin("admin", "123456"));    // "登录成功"
console.log(validateLogin("admin", "wrong"));      // "用户名或密码错误"
console.log(validateLogin("ab", "123456"));        // "用户名至少3个字符"
```

### 3. 斐波那契数列

```javascript
// 生成斐波那契数列
function fibonacci(n) {
    if (n <= 0) return [];
    if (n === 1) return [0];
    if (n === 2) return [0, 1];
    
    let fib = [0, 1];
    for (let i = 2; i < n; i++) {
        fib.push(fib[i-1] + fib[i-2]);
    }
    return fib;
}

console.log(fibonacci(10)); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

// 使用递归（高级）
function fibonacciRecursive(n) {
    if (n <= 0) return 0;
    if (n === 1) return 1;
    return fibonacciRecursive(n - 1) + fibonacciRecursive(n - 2);
}

for (let i = 0; i < 10; i++) {
    console.log(fibonacciRecursive(i));
}
```

## 📋 最佳实践

### 1. 选择合适的循环类型

```javascript
// ✅ 推荐使用for-of遍历数组
let numbers = [1, 2, 3, 4, 5];
for (let num of numbers) {
    console.log(num);
}

// ✅ 推荐使用Object.keys遍历对象
let obj = { a: 1, b: 2, c: 3 };
for (let key of Object.keys(obj)) {
    console.log(key, obj[key]);
}

// ❌ 避免使用for-in遍历数组
for (let index in numbers) {
    // 可能会遍历到原型链上的属性
    console.log(numbers[index]);
}
```

### 2. 避免无限循环

```javascript
// ❌ 危险的无限循环
// while (true) {
//     console.log("这会一直运行！");
// }

// ✅ 始终确保有退出条件
let count = 0;
while (count < 10) {
    console.log(count);
    count++;
}

// ✅ 使用for循环时注意边界
for (let i = 0; i < array.length; i++) {
    // 确保i不会超过数组长度
}
```

### 3. 使用提前返回模式

```javascript
// ❌ 嵌套过深
function process(data) {
    if (data) {
        if (data.value) {
            if (data.value > 0) {
                // 处理逻辑
                return data.value * 2;
            } else {
                return "值必须大于0";
            }
        } else {
            return "缺少value属性";
        }
    } else {
        return "数据不能为空";
    }
}

// ✅ 提前返回模式
function processImproved(data) {
    if (!data) return "数据不能为空";
    if (!data.value) return "缺少value属性";
    if (data.value <= 0) return "值必须大于0";
    
    return data.value * 2;
}
```

## 🎯 总结

流程控制是JavaScript编程的核心技能，掌握好这些概念对于编写高质量的代码至关重要：

- **条件语句**：if/else、switch、三元运算符、短路求值
- **循环语句**：for、while、do-while、for-in、for-of
- **跳转语句**：break、continue、return
- **最佳实践**：选择合适的循环类型、避免无限循环、使用提前返回模式

记住：**代码的可读性比简洁性更重要**！选择合适的流程控制结构，让你的代码既高效又易于理解。🚀
