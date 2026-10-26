#### JS 会“自作主张”转换类型
JS 里 + 有两个身份：数字相加、字符串拼接。看到 + 时，先看两边有没有字符串。
"- "会强行转数字。
#### 调试三板斧
1. 看报错

比如：

```js
console.log(userName);
```

报错：ReferenceError: userName is not defined
*“not defined” 不是“没定义类型”，而是“这个名字不存在”。*
==调试规则：==

看到 ReferenceError: xxx is not defined，就去检查：

1. 是不是拼写错了？
2. 是不是变量创建在另一个作用域里？
3. 是不是根本没写 let/const？
4. 打印中间值
5. 缩小范围

如果一大段代码错了，就把它拆成几小段，一段一段 console.log，看哪一段开始不对。
调试口诀：先看报错行 → 看变量值 → 看类型 → 缩小范围。

#### JS 不会因为你少传参数就报错

```js
function add(a, b) {
  return a + b;
}
console.log(add(2));
```

你以为会报错，因为 add 要两个参数，你只给了一个。

但 JS 的真实行为是：你没传的 b 会自动变成 undefined。

所以实际执行是：

```js
a = 2
b = undefined
return 2 + undefined
```

2 + undefined 的结果是 NaN（Not a Number，不是一个数字）。

所以输出是：

```text
NaN
```
==记住的规则：==
*JS 函数少传参数不报错，缺的参数就是 undefined。*
*undefined 参与数学运算，结果通常是 NaN。*

二二（宽松相等）：先偷偷转类型再比较。
二二二（严格相等）：类型和值都必须一样。

==口诀==：两个等号会转类型，三个等号不转。
调试时你只要记住：

*平时一律用 =，除非你非常清楚自己在干什么。*
*用 == 很容易出现 "0" == false 也是 true 这种坑。*

#### JS 的 truthy / falsy（真值/假值） 规则
必背：JS 里的“假值”只有这几个

在 if 判断里，下面这些会被当成 false：

```js
false
0
-0
0n          // BigInt 的 0，先不用管
""          // 空字符串
null
undefined
NaN
```
除了这些，其他都是“真值”，包括：

```js
"0"      // 非空字符串，是真
" "      // 空格字符串，也是真
[]       // 空数组，是真
{}       // 空对象，是真
-1       // 非零数字，是真
```

这是个巨大的坑：空数组 [] 和空对象 {} 在 if 里都是真。所以判断数组空不空，不能写 if (arr)，要写 if (arr.length === 0)。

 ##### **调试时怎么用**

看到 if (xxx)，先问：

1. xxx 是什么类型？
2. 它是不是上面那 8 个假值之一？
3. 如果它是字符串，是不是空字符串？
4. 如果它是数组/对象，别用 if 判断空，用 length 或 Object.keys()。

#### 前端最常见的报错 Cannot read properties of undefined
##### eg：
```js
let user = {
  name: "小明",
  address: {
    city: "长沙"
  }
};

console.log(user.address.city);   // 长沙
console.log(user.phone.city);     // 这里会怎样？
```

user.phone 是 undefined，然后你又去读 undefined.city，JS 就炸了：

```text
TypeError: Cannot read properties of undefined (reading 'city')
```

翻译：你想读一个 undefined 的 city 属性，但 undefined 根本没有属性。
##### ==调试规则：==

看到 Cannot read properties of undefined (reading 'xxx')，就往前找：哪个变量是 undefined？
常见原因：

1. 接口还没返回，数据是 undefined。
2. 属性名拼错，比如 user.adress.city。
3. 数组越界，arr[5] 是 undefined。
4. 函数没传参数。

防这个报错，常见写法是可选链：

```js
console.log(user.phone?.city);  // undefined，不报错
```

`?.` 的意思是：**如果前面是 `undefined` 或 `null`，就停下，返回 `undefined`，不要继续读。**
##### ==补充：==
`.toString()` 是**把东西转成字符串**的方法。数字、布尔、数组、对象都有这个方法
但 `undefined` 和 `null` **没有** `.toString()`，所以：

```js
undefined.toString()  // 报错
null.toString()       // 报错
```
##### ==所以调试这类报错，正确步骤是：==

1. 看报错信息：`Cannot read properties of undefined (reading 'name')`。
    
2. 知道“有个东西是 undefined，我还去读它的 `name`”。
    
3. **往前找：`.name` 前面那个表达式是什么？** 这里是 `list[1]`。
    
4. 验证：`console.log(list[1])`，发现是 `undefined`。
    
5. 再问：为什么 `list[1]` 是 undefined？因为数组只有 1 个元素，下标 1 越界。
    
**口诀：**
> `reading 'xxx'` 告诉你“想读什么属性”，  
> **`.` 前面那个东西**才是你要检查的 undefined 来源。
==**调试顺序永远是：先看报错 → 找到 `.` 前面的表达式 → 打印它 → 确认它是不是 null/undefined → 再往前找为什么。**==

#### 函数返回值 + 常见报错速查

**看懂函数只问：有没有 `return`？返回的是什么？调用处拿这个返回值干了什么？**

##### 2. 常见报错速查表

|报错|含义|先查什么|
|---|---|---|
|`ReferenceError: xxx is not defined`|变量不存在|拼写、作用域、有没有声明|
|`TypeError: Cannot read properties of undefined (reading 'xxx')`|读 undefined 的属性|`.` 前面的表达式|
|`TypeError: Cannot read properties of null (reading 'xxx')`|读 null 的属性|`.` 前面的表达式|
|`TypeError: xxx is not a function`|把不是函数的东西当函数调用|函数名拼写、变量类型|
|`SyntaxError`|语法写错|括号、引号、逗号|
|`NaN`（不是报错）|数学运算出问题|参与运算的值是不是 undefined/字符串|

##### 3. 看懂一段真实业务代码的步骤

1. 先找**数据**：变量和对象长什么样。
    
2. 再找**函数**：参数是什么，返回什么。
    
3. 再看**流程**：`if` 走哪边，`for` 循环几次。
    
4. 最后看**输出**：打印什么、渲染什么、调用什么接口。
    
5. 不确定就 `console.log`，不要心算。