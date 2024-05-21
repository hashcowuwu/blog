---
layout: post
title: 正则表达式学习笔记`` 
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
限制当前的字符只能被a,b,c所匹配
[^abc]
限制当前的字符只能被a,b,c所匹配

```



参考
[regexone](https://regexone.com/)
