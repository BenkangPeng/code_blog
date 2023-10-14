---
title: Git学习笔记
tags: Tools
abbrlink: a91eaa72
date: 2023-10-09 11:30:57
---

GIt学习笔记

<!-- more -->

### Git设计理念

* **Git直接记录快照，而非差异比较**

* 基于差异(Delta-based)** , 像CSV、Subversion、Perforce。它们如何建立新版本：在版本1的基础上，建立一个记录`版本2(新版本)`与``版本1`差异的文件，我们称其为$\Delta1$ .当我们需要读取版本2时，可以通过**$版本1 + \Delta1$** 计算得出版本2。

* Git快照流**，提交后，建立一个新的快照：对**修改过的文件**建立一个新的快照并保存其索引，对**未修改的文件**并不重新存储，而是只**保留一个链接**指向旧文件

{% gp 2-2 %}

![基于差异比较](https://git-scm.com/book/en/v2/images/deltas.png)

![数据流-快照](https://git-scm.com/book/en/v2/images/snapshots.png)

{% endgp %}

### Git 基础

#### Git工作状态

**三种状态：**`已提交(committed)`、``已修改(modified)``、``已暂存(staged) ``, 对应三个阶段：``工作区``、``暂存区``、``Git目录(仓库)``。

{% gp 2-2 %}

![Git三个阶段](https://git-scm.com/book/en/v2/images/areas.png)

![文件状态变化周期](https://git-scm.com/book/en/v2/images/lifecycle.png)

{% endgp %}

#### Git配置

安装Git后添加用户信息：

```shell
git config --global user.name "benkangpeng"
git config --global user.email benkangpeng@163.com
```

```shell
$ cd /c/user/my_project
$ git init # 初始化git
```

#### Git基本操作

##### 获取mannul

```shell
git <verb> -h
```

ex :  `git add -h`

##### git init

```shell
$ cd /my_project/
$ git init
```

初始化Git仓库，此时会在my_project文件夹中生成`.git`子目录用于存放版本控制文件。注意：**此时还没对项目文件进行跟踪** , 还需要`git add`跟踪所需文件。

git add

```shell
$ git *.c  # 跟踪所有后缀为.c文件
$ git README.md
```

`git add`内涵：

要暂存这次更新，需要运行 `git add` 命令。 这是个**多功能**命令：①可以用它开始跟踪新文件，②或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“<u>精确地将内容添加到**下一次提交**中</u>”而不是“将一个文件添加到项目中”要更加合适。 

##### git status

显示当前文件夹git状况。

> On branch master
>
> No commits yet
>
> Changes to be committed:
>   (use "git rm --cached <file>..." to unstage)
>         new file:   README.md
>
> Untracked files:
>   (use "git add <file>..." to include in what will be committed)
>         CONTRIBUTING.md

如上显示的是，`README.md`已被跟踪，而`CONTRIBUTING.md`未被跟踪。

```shell
$ git add CONTRIBUTING.md
$ echo `My project` > README.md
$ git status
```

> On branch master
>
> No commits yet
>
> Changes to be committed:
>   (use "git rm --cached <file>..." to unstage)
>         new file:   CONTRIBUTING.md
>         new file:   README.md
>
> Changes not staged for commit:
>   (use "git add <file>..." to update what will be committed)
>   (use "git restore <file>..." to discard changes in working directory)
>         modified:   README.md

* 此时`CONTRIBUTING.md`已经被跟踪
* 提示有``待提交的修改`` (changes to be committed) ,  `CONTRIBUTING.md`和`README.md`没有提交。(至于changes , 即`new file` , git将`创立文件`也视为`changes`)
* `Changes not staged for commit` ： 我们刚才向`README,md`写入了`My project` , 但没有将新版本的`README.md`添加进入暂存区`stage` 。可执行操作`git add README.md`
* 上面这条再一次体现了`git add`的双重命令，或将其理解为`为文件提交(commit)做好预备` 

##### .gitignore

创建`.gitignore`文件忽略追踪

```shell
$ touch .gitignore
$ git add .gitignore
# You must add .gitignore into the staging area to make it work .
```

常见语法：

```
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

##### git diff

* `git diff` 查看**同一**文件`已暂存`与`未暂存`的两个版本的差异

  eg.  在`README.md`中添加一行`哈哈` ，no stage ,  使用命令查看:

>$ git diff README.md
>diff --git a/README.md b/README.md
>index 6fcadd0..b2f8c83 100644
>--- a/README.md
>+++ b/README.md
>@@ -1 +1,3 @@
> My project~
>+
>+哈哈
>\ No newline at end of file

* `git diff --staged` 查看``已暂存文件``与``最后一次提交文件``的差异.

```shell
$ git diff --staged README.md
```



##### git commit

每次准备提交前，先用 `git status` 看下，你所需要的文件是不是都**已暂存**起来了， 然后再运行提交命令 `git commit`：

```shell
$ git commit -m "the first commit"
```

* `-m` : 添加提交信息message
* "the first commit" : 提交的信息，区分每一次提交做了哪些大体的修改

* `git commit -a`跳过暂存区，直接将为暂存的文件commit .

##### git rm

* 如果你想从磁盘根本删除``未暂存的文件``:

```shell
$ git rm PROJECT.md
```

* 从暂存区删除(取消跟踪)文件(仍保存在磁盘里）：

```shell
$ git rm -f PROJECT.md #-force , 强制删除
```

##### git mv

* `git mv` 移动文件 or 重命名文件。

```shell
$ git mv Project.md PROJECT.md
```

上述命令相当于：

```shell
$ mv Project.md PROJECT.md
$ git rm Project.md # 从暂存区移除Project.md
$ git add PROJECT.md # 跟踪PROJECT.md
```

##### git log

* `git log` 显示所有提交日志
* `git log -2` 显示最近两条日志
* `git log -p` 按补丁格式显示每个提交引入的差异
* 此外还有一些参数过滤日志内容，例如`--since``--author`等，可查文档。

##### git commit --amend

* 提交后发现上一次提交有些许瑕疵，例如message写错了、没有跟踪上某个文件 ，但又不想再一次提交，想覆盖上一次提交。

```shell
$ git commit --amend -m "the right message"
```

##### git reset

* 强制重置(删除)某个commit, 例如`git reset --hard HEAD~1`删除上一次提交。
* 取消暂存的文件，例如将`CONTRIBUTING.md`文件取出``stageing area` , `git reset HEAD CONTRIBUTING.md`