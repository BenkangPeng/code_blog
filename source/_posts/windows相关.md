---
title: windows相关
tags: windows
abbrlink: 918da884
date: 2024-03-06 08:59:02
---

......

<!-- more -->

### 磁盘扩容

参考文献：[无损扩容分区– DiskGenius](https://www.diskgenius.cn/help/extend-partition.php)

使用软件：[图吧工具箱 - 最纯净的硬件工具箱 (tbtool.cn)](https://www.tbtool.cn/)

遇到的问题：

- 参考文献中的第3步中的“调整后的容量”是指腾出空闲空间的盘，在扩充目的盘后剩余的空间大小。例如，原有C盘50G，D盘100G，想在D盘腾出10G给C盘，那么“调整后的容量”应该是90G
- 如果重启时，windows不能自动进入WinPE的话，可以尝试安装一个[微PE工具箱](https://www.wepe.com.cn/) ，安装之后电脑开机时，会出现一个进入`WinPE`的选项。如果要删除`WinPE`的开机引导的话，参考[微PE工具箱怎么卸载](https://blog.csdn.net/lezeqe/article/details/105086599) ，其实就是把开机引导删除即可。
- windows重启进入`WinPE`后，会自动化操作，不要打断。可能遇到:"检测到下列文件系统错误，分区容量未做调整， 无效的的文件记录".   可参考[检测到下列文件系统错误解决方案](https://blog.csdn.net/QKK612501/article/details/115298382) 



### 使用Clash后，不打开Clash就无法上网

把Clash的系统代理关了就行

