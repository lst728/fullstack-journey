***DOM 就是浏览器把 HTML 变成的一棵“元素树”，JS 通过 DOM 找到元素、改内容、改样式、绑事件。***

## 查询元素：最常用的三个
### 1. `getElementById`：按 id 找

```html
<div id="app">你好</div>
```

```js
// 找 id 为 app 的元素
let app = document.getElementById("app");
console.log(app); // <div id="app">你好</div>
```
### 2. `querySelector`：按 CSS 选择器找**第一个**

```html
<div class="card">卡片1</div>
<div class="card">卡片2</div>
```


```js
// 找第一个 class 为 card 的元素
let card = document.querySelector(".card");
console.log(card.textContent); // 卡片1

```
选择器写法和 CSS 一样：

```js
document.querySelector("#app");      // id
document.querySelector(".card");     // class
document.querySelector("button");    // 标签
document.querySelector(".card h2");  // 后代
```

**只返回第一个匹配的元素。**
### 3. `querySelectorAll`：找所有匹配的元素

```js
// 找所有 class 为 card 的元素
let cards = document.querySelectorAll(".card");
console.log(cards.length); // 2
console.log(cards[0].textContent); // 卡片1
```

注意：它返回的不是普通数组，是 **NodeList**。可以下标访问、可以 `forEach`，但 `map/filter` 不一定有。想用数组方法，先转数组：
```js
let arr = [...cards];
```
## 操作元素：改内容、改样式、改属性
### 1. 改文字：`textContent`

```js
let title = document.querySelector("h1");
title.textContent = "新的标题";
```

只改文字，不解析 HTML。**推荐优先用这个。**

### 2. 改 HTML：`innerHTML`

```js
let box = document.querySelector(".box");
box.innerHTML = "<strong>加粗文字</strong>";

```
会解析 HTML。**注意安全**：不要直接把用户输入塞进 `innerHTML`，有 XSS 风险。

### 3. 改样式：`style` 和 `classList`

```js
let box = document.querySelector(".box");
// 直接改行内样式
box.style.color = "red";
box.style.backgroundColor = "yellow";
// 加/删 class，更推荐
box.classList.add("active");
box.classList.remove("hidden");
box.classList.toggle("open"); // 有就删，没有就加

```
调试时优先查 `classList`，因为样式通常写在 CSS 里。

### 4. 改属性：`setAttribute` / `getAttribute`

```js
letimg = document.querySelector("img");
img.setAttribute("src", "new.png");
console.log(img.getAttribute("src")); // new.png

```
常见属性也可以直接点：

```js
img.src = "new.png";
```

### 5. 读自定义属性：`dataset`

```html
<div id="user" data-id="123" data-name="小明"></div>
```

```js
let user = document.querySelector("#user");
console.log(user.dataset.id);   // "123"
console.log(user.dataset.name); // "小明"

```
`data-xxx` 对应 `dataset.xxx`。

---

## 四、绑事件：`addEventListener`

```js
<button id="btn">点我</button>
```

```js
let btn = document.querySelector("#btn");
// 给按钮绑定点击事件
btn.addEventListener("click", function () {
  console.log("按钮被点击了");
});
```

常见事件：

```js
"click"    // 点击
"input"    // 输入框输入
"change"   // 值改变
"submit"   // 表单提交
"keydown"  // 按键

```
事件函数里常用 `event`：

```
btn.addEventListener("click", function (event) {
  console.log(event.target); // 被点击的元素
});
```

---

## 五、调试 DOM 最常见的报错

```js
let btn = document.querySelector("#btn");
btn.addEventListener("click", ...);

```
如果页面里没有 `#btn`，`btn` 是 `null`，下一行就报：


```text
TypeError: Cannot read properties of null (reading 'addEventListener')
```

**调试步骤：**

1. 看报错：读 `null` 的属性。
    
2. 打印查询结果：`console.log(btn)`，确认是不是 `null`。
    
3. 检查选择器拼写、元素是否存在。
    
4. 检查脚本执行时元素是否已经加载。  
    如果 `<script>` 写在 `<head>` 里，DOM 还没生成，就会查不到。常见解决：把脚本放 `</body>` 前，或用 `DOMContentLoaded`。
    

```js
document.addEventListener("DOMContentLoaded", function () {
  // 等 HTML 加载完再查元素
  let btn = document.querySelector("#btn");
  console.log(btn);
});
```
## 一张速查表
|目的|写法|
|---|---|
|按 id 找|`document.getElementById("app")`|
|找第一个|`document.querySelector(".card")`|
|找所有|`document.querySelectorAll(".card")`|
|改文字|`el.textContent = "新文字"`|
|改 HTML|`el.innerHTML = "<b>文字</b>"`|
|加 class|`el.classList.add("active")`|
|删 class|`el.classList.remove("active")`|
|切换 class|`el.classList.toggle("open")`|
|改样式|`el.style.color = "red"`|
|读 data 属性|`el.dataset.id`|
|绑事件|`el.addEventListener("click", fn)`|
|查不到元素|结果是 `null`，再操作会报错|
## **事件流与事件委托**
**你点一个按钮，事件不只发生在按钮上，它会从最外层一路传到按钮，再从按钮一路传回最外层。**
## 一、事件流：捕获 → 目标 → 冒泡

```html
<div id="outer">
  <button id="btn">点我</button>
</div>
```
点按钮时，事件走三个阶段：

| 阶段   | 方向   | 说明                                               |
| ---- | ---- | ------------------------------------------------ |
| 捕获阶段 | 从外到内 | `document` → `html` → `body` → `#outer` → `#btn` |
| 目标阶段 | 到达按钮 | 事件到达 `#btn` 本身                                   |
| 冒泡阶段 | 从内到外 | `#btn` → `#outer` → `body` → `html` → `document` |
**默认情况下，`addEventListener` 绑在冒泡阶段。** 所以内层元素的事件会“冒泡”到外层。

```js
// 给外层 div 绑点击
document.querySelector("#outer").addEventListener("click", function () {
  console.log("外层被点击");
});
// 给按钮绑点击
document.querySelector("#btn").addEventListener("click", function () {
  console.log("按钮被点击");
});
```

你点按钮，控制台会打印：

```text
按钮被点击
外层被点击
```

因为按钮的点击事件**冒泡**到了外层 div。你点外层 div 的空白处，只打印“外层被点击”。

**调试时看到“我只点了按钮，怎么外层也触发了”，先想到冒泡。**

## 二、阻止冒泡：`event.stopPropagation()`

如果不想让事件继续往外传：

```js
document.querySelector("#btn").addEventListener("click", function (event) {
  event.stopPropagation(); // 阻止冒泡
  console.log("按钮被点击");
});

```
这样点按钮时，外层不会再触发。

**注意：** 不要滥用，先确认真的需要阻止，否则可能影响其他逻辑。
## 三、事件委托：把事件绑在父元素上
真实项目里，列表可能有几十个按钮。如果每个都绑事件，很麻烦，而且动态新增的元素不会自动有事件。

**事件委托**利用冒泡：把事件绑在父元素上，==通过 `event.target`== 判断实际点的是谁

```html
<ul id="list">
  <li>苹果</li>
  <li>香蕉</li>
  <li>橙子</li>
</ul>
```

```js
// 只给 ul 绑一次事件
document.querySelector("#list").addEventListener("click", function (event) {
  // event.target 是实际被点击的元素
  if (event.target.tagName === "LI") {
    console.log("你点了:", event.target.textContent);
  }
});
```
好处：

- 只绑一次，性能好。
    
- 动态新增的 `<li>` 也能自动生效。
    

**看懂事件委托只问：事件绑在谁身上？`event.target` 是谁？怎么判断该不该处理？**

## 四、`event.target` 和 `this` / `currentTarget` 的区别

**记忆：**

- `event.target`：**你真正点到的那个元素**。
    
- `event.currentTarget`：**事件绑在谁身上**。
    
- 普通函数里 `this` 通常等于 `currentTarget`；箭头函数没有自己的 `this`。

## 六、速查表
| 概念     | 写法                                       | 作用         |
| ------ | ---------------------------------------- | ---------- |
| 绑事件    | `el.addEventListener("click", fn)`       | 默认冒泡阶段触发   |
| 捕获阶段绑  | `el.addEventListener("click", fn, true)` | 很少用，知道有这回事 |
| 阻止冒泡   | `event.stopPropagation()`                | 不让事件继续往外传  |
| 实际点击元素 | `event.target`                           | 你点到的那个     |
| 绑定元素   | `event.currentTarget`                    | 事件绑在谁身上    |
| 往上找匹配  | `event.target.closest("button")`         | 判断是否点在按钮内  |
| 事件委托   | 绑父元素，判断 `event.target`                   | 动态元素也能生效   |
