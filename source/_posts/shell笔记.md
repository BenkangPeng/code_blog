---
title: shell笔记
tags: linux
abbrlink: c3711ab9
date: 2022-03-06 08:58:17
---

shell笔记

<!-- more -->

- ### 声明脚本执行器

```shell
#!/usr/bin/bash
```

可以用命令`which bash`查看`bash`位置

- ### shell传递参数

```shell
./test.sh 1 2 3 4
```

`$1`接受参数`1` , `$2`接受参数2……

- ### 算数运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，`expr` 最常用。

```shell
a=$1
b=$2
val=`expr $a + $b`
echo "a + b = $val"
val=`expr $a - $b`
echo "a - b = $val"
val=`expr $a \* $b`
echo "a * b = $val"
val=`expr $a / $b`
echo "a / b = $val"

if [ $a == $b ]
then
    echo "a is equal to b"
fi
if [ $a != $b ]  #严格遵循空格
then 
    echo "a isn't equal to b"
fi
```

**乘法需用`\*`转义表示**



### **关系运算符**

```shell
if [ $a -eq $b ]
then
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
```

### **显示命令执行结果**

```shell
echo `date`
```



### **shell输入**

```shell
read name
echo "$name is excellent!"
```

### for循环

```shell
for loop in 1 2 3 4 5
do 
    echo "loop is $loop"
done
```

### while循环

```shell
int=5
while [ $int -ge 0 ]
do
    echo "int = $int"
    let "int--"
done
```

### sed -i

`sed`是一个在Unix和类Unix环境中使用的流编辑器，它可以对输入的文本（文件或管道）进行基本的文本转换。

```shell
sed -i 's/old_string/new_string/g' filename
```

- `-i`：表示直接修改文件内容，而不是输出到标准输出。
- `s`：表示替换操作。
- `old_string`：要被替换的字符串。
- `new_string`：用于替换的新字符串。
- `g`：表示全局替换，即一行中的所有匹配都会被替换。
- `filename`：要进行替换的文件名。

```shell
sed -i '10c apple' banana.txt
```

- 将`banana.txt`的第十行修改成`apple`.

### grep命令

正则匹配字符串

```shell
benkangpeng@DESKTOP-FR84659:~/helloworld$ ls | grep ".v"
count4.v
count4_tb.v
counter.v
shift_register.v
```

↑匹配显示所有`.v`后缀的文件

