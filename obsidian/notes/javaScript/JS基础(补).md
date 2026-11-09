[[JS 地基学习]]
## 函数声明 vs 函数表达式

**看懂时只问一句：** 这行是以 `function` 开头，还是 `const/let` 开头？
*`function add()` 是声明；`const add = function()` 是表达式。*

**调试坑：** 报 `Cannot access 'xxx' before initialization`，通常是函数表达式或 `const/let` 变量在定义前被用了。
## 参数：形参、实参、少传、多传
规则：

| 情况   | 结果                                                    |
| ---- | ----------------------------------------------------- |
| 少传参数 | 缺的形参是 `undefined`                                     |
| 多传参数 | 多余的被忽略（但可用 `arguments` 或剩余参数拿到）                       |
| 默认参数 | `function greet(name = "陌生人")`，没传或传 `undefined` 时用默认值 |
|      |                                                       |
```js
function greet(name = "陌生人") {
  console.log("你好，" + name);
}

greet();          // 你好，陌生人
greet("小明");    // 你好，小明
greet(undefined); // 你好，陌生人
```

## 返回值：`return` 的三个规则
1. **没有 `return`，返回 `undefined`。**
2. **`return` 后面的代码不执行**
3. **`return` 只能返回一个值。** 想返回多个，用对象或数组：
==**看懂函数只问==：有没有 `return`？返回什么？调用处拿返回值干了什么？**

## 箭头函数
箭头函数是函数表达式的简写：
简写规则：

```js
const add = (a, b) => a + b;        // 单表达式，省 return 和花括号
const square = n => n * n;          // 单参数，省括号
const getUser = () => ({ name: "小明" }); // 返回对象，必须加圆括号
```
最后一条是坑：`() => { name: "小明" }` 会被当成函数体，返回 `undefined`。返回对象字面量要写成 `() => ({ ... })`。

**箭头函数没有自己的 `arguments`**，看到 `arguments` 基本可以判断是普通函数。
## ==🔴this的核心区别==
**普通函数的 `this` 看“谁调用它”；箭头函数的 `this` 看“它写在哪”**
#### 普通函数：谁调用，`this` 就是谁

```js
const user = {
  name: "小明",
  sayHi: function () {
    console.log(this.name);
  }
};
user.sayHi(); // 小明
```

`user.sayHi()` 是 `user` 在调用 `sayHi`，所以 `this` 是 `user`，`this.name` 就是 `"小明"`。
#### 箭头函数：写在哪，`this` 就是哪

```js
const user = {
  nam
  e: "小明",
  sayHi: () => {
    console.log(this.name);
  }
};
user.sayHi(); // undefined
```

虽然看起来是 `user.sayHi()`，但箭头函数**不认调用者**。它的 `this` 在**定义时**就固定了，继承外层作用域的 `this`。这里外层是全局，`this.name` 不是 `user.name`，所以是 `undefined`。

**箭头函数像自带家庭住址的人：不管谁雇他，他都回自己家。**  
而且 `call`、`apply`、`bind` 也改不了箭头函数的 `this`。

|函数类型|`this` 由什么决定|记忆|
|---|---|---|
|普通函数|**调用方式**|谁调用，`this` 就是谁|
|箭头函数|**定义位置**|写在哪，`this` 就是外层那个|
## 数组
#### 常见方法
| 方法        | 返回值                    | 用途          |
| --------- | ---------------------- | ----------- |
| `map`     | 新数组                    | 每项转换，要结果    |
| `filter`  | 新数组（可能变短）              | 筛选，要结果      |
| `find`    | 第一个匹配的元素 / `undefined` | 找一个，要结果     |
| `forEach` | **`undefined`**        | 只想遍历做事，不要结果 |
#### 常用操作
### 增删（会改原数组）

#### 1. `push`：末尾加一个

```js
let arr = [1, 2];
arr.push(3);        // 返回值是新长度 3
console.log(arr);   // [1, 2, 3]
```

**改原数组。**

#### 2. `pop`：末尾删一个

```js
let arr = [1, 2, 3];
let last = arr.pop(); // 返回被删的 3
console.log(arr);     // [1, 2]
```

**改原数组。**

#### 3. `unshift`：开头加一个

```js
let arr = [2, 3];
arr.unshift(1);     // 返回新长度 3
console.log(arr);   // [1, 2, 3]
```

**改原数组。**

#### 4. `shift`：开头删一个

```js
let arr = [1, 2, 3];
let first = arr.shift(); // 返回被删的 1
console.log(arr);        // [2, 3]
```

**改原数组。**

#### 5. `splice`：从中间删/加（最灵活，也最容易看错）

```js
let arr = [1, 2, 3, 4];
// 从下标 1 开始，删 2 个
arr.splice(1, 2);
console.log(arr); // [1, 4]
// 从下标 1 开始，删 0 个，插入 9、8
let arr2 = [1, 2, 3];
arr2.splice(1, 0, 9, 8);
console.log(arr2); // [1, 9, 8, 2, 3]
```

**改原数组。** 参数：`splice(起点下标, 删几个, 要插入的东西...)`。

---

### 截取/合并（不改原数组）

#### 6. `slice`：截取一段，返回新数组


```js
let arr = [1, 2, 3, 4];
let part = arr.slice(1, 3); // 从下标 1 到 3，不含 3
console.log(part);          // [2, 3]
console.log(arr);           // [1, 2, 3, 4]，原数组没变

```
**不改原数组。** 记住：`slice` 是“切一片”，`splice` 是“动刀改原数组”。

#### 7. `concat`：合并数组

```js
let a = [1, 2];
let b = [3, 4];
let c = a.concat(b);
console.log(c); // [1, 2, 3, 4]
```

**不改原数组。** 更常用的是展开：`[...a, ...b]`。
## **解构和展开**
### 一、解构：把数组/对象里的值“拆出来”赋值给变量
#### 1. 数组解构

```js
// 解构写法
let [x, y] = [1, 2, 3];
console.log(x); // 1
console.log(y); // 2
```
按**位置**取，不是按名字。

跳过某一项：

```js
let [first, , third] = [1, 2, 3];
console.log(first); // 1
console.log(third); // 3

```
默认值：

```js
let [a = 10, b = 20] = [1];
console.log(a); // 1
console.log(b); // 20，因为没给第二个
```
#### 2. 对象解构

```js
// 解构写法：按属性名取
let { name, age } = user;
console.log(name); // 小明
console.log(age);  // 18
```
### 二、展开：把数组/对象“拆开”放进另一个
#### 1. 展开数组

```js
let a = [1, 2];
let b = [3, 4];
let c = [...a, ...b];
console.log(c); // [1, 2, 3, 4]
```

`...a` 的意思是“把 a 里的元素一个个拿出来”。

也可以加新元素：

```js
let arr = [1, 2];
let newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```
#### 2. 展开对象

```js
let user = { name: "小明", age: 18 };

let newUser = { ...user, age: 19 };
console.log(newUser); // { name: "小明", age: 19 }
```
后面写的属性会覆盖前面的。
**调试时注意：** 看到 `...` 不要以为“完全独立”，嵌套层可能还共享。
