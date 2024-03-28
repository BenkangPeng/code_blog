---
title: Gdb学习笔记
abbrlink: a06a32e
date: 2022-10-26 23:26:49
tags:
---

以前学过一个C的调试工具`gdb`，重新看了一下资料复习。资料是`LinuxC`书上的一个章节，当然这本书也是十分推荐。[Download Pdf](https://link.zhihu.com/?target=http%3A//staff.ustc.edu.cn/~guoyan/os12/LinuxC.pdf)    [View Online](https://akaedu.github.io/book/)  [相关讨论](https://www.zhihu.com/question/34069391)

<!-- more -->

# 单步执行

```cpp
//示例程序
#include <stdio.h>
int add_range(int low, int high)
{
    int i, sum;
    for (i = low; i <= high; i++)
        sum = sum + i;
    return sum;
}
int main(void)
{
    int result[100];
    result[0] = add_range(1, 10);
    result[1] = add_range(1, 100);
    printf("result[0]=%d\nresult[1]=%d\n", result[0], result[1]);
    return 0;
}
```

* **生成并进入`gdb`**

```shell
$ g++ -g test.cpp -o main 
$ gdb main
```

* **单步执行**
  * `list`列出源程序. `list 23`表示从第23行开始列出源程序。简写为`l`
  * `回车`重复执行上一次的指令
  * `n` 表示`next`下一步
  * `s` ， `step`, 进入被调用函数

```shell
$ start # 开始运行程序
Thread 1 hit Temporary breakpoint 1, main () at test.cpp:12
12          result[0] = add_range(1, 10);
$ n # next单步运行到下一步
(gdb) n
13          result[1] = add_range(1, 100);
#如果我们要进入子函数add_range查看运行过程呢  -> `s`表示step
(gdb) s
add_range (low=1, high=100) at test.cpp:5
5           for (i = low; i <= high; i++)
```

* **打印参数值**
  * `i locals`显示当前栈帧变量信息
  * `p <variable>` 打印变量
  * `set var <variable> = <data>`修改变量

```shell
Thread 1 hit Temporary breakpoint 1, main () at test.cpp:12
12          result[0] = add_range(1, 10);
(gdb) s
add_range (low=1, high=10) at test.cpp:5
5           for (i = low; i <= high; i++)
(gdb) i locals  # info local 显示当前栈帧中的变量
i = 32760
sum = 501247388
#此处就可以发现程序的问题：没有赋初值
(gdb) n
6               sum = sum + i;
(gdb) i locals
i = 1
sum = 501247388
(gdb) set var sum = 0 # 修改sum的值
(gdb) n
5           for (i = low; i <= high; i++)
(gdb) p sum # print sum
$1 = 1 # $1是临时变量，临时显示
(gdb)
```

* **栈帧**
  * `bt`显示栈帧
  * `f <numble>`选中、进入栈帧

```cpp
Thread 1 hit Temporary breakpoint 1, main () at test.c:10
10          int result[5] = {0};
(gdb) n
11          result[0] = Sum(1 , 10) ;
(gdb) s
Sum (low=1, high=10) at test.c:3
3           int i = 0 , sum = 0 ;
(gdb) n
4           for(i = low ; i <= high ; i++){
(gdb)
5               sum += i ;
(gdb) p result
No symbol "result" in current context.
// 当前栈帧没有"result"
(gdb) bt // 显示当前栈帧 backtrace
#0  Sum (low=1, high=10) at test.c:5
#1  0x00000000004015c2 in main () at test.c:11
(gdb) f 1 // frame 1 , 1为bt列出来的栈帧编号
#1  0x00000000004015c2 in main () at test.c:11
11          result[0] = Sum(1 , 10) ;
(gdb) p result
$1 = {0, 0, 0, 0, 0}
```



# 断点调试

```c
#include <stdio.h>
int main(void)
{
    int sum = 0, i = 0;
    char input[8] ;
    while (1) {
        scanf("%s", input);
        for (i = 0; input[i] != '\0'; i++)
        sum = sum*10 + input[i] - '0';
        printf("input=%d\n", sum);
    }
    return 0;
}
```

以上C程序的功能是：输入``字符型``的数字(`最高七位`），输出对应的``整数``，如此循环。

但又是类似的结果，第一次输出正确，之后的输出错误。(其实一眼能看出sum没有置零)

接下来进行`debug`:

* `display <variable>` 设置变量显示 ， `undisplay <variable>`关闭某个变量显示

```cpp
4           int sum = 0, i = 0;
(gdb) n
7               scanf("%s", input);
(gdb)
1234
8               for (i = 0; input[i] != '\0'; i++)
(gdb) display sum	//显示变量
1: sum = 0
(gdb) display input[i]
2: input[i] = 49 '1'
(gdb) n
9               sum = sum*10 + input[i] - '0';
1: sum = 0
2: input[i] = 49 '1'
(gdb) n
8               for (i = 0; input[i] != '\0'; i++)
1: sum = 1
2: input[i] = 49 '1'
(gdb)
9               sum = sum*10 + input[i] - '0';
1: sum = 1
2: input[i] = 50 '2'
(gdb)
8               for (i = 0; input[i] != '\0'; i++)
1: sum = 12
2: input[i] = 50 '2'
```



**就这样跟踪了几个循环似乎没有错误。如果我们不像这样重复跟踪，可以设置断点，让程序自然运行到我们想要停止的位置。**

* `b <lineNumber>`设置断点 , `b` :  `breakpoint` 
* `c` 连续执行，直到断点处 `continue`

```shell
(gdb) c
Continuing.
[New Thread 18408.0x4c14]
input=1234

Thread 1 hit Breakpoint 2, main () at test.c:7
7               scanf("%s", input);
1: sum = 1234
2: input[i] = 0 '\000'
(gdb) n
5678
8               for (i = 0; input[i] != '\0'; i++)
1: sum = 1234
(gdb) n
9               sum = sum*10 + input[i] - '0';
1: sum = 1234
```

**如此便找到问题所在：下一次循环中，sum会在上一次循环输入的基础上进行累加**

只要在每一次输入输出结束后将sum置零即可，如下：

```c
#include <stdio.h>
int main(void)
{
    int sum = 0, i = 0;
    char input[8] ;
    while (1) {
        scanf("%s", input);
        for (i = 0; input[i] != '\0'; i++)
        sum = sum*10 + input[i] - '0';
        printf("input=%d\n", sum);
        sum = 0 ;
    }
    return 0;
}
```

* `i b` 查看所有断点信息`info breakpoints`
* `delete breakpoints <num>` 删除所指的编号断点
* `disable breakpoints <num>`禁用所指编号断点
* `enable breakpoints <num>`
* `run` 从头开始连续而非单步执行程序
* ` b <num> if <command>` 设立触发断点的条件，例如`b 7 if sum != 0`



# 观察点

依然是之前的例子：

```cpp
#include<iostream>
using namespace std ;
int main(){
   char input[5] ;
   int sum ;
   while(1){
    sum = 0 ;
    scanf("%s" , input) ;
    for(int i = 0 ; input[i] != '\0' ; i++){
        sum = sum * 10 + input[i] - '0' ;
    }
    cout << "input = " << sum << endl ;
   } 
   return 0 ;
}
```

**如果我们没有注意到数组长度，输入了长度大于4的字符串，`input`产生了越界会发生什么**

```shell
Thread 1 hit Temporary breakpoint 6, main () at test.cpp:7
7           sum = 0 ;
(gdb) n
8           scanf("%s" , input) ;
(gdb)
12345
9           for(int i = 0 ; input[i] != '\0' ; i++){
(gdb) c
Continuing.
input = 123407
```

**我们发现输出也是不对的 ， 但 ：**

```shell
(gdb) n
8           scanf("%s" , input) ;
(gdb)
12345
9           for(int i = 0 ; input[i] != '\0' ; i++){
(gdb) p input
$13 = "12345"
(gdb) x/8b input # 以byte为单位，打印input后8个byte地址的信息
0x61fe13:       49      50      51      52      53      0       0       0
```

**我们输入`12345`后，查看input内的值，发现按照程序逻辑，`for`循环也会在`i = 5`处结束**

**结果应该仍为12345** ,  **莫非`input[5]有变化？`**

* `watch <varible>`观察点变量被访问修改时，程序中断

```shell
Thread 1 hit Temporary breakpoint 11, main () at test.cpp:7
7           sum = 0 ;
(gdb) n
8           scanf("%s" , input) ;
(gdb)
12345
9           for(int i = 0 ; input[i] != '\0' ; i++){
(gdb) i watchpoints
No watchpoints.
(gdb) watch input [5]
Hardware watchpoint 12: input [5]
(gdb) c
Continuing.

Thread 1 hit Hardware watchpoint 12: input [5]

Old value = 0 '\000'
New value = 1 '\001'
0x0000000000401607 in main () at test.cpp:9
9           for(int i = 0 ; input[i] != '\0' ; i++){
(gdb) c
Continuing.

Thread 1 hit Hardware watchpoint 12: input [5]

Old value = 1 '\001'
New value = 2 '\002'
0x0000000000401607 in main () at test.cpp:9
9           for(int i = 0 ; input[i] != '\0' ; i++){
(gdb) c
Continuing.

Thread 1 hit Hardware watchpoint 12: input [5]

Old value = 2 '\002'
New value = 3 '\003'
0x0000000000401607 in main () at test.cpp:9
9           for(int i = 0 ; input[i] != '\0' ; i++){
```

**我们发现每一次for循环运行时`input[5]`会＋1 ， 可知 i = 5 时 ， sum = 12345 * 10  + 5 - 48  = 123407**

**也就是说`input[5]对应的就是i` , 换句话说，i的地址是 &input[5]**

**修改**：

```cpp
#include<iostream>
using namespace std ;
int main(){
   char input[5] ;
   int sum ;
   while(1){
    sum = 0 ;
    scanf("%s" , input) ;
    for(int i = 0 ; input[i] != '\0' ; i++){
        if(input[i] < '0' || input[i] > '9'){
            cout << "invalid input !" << endl ;
            sum = -1 ;
            break;
        }
        sum = sum * 10 + input[i] - '0' ;
    }
    cout << "input = " << sum << endl ;
   } 
   return 0 ;
}
```