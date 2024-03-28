---
title: Customized Website Article Link
abbrlink: 4bc5178d
date: 2021-10-01 17:00:00
tags: Web
---

### Abstract

In the default setting of Hexo and Next , my article 's usl is long and ugly ,  so I want to customize my article's lick ,which is concise and static .  Fortunately , I finally find a hexo plugin called `hexo-abbrlink` , which is really amazing !   Here is the link :  [rozbo/hexo-abbrlink: create one and only link for every post for hexo (github.com)](https://github.com/rozbo/hexo-abbrlink)

<!-- more -->

### Install and Configure it

1. **Add plugin to Hexo :**

```shell
npm install hexo-abbrlink --save
```

2. **Modify `config.yml`**

**Change the origin code** 

```yml
permalink: :year/:month/:day/:title/
```

**into**(You can choose one of two ways below :)

```yml
permalink: posts/:abbrlink/
```

**or**

```yml
permalink: posts/:addrlink.html
```

<strong style = "color = red ">Be careful : </strong> 

There is a `/` in the end of `permalink: posts/:abbrlink/` ,  and not in `permalink: posts/:addrlink.html` !    This `/`  is really important .



3. **Add the codes below in the end of file `config.yml`**

```yml
# abbrlink config
abbrlink:
  alg: crc32      #support crc16(default) and crc32
  rep: hex        #support dec(default) and hex
  drafts: false   #(true)Process draft,(false)Do not process draft. false(default) 
  # Generate categories from directory-tree
  # depth: the max_depth of directory-tree you want to generate, should > 0
  auto_category:
     enable: true  #true(default)
     depth:        #3(default)
     over_write: false 
  auto_title: false #enable auto title, it can auto fill the title by path
  auto_date: false #enable auto date, it can auto fill the date by time today
  force: false #enable force mode,in this mode, the plugin will ignore the cache, and calc the abbrlink for every post even it already had abbrlink. This only updates abbrlink rather than other front variables.
```

You can modifly the parameters `alg` and `rep` according to your preference , and their meaning is as follow :

```yml
alg -- Algorithm (currently support crc16 and crc32, which crc16 is default)
rep -- Represent (the generated link could be presented in hex or dec value)
```

4. **Final Nagging**

* If  your article 's  URL  is still the style `:year/:month/:day/:title/` ,  you may forget to change `permalink: :year/:month/:day/:title/`  into  `permalink: posts/:abbrlink/`  or `permalink: posts/:addrlink.html`  .   Another reason may be `"GitHub just can't response at once"`  , so just wait for minutes patiently ~
* Just don't forget the `/` in the end of `permalink: posts/:abbrlink/`

<del>Actually , I had fallen into two traps mentioned above .</del>

* **That's all .**