---
title: Deploy your website
abbrlink: f1f00c7b
date: 2021-10-01 16:00:00
tags: Web
---

### 摘要

我们在使用[Hexo+NexT搭建](https://benkangpeng.gitee.io/2023/09/18/Hello%20Hexo-NEXT/)好个人博客后,只能在本地端口访问，其他人无法访问(有一种内网穿透的方法可以将电脑端口暴露，从而达到”上线“的功能。尚未了解    找到了:[Here](https://www.bilibili.com/video/BV1z14y1r7Fc/?spm_id_from=333.999.0.0&vd_source=da5120fea3f8bb8d2fe1984a02a9a745))。

我们可以使用`Github pages` 和`Gitee pages` 上线网站从而白嫖一台只能搭载`静态网页`的服务器  （<del>是不是有种白嫖的快乐</del>）以下是上线步骤。

<!-- more -->

### Github pages

#### 主要步骤

* **注册GitHub、新建仓库**

  新建一个仓库，注意仓库名一定是用户名与github.io的组合，例如`benkangpeng.github.io`

* **生成ssh-key**

  如果未配置过git ，未获得`ssh key` 的话，需要配置`git`并获得`ssh key`(我理解为电脑硬件、邮箱对应的编码。通过该编码可以找到唯一的计算机，`GitHub`就是通过`ssh key`与计算机进行加密数据传输的。计算机网络没学 ，一定得补补:cry:)

  ```shell
  git config --global user.name "benkangpeng"
  git config --global user.email "benkangpeng@163.com"
  ssh-keygen -t rsa -C "benkangpeng@163.com"  # 之后三次回车
  ```

  根据终端输出的信息找到`id_rsa.pub`的位置,或者通过以下命令查看：

  ```shell
  cat ~/.ssh/id_rsa.pub
  ```

* **Connect computer with GitHub by ssh-key**

  **将ssh-key加入到github上**：点击`个人头像进入个人空间` → `SSH and GPG keys`→ `New SSH key `,Title随意(最好标明是哪台电脑），`Key`粘上刚才复制的`id_rsa.pub` ，添加成功后会收到Github的邮件。

  **尝试本机与Github通信**：

  ```shell
  ssh -T git@github.com
  ```

  > Hi BenkangPeng! You've successfully authenticated, but GitHub does not provide shell access.

  若显示以上结果，则表明通信成功。

* 修改`Hexo` 配置文件

  `_config.yml `文件中找到`deploy`一项，修改为

  ```yml
  deploy:
    type: 'git'
    repository: https://github.com/benkangpeng/benkangpeng.github.io.git
  ```

  其中，`type`表示`the type of site deploying` , 即网站发布的方式，我们通过`git`发布

  `repository`则填写之前建立的仓库的地址，后加上后缀`.git`

* 安装`hexo部署插件`(最好在根目录安装插件)

  ```shell
  benkangpeng@DESKTOP-FR84659 MINGW64 /d/Hexo/blog (master)
  npm install hexo-deployer-git --save
  ```

* 将文件上传到`Github`

  ```shell
  hexo clean
  hexo g  # hexo generate
  hexo d  # hexo deploy
  ```

  此时相关文件已上传到对应的仓库中，但如果此时你输入`https:\\benkangpeng.github.io`结果是`404`，原因是未将该仓库设置为`GitHub pages` .

  在仓库页面右边的侧边栏中有一个`设置`的齿轮图标，点击→勾选`Use your GitHub Pages website`→`Save Changes`

* **大功告成**，输入`https:\\benkangpeng.github.io`查看网页吧！



### Gitee pages

因为Gitee pages的部署与Github类似(<del>毕竟抄来的</del>)，以下步骤较为简略，可参考着Github的配置步骤进行。

#### 主要步骤:

* **Gitee注册账号**并实名认证(对，就是这么狗:dog:)

* **新建仓库**

  此处一定要注意仓库名与用户名一致，例如我的是`benkangpeng`,与Github不同(GitHub是benkangpeng.github.io)

* **Git全局设置、获取ssh-key**(同上面的Github)

  已经设置过全局设置、获取过ssh-key，则无需进行操作。重复操作会改变计算机的ssh-key，与之前在Github绑定的ssh-key不同了，导致Github与计算机无法通信。最优解：固定计算机的ssh-key不变，Github、Gitee共用一个。此后也不要变更ssh-key.

  ```shell
  git config --global user.name "benkangpeng"
  git config --global user.email "benkangpeng@163.com"
  ssh-keygen -t rsa -C "benkangpeng@163.com"  # 之后三次回车
  ```

  根据终端输出的信息找到`id_rsa.pub`的位置,或者通过以下命令查看：

  ```shell
  cat ~/.ssh/id_rsa.pub
  ```

* **SSH-key填入Gitee**

  点头像→设置→SSH公钥

* **第一次通信**

  ```shell
  ssh -T git@gitee.com
  ```

  > Hi BenkangPeng(@benkangpeng)! You've successfully authenticated, but GITEE.COM does not provide shell access.

  <del>咱也不懂`does not provide shell access`是啥意思，Gitee垃圾就完事了</del>

* **修改`hexo`  `_config.yml`**

  ```yml
  deploy:
    type: 'git'
    #repository: https://github.com/benkangpeng/benkangpeng.github.io.git
    repository: https://gitee.com/benkangpeng/benkangpeng.git
  ```

* **安装hexo部署插件**(安装过的无需重复安装)

  ```shell
  npm install hexo-deployer-git --save
  ```

* **部署、上传到Gitee**

  ```shell
  hexo g
  hexo d
  ```

  此时可以看到相关文件已上传到Gitee仓库中。

* **将仓库设置为Gitee Pages**

  ①进入仓库→上面的边框 `管理`→向下滑，勾选`开源`→`保存`

  ②进入仓库→上面的边框 `服务`→`Gitee Pages` → 勾选`强制使用HTTPS`→`启动`

  {% note info %}

  dog东西，今天才发现`gitee pages`不像`github pages` ： `github pages` 仓库中一有更新，延迟一会后便可访问到更新后的网页，而`gitee pages`更新仓库后，还要手动执行上面的第②步更新部署......或者你可以买`gitee pages pro` , ￥99/年......特色……

  {endnote}

  <del>据说强制HTTPS网站要安全一些，没学计算机网络的憨憨不懂</del>

* **大功告成**

  <a href="https:\\benkangpeng.gitee.io"></a>

### 参考资料(详细视频)

感谢大佬：[超详细！0成本搭建个人网站！！【建议收藏】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ts4y1f7Gu/?spm_id_from=333.999.0.0&vd_source=da5120fea3f8bb8d2fe1984a02a9a745)