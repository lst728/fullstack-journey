[[JS 地基学习]]
## 函数声明 vs 函数表达式
`function add()` 是声明；`const add = function()` 是表达式。

**调试坑：** 报 `Cannot access 'xxx' before initialization`，通常是函数表达式或 `const/let` 变量在定义前被用了。
## 参数：形参、实参、少传、多传
