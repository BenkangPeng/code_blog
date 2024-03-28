---
title: Instruction Of Typora
tags: Software
abbrlink: bfc69175
date: 2021-10-01 19:00:00
---



<p>Typora是一个基于markdown语法的轻量级软件，以下是我在学习markdown语法时的一些笔记。</p>

<!-- more -->

<a href="https://pan.baidu.com/s/1A0PSXm8SgOlyFYxgQ7yaDg?pwd=jwuf#list/path=%2F">Typora软件分享</a>提取码：jwuf





### Living preview

Living preview(实时预览)这也算是Typora的一个特点吧相当于有道云笔记中的右窗口预览，**可自动隐去格式说明中的格式语句** 。 注意一定要在符号与正文之间**加空格**，目的在于区分内容与格式符。文字换行也与一般不同，要**先加空格再enter**。

### 具体符号

#### headers

随着标题的井字符数量的增加，标题优先级逐渐下降。

> ```
> # 大标题  （ Ctrl + 1 ） 
> ## 第二号标题 （ Ctrl + 2 ) 
> ### 第三号标题  
> 一直可以到第六号标题
> ```

#### Blockquote

> ```
> 语法 ： > 引用内容  
> 例如输入下列语句：
> > 心志要坚，意趣要乐。  
> 输出结果为
> ```

> 心志要坚，意趣要乐

#### List

> ```
> 就如平常一样输入序号就行了，此后换行就会自动排序。  
> 格式 ：1. 第一点        
> 2. 第二点
> 3. * 空圆
>    - 方形
> * 实圆
> ```

1. 第一点
2. 第二点
3. - 星号
     - 

- 

#### Task List

> ```
> 格式 
> - [ ] 未完成任务    - [x] 已完成任务
> 可点击任务点完成任务  
> ```

-  未完成任务    
-  已完成任务

#### Code Block

> ```
> 格式  
> ​```language
> 代码段
> ​```  
> 其实后面那三个顿号电脑会自动显示的（注意是顿号，  
> 英文输入法下的Esc下边那个键）  
> 代码段也有一个神奇的功能，就是能完整显示Typora的代码符，   
> 要不然本文的一个个事例又是怎么保留住这些Typora符呢。想想也知道，  
> 设计一个这样的文本格式，  
> 当然也要兼顾代码的特殊符号呀，比如代码里的#、*是不能参与Typora的格式符的。  
> 代码语句的输入  
> 格式 
> ```

```
// Reverse Bits Ordering
module top_module(
    input [99:0] in ,
    output [99:0] out 
) ;
    genvar i ;
    generate
        for(i = 0 ; i < 100 ; i++) begin : generate_name
            assign out[i] = in [99 - i] ;
        end
    endgenerate
endmodule
```

#### Math Blocks

```
$ 单行输入 $
$ \mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}  
$

```





#### Table

> ```
> | A  | B |
> | C  | D |
> | E  | F |  
> 点击表格内有功能选择
> ```

|  A   |  B   |
| :--: | :--: |
|  C   |  D   |
|  E   |  F   |



#### 分割线

> ```
> 格式 
> --- 
> 或者 
> *** 
> 例如 
> 我是分割线 
> 
> ---
> 
> 我是分割线 
> 
> ***
> ```

我是分割线 

------

我是分割线 

------



#### 目录

> ```
> 格式 
> 输入[toc]再回车就行了 （table of content) 
> 就会生成一个目录，点击相应标记即可跳转 
> 例如 
> [toc] 
> ```



#### Link

> ```
> 格式 
> [给链接起个名字](写上具体网址)
> [知乎](https://www.zhihu.com/) 注意用英文输入法下的括号  
> 在markdown文件中按住Ctrl键同时点击网址即可直接打开
> ```

[知乎](www.zhihu.com)

#### Insert Pictures

> ```
> 格式  
> ![起个名字](存储位置) 
> 例如 
> ![思维导图](D:\desktop\Typora.png) 
> 当然啦，直接把图片拖过来复制一下不香吗
> ```

- 可将typora设置如下，可避免图片丢失导致markdown文件中图片无法查看（也可以使用网络图床存储）

  ![img](file://D:/Software/Typora/Pictures/1693142385073.png?lastModify=1695005959)