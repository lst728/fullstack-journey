## 1. CSS 是什么
HTML 是结构，CSS 是样式。

```html
<p class="note">你好</p>
```

```css
.note {
  color: red;
  font-size: 18px;
}
```

CSS 规则由两部分组成：

```css
选择器 {
  属性: 值;
  属性: 值;
}
```

- `.note`：选择器，选中要设置样式的元素
    
- `color: red;`：一条声明
    
- `color`：属性
    
- `red`：值
    
- 多条声明用分号隔开
## 2.引入方式
**外部样式表（推荐）**

```
<link rel="stylesheet" href="style.css">
```
**内部样式**

```html
<style>
  p { color: red; }
</style>
```
**行内样式（不推荐）**

```html
<p style="color: red;">红色文字</p>
```
## 3. 选择器

|选择器|写法|作用|
|---|---|---|
|元素|`p`|所有 `<p>`|
|类|`.note`|`class="note"` 的元素|
|ID|`#intro`|`id="intro"` 的元素|
|群组|`h1, h2, p`|同时选中多个|
|后代|`div p`|`div` 里面所有 `p`|
|子元素|`div > p`|`div` 的直接子元素 `p`|
|通配|`*`|所有元素|
|伪类|`a:hover`|鼠标悬停时的链接|

示例：

```html
<p class="note">段落</p>
<p id="intro">介绍</p>
<div>
  <p>div 里的段落</p>
</div>
```


```css
p { color: #333; }
.note { color: red; }
#intro { font-weight: bold; }
div p { background: #f0f0f0; }
a:hover { color: orange; }
```

优先级：行内 > 内部/外部（后面会细说）。
## 4. 常用属性

**文本**

```css
p {
  color: #333;
  font-size: 16px;
  font-family: Arial, "Microsoft YaHei", sans-serif;
  font-weight: bold;
  line-height: 1.6;
  text-align: center;
  text-decoration: underline;
}
```

**背景**

```css
body {
  background-color: #f5f5f5;
  background-image: url("bg.jpg");
  background-size: cover;
  background-position: center;
}
```

**边框和圆角**


```css
.box {
  border: 1px solid #ccc;
  border-radius: 8px;
}
```

**尺寸**

```css
.box {
  width: 300px;
  max-width: 100%;
  height: 200px;
}
```
## 5. 盒模型

每个元素都是一个盒子：

```text
margin  外边距
border  边框
padding 内边距
content 内容（width宽，height高）
```
box-sizing: content-box（内容盒子）
```css
.box {
  width: 200px;
  padding: 10px;
  border: 2px solid #333;
  margin: 20px;
}

```
默认情况下，`width` 只算内容区，实际占宽 = `200 + 10*2 + 2*2 = 224px`。
margin外边距一般不计算在内

推荐加上：
box-sizing: border-box（边框盒子）
```css
* {
  box-sizing: border-box;
}
```

这样 `width` 就包含 `padding` 和 `border`，更好算。
width 就是整个盒子的总宽度，包含 content + padding + border。
#### 通俗比喻
- `content‑box`：买一个 200px 的纸箱，你往内壁垫泡沫（padding）、钉木板边框（border），**纸箱整体会变大向外扩张**。
- `border‑box`：固定纸箱外部总大小 200px，你垫泡沫、加边框，只能**挤压箱子里面存放物品的空间**，箱子外壳大小不变。
边距塌陷：相邻盒子，边距矛盾时取大的
#### 学块级元素、行内元素、inline-block 的区别。
- block：霸道，自己占一整行，可以随便设宽高。
- inline：温顺，跟文字挤一行，不能设置宽高。
- inline‑block：折中，挤一行，又能设置宽高