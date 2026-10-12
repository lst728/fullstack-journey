 WSL Ubuntu 是 Windows 里的 Linux 子系统。
 你的 Windows 电脑
├─ PowerShell        ← Windows 自带的命令行终端
├─ WSL2
│   └─ Ubuntu        ← 你练 Linux 命令的完整 Linux 环境
└─ Docker Desktop
    └─ Docker 引擎   ← 通常也借助 WSL2 后端运行
        └─ ubuntu 容器 ← 临时、隔离的沙盒
 ## 常用命令练习路线

1. 文件和目录
      pwd、ls、cd、mkdir、touch、cp、mv、rm、tree、file、stat
2. 查看和编辑
      cat、less、head、tail、echo、nano、vim
3. 重定向和管道
**>、>>、2>、|
4. 查找和文本处理
      find、grep、wc、sort、uniq、cut、tr、sed、awk
5. 权限和用户
      chmod、chown、umask、useradd、passwd、su、id
6. 进程管理
      ps、top、htop、kill、jobs、bg、fg、&
7. 网络
      ip、ping、curl、wget、ss、netstat
8. 压缩和包管理
      tar、gzip、zip、unzip、apt update、apt install、apt remove
    
9. Shell 脚本
      #!/bin/bash、变量、if、for、chmod +x、./script.sh

# Linux 常用命令练习（WSL Ubuntu / Docker Ubuntu）

> 适用环境：Windows + WSL2 Ubuntu。  
> 也可以把命令复制到 Docker Ubuntu 容器里练习；容器里通常默认是 root，可以去掉 `sudo`。  
> 注意：`rm -rf` 很危险，执行前一定确认路径。

---

## 0. 进入练习环境

在 Windows PowerShell 中执行：

```powershell
wsl -d Ubuntu
```

进入 Ubuntu 后，先安装常用工具：

```bash
sudo apt update
# 更新软件包索引

sudo apt install -y vim nano curl wget git tree htop iproute2 iputils-ping net-tools procps less man-db sudo unzip zip
# 安装练习常用工具
```

---

## 1. 进入练习目录

```bash
cd ~
# 回到家目录，~ 表示当前用户的家目录

mkdir -p linux-practice/{docs,logs,backup,scripts}
# 创建练习目录和四个子目录

cd linux-practice
# 进入练习目录

pwd
# 显示当前所在路径

ls -la
# 列出当前目录所有文件，包括隐藏文件

tree
# 树状显示目录结构
```

---

## 2. 文件与目录基础

```bash
touch docs/readme.txt
# 创建一个空文件

echo "Hello Linux" > docs/readme.txt
# 把 Hello Linux 写入文件，> 表示覆盖原内容

echo "第二行" >> docs/readme.txt
# 追加一行到文件末尾，>> 表示追加

cat docs/readme.txt
# 显示文件全部内容

cp docs/readme.txt docs/readme.bak
# 复制文件

mv docs/readme.bak logs/
# 移动文件到 logs 目录，也可以用来重命名

ls -l docs logs
# 查看两个目录里的文件详情

rm logs/readme.bak
# 删除文件
```

---

## 3. 查看与编辑文件

```bash
head -n 1 docs/readme.txt
# 查看文件前 1 行

tail -n 1 docs/readme.txt
# 查看文件后 1 行

less docs/readme.txt
# 分页查看文件，按 q 退出

nano docs/readme.txt
# 用 nano 编辑文件
# Ctrl + O 保存，回车确认
# Ctrl + X 退出
```

---

## 4. 重定向与管道

```bash
echo "apple" > fruits.txt
# 覆盖写入 apple

echo "banana" >> fruits.txt
# 追加 banana

echo "apple" >> fruits.txt
# 再追加一个 apple

cat fruits.txt
# 查看文件内容

cat fruits.txt | sort
# 把内容排序，| 是管道，把左边结果交给右边命令

cat fruits.txt | sort | uniq -c
# 排序后去重，并统计每个词出现次数

cat fruits.txt | grep apple
# 过滤出包含 apple 的行
```

---

## 5. 查找与文本处理

```bash
find . -type f
# 查找当前目录下所有普通文件

find . -name "*.txt"
# 查找所有 .txt 文件

grep -r "Linux" .
# 递归搜索当前目录，找出包含 Linux 的文件

wc -l fruits.txt
# 统计文件行数

sort fruits.txt | uniq
# 排序后去重

tr 'a-z' 'A-Z' < fruits.txt
# 把小写字母转换成大写

sed 's/apple/orange/' fruits.txt
# 把每行第一个 apple 替换成 orange，只输出到屏幕，不改文件

awk '{print NR, $0}' fruits.txt
# 输出行号和整行内容
```

---

## 6. 权限与用户

```bash
touch scripts/hello.sh
# 创建脚本文件

ls -l scripts/hello.sh
# 查看权限，通常显示 -rw-r--r--

chmod +x scripts/hello.sh
# 增加可执行权限

ls -l scripts/hello.sh
# 再看权限，应该变成 -rwxr-xr-x

umask
# 查看默认权限掩码

id
# 查看当前用户、用户组信息

whoami
# 查看当前用户名
```

可选：练习创建用户。

```bash
sudo useradd -m testuser
# 创建用户 testuser，并创建家目录

sudo passwd testuser
# 给 testuser 设置密码

su - testuser
# 切换到 testuser

whoami
# 应该显示 testuser

exit
# 退回原来的用户

sudo userdel -r testuser
# 删除 testuser 和它的家目录
```

---

## 7. 进程管理

```bash
sleep 1000 &
# 后台运行 sleep 1000，& 表示放到后台

jobs
# 查看当前后台任务

ps aux | grep sleep
# 查看系统中和 sleep 相关的进程

kill %1
# 杀掉 1 号后台任务

jobs
# 再查看，应该已经结束
```

---

## 8. 网络命令

```bash
ip addr
# 查看网卡和 IP 地址

ping -c 4 8.8.8.8
# ping 4 次，测试网络连通性

curl -I https://www.example.com
# 查看 HTTP 响应头

wget -O example.html https://www.example.com
# 下载网页并保存为 example.html

ls -lh example.html
# 查看文件大小
```

---

## 9. 压缩与解压

```bash
tar -czf backup/practice.tar.gz docs logs fruits.txt
# 把 docs、logs、fruits.txt 打包并压缩成 practice.tar.gz

tar -tzf backup/practice.tar.gz
# 查看压缩包内容，不解压

mkdir -p restore
# 创建解压目录

tar -xzf backup/practice.tar.gz -C restore
# 解压到 restore 目录

ls -R restore
# 递归查看解压后的内容
```

---

## 10. 包管理

```bash
sudo apt update
# 更新软件包索引

apt search tree
# 搜索 tree 软件包

sudo apt install -y cowsay
# 安装 cowsay 小工具

cowsay "Hello Linux"
# 运行 cowsay

sudo apt remove -y cowsay
# 卸载 cowsay

sudo apt autoremove -y
# 自动清理不再需要的依赖
```

---

## 11. Shell 脚本入门

```bash
cat > scripts/backup.sh <<'EOF'
#!/bin/bash
date_str=$(date +%F)
tar -czf ~/linux-practice/backup/backup-$date_str.tar.gz ~/linux-practice/docs ~/linux-practice/logs
echo "备份完成：backup-$date_str.tar.gz"
EOF
# 创建一个备份脚本

chmod +x scripts/backup.sh
# 给脚本增加执行权限

./scripts/backup.sh
# 运行脚本

ls -lh backup
# 查看生成的备份文件
```

---

## 12. 清理练习目录（可选）

确认你不再需要这些练习文件后，再执行：

```bash
cd ~
# 回到家目录

rm -rf ~/linux-practice
# 删除整个练习目录
# 注意：rm -rf 很危险，确认路径正确再执行
```

---

## 13. 学习建议

- 每做一步，都用 `ls`、`tree`、`cat` 看看结果。
- 想查命令用法，用：

```bash
man ls
man grep
man tar
```
---

## 14. C/C++ 编译与调试常用命令

> 以下命令在 WSL Ubuntu 中练习。  
> 如果在 Docker Ubuntu 容器里，默认是 root，可以去掉 `sudo`。

---

### 14.1 安装编译工具

```bash
sudo apt update
# 更新软件包索引

sudo apt install -y build-essential gdb
# build-essential 包含 gcc、g++、make、libc6-dev 等
# gdb 是调试器
```

检查是否安装成功：

```bash
gcc --version
# 查看 gcc 版本

g++ --version
# 查看 g++ 版本

make --version
# 查看 make 版本

gdb --version
# 查看 gdb 版本
```

---

### 14.2 编写第一个 C 程序

```bash
cd ~/linux-practice
# 进入之前的练习目录

mkdir -p c-practice
# 创建 C 语言练习目录

cd c-practice
# 进入目录

cat > hello.c <<'EOF'
#include <stdio.h>

int main(void) {
    printf("Hello, C!\n");
    return 0;
}
EOF
# 创建 hello.c
```

查看文件：

```bash
cat hello.c
# 显示 hello.c 内容

ls -l hello.c
# 查看文件权限和大小
```

---

### 14.3 编译并运行

```bash
gcc hello.c -o hello
# 把 hello.c 编译成可执行文件 hello

ls -l hello
# 查看生成的可执行文件

./hello
# 运行当前目录下的 hello
# ./ 表示当前目录
```

常用编译选项：

```bash
gcc -Wall -Wextra -g -std=c11 hello.c -o hello
# -Wall：开启大部分警告
# -Wextra：开启额外警告
# -g：生成调试信息，方便 gdb 调试
# -std=c11：使用 C11 标准
# -o hello：输出文件名为 hello

./hello
# 运行程序
```

---

### 14.4 分步编译：预处理、编译、汇编、链接

C 程序从源码到可执行文件大致过程：

```text
hello.c  ->  hello.i  ->  hello.s  ->  hello.o  ->  hello
  源码       预处理      汇编       目标文件     可执行文件
```

#### 1）预处理

```bash
gcc -E hello.c -o hello.i
# -E 只做预处理，展开头文件和宏
# 生成 hello.i
```

查看：

```bash
head -n 20 hello.i
# 查看预处理结果前 20 行
```

#### 2）编译为汇编

```bash
gcc -S hello.c -o hello.s
# -S 把 C 代码编译成汇编代码
# 生成 hello.s
```

查看：

```bash
head -n 30 hello.s
# 查看汇编代码
```

#### 3）汇编为目标文件

```bash
gcc -c hello.c -o hello.o
# -c 只编译和汇编，不链接
# 生成目标文件 hello.o
```

也可以用汇编器：

```bash
as hello.s -o hello.o
# 把汇编文件 hello.s 汇编成 hello.o
```

#### 4）链接为可执行文件

```bash
gcc hello.o -o hello
# 把目标文件链接成可执行文件

./hello
# 运行
```

查看文件类型：

```bash
file hello.c hello.i hello.s hello.o hello
# file 查看文件类型
```

---

### 14.5 多文件编译

创建头文件：

```bash
cat > math_utils.h <<'EOF'
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

int add(int a, int b);

#endif
EOF
# 创建 math_utils.h
```

创建实现文件：

```bash
cat > math_utils.c <<'EOF'
#include "math_utils.h"

int add(int a, int b) {
    return a + b;
}
EOF
# 创建 math_utils.c
```

创建主程序：

```bash
cat > main.c <<'EOF'
#include <stdio.h>
#include "math_utils.h"

int main(void) {
    int result = add(1, 2);
    printf("1 + 2 = %d\n", result);
    return 0;
}
EOF
# 创建 main.c
```

一次性编译多个源文件：

```bash
gcc main.c math_utils.c -o app
# 把多个 .c 文件一起编译链接成 app

./app
# 运行
```

分步编译：

```bash
gcc -c main.c -o main.o
# 编译 main.c

gcc -c math_utils.c -o math_utils.o
# 编译 math_utils.c

gcc main.o math_utils.o -o app
# 链接两个目标文件

./app
# 运行
```

---

### 14.6 指定头文件目录：-I

如果头文件和源文件分开存放：

```bash
mkdir -p include src
# 创建 include 和 src 目录

mv math_utils.h include/
# 把头文件移动到 include

mv main.c math_utils.c src/
# 把源文件移动到 src

gcc -Iinclude src/main.c src/math_utils.c -o app
# -Iinclude：告诉编译器去 include 目录找头文件

./app
# 运行
```

---

### 14.7 静态库

静态库一般以 `.a` 结尾，链接时会被复制进可执行文件。

```bash
cd ~/linux-practice/c-practice
# 回到 C 练习目录

gcc -c math_utils.c -o math_utils.o
# 先生成目标文件

ar rcs libmathutils.a math_utils.o
# ar 创建静态库
# rcs：r 插入，c 创建，s 生成索引

gcc main.c -L. -lmathutils -o app_static
# -L.：在当前目录找库
# -lmathutils：链接 libmathutils.a
# 注意名字规则：-lmathutils 对应 libmathutils.a

./app_static
# 运行静态链接程序
```

查看静态库内容：

```bash
ar t libmathutils.a
# 列出静态库中的目标文件
```

---

### 14.8 动态库

动态库一般以 `.so` 结尾，运行时才加载。

```bash
gcc -fPIC -c math_utils.c -o math_utils.o
# -fPIC：生成位置无关代码，动态库通常需要

gcc -shared -o libmathutils.so math_utils.o
# -shared：生成动态库

gcc main.c -L. -lmathutils -o app_shared
# 链接动态库

LD_LIBRARY_PATH=. ./app_shared
# 临时告诉系统在当前目录找动态库
# 否则可能报错：error while loading shared libraries

ldd app_shared
# 查看可执行文件依赖哪些动态库
```

如果同目录同时有 `libmathutils.a` 和 `libmathutils.so`，链接器默认可能优先选动态库。  
想明确使用动态库，可以：

```bash
gcc main.c -L. -Wl,-Bdynamic -lmathutils -o app_shared
# -Wl,-Bdynamic：告诉链接器优先使用动态库
```

也可以写死运行时搜索路径：

```bash
gcc main.c -L. -lmathutils -Wl,-rpath,'$ORIGIN' -o app_shared
# -Wl,-rpath,'$ORIGIN'：程序运行时在可执行文件所在目录找库

./app_shared
# 直接运行
```

---

### 14.9 Makefile 入门

创建 `Makefile`：

```make
CC = gcc
CFLAGS = -Wall -Wextra -g -std=c11
TARGET = app
OBJS = main.o math_utils.o

$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)
```

注意：`$(CC)`、`$(CFLAGS)` 等命令行前面必须是 **Tab**，不能是空格。

使用：

```bash
make
# 根据 Makefile 编译

./app
# 运行

make clean
# 删除编译生成的文件
```

Makefile 变量说明：

```text
CC      编译器，比如 gcc
CFLAGS  编译选项
TARGET  最终可执行文件名
OBJS    目标文件列表
$@      当前规则的目标文件
$<      当前规则的第一个依赖文件
$^      当前规则的所有依赖文件
```

---

### 14.10 GDB 调试基础

编译时加 `-g`：

```bash
gcc -g hello.c -o hello_g
# 生成带调试信息的可执行文件

gdb -q ./hello_g
# -q 表示安静模式，进入 gdb
```

进入 gdb 后常用命令：

```text
break main        # 在 main 函数处下断点
run               # 运行程序
next              # 单步执行，不进入函数
step              # 单步执行，进入函数
print 变量名       # 打印变量值
continue          # 继续运行到下一个断点
list              # 查看源代码
quit              # 退出 gdb
```

也可以直接在命令行执行：

```bash
gdb -q ./hello_g -ex "break main" -ex "run" -ex "next" -ex "quit"
# 自动断在 main，运行一步后退出
```

---

### 14.11 常用辅助工具

```bash
file hello
# 查看文件类型

ldd hello
# 查看可执行文件依赖的动态库

nm hello.o
# 查看目标文件中的符号

objdump -d hello
# 反汇编可执行文件

objdump -d hello | head -n 40
# 只看反汇编前 40 行

size hello
# 查看可执行文件各段大小

strings hello
# 查看文件中的可打印字符串

strings hello | head -n 20
# 只看前 20 条字符串
```

---

### 14.12 C++ 编译命令

安装 `g++` 通常已经包含在 `build-essential` 中。

创建 C++ 文件：

```bash
cat > hello.cpp <<'EOF'
#include <iostream>

int main() {
    std::cout << "Hello, C++!" << std::endl;
    return 0;
}
EOF
# 创建 hello.cpp
```

编译运行：

```bash
g++ hello.cpp -o hello_cpp -std=c++17 -Wall -Wextra -g
# 使用 C++17 标准编译

./hello_cpp
# 运行
```

分步编译和 C 类似：

```bash
g++ -E hello.cpp -o hello.ii
# 预处理

g++ -S hello.cpp -o hello.s
# 编译为汇编

g++ -c hello.cpp -o hello.o
# 汇编为目标文件

g++ hello.o -o hello_cpp
# 链接
```

---

### 14.13 常见错误与排查

#### 1）`gcc: command not found`

```bash
sudo apt update
sudo apt install -y build-essential
# 安装编译工具
```

#### 2）`fatal error: xxx.h: No such file or directory`

```bash
gcc -I头文件目录 main.c -o app
# 用 -I 指定头文件搜索路径
```

#### 3）`undefined reference to ...`

通常是源文件或库没链接：

```bash
gcc main.c math_utils.c -o app
# 把相关源文件都加上

gcc main.c -L. -lmathutils -o app
# 或者链接对应库
```

#### 4）`error while loading shared libraries: libxxx.so`

运行时找不到动态库：

```bash
LD_LIBRARY_PATH=. ./app
# 临时指定动态库路径
```

或编译时加 rpath：

```bash
gcc main.c -L. -lmathutils -Wl,-rpath,'$ORIGIN' -o app
# 让程序在自身目录找动态库
```

#### 5）`Permission denied`

```bash
chmod +x app
# 增加可执行权限

./app
# 用 ./ 运行当前目录程序
```

#### 6）程序崩溃、段错误

```bash
gcc -g main.c -o app
# 加 -g 生成调试信息

gdb ./app
# 用 gdb 调试
```

---

### 14.14 C 编译练习任务

按顺序完成：

1. 写 `hello.c`，用 `gcc` 编译并运行。
2. 用 `-E`、`-S`、`-c` 分步生成 `hello.i`、`hello.s`、`hello.o`。
3. 写 `main.c`、`math_utils.c`、`math_utils.h`，多文件编译。
4. 把 `math_utils.c` 做成静态库 `libmathutils.a`。
5. 把 `math_utils.c` 做成动态库 `libmathutils.so`。
6. 写一个 `Makefile`，用 `make` 和 `make clean`。
7. 用 `gdb` 给程序下断点、单步执行、查看变量。
8. 用 `file`、`ldd`、`nm`、`objdump`、`size`、`strings` 查看生成的文件。
- 按 `q` 退出手册。
- Docker Ubuntu 容器里通常直接是 root，很多命令可以去掉 `sudo`。
- 不要随便在重要目录执行 `rm -rf`，尤其是挂载的 Windows 目录。