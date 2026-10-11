## 本地建文件夹推git操作
新建文件夹，文件夹地址栏输入 `cmd` 回车，直接打开终端（Windows）
```
git init
```
==🟢作用==：**把当前文件夹变成 Git 本地仓库**，执行完会出现隐藏文件夹 `.git`
git add
```
git add .
```
==🟢作用==：把当前目录**所有改动的文件**加入暂存区
git commit -m "init"
```
git commit -m "init"
```
==🟢作用==：把暂存区文件提交到本地仓库；`-m` 后面双引号里是**提交备注**，init 代表项目初始化
关联远程 GitHub 仓库
```
git remote add origin https://github.com/你的用户名/仓库名.git
```
小检查：输完这条，可以执行下面命令验证是否关联成功
```
git remote -v
```
能看到 origin 对应的 github 地址，就没问题
推送代码到 GitHub
```
git push -u origin master
```
==🟣*补充：终端命令创建（Windows cmd）*==
```
type nul > README.md
```