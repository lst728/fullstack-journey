## 标签 Tag
标签是尖括号包起来的标记
有些标签没有结束标签，叫空元素
初学规则：**双标签成对写，空元素不写结束标签。**
## 元素 Element
元素 = 开始标签 + 内容 + 结束标签：
```
<p>你好</p>
```
元素可以嵌套，
```
<p>这是<strong>加粗</strong>文字</p>
```
但不要交叉

```
<!-- 错 -->
<p><strong>文字</p></strong>
```
## 属性 Attribute
```
<p class="note" id="intro">你好</p>
```
- 多个属性用空格分隔
    
- 属性值建议始终用双引号
    
- 属性名推荐小写
#### 常见属性：
```
<a href="https://example.com">链接</a>
<img src="a.jpg" alt="描述">
<input type="text" placeholder="请输入">
```
## 文档结构
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>我的第一个页面</title>
</head>
<body>
  <h1>你好，HTML</h1>
  <p>这是我的第一个页面。</p>
</body>
</html>
```
逐层理解：

|部分|作用|
|---|---|
|`<!DOCTYPE html>`|声明这是 HTML5 文档，必须第一行|
|`<html lang="zh-CN">`|根元素，所有内容都包在里面；`lang` 表示中文|
|`<head>`|页面元信息，不显示在页面上|
|`<meta charset="UTF-8">`|字符编码，防止中文乱码|
|`<meta name="viewport" ...>`|移动端适配|
|`<title>`|浏览器标签页标题|
|`<body>`|用户真正看到的内容|
结构树大致是：

text

html
├── head
│   ├── meta
│   ├── meta
│   └── title
└── body
    ├── h1
    └── p
## 1. 标题：h1 ~ h6
- `h1` 最重要，`h6` 最次要。
    
- 一个页面通常只写一个 `h1`。
## 2. 段落：p

html

```
<p>这是一个段落。</p>
<p>这是另一个段落。</p>
```

- `p` 是块级元素，独占一行。
    
- 浏览器会自动在段落之间加间距。
    
- `p` 里不要再放 `div`、`p`、`h1` 等块级元素。
- 标题用来组织内容，不要为了“字大”而用标题。
## 3. 文本强调：strong、em、br、hr
```
<p>这是<strong>重要</strong>内容，这是<em>强调</em>内容。</p>
<p>第一行<br>第二行</p>
<hr>

```
|标签|作用|类型|
|---|---|---|
|`strong`|重要，默认加粗|行内|
|`em`|强调，默认斜体|行内|
|`br`|强制换行|空元素|
|`hr`|水平分割线|空元素|
## 4. 链接：a
```
<a href="https://example.com">去示例网站</a>
<a href="https://example.com" target="_blank" rel="noopener">新窗口打开</a>
```
常用属性：

|属性|作用|
|---|---|
|`href`|链接地址，必写|
|`target="_blank"`|新标签页打开|
|`rel="noopener"`|配合 `_blank`，更安全|
|`title`|鼠标悬停提示|
|`download`|下载链接|
链接文字要有意义，不要写“点这里”。
## 5. 图片：img
```
<img src="cat.jpg" alt="一只橘猫" width="300">
```
|属性|作用|
|---|---|
|`src`|图片路径，必写|
|`alt`|图片加载失败或读屏时显示的文字，必写|
|`width` / `height`|宽高，单位像素|
|`title`|悬停提示|

`img` 是空元素，不需要 `</img>`。
## 6. 列表：ul、ol、li
无序列表：

```
<ul>
  <li>苹果</li>
  <li>香蕉</li>
  <li>橘子</li>
</ul>
```

有序列表：

```
<ol>
  <li>打开编辑器</li>
  <li>写 HTML</li>
  <li>用浏览器打开</li>
</ol>
```

规则：

- `ul` 和 `ol` 的直接子元素只能是 `li`。
    
- `li` 里可以放其他标签，比如 `a`、`p`、`ul`。
- 嵌套列表：

```
<ul>
  <li>水果
    <ul>
      <li>苹果</li>
      <li>香蕉</li>
    </ul>
  </li>
  <li>蔬菜</li>
</ul>
```


