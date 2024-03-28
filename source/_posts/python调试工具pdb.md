---
title: python调试工具pdb
tags: Python
abbrlink: 9e73dd3c
date: 2024-03-06 09:27:24
---

`pdb`是一个内置于`Python`的调试工具，使用方法与`gdb`类似。

<!-- more -->

* 启动

```python
python -m pdb test.py
```

* 步进

不同于`gdb`需要输入`start`开始运行，进入`pdb`后程序已经开始运行。

`l`: list 列出源码

`ll`: list all

回车重复上一条命令

`n`: next下一步

`s`: step下一步，可进入函数

* 断点

`b <linenum>`: 在第`linenum`行设置断点

`b`:输出所有断点信息

`disable <Num>`:禁用断点

`c`: continue持续运行程序，直到断点

* 打印

`whatis <variable>`查看变量属性

`p <variable>`打印变量

