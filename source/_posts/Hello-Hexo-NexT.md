---
title: Hello Hexo-NexT
tags: Web
abbrlink: b3bdf13b
date: 2021-10-01 15:00:00
---

### Abstract

<strong style="color : rgb(128,109,158) ; font-size : 24px">Hello Hexo and NexT !</strong>

<strong style = "color : rgb(128,109,158) ; font-size : 24px"> Hexo and NexT yyds ! </strong>

基于Hexo和NexT主题，我的第一个个人网站诞生啦！

这篇文章记录了建立网站的点点滴滴，并持续更新我对心肝宝贝[My Website](https://benkangpeng.github.io/)的持续美化、修缮。

<!-- more -->

### 建站

Hexo对中文的支持十分友好：[Hexo](https://hexo.io/zh-cn/)  ,   直接按照[Hexo官方文档](https://hexo.io/zh-cn/docs/)进行即可。

#### 1. 安装前提

首先确保计算机上已经安装[Node.js](https://nodejs.org/en) 和[Git](https://git-scm.com/download/win)。进不去网站的话，安装包已经屯好了：[百度网盘](https://pan.baidu.com/s/1fJ36mQkLS5qLYJSBXTeb0g?pwd=91bs)   [蓝奏云](https://pbk.lanzoum.com/i0b4O19nlu3i)

#### 2. 建立Hexo初始网站

* **安装Hexo(任意位置)**

```shell
npm install -g hexo-cli
```

* **建立一个文件夹存放所有博客文件：**

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo
$ pwd
/d/Hexo
```

* **在/d/Hexo文件夹使用如下方法创建一个blog(可修改名称) , 并进入：**

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo
$ hexo init blog

benkangpeng@DESKTOP MINGW64 /d/Hexo
$ cd ./blog/
```

blog文件夹内存放的就是网站blog源文件,如果想再建立一个网站，同样在Hexo文件夹下使用`hexo init blog_2`创建blog_2文件夹……

* **在blog文件夹中加入hexo相关配置：**

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo/blog
$ npm install
```

* **查看网页**

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo/blog
$ hexo g && hexo s  # 完整写法： hexo generate && hexo server
```

进入INFO中的端口`http://localhost:4000/`即可看到hexo博客框架:

<div style = "display : flex ; align-items : center ; justify-content: center;">
    <img src="https://pic.imgdb.cn/item/650fe8a2c458853aef63bf52.jpg" alt = "hexo初始网页.jpg" style = "width : 80% ">
</div>
* **写文章**

```shell
$ hexo new "第一篇文章"
```

Hexo即在`/blog/source/_posts/`创建了一个`第一篇文章.md` 。

实时编辑，在`https://localhost:4000`端口中刷新便可实时显示。

设置hexo主题

我们选择hexo-NexT主题——一个简洁、使用人数多、插件丰富的主题。更多主题可在[Themes | Hexo](https://hexo.io/themes/)找到。

### 配置NexT主题

我们选择hexo-NexT主题——一个简洁、使用人数多、插件丰富的主题。

Next的最新官网是[NexT - Theme for Hexo (theme-next.js.org)](https://theme-next.js.org/) ， 而不是[Home Page | Theme-Next](https://theme-next.org/) ， 原因详见：[Issue](https://github.com/next-theme/hexo-theme-next/issues/4#issuecomment-626205848)

* **安装NexT**

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo/blog
$ npm install hexo-theme-next
```

* **配置网站**

你会发现文件中有两个配置文件，在blog中有一个`_config.yml` ， 即Hexo的配置文件 ， `\Hexo\blog_test\node_modules\hexo-theme-next`中有一个`_config.yml` ， 即NexT主题的配置文件。

> However, we do not recommend directly modifying the NexT config file. It is quite often running into conflict status when updating NexT theme via `git pull`, or need to merge configurations manually when upgrading to new releases. For the theme installed through npm, it is also difficult to modify the NexT config file in `node_modules`.

In order to resolve this issue, we recommend using the [Alternate Theme Config](https://theme-next.js.org/docs/getting-started/configuration) feature to configure theme NexT.

官网提示我们，在修改NexT配置文件时不要直接修改 `\Hexo\blog_test\node_modules\hexo-theme-next` ， 否则可能引起冲突。官方提供给我们一个方法[Alternate Theme Config](https://theme-next.js.org/docs/getting-started/configuration) ， 简而言之将 `\Hexo\blog_test\node_modules\hexo-theme-next\_config.yml`复制到`blog\`下，更名为`_config.next.yml` , 这样直接修改`blog\`下的两个配置文件即可对Hexo和NexT均进行配置。

可手动配置，也可在相应位置执行命令：

```shell
benkangpeng@DESKTOP MINGW64 /d/Hexo/blog
$ cp node_modules/hexo-theme-next/_config.yml _config.next.yml
```

* **Hexo Configuration**

Edit `_config.yml`

```yml
# Site
title: Benkang Peng
subtitle: 'Personal Website'
description: ''
keywords:
author: Benkang Peng
language: en
timezone: 'Asia/Shanghai'
```

```yml
# theme: landscape  记得修改主题为NexT
theme: next
```

* **NexT Configuration**

Edit：

```yml
minify: true
scheme: Gemini
darkmode: true
```

* **Configuring Favicon**

We can visit [Favicon.ico图标生成器](https://www.logosc.cn/logo/favicon) to generate our favicon by inputting characters .

Put  the favicon files in `Hexo\blog_test\node_modules\hexo-theme-next\source\images` , then edit `NexT config file ` according to the filename:

```yml
favicon:
  small: /images/favicon-16x16.png
  medium: /images/favicon-32x32.png
  apple_touch_icon: /images/apple-touch-icon.png
  #safari_pinned_tab: /images/logo.svg
  #android_manifest: /manifest.json
```

* **Also add the avatar :**

For example , I add `GNU-Linux-Logo-Penguin-SVG.jpg` to `Hexo\blog_test\node_modules\hexo-theme-next\source\images`

Edit `_config.next.yml`:

```yml
avatar:
  url: /images/GNU-Linux-Logo-Penguin-SVG.jpg
```

* **Configuring Menu**

We can add the `Menu items ` by removing the comments in `_config.next.yml` :

```yml
menu:
  home: / || fa fa-pie-chart
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  #categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

Menu settings item's format :  `key: /link/ || icon` .    `Key` is the name of menu items(home , tags , etc.) ,  and link is the target link to relative url inside your site .  For example , if the menu item is `other: /other/ || icon` , it will go to `https://benkangpeng.github.io/other/` when you click it . 

> Except home and archives, all custom pages under menu section need to be created manually.

Tips above means that , when you add new menu items , you should also create relative files except `home and archives` as they can create relative files automatically .

For example , three steps to add menu item `tags` :

**①Removing the comment of tags :**

```yml
menu:
  home: / || fa fa-pie-chart
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
```

**②Create relative files :**

```yml
$ hexo new page tags
```

**③Editting the markdown file `index.md` of `\blog_test\source\tags\`**

```yml
title: Tags
date: 2023-09-18 09:10:10
type: "tags"
```

That's all .  As for the ways of the offical doc ,  I'm  absolutely confused ……

### Beautify

#### Custom pages

If we want to add a `.html` to the website and don't want NexT to influence that `.html` ' s style , which means `.html` can be displayed on its own style ,  how do we do ?

The answer is : **Prevent Hexo from rendering my .html files.**

* Now create a new page :

```shell
$ hexo new page Other
```

* Edit the `_config.next.yml` to display `Other` in menu

```yml
menu:
  Other: /other/index.html || fa fa-external-link
```

Find the name of  font-awesome icons from [Font Awesome](https://fontawesome.dashgame.com/)

* Edit the `_config.yml` to make sure folder `/Other` cann't be rendered by Hexo .

```yml
skip_render:
 - "Other/**"
```

* Delete the `index.md` in `/source/Other` , and create `index.html`

We can write something in `index.html` , which will keep its style and not be rendered by hexo .

#### Read more

It's a common need to show some part of article in home page and a `Read more` button to view more .

The best way is : Use `<!-- more -->` in the article to break the article manually .

#### Post Wordcount

Install plugin `hexo-word-counter` :

```shell
$ npm install hexo-word-counter
```

Edit `_config.yml`:

```yml
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
  exclude_codeblock: false
  awl: 2    
  wpm: 275
  suffix: "mins."
```

#### Tag Icon

By default, tags at the bottom of posts have a symbol # at there left side.

If you prefer icon instead of symbol, edit `_config.next.yml` like following:

#### Codeblock Style

You can go to [Highlight (theme-next.js.org)](https://theme-next.js.org/highlight/) to choose the theme you like and it will give the way to edit the `.yml` .

```yml
# _config.yml
highlight:
  enable: true 
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''
```

```yml
# _config.next.yml

codeblock:
  # Code Highlight theme
  # All available themes: https://theme-next.js.org/highlight/
  theme:
    light: atom-one-dark
    dark: atom-one-dark
  prism:
    light: prism
    dark: prism-dark
  # Add copy button on codeblock
  copy_button:
    enable: true
    show_result: true
    # Available values: default | flat | mac
    style: mac
  # Fold code block
  fold:
    enable: true
    height: 500
```



#### Back To Top

```yml
# _config.next.yml
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: true
```

#### Reading Progress

```yml
# _config.next.yml
# Reading progress bar
reading_progress:
  enable: true
  # Available values: left | right
  start_at: left
  # Available values: top | bottom
  position: top
  reversed: false
  color: "#37c6c0"
  height: 3px
```

#### GitHub Banner

NexT provide `Follow me on GitHub` banner on the right-top corner .

```yml
# _config.next.yml
# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: true
  permalink: https://github.com/benkangpeng
```



### Tag Plugin

#### Button

NexT provide the tag plugin `button` , which can jump to corresponding link .

* **Usage**

```
{% button url , text , icon[class] , [title] %}
```

or

```
{% btn url, text, icon [class], [title] %}
```

* * `url` : Absolute or relative path to URL.
  * `text` : Button text. Required if no icon specified.
  * `icon` : Font Awesome icon name. Required if no text specified.
  * `[class]` : *Optional parameter.* Font Awesome class(es): `fa-fw` | `fa-lg` | `fa-2x` | `fa-3x` | `fa-4x` | `fa-5x`
  * `[title]` : *Optional parameter.* Tooltip at mouseover.

eg.

```
{% btn # , Text , home , mouseover %}{% btn # , Superpowers , fa fa-external-link , mouseon %}
```

{% btn # , Text , home , mouseover %}{% btn # , Superpowers , fa-superpowers , mouseon %}

<div style = "display : flex ; align-items : center ; justify-content: center;">{% btn https://github.com, GitHub, fab fa-github fa-fw fa-lg, GitHub %}</div>

* **We can also use `Button` inside text , for example :**

```
For search engineer , I prefer {% btn https://cn.bing.com , Bing , fa fa-search , Bing %} instead of {% btn https://baidu.com , Baidu , fa fa-bomb , Baidu %}
```

For search engineer , I prefer {% btn https://cn.bing.com , Bing , fa fa-search , Bing %} instead of {% btn https://baidu.com , Baidu , fa fa-bomb , Baidu %}.

* **Button margin**

  Well , maybe this feature is useful for me to build a aggregation of commonly used website .

```
<div style = "display : flex ; align-items : center ; justify-content : center">
		<div>{% btn #,, heading %}{% btn #,, fab fa-edge %}{% btn #,, times %}{% btn #,, circle-notch %}</div>
		<div>{% btn #,, italic %}{% btn #,, fab fa-scribd %}</div>
		<div>{% btn #,, fab fa-google %}{% btn #,, fab fa-chrome %}{% btn #,, fab fa-opera %}{% btn #,, gem
			fa-rotate-270 %}</div>
	</div>
```

<div style = "display : flex ; align-items : center ; justify-content : center">
		<div>{% btn #,, heading %}{% btn #,, fab fa-edge %}{% btn #,, times %}{% btn #,, circle-notch %}</div>
		<div>{% btn #,, italic %}{% btn #,, fab fa-scribd %}</div>
		<div>{% btn #,, fab fa-google %}{% btn #,, fab fa-chrome %}{% btn #,, fab fa-opera %}{% btn #,, gem
			fa-rotate-270 %}</div>
	</div>



#### Group Pictures

`Group Pictures` is also an awesome plugin ! Maybe I will use `group picture` to build my photo repo .

* Usage

```
{% gp [number]-[layout] %}
{% endgp %}
```

`[number]` : optional . Total number of pictures .

`[layout]` : optional . The index of the layout, which can be obtained according to the figure below. For example, if you want to apply the second layout to 4 pictures, then use .

```
{% grouppicture 4-2 %}{% endgrouppicture %}
```

{% gp 2-2 %}

![group-picture-1.png](https://theme-next.js.org/images/group-picture-1.png)

![group-picture-2.png](https://theme-next.js.org/images/group-picture-2.png)

{% endgp %}

{% gp 5-2 %}

![NEXT.jpg](https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg)

![NEXT.jpg](https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg)

![NEXT.jpg](https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg)

![NEXT.jpg](https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg)

![NEXT.jpg](https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg)



{% endgp %}

{% note info %}

It's recommended to enable `fancybox` before using `group pictures` .

```yml
# _config.next.yml
fancybox: true
```

{% endnote %}



#### Link Grid

`Link Grid` is simple to `group pictures` , which may be called "Group Links" . It may be really awesome to code a page containing some commonly used links by `link grid` , I think .

* Usage

```
{% lg [image] [delimiter] [comment] %}
{% endlg %}
```

`[image]` : Optional parameter. Default image URL.
`[delimiter]` : Optional parameter. If the optional delimiter parameter is given, it is interpreted as the delimiter of items in each line.
`[comment]` : Optional parameter. If the optional comment parameter is given, it is interpreted as the symbol to comment out a line.

`{% lg [image]  %}`

```
{% lg %}
Theme NexT | https://theme-next.js.org/ | Stay Simple. Stay NexT. | https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg
Theme NexT | https://theme-next.js.org/ | Stay Simple. Stay NexT. | https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg
Theme NexT | https://theme-next.js.org/ | Stay Simple. Stay NexT. | https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg
Theme NexT | https://theme-next.js.org/ | Stay Simple. Stay NexT. | https://pic.imgdb.cn/item/65113dadc458853aef1da094.jpg
{% endlg %}
```

Well , maybe I get in trouble …… I can't add the code above here , otherwise there are many bugs after running `hexo clean && hexo g && hexo s` .  But in other file , such as `External-link` in menu ,  it works .I can't solve this problem ......

#### Mermaid & WaveDrom

Two plugins above is useful for flow chart painting .

#### Note

* **Settings**

```yml
# NexT config file
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```

* **Usage**

```
{% note [class] [no-icon] [summary] %}
Content
{% endnote %}
```

`[class]` : Optional . Supported values : `default` | `primary` | `success` | `info` | `warning` | `danger` 

`[no-icon]` : Optional . Disable icon in note .

`[summary]` : Optional . Summary of note .

* **Examples**

```
{% note %}
without define class style
{% endnote %}
```

{% note %}

without define class style
{% endnote %}





```
{% note default %}
default style
{% endnote %}
```

{% note default %}

default style

{% endnote %}





```
{% note primary %}
#### Primary Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note primary %}
#### Primary Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note info %}
#### Info Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note info %}
#### Info Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}





```
{% note success %}
#### Success Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note success %}
#### Success Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}





```
{% note danger %}
#### Danger Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note danger %}
#### Danger Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note info no-icon %}
#### No icon note
Note **without** icon: `note info no-icon`
{% endnote %}
```

{% note info no-icon %}
#### No icon note
Note **without** icon: `note info no-icon`
{% endnote %}



```
{% note primary This is a summary %}
#### Details and summary
Note with summary: `note primary This is a summary`
{% endnote %}
```

{% note primary This is a summary %}
#### Details and summary
Note with summary: `note primary This is a summary`
{% endnote %}

#### PDF

NexT also provide the `PDF` plugin which can add the pdf viewer in the website .



### Third Party Plugins

#### Pjax

> > Easily enable fast AJAX navigation on any website (using pushState() + XHR)
>
> Pjax is **a standalone JavaScript module** that uses [AJAX](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX) (XmlHttpRequest) and [pushState()](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Manipulating_the_browser_history) to deliver a fast browsing experience.
>
> *It allows you to completely transform the user experience of standard websites (server-side generated or static ones) to make users feel like they are browsing an app, especially for those with low bandwidth connections.*
>
> **No more full page reloads. No more multiple HTTP requests.**
>
> *Pjax does not rely on other libraries, like jQuery or similar. It is written entirely in vanilla JS.*

``"Make users feel like they are browsing an app" `` ?  True or False ?  Unbelievable !

Just Edit in `_config.next.yml`:

```yml
pjax: true
```

#### Math Equations

NexT also provide some plugins to render math equations . As I don't plan to write math notes on my blog (It's convient to write them in LaTex , isn't it ?) , so maybe next time !

#### Lazyload

It delays loading of images in long web pages. Images outside of viewport will not be loaded before user scrolls to them. 

```yml
# _config.next.yml
lazyload: true
```

#### Animation

NexT provide the animation behavoir . But for me , it's unnecessary , and I can close animation function to speed my website .

```yml
motion:
  enable: false
  async: false
  transition:
    # All available transition variants: https://theme-next.js.org/animate/
    menu_item: fadeInDown
    post_block: fadeIn
    post_header: fadeInDown
    post_body: fadeInDown
    coll_header: fadeInLeft
    # Only for Pisces | Gemini.
    sidebar: fadeInUp
```

#### Quicklink

[Quicklink](https://github.com/GoogleChromeLabs/quicklink) is a JavaScript plugin that faster subsequent page-loads by prefetching in-viewport links during idle time. Chrome, Firefox, Edge are supported without polyfills.

You can enable it by setting value `quicklink.enable` to `true` in NexT config file.

```yml
# _config.next.yml
quicklink:
  enable: true

  # Home page and archive page can be controlled through home and archive options below.
  # This configuration item is independent of `enable`.
  home: true
  archive: false
  Me: true 
  # "Me" is a menu item I add .
  # Default (true) will initialize quicklink after the load event fires.
  delay: true
  # Custom a time in milliseconds by which the browser must execute prefetching.
  timeout: 3000
  # Default (true) will attempt to use the fetch() API if supported (rather than link[rel=prefetch]).
  priority: true
```

#### AddToAny

Share your artical to external patform by `AddToAny` .

On my view , it's useless .........

```yml
# AddToAny Share. See: https://www.addtoany.com
addtoany:
  enable: true
  buttons:
    - wechat
    - facebook
    - twitter
```

#### Local Search

Plugin `hexo-generator-searchdb` provide our website with a local search engineer .

* Installation

```shell
$ npm install hexo-generator-searchdb
```

* Edit in `_config.yml`

```yml
search:
  path: search.xml
  field: post
  content: true
  format: html
```

* Edit in `_config.next.yml`

```yml
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
```



<strong style="color : orange ; font-size : 30px">All right , it's all about Hexo beautifying !</strong>











