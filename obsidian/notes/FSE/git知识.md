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
```
E:\fullstack>git push
Enumerating objects: 5, done.
枚举文件对象：5个，完成。
Counting objects: 100% (5/5), done.
统计对象：100%（5/5），完成。
Delta compression using up to 20 threads
差分压缩，最多使用20个线程
Compressing objects: 100% (3/3), done.
压缩对象：100%（3/3），完成。
Writing objects: 100% (4/4), 980 bytes | 490.00 KiB/s, done.
上传对象：100%（4/4），大小980字节，速度490KB/s，完成。
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
总共4个对象（0处改动），复用0个，打包复用0个
To https://github.com/lst728/fullstack-journey.git
推送到远程仓库地址 https://github.com/lst728/fullstack-journey.git
  5c3cdec..cbb766f  master -> master
本地master分支成功推送到远程master分支（两个版本哈希）

E:\fullstack>git status
On branch master
当前在master分支
Your branch is up to date with 'origin/master'.
本地分支和远程仓库origin/master保持同步
nothing to commit, working tree clean
✅ 没有需要提交的内容，工作区干净
```
以后更新笔记固定 3 条命令（在 E:\fullstack 目录执行）
```
git add .
git commit -m "更新笔记：xxx"
git push
```