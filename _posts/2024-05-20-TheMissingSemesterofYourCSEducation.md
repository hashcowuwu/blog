---
layout: post
title: The Missing Semester of Your CS Education 通关记录 
date: 2023-05-20
Author: Shengbin 
tags: [Linux]
comments: true
toc: true
---

## The Missing Semester of Your CS Education  通关记录

#### 本机环境

```shell
root@tux:~# neofetch 
            .-/+oossssoo+/-.               root@tux 
        `:+ssssssssssssssssss+:`           -------- 
      -+ssssssssssssssssssyyssss+-         OS: Ubuntu 22.04.4 LTS x86_64 
    .ossssssssssssssssssdMMMNysssso.       Host: VMware Virtual Platform None 
   /ssssssssssshdmmNNmmyNMMMMhssssss/      Kernel: 6.5.0-35-generic 
  +ssssssssshmydMMMMMMMNddddyssssssss+     Uptime: 9 mins 
 /sssssssshNMMMyhhyyyyhmNMMMNhssssssss/    Packages: 1686 (dpkg), 9 (snap) 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Shell: bash 5.1.16 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Resolution: 2286x1256 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   Terminal: /dev/pts/1 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   CPU: Intel Xeon E5-2673 v3 (2) @ 2.394GHz 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   GPU: 00:0f.0 VMware SVGA II Adapter 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Memory: 1227MiB / 3870MiB 
 /sssssssshNMMMyhhyyyyhdNMMMNhssssssss/
  +sssssssssdmydMMMMMMMMddddyssssssss+                             
   /ssssssssssshdmNNNNmyNMMMMhssssss/                              
    .ossssssssssssssssssdMMMNysssso.
      -+sssssssssssssssssyyyssss+-
        `:+ssssssssssssssssss+:`
            .-/+oossssoo+/-.

```

### 课程概览与 shell

1.打印shell变量

```shell
root@tux:~# echo $SHELL
/bin/bash
```
2.创建文件夹
```shell
root@tux:/tmp# mkdir missing
```

3.查看touch man文件

```shell
root@tux:~# man touch
```
4.创建文件
```shell
root@tux:/tmp/missing# touch semester
```

5.写入文件
```shell 
root@tux:/tmp/missing# vim semester 
root@tux:/tmp/missing# cat semester 
 #!/bin/sh
 curl --head --silent https://missing.csail.mit.edu
```
6.尝试执行文件

```shell
root@tux:/tmp/missing# ./semester
bash: ./semester: Permission denied
root@tux:/tmp/missing# ll
total 12
drwxr-xr-x  2 root root 4096  5月 20 19:17 ./
drwxrwxrwt 22 root root 4096  5月 20 19:17 ../
-rw-r--r--  1 root root   63  5月 20 19:17 semester
```
7～8.改变文件权限使其能执行

```Shell
   25  man chmod
   26  chmod +X semester 
   27  chmod +x semester 
   28  ./semester 
```
通过shebang加载第一个参数`#!/bin/sh`作为接受输入的文件，`/bin/sh` 和 `/bin/bash`的区别在与`/bin/sh`是POSIX SHELL 在不同系统的实现不同，而且无法支持一些高级特性如数组、关联数组、进程替换等。 `/bin/bash` 是 GNU SHELL 兼任POSIX SHELL

9.通过`| > grep`获取文件内容
```shell
root@tux:/tmp/missing# ./semester | grep last-modified >  last-modified.txt
root@tux:/tmp/missing# cat last-modified.txt 
last-modified: Sat, 18 May 2024 14:18:01 GMT
```

10.通过文件查看CPU温度

虚拟机中无法获取传感器所以这个实验是在ArchLinux中完成的

```shell
virtual/thermal/thermal_zone0🔒 
❯ cat temp
51000
```

### Shell 工具和脚本


1.阅读 man ls ，然后使用ls 命令进行如下操作：所有文件（包括隐藏文件）文件打印以人类可以理解的格式输出 (例如，使用454M 而不是 454279954)文件以最近访问顺序排序 以彩色文本显示输出结果

```shell
root@tux:~# ll -hat
total 52K
drwx------  8 root root 4.0K  5月 20 19:52 ./
drwxr-xr-x  2 root root 4.0K  5月 20 19:52 code/
-rw-------  1 root root   64  5月 20 19:39 .lesshst
-rw-------  1 root root 1.6K  5月 20 19:17 .viminfo
drwxr-xr-x  3 root root 4.0K  5月 20 19:11 .config/
drwxr-xr-x  4 root root 4.0K  5月 20 19:10 .terminfo/
drwxr-xr-x  3 root root 4.0K  5月 20 19:10 .local/
drwx------  2 root root 4.0K  5月 20 19:10 .cache/
-rw-------  1 root root    6  5月 20 19:07 .bash_history
drwx------  5 root root 4.0K  5月 20 19:00 snap/
drwxr-xr-x 20 root root 4.0K  5月 20 18:52 ../
-rw-r--r--  1 root root 3.1K 10月 15  2021 .bashrc
-rw-r--r--  1 root root  161  7月  9  2019 .profile
```

2.编写两个bash函数 marco 和 polo 执行下面的操作。 每当你执行 marco 时，当前的工作目录应当以某种形式保存，当执行 polo 时，无论现在处在什么目录下，都应当 cd 回到当时执行 marco 的目录。 为了方便debug，你可以把代码写在单独的文件 marco.sh 中，并通过 source marco.sh命令，（重新）加载函数。

```shell
#! /bin/sh

marco() {
	echo "$(pwd)" > $HOME/marco_history.log
	echo "save pwd $(pwd)"
}

polo(){
	cd "$(cat "$HOME/marco_history.log")" 
}
root@tux:~/code# source marco.sh 
```


3.测试脚本多少次内能正常运行


