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

```shell
 #!/usr/bin/bash
 echo > out.log
 for ((count=0;;count++))
 do
     ./buggy.sh &>> out.log #$?：上一个命令的退出状态
     if [[ $? -ne 0 ]]; then
         echo "failed after $count times"
         break

     fi
 done
```

4.递归查找hmtl文件

* 用find自带命令，递归在每条目录执行压缩

```shell
root@tux:~# find . -name "*.html" -type f -exec tar -czvf html_files.tar.gz {} +
./workspace/dir4/34file.html
./workspace/dir4/17file.html
./workspace/dir4/21file.html
./workspace/dir4/9file.html
./workspace/dir4/32file.html
./workspace/dir4/11file.html
```

* 用xargs

xargs 命令从标准输入读取数据，并将这些数据作为参数传递给指定的命令

```shell
find  -name "*.html" -type f -print0  | xargs -0 tar -czvf html_files1.tar.gz
```

5.列出最近是用的文件

```shell
find . -type f -print0 | xargs -0 ls -t
```


### 编辑器 (Vim)

用了很多年vim所以这章速通了，只记录最后一题


#### vim 中的宏

###### 记录宏

选择一个寄存器(a~z),选择a，键入qa开始记录 在记录模式下，执行你想要记录的编辑命令。例如，你可以移动光标、删除文本、插入文本等,操作完成后q 保存

###### 执行宏

要执行你记录的宏，输入 @ 后跟寄存器字母。例如，如果你将宏记录在寄存器 a 中，输入 @a 来执行宏。
重复执行宏：如果你想要多次执行宏，可以使用 @@ 来重复执行最后一次执行的宏，或者使用 `[次数]@[寄存器]` 来指定执行次数。例如，10@a 将执行寄存器 a 中的宏 10 次。

#### Solution-数据清洗


##### sed

sed 是一个基于文本编辑器ed构建的”流编辑器” 。在 sed 中，您基本上是利用一些简短的命令来修改文件，而不是直接操作文件的内容（尽管您也可以选择这样做
grep 也支持正则表达式

以上都是`-E` 选项

1.学习正则表达式


2.统计words文件 (/usr/share/dict/words) 中包含至少三个a 且不以's 结尾的单词个数。这些单词中，出现频率前三的末尾两个字母是什么？ sed的 y命令，或者 tr 程序也许可以帮你解决大小写的问题。共存在多少种词尾两字母组合？还有一个很 有挑战性的问题：哪个组合从未出现过？


##### tr 

tr 是一个 Linux 命令行工具，用于字符转换或删除。它是 "translate" 或 "type" 的缩写。tr 命令通常用于将输入数据中的一组字符转换为另一组字符，或者删除指定的字符。tr 命令非常简单，但功能强大，特别适合处理文本数据。

```shell
tr [选项] 集合1 [集合2]
```

小坑

```shell 
grep -E "(.*+a{3}.*)$"
```
这样是不行因为`.*`会吞噬所有的字符

```shell
cat /usr/share/dict/words | tr "A-Z" "a-z" | grep -E "^([^a]*a){3}.*$" | grep -v "'s" | wc -l
```

3. sed特性

sed会在处理是会尝试清除目标文件缓存

4. 查看之前三次重启启动信息中不同的部分(参见 journalctl的-b 选项)。将这一任务分为几个步骤，首先获取之前三次启动的启动日志，也许获取启动日志的命令就有合适的选项可以帮助您提取前三次启动的日志，亦或者您可以使用sed '0,/STRING/d' 来删除STRING匹配到的字符串前面的全部内容。然后，过滤掉每次都不相同的部分，例如时间戳。下一步，重复记录输入行并对其计数(可以使用uniq )。最后，删除所有出现过3次的内容（因为这些内容是三次启动日志中的重复部分）





