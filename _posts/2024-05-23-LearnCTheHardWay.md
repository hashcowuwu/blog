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
==4210== Memcheck, a memory error detector
==4210== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==4210== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==4210== Command: ./ex3
==4210== 
I am -16776568 years old.
==4210== Conditional jump or move depends on uninitialised value(s)
==4210==    at 0x48E1AD6: __vfprintf_internal (vfprintf-internal.c:1516)
==4210==    by 0x48CB79E: printf (printf.c:33)
==4210==    by 0x109188: main (ex3.c:8)
==4210== 
==4210== Use of uninitialised value of size 8
==4210==    at 0x48C52EB: _itoa_word (_itoa.c:177)
==4210==    by 0x48E0ABD: __vfprintf_internal (vfprintf-internal.c:1516)
==4210==    by 0x48CB79E: printf (printf.c:33)
==4210==    by 0x109188: main (ex3.c:8)
==4210== 
==4210== Conditional jump or move depends on uninitialised value(s)
==4210==    at 0x48C52FC: _itoa_word (_itoa.c:177)
==4210==    by 0x48E0ABD: __vfprintf_internal (vfprintf-internal.c:1516)
==4210==    by 0x48CB79E: printf (printf.c:33)
==4210==    by 0x109188: main (ex3.c:8)
==4210== 
==4210== Conditional jump or move depends on uninitialised value(s)
==4210==    at 0x48E15C3: __vfprintf_internal (vfprintf-internal.c:1516)
==4210==    by 0x48CB79E: printf (printf.c:33)
==4210==    by 0x109188: main (ex3.c:8)
==4210== 
==4210== Conditional jump or move depends on uninitialised value(s)
==4210==    at 0x48E0C05: __vfprintf_internal (vfprintf-internal.c:1516)
==4210==    by 0x48CB79E: printf (printf.c:33)
==4210==    by 0x109188: main (ex3.c:8)
==4210== 
I am 0 inches tall.
==4210== 
==4210== HEAP SUMMARY:
==4210==     in use at exit: 0 bytes in 0 blocks
==4210==   total heap usage: 1 allocs, 1 frees, 1,024 bytes allocated
==4210== 
==4210== All heap blocks were freed -- no leaks are possible
==4210== 
==4210== Use --track-origins=yes to see where uninitialised values come from
==4210== For lists of detected and suppressed errors, rerun with: -s
==4210== ERROR SUMMARY: 5 errors from 5 contexts (suppressed: 0 from 0)
```


```shell
    ==4210== Memcheck, a memory error detector：表明正在使用Memcheck工具。

    ==4210== Command: ./ex3：表明正在运行的程序是./ex3。

    ==4210== Conditional jump or move depends on uninitialised value(s)：这表明程序中的条件跳转或移动依赖于未初始化的值。

    ==4210== Use of uninitialised value of size 8：这表明程序使用了未初始化的值，大小为8字节。

    ==4210== at 0x48E1AD6: __vfprintf_internal (vfprintf-internal.c:1516)：这是错误发生时的函数调用栈信息，指出了错误发生在__vfprintf_internal函数的第1516行。

    ==4210== HEAP SUMMARY:：这部分总结了程序运行结束时堆内存的使用情况。

    ==4210== All heap blocks were freed -- no leaks are possible：表明程序没有内存泄漏。

    ==4210== Use --track-origins=yes to see where uninitialised values come from：建议使用--track-origins=yes选项来追踪未初始化值的来源。

    ==4210== ERROR SUMMARY: 5 errors from 5 contexts (suppressed: 0 from 0)：总结了检测到的错误总数，共有5个错误，每个错误对应一个上下文，没有被抑制的错误。

根据这些信息，可以推断出程序ex3在执行printf函数时，尝试打印一个未初始化的变量，导致了一系列的未初始化值使用错误。这通常意味着程序中有一个变量在使用前没有被正确初始化。为了解决这个问题，需要检查程序中与打印相关的代码，确保所有变量在使用前都已经初始化。
```




