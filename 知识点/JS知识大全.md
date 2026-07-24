# JavaScript 核心语法大全

> 📖 一份系统、通俗的 JS 核心语法笔记，适合从零开始学习或随时查阅。

---

## 目录

- [[JS知识大全#一、变量与数据类型]]
- [[JS知识大全#二、运算符]]
- [[JS知识大全#三、流程控制]]
- [[JS知识大全#四、函数]]
- [[JS知识大全#五、数组]]
- [[JS知识大全#六、对象]]
- [[JS知识大全#七、字符串]]
- [[JS知识大全#八、解构赋值]]
- [[JS知识大全#九、展开运算符与剩余参数]]
- [[JS知识大全#十、Map 与 Set]]
- [[JS知识大全#十一、ES6+ 常用新特性]]
- [[JS知识大全#十二、异步编程]]
- [[JS知识大全#十三、错误处理]]
- [[JS知识大全#十四、模块化]]
- [[JS知识大全#十五、实用技巧与常见陷阱]]

---

## 一、变量与数据类型

### 1.1 声明变量的三种方式

```javascript
// var —— 老语法，有变量提升，尽量少用
var name = '张三';

// let —— 块级作用域，值可以改变
let age = 18;
age = 19; // ✅ 可以重新赋值

// const —— 块级作用域，声明时必须赋值，且不能重新赋值
const PI = 3.14159;
// PI = 3; // ❌ 报错：Assignment to constant variable
```

> 💡 **口诀**：默认用 `const`，需要改值时用 `let`，忘掉 `var`。

**三者对比：**

| 特性         | `var` | `let` | `const` |
| ------------ | ----- | ----- | ------- |
| 作用域       | 函数级 | 块级  | 块级    |
| 变量提升     | ✅ 是 | ❌ 否 | ❌ 否   |
| 可重复赋值   | ✅ 是 | ✅ 是 | ❌ 否   |
| 声明时不赋值 | ✅ 可以 | ✅ 可以 | ❌ 报错 |

### 1.2 数据类型（8 种）

JavaScript 的数据类型分为 **基本类型** 和 **引用类型** 两大类：

```
基本类型（存在栈内存，按值访问）
├── string    字符串
├── number    数字
├── boolean   布尔值
├── null      空值
├── undefined 未定义
├── symbol    唯一标识符（ES6）
└── bigint    大整数（ES2020）

引用类型（存在堆内存，按引用访问）
└── object    对象（包括数组、函数、日期、正则等）
```

```javascript
// ---- 基本类型 ----
let str = 'Hello';           // string
let num = 42;                // number
let float = 3.14;            // number（JS 不区分整数和浮点数）
let bool = true;             // boolean
let nothing = null;          // null —— "故意为空"
let notDefined = undefined;  // undefined —— "还没赋值"
let id = Symbol('id');       // symbol —— 每个都是独一无二的
let big = 9007199254740991n; // bigint —— 超大整数

// ---- 引用类型 ----
let arr = [1, 2, 3];         // Array 也是对象
let obj = { name: '张三' };  // Object
let fn = function() {};      // Function 也是对象
let now = new Date();        // Date
```

### 1.3 typeof 运算符

```javascript
typeof 'hello'     // "string"
typeof 42          // "number"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object"  ← 历史遗留 Bug！
typeof []          // "object"  ← 数组也是对象
typeof {}          // "object"
typeof function(){} // "function"
```

> ⚠️ `typeof null === "object"` 是 JS 的著名 Bug，判断 null 请用 `value === null`。

### 1.4 类型转换

```javascript
// ---- 转字符串 ----
String(123)        // "123"
String(true)       // "true"
(123).toString()   // "123"
'' + 123           // "123"  （隐式转换）

// ---- 转数字 ----
Number('123')      // 123
Number('abc')      // NaN（不是一个数字）
Number(true)       // 1
Number(null)       // 0
Number(undefined)  // NaN
parseInt('42px')   // 42（从头解析数字）
parseFloat('3.14') // 3.14
+'123'             // 123  （隐式转换）

// ---- 转布尔 ----
Boolean(0)         // false
Boolean('')        // false
Boolean(null)      // false
Boolean(undefined) // false
Boolean(NaN)       // false
// 以上 5 个是 "假值"（falsy），其余全是 "真值"（truthy）
!!'hello'          // true  （双重取反 = 转布尔）
```

### 1.5 == 与 === 的区别

```javascript
// == 宽松相等（会自动类型转换）
'5' == 5        // true  → 字符串 '5' 转成了数字 5
null == undefined // true
0 == ''          // true

// === 严格相等（不做类型转换，类型不同直接 false）
'5' === 5       // false
null === undefined // false
0 === ''         // false
```

> 💡 **始终使用 `===`**，避免隐式转换带来的意外。

---

## 二、运算符

### 2.1 算术运算符

```javascript
10 + 3    // 13   加
10 - 3    // 7    减
10 * 3    // 30   乘
10 / 3    // 3.333...  除
10 % 3    // 1    取模（余数）
10 ** 3   // 1000 幂运算（ES7）
```

### 2.2 自增与自减

```javascript
let a = 5;
a++;  // 先使用 a(5)，再 a 变成 6
++a;  // a 先变成 7，再使用 a(7)
a--;  // 先使用 a(7)，再 a 变成 6
--a;  // a 先变成 5，再使用 a(5)
```

### 2.3 赋值运算符

```javascript
let x = 10;
x += 5;  // x = x + 5  → 15
x -= 3;  // x = x - 3  → 12
x *= 2;  // x = x * 2  → 24
x /= 4;  // x = x / 4  → 6
x %= 4;  // x = x % 4  → 2
```

### 2.4 比较运算符

```javascript
5 > 3     // true
5 < 3     // false
5 >= 5    // true
5 <= 4    // false
5 === 5   // true  （推荐）
5 !== '5' // true
```

### 2.5 逻辑运算符

```javascript
true && false   // false  —— 与（两个都为 true 才为 true）
true || false   // true   —— 或（有一个 true 就为 true）
!true           // false  —— 非（取反）

// 短路运算 —— 非常常用的技巧
let name = user && user.name;      // 如果 user 不存在，不会报错
let value = input || '默认值';     // 如果 input 为空，使用默认值

// 空值合并运算符 ??（ES2020）—— 只在 null/undefined 时取右边
let v = 0 || '默认';     // "默认"  ← 0 是假值，被跳过了
let v2 = 0 ?? '默认';    // 0       ← 0 不是 null/undefined，保留
```

### 2.6 三元运算符

```javascript
// 条件 ? 值1（true时） : 值2（false时）
let score = 85;
let grade = score >= 60 ? '及格' : '不及格';  // "及格"

// 等价于：
// if (score >= 60) { grade = '及格'; } else { grade = '不及格'; }
```

---

## 三、流程控制

### 3.1 if / else if / else

```javascript
let score = 85;

if (score >= 90) {
    console.log('优秀');
} else if (score >= 80) {
    console.log('良好');    // ← 会执行这个
} else if (score >= 60) {
    console.log('及格');
} else {
    console.log('不及格');
}
```

### 3.2 switch

```javascript
let day = '周一';

switch (day) {
    case '周一':
    case '周二':
    case '周三':
    case '周四':
    case '周五':
        console.log('工作日');
        break;              // 别忘了 break！否则会"穿透"到下一个 case
    case '周六':
    case '周日':
        console.log('周末');
        break;
    default:
        console.log('无效');
}
```

### 3.3 for 循环

```javascript
// ---- 标准 for 循环 ----
for (let i = 0; i < 5; i++) {
    console.log(i);  // 0, 1, 2, 3, 4
}

// ---- for...of（遍历值，用于数组/字符串等可迭代对象）----
let fruits = ['🍎', '🍌', '🍇'];
for (let fruit of fruits) {
    console.log(fruit);  // 🍎 🍌 🍇
}

// ---- for...in（遍历键/下标，用于对象或数组）----
let person = { name: '张三', age: 18 };
for (let key in person) {
    console.log(key, person[key]);  // name 张三, age 18
}
```

> ⚠️ **遍历数组用 `for...of`，遍历对象用 `for...in`**。不要用 `for...in` 遍历数组（键是字符串，顺序不保证）。

### 3.4 while 与 do...while

```javascript
// while —— 先判断再执行
let n = 5;
while (n > 0) {
    console.log(n);  // 5, 4, 3, 2, 1
    n--;
}

// do...while —— 先执行再判断（至少执行一次）
let m = 0;
do {
    console.log('至少执行一次');
} while (m > 0);
```

### 3.5 break 与 continue

```javascript
// break —— 立即跳出整个循环
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i);  // 0, 1, 2, 3, 4
}

// continue —— 跳过本次，继续下一次
for (let i = 0; i < 5; i++) {
    if (i === 2) continue;
    console.log(i);  // 0, 1, 3, 4
}
```

---

## 四、函数

### 4.1 函数声明与函数表达式

```javascript
// ---- 函数声明（有提升，可以在声明前调用）----
function greet(name) {
    return `你好，${name}！`;
}

// ---- 函数表达式（没有提升）----
const greet2 = function(name) {
    return `你好，${name}！`;
};

// ---- 箭头函数（ES6，更简洁，没有自己的 this）----
const greet3 = (name) => {
    return `你好，${name}！`;
};

// 只有一个参数时可以省略括号
const double = x => x * 2;

// 只有一行 return 时可以省略花括号和 return
const add = (a, b) => a + b;
```

### 4.2 参数

```javascript
// ---- 默认参数（ES6）----
function greet(name = '陌生人') {
    return `你好，${name}！`;
}
greet();       // "你好，陌生人！"
greet('张三'); // "你好，张三！"

// ---- 剩余参数（把多余的参数收集为数组）----
function sum(...numbers) {
    return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4);  // 10

// ---- arguments 对象（老语法，箭头函数没有）----
function oldSum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}
```

### 4.3 作用域与闭包

```javascript
// ---- 作用域：内层可以访问外层，外层不能访问内层 ----
let outer = '我是外层';
function fn() {
    let inner = '我是内层';
    console.log(outer);  // ✅ 可以访问
}
// console.log(inner);   // ❌ 报错，inner 在函数外不存在

// ---- 闭包：函数记住了它被创建时的环境 ----
function createCounter() {
    let count = 0;       // 这个变量被"关"在闭包里
    return function() {
        count++;
        return count;
    };
}
const counter = createCounter();
counter();  // 1
counter();  // 2
counter();  // 3
// count 变量在外面无法直接访问，只能通过 counter() 操作
```

### 4.4 this 关键字

```javascript
// this 的值取决于函数怎么被调用，而不是在哪里定义

// 1️⃣ 全局上下文
console.log(this);  // 浏览器中是 window

// 2️⃣ 对象方法中 —— this 指向调用它的对象
const person = {
    name: '张三',
    sayHi() {
        console.log(this.name);  // "张三"
    }
};
person.sayHi();

// 3️⃣ 普通函数 —— 严格模式下是 undefined，非严格模式下是 window
function show() {
    console.log(this);
}
show();  // window 或 undefined

// 4️⃣ 箭头函数 —— 没有自己的 this，继承外层的 this
const team = {
    name: '火箭队',
    members: ['小智', '小霞'],
    showMembers() {
        this.members.forEach(member => {
            // 箭头函数的 this 继承自 showMembers 的 this
            console.log(`${member} 属于 ${this.name}`);
        });
    }
};

// 5️⃣ 手动改变 this
function say(greeting) {
    console.log(`${greeting}，我是${this.name}`);
}
const zhang = { name: '张三' };
say.call(zhang, '你好');     // "你好，我是张三"（立即调用，逐个传参）
say.apply(zhang, ['你好']);  // "你好，我是张三"（立即调用，数组传参）
const bound = say.bind(zhang); // 返回新函数，this 永久绑定
bound('你好');               // "你好，我是张三"
```

---

## 五、数组

### 5.1 创建数组

```javascript
const arr1 = [1, 2, 3];
const arr2 = new Array(3);       // [ , , ] 3 个空位
const arr3 = Array.from('hello'); // ['h','e','l','l','o']
const arr4 = Array.of(1, 2, 3);  // [1, 2, 3]
```

### 5.2 增删方法

```javascript
const arr = [1, 2, 3];

// 尾部操作
arr.push(4, 5);   // 返回新长度 5，arr = [1,2,3,4,5]
arr.pop();         // 返回被删元素 5，arr = [1,2,3,4]

// 头部操作
arr.unshift(0);    // 返回新长度 5，arr = [0,1,2,3,4]
arr.shift();       // 返回被删元素 0，arr = [1,2,3,4]

// 任意位置
arr.splice(1, 1);        // 从下标1开始删1个 → arr = [1,3,4]
arr.splice(1, 0, 'a');   // 在下标1插入 → arr = [1,'a',3,4]
arr.splice(1, 1, 'b');   // 从下标1替换1个 → arr = [1,'b',3,4]
```

### 5.3 遍历与变换方法（⭐ 最重要）

```javascript
const nums = [1, 2, 3, 4, 5];

// ---- map：对每个元素做变换，返回新数组 ----
const doubled = nums.map(n => n * 2);
// [2, 4, 6, 8, 10]

// ---- filter：过滤出满足条件的元素 ----
const evens = nums.filter(n => n % 2 === 0);
// [2, 4]

// ---- reduce：把数组"归约"为一个值 ----
const sum = nums.reduce((total, n) => total + n, 0);
// 15（0 是初始值）

// ---- find：找到第一个满足条件的元素 ----
const found = nums.find(n => n > 3);
// 4

// ---- findIndex：找到第一个满足条件的下标 ----
const idx = nums.findIndex(n => n > 3);
// 3

// ---- some：至少有一个满足条件吗？ ----
nums.some(n => n > 4);   // true

// ---- every：所有都满足条件吗？ ----
nums.every(n => n > 0);  // true

// ---- forEach：遍历，没有返回值 ----
nums.forEach((n, i) => {
    console.log(`下标${i}的值是${n}`);
});
```

### 5.4 其他常用方法

```javascript
const arr = [3, 1, 4, 1, 5, 9];

// 排序
arr.sort();                    // [1, 1, 3, 4, 5, 9]（按字符串排序！）
arr.sort((a, b) => a - b);    // [1, 1, 3, 4, 5, 9]（数字升序）
arr.sort((a, b) => b - a);    // [9, 5, 4, 3, 1, 1]（数字降序）

// 反转
arr.reverse();

// 连接
[1, 2].concat([3, 4]);        // [1, 2, 3, 4]
[1, 2, 3].join('-');          // "1-2-3"

// 截取（不改变原数组）
[1, 2, 3, 4, 5].slice(1, 3);  // [2, 3]（左闭右开）

// 判断包含
[1, 2, 3].includes(2);        // true

// 扁平化
[1, [2, [3]]].flat(Infinity); // [1, 2, 3]

// 去重（利用 Set）
const unique = [...new Set([1, 2, 2, 3, 3])]; // [1, 2, 3]
```

### 5.5 浅拷贝 vs 深拷贝

```javascript
// 浅拷贝 —— 只拷贝一层
const original = { name: '张三', scores: [90, 85] };
const shallow = { ...original };
// 或者 Object.assign({}, original)

shallow.name = '李四';
console.log(original.name);  // "张三" ✅ 不受影响

shallow.scores.push(100);
console.log(original.scores); // [90, 85, 100] ❌ 被影响了！因为嵌套对象共享引用

// 深拷贝 —— 完全独立的副本
const deep = structuredClone(original);  // 现代方案（推荐）
// 或者 JSON 方案（有局限：不能处理函数、undefined、循环引用等）
const deep2 = JSON.parse(JSON.stringify(original));
```

---

## 六、对象

### 6.1 对象基础

```javascript
// 创建对象
const person = {
    name: '张三',
    age: 18,
    'favorite color': '蓝色',   // 键名有空格时需要引号
    sayHi() {
        console.log(`你好，我是${this.name}`);
    }
};

// 访问属性
person.name;           // "张三"（点语法）
person['favorite color']; // "蓝色"（方括号语法，用于动态键名或特殊键名）

// 增删改属性
person.email = 'zs@test.com'; // 新增
person.age = 19;               // 修改
delete person.email;           // 删除

// 判断属性是否存在
'name' in person;              // true
person.hasOwnProperty('age');  // true
```

### 6.2 对象简写（ES6）

```javascript
const name = '张三';
const age = 18;

// 属性简写：变量名和属性名相同时可以省略
const person = { name, age };
// 等价于 { name: name, age: age }

// 计算属性名
const key = 'color';
const obj = { [key]: '红色' };  // { color: '红色' }
```

### 6.3 对象常用方法

```javascript
const person = { name: '张三', age: 18, city: '北京' };

Object.keys(person);    // ['name', 'age', 'city']   → 所有键
Object.values(person);  // ['张三', 18, '北京']       → 所有值
Object.entries(person); // [['name','张三'], ['age',18], ['city','北京']] → 键值对

// 遍历对象
for (const [key, value] of Object.entries(person)) {
    console.log(`${key}: ${value}`);
}

// 合并对象（后者覆盖前者同名属性）
const defaults = { color: '红', size: 'M' };
const custom = { size: 'L', weight: '100g' };
const merged = { ...defaults, ...custom };
// { color: '红', size: 'L', weight: '100g' }

// 解构赋值（后面会详细讲）
const { name, age } = person;
```

### 6.4 可选链操作符 ?.（ES2020）

```javascript
const user = {
    name: '张三',
    address: {
        city: '北京'
    }
};

// 老写法 —— 繁琐且容易出错
const zip1 = user && user.address && user.address.zipCode;

// 新写法 —— 链上遇到 null/undefined 直接返回 undefined，不报错
const zip2 = user?.address?.zipCode;  // undefined（不报错）

// 也可以用于方法和数组
const result = user.someMethod?.();    // 如果方法不存在返回 undefined
const item = user.list?.[0];           // 如果 list 不存在返回 undefined
```

---

## 七、字符串

### 7.1 基本操作

```javascript
const str = 'Hello, World!';

str.length;             // 13
str[0];                 // 'H'
str.charAt(0);          // 'H'
str.indexOf('World');   // 7（找不到返回 -1）
str.includes('World');  // true
str.startsWith('Hello'); // true
str.endsWith('!');       // true
str.toUpperCase();       // "HELLO, WORLD!"
str.toLowerCase();       // "hello, world!"
str.trim();              // 去除首尾空格
str.replace('World', 'JS'); // "Hello, JS!"
str.split(', ');         // ['Hello', 'World!']
```

### 7.2 模板字符串（ES6）

```javascript
const name = '张三';
const age = 18;

// 反引号 `` 里面可以用 ${} 嵌入变量或表达式
const msg = `我叫${name}，今年${age}岁`;
const calc = `明年我${age + 1}岁`;

// 支持多行
const html = `
<div>
    <h1>${name}</h1>
    <p>年龄：${age}</p>
</div>
`;
```

### 7.3 常见字符串操作

```javascript
// 截取（都是左闭右开，不会修改原字符串）
'abcdef'.slice(1, 4);     // "bcd"
'abcdef'.substring(1, 4); // "bcd"
'abcdef'.slice(-3);       // "def"（支持负数）
'abcdef'.substring(-3);   // "abcdef"（负数当 0）

// 重复与填充
'ha'.repeat(3);           // "hahaha"
'5'.padStart(3, '0');     // "005"
'5'.padEnd(3, '0');       // "500"

// 正则匹配
'hello123world'.match(/\d+/);  // ['123']
'hello123world'.match(/\d+/g); // ['123']
'a-b-c'.split(/-/);            // ['a', 'b', 'c']
```

---

## 八、解构赋值

> 从数组或对象中"拆"出值，赋给变量。

### 8.1 数组解构

```javascript
const [a, b, c] = [1, 2, 3];
// a=1, b=2, c=3

// 跳过元素
const [first, , third] = [1, 2, 3];
// first=1, third=3

// 剩余元素
const [head, ...rest] = [1, 2, 3, 4];
// head=1, rest=[2, 3, 4]

// 默认值
const [x = 10, y = 20] = [5];
// x=5, y=20（没有对应的值就用默认值）

// 交换变量
let m = 1, n = 2;
[m, n] = [n, m];  // m=2, n=1
```

### 8.2 对象解构

```javascript
const person = { name: '张三', age: 18, city: '北京' };

// 基本解构
const { name, age } = person;
// name='张三', age=18

// 重命名
const { name: userName } = person;
// userName='张三'（name 变量不存在了）

// 默认值
const { score = 0 } = person;
// score=0（person 里没有 score）

// 嵌套解构
const data = { user: { info: { name: '张三' } } };
const { user: { info: { name: n2 } } } = data;
// n2='张三'
```

### 8.3 函数参数解构

```javascript
// 对象解构参数 —— 顺序无关、有默认值
function createUser({ name = '匿名', age = 0, role = 'user' } = {}) {
    console.log(name, age, role);
}

createUser({ age: 25, name: '张三' }); // "张三 25 user"
createUser();                          // "匿名 0 user"
```

---

## 九、展开运算符与剩余参数

```javascript
// ---- 展开数组 ----
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2];  // [1, 2, 3, 4]

// ---- 展开对象 ----
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };     // { a:1, b:2, c:3 }

// ---- 复制（浅拷贝）----
const copy = [...arr1];             // 数组浅拷贝
const copy2 = { ...obj1 };          // 对象浅拷贝

// ---- 函数调用时展开 ----
const nums = [3, 7, 1, 9];
Math.max(...nums);                   // 9（等价于 Math.max(3,7,1,9)）

// ---- 剩余参数（函数参数收集）----
function log(first, ...others) {
    console.log('第一个：', first);
    console.log('其余的：', others);
}
log('a', 'b', 'c', 'd');
// 第一个： a
// 其余的： ['b', 'c', 'd']
```

---

## 十、Map 与 Set

### 10.1 Map

```javascript
// Map —— 键可以是任何类型（不像普通对象的键只能是字符串/Symbol）
const map = new Map();

map.set('name', '张三');
map.set(42, '数字键');
map.set(true, '布尔键');

map.get('name');     // "张三"
map.has(42);          // true
map.delete(true);
map.size;             // 2

// 遍历
for (const [key, value] of map) {
    console.log(key, value);
}

// 与对象互转
const obj = Object.fromEntries(map);  // Map → 普通对象
const map2 = new Map(Object.entries(obj)); // 普通对象 → Map
```

### 10.2 Set

```javascript
// Set —— 值唯一，自动去重
const set = new Set([1, 2, 2, 3, 3]);
// Set { 1, 2, 3 }

set.add(4);
set.has(2);    // true
set.delete(1);
set.size;      // 3

// 经典用法：数组去重
const arr = [1, 2, 2, 3, 3, 3];
const unique = [...new Set(arr)]; // [1, 2, 3]

// 集合运算
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);

// 并集
const union = new Set([...a, ...b]);          // {1,2,3,4}
// 交集
const intersection = new Set([...a].filter(x => b.has(x))); // {2,3}
// 差集
const diff = new Set([...a].filter(x => !b.has(x)));        // {1}
```

---

## 十一、ES6+ 常用新特性

### 11.1 类（class）

```javascript
class Animal {
    // 构造函数，new 的时候自动调用
    constructor(name, sound) {
        this.name = name;
        this.sound = sound;
    }

    // 实例方法
    speak() {
        console.log(`${this.name}: ${this.sound}！`);
    }

    // 静态方法 —— 通过类名调用，不通过实例调用
    static isAnimal(obj) {
        return obj instanceof Animal;
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name, '汪汪');  // 调用父类的 constructor
    }

    // 重写父类方法
    speak() {
        super.speak();          // 先调用父类的方法
        console.log('（摇尾巴）');
    }
}

const dog = new Dog('旺财');
dog.speak();
// 旺财: 汪汪！
// （摇尾巴）
```

### 11.2 Promise

```javascript
// Promise —— 代表一个"未来会有结果"的操作
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('成功！');    // 成功时调用
        // reject('失败！');  // 失败时调用
    }, 1000);
});

// 使用
p
    .then(result => console.log(result))   // "成功！"
    .catch(error => console.error(error))  // 失败时
    .finally(() => console.log('结束'));    // 无论如何

// ---- Promise 实用方法 ----
const p1 = fetch('/api/a');
const p2 = fetch('/api/b');

Promise.all([p1, p2]);       // 全部成功才成功，一个失败就失败
Promise.allSettled([p1, p2]); // 等全部完成，不管成功还是失败
Promise.race([p1, p2]);       // 谁先完成就用谁的结果
Promise.any([p1, p2]);        // 谁先成功就用谁，全部失败才失败
```

### 11.3 async / await

```javascript
// async/await —— 让异步代码看起来像同步代码，更易读
async function fetchUser() {
    try {
        const response = await fetch('/api/user');  // 等待 Promise 完成
        const user = await response.json();         // 再等待解析 JSON
        console.log(user);
        return user;
    } catch (error) {
        console.error('请求失败：', error);
    }
}

// 并发请求 —— 不要一个一个 await，用 Promise.all 并行
async function fetchAll() {
    const [users, posts] = await Promise.all([
        fetch('/api/users').then(r => r.json()),
        fetch('/api/posts').then(r => r.json())
    ]);
    return { users, posts };
}
```

### 11.4 迭代器与生成器

```javascript
// 生成器 —— 可以暂停和恢复的函数
function* countUp(max) {
    for (let i = 1; i <= max; i++) {
        yield i;  // 暂停，返回 i
    }
}

const gen = countUp(3);
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: undefined, done: true }

// 用 for...of 消费生成器
for (const n of countUp(5)) {
    console.log(n); // 1, 2, 3, 4, 5
}
```

### 11.5 Symbol 与迭代协议

```javascript
// Symbol —— 独一无二的标识符
const sym1 = Symbol('desc');
const sym2 = Symbol('desc');
sym1 === sym2;  // false（每个 Symbol 都是唯一的）

// 常用于对象的"私有"属性或自定义迭代行为
const collection = {
    items: ['a', 'b', 'c'],
    [Symbol.iterator]() {
        let index = 0;
        const items = this.items;
        return {
            next() {
                return index < items.length
                    ? { value: items[index++], done: false }
                    : { done: true };
            }
        };
    }
};

for (const item of collection) {
    console.log(item); // a, b, c
}
```

---

## 十二、异步编程

### 12.1 回调函数

```javascript
// 最原始的异步方式 —— 回调地狱（callback hell）
setTimeout(() => {
    console.log('第一步');
    setTimeout(() => {
        console.log('第二步');
        setTimeout(() => {
            console.log('第三步');
            // 越嵌越深，难以维护...
        }, 1000);
    }, 1000);
}, 1000);
```

> 回调地狱 → Promise → async/await 是 JS 异步编程的演进路线。

### 12.2 事件循环（Event Loop）

```
JS 是单线程的，但通过事件循环实现异步：

┌───────────────────────┐
│      调用栈 (Stack)      │  ← 同步代码在这里执行
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│    微任务队列 (Microtask) │  ← Promise.then / await / MutationObserver
│    （优先级高，全部清空）   │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│    宏任务队列 (Macrotask) │  ← setTimeout / setInterval / I/O
│    （每次只取一个执行）    │
└───────────────────────┘

执行顺序：同步代码 → 所有微任务 → 一个宏任务 → 所有微任务 → 一个宏任务 → ...
```

```javascript
console.log('1');                // 同步

setTimeout(() => console.log('2'), 0);  // 宏任务

Promise.resolve().then(() => console.log('3')); // 微任务

console.log('4');                // 同步

// 输出顺序：1 → 4 → 3 → 2
```

### 12.3 常见异步模式

```javascript
// ---- 顺序执行 ----
async function sequential() {
    const a = await step1();  // 等 step1 完成
    const b = await step2(a); // 再执行 step2
    return b;
}

// ---- 并行执行 ----
async function parallel() {
    const [a, b, c] = await Promise.all([
        fetchA(),
        fetchB(),
        fetchC()
    ]);
    // 三个请求同时发出，全部完成后继续
}

// ---- 竞争执行 ----
async function race() {
    const fastest = await Promise.race([
        fetchFromCDN1(),
        fetchFromCDN2()
    ]);
    // 谁先返回用谁
}

// ---- 超时控制 ----
function withTimeout(promise, ms) {
    const timeout = new Promise((_, reject) => {
        setTimeout(() => reject(new Error('超时')), ms);
    });
    return Promise.race([promise, timeout]);
}
```

---

## 十三、错误处理

### 13.1 try...catch

```javascript
try {
    // 可能出错的代码
    const data = JSON.parse('invalid json');
} catch (error) {
    console.error('解析失败：', error.message);
} finally {
    // 无论成功失败都会执行（清理资源等）
    console.log('处理完毕');
}
```

### 13.2 自定义错误

```javascript
class ValidationError extends Error {
    constructor(field, message) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}

function setAge(age) {
    if (age < 0 || age > 150) {
        throw new ValidationError('age', '年龄必须在 0-150 之间');
    }
}

try {
    setAge(-5);
} catch (e) {
    if (e instanceof ValidationError) {
        console.error(`字段 ${e.field}: ${e.message}`);
    } else {
        throw e;  // 不认识的错误，重新抛出
    }
}
```

### 13.3 异步错误处理

```javascript
// Promise 链
fetch('/api/data')
    .then(res => res.json())
    .catch(err => console.error('请求失败：', err));

// async/await
async function getData() {
    try {
        const res = await fetch('/api/data');
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return await res.json();
    } catch (err) {
        console.error('出错了：', err);
    }
}
```

---

## 十四、模块化

### 14.1 ES Module（推荐）

```javascript
// ---- math.js（导出）----

// 命名导出 —— 可以导出多个
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function multiply(a, b) { return a * b; }

// 默认导出 —— 每个模块只能有一个
export default class Calculator {
    // ...
}
```

```javascript
// ---- app.js（导入）----

// 导入默认导出
import Calculator from './math.js';

// 导入命名导出
import { add, multiply, PI } from './math.js';

// 全部导入为一个对象
import * as math from './math.js';
math.add(1, 2);

// 重命名导入
import { add as sum } from './math.js';

// 动态导入（按需加载）
const module = await import('./heavy-module.js');
module.doSomething();
```

### 14.2 CommonJS（Node.js 传统方案）

```javascript
// ---- 导出 ----
const PI = 3.14159;
function add(a, b) { return a + b; }

module.exports = { PI, add };
// 或者
exports.PI = PI;
exports.add = add;

// ---- 导入 ----
const { PI, add } = require('./math');
const math = require('./math');  // 整体导入
```

---

## 十五、实用技巧与常见陷阱

### 15.1 常用技巧

```javascript
// ---- 1. 数组去重 ----
const unique = [...new Set([1, 2, 2, 3])];

// ---- 2. 快速生成数字数组 ----
const range = Array.from({ length: 5 }, (_, i) => i + 1);
// [1, 2, 3, 4, 5]

// ---- 3. 对象转查询字符串 ----
const params = { name: '张三', age: 18 };
const query = new URLSearchParams(params).toString();
// "name=%E5%BC%A0%E4%B8%89&age=18"

// ---- 4. 快速取整 ----
~~3.7;       // 3（双按位非）
3.7 | 0;    // 3（按位或）
Math.trunc(3.7); // 3（推荐，语义清晰）

// ---- 5. 随机整数（min 到 max，含两端）----
function randomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

// ---- 6. 打乱数组 ----
const arr = [1, 2, 3, 4, 5];
arr.sort(() => Math.random() - 0.5);

// ---- 7. 对象属性存在性检查 ----
const obj = { a: 1, b: undefined };
obj.a;          // 1
obj.b;          // undefined（存在，值就是 undefined）
obj.c;          // undefined（不存在）
obj.b ?? '缺';  // undefined（因为 b 不是 null/undefined）
obj.c ?? '缺';  // "缺"
obj.b || '缺';  // "缺"（b 是假值，被跳过）
```

### 15.2 常见陷阱

```javascript
// ⚠️ 1. 浮点数精度问题
0.1 + 0.2;                  // 0.30000000000000004
0.1 + 0.2 === 0.3;          // false
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON;  // true（正确的比较方式）

// ⚠️ 2. 数组排序默认按字符串
[10, 9, 2, 100].sort();         // [10, 100, 2, 9] ❌
[10, 9, 2, 100].sort((a,b) => a - b); // [2, 9, 10, 100] ✅

// ⚠️ 3. var 的变量提升
console.log(x); // undefined（不报错！因为 var 提升了）
var x = 10;

console.log(y); // ❌ ReferenceError
let y = 10;

// ⚠️ 4. for...in 遍历数组
const arr = ['a', 'b', 'c'];
for (let i in arr) {
    console.log(typeof i); // "string" ← 键是字符串不是数字！
}

// ⚠️ 5. 对象比较是引用比较
{} === {};           // false（两个不同的对象）
[] === [];           // false（两个不同的数组）
const a = { x: 1 };
const b = a;
a === b;             // true（指向同一个对象）
a.x = 2;
console.log(b.x);   // 2（a 和 b 是同一个对象）

// ⚠️ 6. parseInt 的坑
['1', '2', '3'].map(parseInt);  // [1, NaN, NaN] ❌
// 因为 parseInt 接收 (value, index) 作为参数
// 正确写法：
['1', '2', '3'].map(Number);    // [1, 2, 3] ✅
['1', '2', '3'].map(s => parseInt(s, 10)); // [1, 2, 3] ✅
```

---

## 附录：速查表

### 常用内置对象

| 对象          | 用途                  | 常用方法                                       |
| ------------- | --------------------- | ---------------------------------------------- |
| `Math`        | 数学运算              | `random()`, `floor()`, `ceil()`, `max()`, `min()`, `abs()` |
| `Date`        | 日期时间              | `new Date()`, `getFullYear()`, `toISOString()` |
| `JSON`        | JSON 解析与序列化     | `parse()`, `stringify()`                       |
| `RegExp`      | 正则表达式            | `test()`, `match()`, `replace()`               |
| `Math.random` | 0-1 之间的随机小数    | 配合 `Math.floor()` 生成随机整数               |
| `console`     | 控制台输出            | `log()`, `error()`, `warn()`, `table()`        |

### 数组方法速查

| 方法          | 是否改变原数组 | 返回值           | 用途       |
| ------------- | -------------- | ---------------- | ---------- |
| `push/pop`    | ✅ 是          | 新长度/被删元素  | 尾部增删   |
| `shift/unshift`| ✅ 是         | 新长度/被删元素  | 头部增删   |
| `splice`      | ✅ 是          | 被删元素数组     | 任意位置增删改 |
| `map`         | ❌ 否          | 新数组           | 映射变换   |
| `filter`      | ❌ 否          | 新数组           | 过滤       |
| `reduce`      | ❌ 否          | 单个值           | 归约       |
| `find`        | ❌ 否          | 元素或undefined  | 查找一个   |
| `sort`        | ✅ 是          | 排序后数组       | 排序       |
| `reverse`     | ✅ 是          | 反转后数组       | 反转       |
| `slice`       | ❌ 否          | 新数组           | 截取       |
| `concat`      | ❌ 否          | 新数组           | 合并       |
| `includes`    | ❌ 否          | boolean          | 判断包含   |
| `flat`        | ❌ 否          | 新数组           | 扁平化     |

---

> 📝 **最后的话**：JavaScript 的核心其实就是 **变量 → 数据类型 → 运算符 → 流程控制 → 函数 → 对象/数组 → 异步**。把这些吃透了，剩下的都是在这些基础上的应用和封装。多写代码、多踩坑、多查文档，就是最好的学习方法。

> 🔗 推荐查阅：[MDN Web Docs](https://developer.mozilla.org/zh-CN/) —— 最权威的 JS 文档。
