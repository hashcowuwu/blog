---
layout: post
title: Learn C The Hard Way通关记录 
date: 2023-05-23
Author: Shengbin 
tags: [c]
comments: true
toc: true
---


## 本机环境

注意本课程在archlinux上可能需要额外配置故使用ubuntu来完成,因为已经学过一次c了所以略过部分内容


```shell
            .-/+oossssoo+/-.               root@tux 
        `:+ssssssssssssssssss+:`           -------- 
      -+ssssssssssssssssssyyssss+-         OS: Ubuntu 22.04.4 LTS x86_64 
    .ossssssssssssssssssdMMMNysssso.       Host: VMware Virtual Platform None 
   /ssssssssssshdmmNNmmyNMMMMhssssss/      Kernel: 6.5.0-35-generic 
  +ssssssssshmydMMMMMMMNddddyssssssss+     Uptime: 3 mins 
 /sssssssshNMMMyhhyyyyhmNMMMNhssssssss/    Packages: 1751 (dpkg), 9 (snap) 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Shell: fish 3.3.1 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Resolution: 1021x1256 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   Terminal: /dev/pts/0 
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   CPU: Intel Xeon E5-2673 v3 (2) @ 2.394GHz 
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   GPU: 00:0f.0 VMware SVGA II Adapter 
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Memory: 547MiB / 3870MiB 
 /sssssssshNMMMyhhyyyyhdNMMMNhssssssss/
  +sssssssssdmydMMMMMMMMddddyssssssss+                             
   /ssssssssssshdmNNNNmyNMMMMhssssss/                              
    .ossssssssssssssssssdMMMNysssso.
      -+sssssssssssssssssyyyssss+-
        `:+ssssssssssssssssss+:`
            .-/+oossssoo+/-.

```

## 用make来替代python

简而言之

```shell
cd ~/code 
vim test.c 
#include <stdio.h>

int main() {
	puts("Hello world");
	return 0;
}
make test 
```


如何编写一个Makefile 文件？  

简单的皮包,定义编译指令，定义最终结果

```Makefile
CFLAGS=-Wall -g

all:ex4

clean:
        rm -f ex4
```

## Valgrind

Valgrind是一个用于内存调试、内存泄漏检测以及性能分析的软件工具


有程序

```shell
root@tux ~/c/valgrind# cat ex3.c 
#include <stdio.h>

int main(){
	int age = 10;
	int height;

	printf("I am %d years old.\n");
	printf("I am %d inches tall.\n",height);

	return 0;
}
```

通过valgrind 运行调试一下


```
root@tux ~/c/valgrind# valgrind ./ex3 
```




