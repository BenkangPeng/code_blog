---
title: centos学习笔记
tags: linux
abbrlink: d2783502
date: 2021-03-06 08:37:02
---

......

<!-- more -->

### 强制修改密码

如果将密码修改得很简单，`centos`会拒绝修改，可以如下强制修改：

```bash
$ sudo su
# echo <password> | passwd --stdin <username> 
```

[Link](https://blog.csdn.net/chy555chy/article/details/112258353#:~:text=1%20%E6%B7%BB%E5%8A%A0%E7%94%A8%E6%88%B7%EF%BC%9A%20useradd,%3C%E7%94%A8%E6%88%B7%E5%90%8D%3E%202%20%E4%BF%AE%E6%94%B9%E5%BD%93%E5%89%8D%E7%94%A8%E6%88%B7%E5%AF%86%E7%A0%81%EF%BC%88%E5%AD%98%E5%9C%A8%E5%AE%89%E5%85%A8%E6%80%A7%E6%A0%A1%E9%AA%8C%EF%BC%89%EF%BC%9A%20passwd)

### 重定向cd

在 CentOS 系统中，如果您希望在每次使用 `cd` 命令进入目录后不自动列出所有文件，可以通过修改 Bash 配置文件来实现。

您可以通过编辑用户的 `.bashrc` 或全局的 `/etc/profile` 文件来达到目的。请按照以下步骤操作：

1. 打开终端。

2. 输入 `vi ~/.bashrc` 来编辑当前用户的 `.bashrc` 文件；或者使用 `vi /etc/profile` 来编辑全局的配置文件，这需要管理员权限（sudo）。

3. 在文件末尾添加以下行：

   ```shell
   alias cd='cd >/dev/null 2>&1'
   ```

4. 保存并退出编辑器。如果是编辑的 `/etc/profile`，为了使更改立即生效，需要运行 `source /etc/profile` 或重新登录。**有时`etc/profile`未生效，cd进入目录仍会`ls` , 这时重新`source /etc/profile`即可。**

这样设置后，每次使用 `cd` 命令切换目录时，不会自动执行 `ls` 命令，从而不会显示目录下的文件列表。

请注意，上述方法会将 `cd` 命令的输出重定向到 `/dev/null`，这意味着即使 `cd` 命令有输出（通常不会有），也不会在终端上显示。如果您希望恢复默认的行为，只需删除或者注释掉 `.bashrc` 或 `/etc/profile` 文件中添加的那行别名配置即可。

### CentOS 7安装最新版git

安装[ius-release](https://ius.io/setup) 

```shell
$ yum install \
https://repo.ius.io/ius-release-el7.rpm \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

查看git版本：

```shell
$yum provides git
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * epel: mirror.nyist.edu.cn
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
git-1.8.3.1-23.el7_8.x86_64 : Fast Version Control System
Repo        : base



git-1.8.3.1-24.el7_9.x86_64 : Fast Version Control System
Repo        : updates



git-1.8.3.1-25.el7_9.x86_64 : Fast Version Control System
Repo        : updates



git236-2.36.5-1.el7.ius.x86_64 : Fast Version Control System
Repo        : ius
Matched from:
Provides    : git = 2.36.5-1.el7.ius



git236-2.36.6-1.el7.ius.x86_64 : Fast Version Control System
Repo        : ius
Matched from:
Provides    : git = 2.36.6-1.el7.ius



git236-2.36.6-1.el7.ius.x86_64 : Fast Version Control System
Repo        : @ius
Matched from:
Provides    : git = 2.36.6-1.el7.ius

```

安装git2.36:

```shell
$ yum remove git -y #卸载老版本
$ yum install git236 -y
```

查看新版本

```shell
$ git --version
git version 2.36.6
```

### CentOS 7 多版本gcc

担心`VCS`,`verdi`不兼容高版本`gcc` (现在vcs,verdi使用gcc-4.8编译) ， 想在centos上安装多个版本gcc，查询资料，决定用`Software Collections (SCL)`来管理多版本gcc。(SCL类似于anaconda)

1. 安装centos-release-scl

   ```shell
   $ sudo yum install centos-release-scl 
   ```

2. 安装特定版本的`devtoolset`

   ```shell
   sudo yum install devtoolset-10-gcc*
   ```

   这里安装的是gcc-10.x版本，如果要安装其他版本的GCC，只需将`devtoolset-10`替换为相应的版本号，如`devtoolset-7`。

3. 激活特定的devtoolset版本

   ```shell
   $ gcc -v 2>&1 | tail -n 1 #激活前版本
   gcc version 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
   $ scl enable devtoolset-10 bash #激活
   $ gcc -v 2>&1 | tail -n 1
   gcc version 10.2.1 20210130 (Red Hat 10.2.1-11) (GCC) 
   $ exit #退出当前版本
   $ gcc -v 2>&1 | tail -n 1 
   gcc version 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
   
   ```

   注意，scl激活只对当前终端有效，其他终端仍是默认版本。这种方法不需要手动下载源码编译，而是通过SCL来提供和管理系统范围内的不同版本的工具链。



### pip安装太慢

pip安装太慢，使用其他镜像安装：

```shell
$ pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple numpy

```

### CentOS7 美化

参考[文章](https://zhuanlan.zhihu.com/p/68845973)

### 初试Tmux

参考链接[Tmux新手教学](https://www.bilibili.com/video/BV1Mj411N7xS/?spm_id_from=333.337.search-card.all.click&vd_source=da5120fea3f8bb8d2fe1984a02a9a745)       [bryant-video/tmux-tutorial](https://github.com/bryant-video/tmux-tutorial)

配置文件`~/.tmux.conf`:

```shell
unbind %
bind | split-window -h -c "#{pane_current_path}"

unbind '"'
bind - split-window -v -c "#{pane_current_path}"

unbind r
bind r source-file ~/.tmux.conf

bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5
bind -r m resize-pane -Z


set -g mouse on
set -g mode-keys vi
set -sg escape-time 10 # make delay shorter

### Copy Mode
bind-key -T copy-mode-vi 'v' send -X begin-selection # start selecting text with "v"
bind-key -T copy-mode-vi 'y' send -X copy-selection # copy text with "y"


### Plugins 
set -g @plugin 'tmux-plugins/tpm'

set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'jimeh/tmux-themepack'
set -g @plugin 'tmux-plugins/tmux-resurrect' # persist tmux sessions after computer restart
set -g @plugin 'tmux-plugins/tmux-continuum' # automatically saves sessions for you every 15 minutes

set -g @resurrect-capture-pane-contents 'on'
set -g @continuum-restore 'on'
run '~/.tmux/plugins/tpm/tpm' # Initialize TPM

```

|                 命令                  |               功能               |
| :-----------------------------------: | :------------------------------: |
|     `tmux new -s <session name>`      |    create a new tmux session     |
|      tmux a -t `[Session Name]`       |  attach to an existing session   |
| tmux kill-session -t `[Session Name]` |    Delete a specific Session     |
|              tmux detach              |      detach current session      |
|            `prefix` + `|`             |   Split windows left and right   |
|            `prefix` + `-`             |    Split windows up and down     |
|           `prefix` + `↑↓←→`           |           switch panne           |
|           `prefix` + `jkhl`           |           Resize pane            |
|            `prefix` + `m`             | Maximize/Unmaximize current pane |
|            `prefix` + `c`             |          create window           |
|            `prefix` + `,`             |   Rename current active window   |
|            `prefix` + `&`             |       Kill current window        |
|         `prefix` + `n` or `p`         |       Next/Previous window       |
|            `prefix` + `w`             |  List all Sessions and windows   |
|            `prefix` + `,`             |   Rename current active window   |
|             `prefix`+`x`              |       delete current pane        |
|                                       |                                  |
|                                       |                                  |
|                                       |                                  |
|                                       |                                  |



在`tmux`中，选择文本时需要按住`shift` , 用`ctrl+shift+c`复制，`ctrl+shift+v`粘贴

```
# Character
You are a scientist who is proficient in computer science, physics, and is able to explain all relevant problems in detail, with pictures and texts

## Skills
### Skill 1: Proficient in Computer Science
- Proficient in all programming languages such as C/C++, Rust,Python,Systemverilog,Java, etc
- You can search for information from Github and stackoverflow to answer programming related questions

### Skill 2: Proficient in physics
- Proficient in solid state physics, semiconductor physics, semiconductor device physics, and quantum mechanics
- Able to combine formulas and graphs to explain physics problems

.

## Constraints:
- Output your answers in markdown format

```



#linux