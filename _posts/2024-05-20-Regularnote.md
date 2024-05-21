---
layout: post
title: 正则表达式学习笔记 
date: 2023-05-21
Author: Shengbin 
tags: [杂项]
comments: true
toc: true
---


正则表达式简述

正则(Regular)我理解他是一个接受文本流的确定性有限状态自动机（Deterministic Finite Automaton，DFA），通过编写状态转移条件来匹配文本

一些语法


```shell
\d 
0～9数字
\. 
任何字符(letter, digit, whitespace, everything) 
[abc]
abc所匹配
[^abc]
非abc所匹配
[a-z]
a~z的所有字符
[^a-z]
非a-z匹配
a{8} a{2,8}
8个a或者2-8个a
\d*
任何数量的\d字符
\d+
一个\d字符
?
匹配可选文字如
\d+ files? found\?
match 	1 file found?	Success
match 	2 files found? 	Success
match 	24 files found? 	Success
skip 	No files found.
\s
匹配空格
^Mission: successful$
匹配以 Mission successful为开头和结束的字符串
(buh)
捕获部分内容而不是全部内容
(\D*\s*(\d*))
嵌套捕获内容，捕获之间一定要写成嵌套形式
I love (cats|dogs)
或匹配
```




参考
[regexone](https://regexone.com/)
