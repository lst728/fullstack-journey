## 给你一份“看懂 JS 代码检查清单”

以后拿到一段不熟的代码，按这个顺序看：

**1. 先看数据长什么样**

```js
let user = { name: "小明", age: 18 };
let list = [{ id: 1 }, { id: 2 }];
```

问：是对象还是数组？有哪些属性？数组里放的是什么？

**2. 再看函数**


```js
function calc(items) { ... return total; }

```
问：参数是什么？有没有 `return`？返回什么？调用处拿返回值干了什么？

**3. 再看流程**

```js
if (x) { ... } else { ... }
for (let i = 0; i < arr.length; i++) { ... }
```

问：`if` 判断的是不是假值？循环从几开始、到几结束、会不会越界？

**4. 不确定就打印**

```js
console.log("i =", i, "arr[i] =", arr[i]);
console.log("typeof x =", typeof x);
console.log("user =", user);
```

不要心算，让 Console 告诉你。

**5. 看报错先找 `.` 前面**

```text
Cannot read properties of undefined (reading 'price')
```

问：`.price` 前面是谁？打印它，确认是不是 `undefined`。

**6. 记住几个高频坑**

|坑|正确做法|
|---|---|
|`if ("0")` 是真值|字符串状态码用 `===` 比较|
|`[]` 和 `{}` 是真值|判空用 `length` / `Object.keys()`|
|数组循环 `i <= length` 越界|用 `i < length`|
|函数没 `return` 返回 `undefined`|要返回值就写 `return`|
|`user.phone.city` 报错|用 `user.phone?.city`|
|`==` 会转类型|平时用 `===`|