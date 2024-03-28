---
title: Git学习笔记
tags: Tools
abbrlink: a91eaa72
date: 2022-10-09 11:30:57
---

Git学习笔记 by 《[ProGit](https://git-scm.com/book/zh/v2)》

<!-- more -->

# Git设计理念

* **Git直接记录快照，而非差异比较**

* 基于差异**(Delta-based)** , 像CSV、Subversion、Perforce。它们如何建立新版本：在版本1的基础上，建立一个记录`版本2(新版本)`与`版本1`差异的文件，我们称其为$\Delta1$ .当我们需要读取版本2时，可以通过**$版本1 + \Delta1$** 计算得出版本2。

* Git快照流**，提交后，建立一个新的快照：对**修改过的文件**建立一个新的快照并保存其索引，对**未修改的文件**并不重新存储**，而是**只保留一个链接**指向旧文件

{% gp 2-2 %}

![基于差异比较](https://git-scm.com/book/en/v2/images/deltas.png)

![数据流-快照](https://git-scm.com/book/en/v2/images/snapshots.png)

{% endgp %}

# Git 基础

## Git工作状态

**三种状态：**`已提交(committed)`、``已修改(modified)``、``已暂存(staged) ``, 对应三个阶段：``工作区``、``暂存区``、``Git目录(仓库)``。

{% gp 2-2 %}

![Git三个阶段](https://git-scm.com/book/en/v2/images/areas.png)

![文件状态变化周期](https://git-scm.com/book/en/v2/images/lifecycle.png)

{% endgp %}

## Git配置

安装Git后添加用户信息：

```shell
git config --global user.name "benkangpeng"
git config --global user.email benkangpeng@163.com
```

```shell
$ cd /c/user/my_project
$ git init # 初始化git
```

## Git基本操作

### 获取mannul

```shell
git <verb> -h
```

ex :  `git add -h`

### git init

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

### git status

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

### .gitignore

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

### git diff

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



### git commit

每次准备提交前，先用 `git status` 看下，你所需要的文件是不是都**已暂存**起来了， 然后再运行提交命令 `git commit`：

```shell
$ git commit -m "the first commit"
```

* `-m` : 添加提交信息message
* "the first commit" : 提交的信息，区分每一次提交做了哪些大体的修改

* `git commit -a`跳过暂存区，直接将为暂存的文件commit .

### git rm

* 如果你想从磁盘根本删除``未暂存的文件``:

```shell
$ git rm PROJECT.md
```

* 从暂存区删除(取消跟踪)文件(仍保存在磁盘里）：

```shell
$ git rm -f PROJECT.md #-force , 强制删除
```

### git mv

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

### git log

* `git log` 显示所有提交日志
* `git log -2` 显示最近两条日志
* `git log -p` 按补丁格式显示每个提交引入的差异
* `git log --pretty=oneline`显示格式为一行
* 此外还有一些参数过滤日志内容，例如`--since``--author`等，可查文档。

### git commit --amend

* 提交后发现上一次提交有些许瑕疵，例如message写错了、没有跟踪上某个文件 ，但又不想再一次提交，想覆盖上一次提交。

```shell
$ git commit --amend -m "the right message"
```

### git reset

* 强制重置(删除)某个commit, 例如`git reset --hard HEAD~1`删除上一次提交。
* 取消暂存的文件，例如将`CONTRIBUTING.md`文件取出``stageing area` , `git reset HEAD CONTRIBUTING.md`

### *文件版本回溯

案例：`commit log`如下，自从`git clone`以来，我提交了`version 1`和`version2`.整个项目只有两个文件：`check.md`和`README.md`. 我现在想将`check.md`还原到`version 1`的状态，但`README.md`不用还原。(可以理解为我后知后觉意识到`version2`中对`check.md`的修改是错误的。)

> $ git log --pretty=oneline
> bc03ee1185c084b514ff44a9520ab5d29777b600 (HEAD) version2
> 3d44a28a78ffba2eab8d0718e34c3f7463cfa2a1 version 1
> 86d413d821c361c0245859fddd9c6258f698f16f (tag: v1.0, origin/main, origin/HEAD) colle
> ct some ways to github
> 1bfa40c95f5b2a5a086421889e60321ef056fbc0 (tag: v0.1) the first commit by git push
> a41ee579c6756e653d380b2e08426ea74f77841d (tag: v0.0) Create README.md

```shell
$ git reset 3d44a  # 3d44a是version1的效验码，没写完整
Unstaged changes after reset:
M       README.md
M       check.md
#到version1后,本地文件是没有发生变化的(仍是version2的snapshot),因此会提示`unstaged changes
$ git status
HEAD detached from 86d413d
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        modified:   check.md
# 如何恢复check.md，上面的信息也提示我们了“git restore <file>”
$ git restore check.md && git add README.md
$ git log --pretty=oneline
3d44a28a78ffba2eab8d0718e34c3f7463cfa2a1 (HEAD) version 1
86d413d821c361c0245859fddd9c6258f698f16f (tag: v1.0, origin/main, origin/HEAD) colle
ct some ways to github
1bfa40c95f5b2a5a086421889e60321ef056fbc0 (tag: v0.1) the first commit by git push
a41ee579c6756e653d380b2e08426ea74f77841d (tag: v0.0) Create README.md
#END
```

<del>但以上做法删除了version2的提交信息。是否存在方法，只进行单个文件的版本回溯呢？现在还没解决……</del>

### *单文件版本回溯

```shell
$ git log --pretty=oneline
05bfc3c25ca3ee80247f3ec24b4936d2d7e7d2c1 (HEAD) version3
245a8ddd0c2bfc01c2444930e2ed11b04320d354 version2
3d44a28a78ffba2eab8d0718e34c3f7463cfa2a1 version 1
86d413d821c361c0245859fddd9c6258f698f16f (tag: v1.0, origin/main, origin/HEAD) colle
ct some ways to github
1bfa40c95f5b2a5a086421889e60321ef056fbc0 (tag: v0.1) the first commit by git push
a41ee579c6756e653d380b2e08426ea74f77841d (tag: v0.0) Create README.md
```

* 将`check.md`回溯到版本1，其他文件不变，`git checkout <hash> check.md`

```shell
$ git checkout 3d44a28a check.md
Updated 1 path from aab7394
```

### 与远程仓库联动

* `git remote add <name> <url>` 添加远程仓库，例如`git remote add getback https://github.com/BenkangPeng/GetBack.git` , 之后可用名称`getback`代替网址进行操作。类似于`C`里的`#define`
* `git remote -v`显示已添加的远程仓库, `git remote`简略显示。
* `git remote rename name1 name2` 修改仓库**代称**，实际远程仓库(比如Github上的仓库）名称没被修改。

```shell
$ git remote add getback https://github.com/BenkangPeng/GetBack.git
$ git remote -v
getback https://github.com/BenkangPeng/GetBack.git (fetch)
getback https://github.com/BenkangPeng/GetBack.git (push)
$ git remote rename getback GetBack
$ git remote
GetBack
```

* `git fetch` 获取仓库的更新。例如，你在1年以前`clone`了仓库`A` , 但在过去一年内，仓库`A`其他开源贡献者提交了许多内容，甚至产生了分支，`git fetch <url>`将仓库`A`的这些更新数据下载到本地，但不会改变本地状态：**不会自动合并分支或修改你当前的工作**。当然你可以手动合并分支、更新本地仓库。
* `git clone <url>`克隆仓库。将`<url>`对应的仓库克隆到本地，与`git fetch`不同的是，它将覆盖本地工作。

```shell
$ git clone https://github.com/BenkangPeng/GetBack.git
$ cd GetBack/
$ git remote -v
origin  https://github.com/BenkangPeng/GetBack.git (fetch)
origin  https://github.com/BenkangPeng/GetBack.git (push)
#克隆完仓库后，git会自动创建一个origin的远程仓库，对应的就是克隆的仓库url
#对GetBack进行修改……
$ git push origin main
#将本地的main分支(主支)提交到远程仓库origin
```

关于``主分支`` `master`改为`main`，还颇有意味。[github/renaming](https://github.com/github/renaming)        [乐子 on 知乎](https://zhuanlan.zhihu.com/p/257179306)

### Tag

* `git tag -a <v1.0> -m "version 1.0"` 添加`annotated tag` , `git tag v1.0`添加`lightweight`标签。
* `git tag`显示所有标签，`git show <v1.0> `显示某个标签信息
* `git tag <version> <verify code>`为之前某一次`commit`补充`tag`

```shell
$ git log --pretty=oneline
dd0a36aec54a58a9f3427f5f33b1ee97802949b0 (HEAD -> master, tag: v1.0) 
3d4057629e1e3515667c7e826817a9ab5a29f306 the second commit
349775b9754e9113e549a9d734707bff38f2e3b4 the first commit

$ git tag v0.0 34977
```

* 默认情况下，`git push`不会顺带将每一个`commit` 的`tag`提交，需手动设置。`git push origin <tagname>`上传单个`tag`  ,  `git push origin --tags `推送所有`tags`
* `git tag -d <tagname>`删除标签，但不会删除远程仓库(``remote`)的`tags` , 需用`git push origin --delete <tagname>`



# Git分支

```shell
$ git branch testing	#创立分支testing
$ git checkout testing  #切换到testing
#以上两条命令可合并为 git checkout -b testing 
Switched to branch 'testing'
# do some changes
$ git checkout main  #回到主分支
$ git branch #显示所有分支
```

* `git merge <branch>`

```shell
$ git touch polynorminal.cpp
# edit polynorminal.cpp
$ git add polynorminal.cpp
$ git commit -m "touch a new .cpp file"
$ git checkout -b AddFunc2
Switched to a new branch 'AddFunc2'
# edit polynorminal.cpp
$ git commit -a -m "have added the func2"
$ git checkout main
$ git merge AddFunc2
Updating 87ba74e..869b2a7
Fast-forward
 polynorminal.cpp | 11 ++++++++++-
 1 file changed, 10 insertions(+), 1 deletion(-)
```

{% note info %}

在`main`中合并分支时，可能出现合并冲突，这是因为分支中文件修改过大，`git`不知道如何将`main`与`branch`两个文件版本合并，此时需手动合并、修改文件，并`commit`

{% endnote %}

* `git branch -d <branch>`删除分支（但不能删除未合并的分支）
* `git branch --no-merged`查看未合并的分支



### Git版本回溯(by branch)

我一直是用`git reset --hard <hash_id>`进行版本回溯，但这样会丢失之后的commit，导致版本混乱。现在想到一个方法，可以用分支来进行回溯查看。(当然，如果我们确定要回到某个commit重新工作，并确定要丢失之后的commit，也可以硬重置。)

**查看当前分支**

```shell
$ git log --oneline --decorate --graph --all
* a1ebf62 (master) c2b91
| * 831fee6 (testing) 87ab2
|/
* 8a4b1b7 f30ab
* cd426be 34ac2
* d922070 98ca9
* e339fae 完成稠密
| * bfbf7ce (HEAD -> sparse) sparse v1.0
| * eb62cc0 完成主体
|/
* b6a6d16 3rd commit
* f64efae 2nd commit
* 8bd0d82 1st commit
```

现在我们正在`sparse`分支上，因为改动较大，想回到稠密版本上查看稠密版本的源码：

```shell
$ git checkout -b check_dense_version e339fae
# = git branch check_dense_version e339fae && git checkout check_denser_version
Switched to a new branch 'check_dense_version'
$ git log --oneline --decorate --graph --all
* a1ebf62 (master) c2b91
| * 831fee6 (testing) 87ab2
|/
* 8a4b1b7 f30ab
* cd426be 34ac2
* d922070 98ca9
* e339fae (HEAD -> check_dense_version) 完成稠密
| * bfbf7ce (sparse) sparse v1.0
| * eb62cc0 完成主体
|/
* b6a6d16 3rd commit
* f64efae 2nd commit
* 8bd0d82 1st commit
```

这时整个工作目录的文件都变成了`e3319fae`时的版本。在这个版本下，如果我们运行代码导致一些文件发生了修改，工作目录不干净，是不能直接`switch`回`sparse`的：

```shell
$ git switch sparse 
error: Your local changes to the following files would be overwritten by checkout:
        a.txt
Please commit your changes or stash them before you switch branches.
Aborting
```

提示显示如果我们要更换分支，需要`commit` 或者`stash`

* 如果我们不需要提交，可以将文件变更取消掉，再`switch`

```shell
$ git restore .  #将所有被修改的文件复原
$ git switch sparse 
Switched to branch 'sparse'
* a1ebf62 (master) c2b91
| * 831fee6 (testing) 87ab2
|/
* 8a4b1b7 f30ab
* cd426be 34ac2
* d922070 98ca9
* e339fae (check_dense_version) 完成稠密
| * bfbf7ce (HEAD -> sparse) sparse v1.0
| * eb62cc0 完成主体
|/
* b6a6d16 3rd commit
* f64efae 2nd commit
* 8bd0d82 1st commit
```

**`git stash`**

如果我们在`sparse`分支下工作目录有修改未提交，那么切换分支之前需要将修改存入`stash`。

```shell
$ git log --oneline --decorate --graph --all
* a1ebf62 (master) c2b91
* 8a4b1b7 f30ab
* cd426be 34ac2
* d922070 98ca9
* e339fae 完成稠密
| * bfbf7ce (HEAD -> sparse) sparse v1.0
| * eb62cc0 完成主体
|/
* b6a6d16 3rd commit
* f64efae 2nd commit
* 8bd0d82 1st commit

$ git branch temp e339fae
$ git switch temp
error: Your local changes to the following files would be overwritten by checkout:
        a.txt
Please commit your changes or stash them before you switch branches.
Aborting
$ git log --oneline --decorate --graph --all
* a1ebf62 (master) c2b91
* 8a4b1b7 f30ab
* cd426be 34ac2
* d922070 98ca9
* e339fae (temp) 完成稠密
| * bfbf7ce (HEAD -> sparse) sparse v1.0
| * eb62cc0 完成主体
|/
* b6a6d16 3rd commit
* f64efae 2nd commit
* 8bd0d82 1st commit
$ git stash save "uncommit modify in sparse"
Saved working directory and index state On sparse: uncommit modify in sparse
$ git switch temp
Switched to branch 'temp'
$ git switch sparse 
Switched to branch 'sparse'
$ git stash list
stash@{0}: On sparse: uncommit modify in sparse
$ git stash pop 0
On branch sparse
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (6679d6d8daf8a44c45d289fe82c7aab9a5430eb0)
```

**git stash相关命令**

1. `git stash save [message]`：将当前工作目录的修改保存到一个stash列表中，并附带一条可选的描述信息。
2. `git stash list`：列出所有的stash列表。
3. `git stash apply [stash_id]`：将指定的stash应用到当前工作目录，但不从stash列表中删除。
4. `git stash pop [stash_id]`：将指定的stash应用到当前工作目录，并从stash列表中删除。
5. `git stash drop [stash_id]`：从stash列表中删除指定的stash。
6. `git stash branch [branch_name] [stash_id]`：基于指定的stash创建一个新的分支，并将stash应用到新分支上。
7. `git stash pop --index [stash_id]`：将指定的stash应用到当前工作目录和索引（暂存区），并从stash列表中删除





