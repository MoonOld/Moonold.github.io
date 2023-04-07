---
title: "使用Hugo搭建Fix it主题博客"
subtitle: "记录搭建这个博客过程的一些细节和坑点"
draft: false
descriprion: this is a test md
date: 2023-03-06T23:55:17+08:00
type : posts
keywords: abc
tags:
- Hugo
- Fix it
- 技术

categories:
- 环境搭建


lightgallery : true

---
我个人使用的是FixIt主题，可以直接在这个网站得到中英文的文档帮助。[FixIt](https://fixit.lruihao.cn/zh-cn/)
如果是零基础的同学想要依照此博客进行搭建网站，在初学时我建议先把demo抄下来，然后开着本地server进行调试和学习。关于本地server的部分将在后面进行介绍。

## Hugo是什么
![hugo_logo](/images/hugo_logo.svg)

**Hugo** 是一个用Go写的静态页面生成器，可以用 *markdown文档* 直接生成对应的静态网页，目前应该是Github上同类框架的代码仓库里star最多的。如果你不知道什么是静态页面生成器，你可以简单认为，他就是可以让你的markdown文档被转化成一个网页，使用社区提供的主题可以简单地将这些东西转化成一个有组织和结构的网站。也就是说，你至少要会写 ***Markdown文档*** 。
想具体了解Hugo的原理，请参考[官方文档](https://gohugo.io/)。通过文件夹和资源路径映射，就可以很有条理地理清楚Hugo具体的工作方式了。

## Quick Start
首先需要安装Hugo，通过以下官方文档可以获取：[hugo安装教程](https://github.com/gohugoio/hugo#install-hugo-as-your-site-generator-binary-install)  

安装Hugo之后，通过终端，在你想的文件目录下使用```hugo new site sitename```来生成一个hugo配置的网站文件夹。
> 请注意这里的最后一个参数 “sitename”指的是你对自己的网站的命名。你可以使用```hugo new site A```来生成一个名称为A的网站项目。  

你可以通过安装hugo后直接通过```git clone```或者通过子模块```git submodule add```指令来把一个主题安装到自己的site目录下。主题可以去[Hugo主题](https://themes.gohugo.io/)页面中找一个自己喜欢的。考虑到新入手Hugo，并且可能没有扎实的前端和运维基础，我个人相当推荐只使用一个没有什么外部依赖，也不需要修改JS代码之类的主题。对于我自己，我使用的FixIt主题可以直接通过在site主目录下使用一下命令将主题引入。
``` Bash
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
```
虽然Toml的hugo配置化已经足够简单，但是考虑到初学者可能对于复杂的理念和不知具体作用的Option还是会有无头苍蝇的感受，我建议最开始直接将配置抄到自己的config文件里。
引入之后，需要简单配置一下自己的site/目录下的 *config.toml* 文件来配置一下基本属性。入门的时候，推荐直接写到一个config文件里面，因为这样架构比较简单。熟悉之后可以通过配置文件夹的方式去隔离化。以下是我的config配置内容。请简单阅读一下[官方文档](https://gohugo.io/content-management)或者通过[FixIt文档](https://fixit.lruihao.cn/zh-cn/documentation/basics/#theme-configuration)来加深理解。
``` toml
title = "MoonOld Blog"
baseURL = "http://localhost/"
# 设置默认的语言 ["en", "zh-cn", "fr", "pl", ...]
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
enableGitInfo = true


# 更改使用 Hugo 构建网站时使用的默认主题
theme = "FixIt"

[params]
  # FixIt 主题版本
  version = "0.2.X"
  description = "MoonOld古月 Blog"
    externalIcon = true
[params.header]
        desktopMode = "sticky"
    [params.header.title]
        name = "MoonOld Blog"
        logo = "/favicon-32x32.png"
[params.social]
    GitHub = "MoonOld"
    Email = "MoonOld.h@gmail.com"
[params.footer]
    enable = true
    hugo = true
    copyright = true
    author = true
    since = 2023
    wordCount = true
    visitor = true
    [params.footer.order]
        powered = 1
        statistics = 2
        visitor = 3

[params.section]
    paginate = 10
    dateFormate = "2000-02-02"
    [params.section.recentlyUpdated]
        enable = true
        days = 30
        maxCount = 10
[params.home]
    # FixIt 0.2.0 | 新增 RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      #gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/avatar.png"
      # FixIt 0.2.17 | 新增 头像菜单链接的 identifier
      #avatarMenu = ""
      # FixIt 0.2.7 | 更改 主页显示的网站标题（支持 HTML 格式）
      #title = ""
      # 主页显示的网站副标题
      subtitle = "想给狂放的想法安个家"
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      # FixIt 0.2.0 | 新增 免责声明（支持 HTML 格式）
      disclaimer = ""
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
[params.page]
    authorAvatar = true
    readingTime = true
    endFlag=""
    autoBookmark = true
    #author = "MoonOld"
    [params.page.toc]
        enable = true
        auto = true
        position = "right"
    [params.page.code]
        copy = true
        maxShownLines = 10
    [params.page.comment]
        enable = true
        [params.page.comment.artalk]
    [params.page.share]
        Myspace = false
        twitter = false
        facebook = true
        weibo = false
        zhihu = true
        Linkedin = true

[params.typeit]
    # typing speed between each step (measured in milliseconds)
    speed = 100
    # blinking speed of the cursor (measured in milliseconds)
    cursorSpeed = 1000
    # character used for the cursor (HTML format is supported)
    cursorChar = "|"
    # cursor duration after typing finishing (measured in milliseconds, "-1" means unlimited)
    duration = -1
[params.app]
    # optional site title override for the app when added to an iOS home screen or Android launcher
    title = "MoonOld Blog"
    # whether to omit favicon resource links
    noFavicon = false
    # modern SVG favicon to use in place of older style .png and .ico files
    svgFavicon = ""
    # Safari mask icon color
    iconColor = "#5bbad5"
    # Windows v8-10 tile color
    tileColor = "#da532c"
    # FixIt 0.2.12 | CHANGED Android browser theme color
    [params.app.themeColor]
     # light = "#f8f8f8"
     # dark = "#252627"
[params.search]
    enable = true
    # type of search engine ["lunr", "algolia", "fuse"]
    type = "lunr"
    # max index length of the chunked content
    contentLength = 4000
    # placeholder of the search bar
    placeholder = ""
    # FixIt 0.2.1 | NEW max number of results length
    maxResultLength = 10
    # FixIt 0.2.3 | NEW snippet length of the result
    snippetLength = 30
    # FixIt 0.2.1 | NEW HTML tag name of the highlight part in results
    highlightTag = "em"
    # FixIt 0.2.4 | NEW whether to use the absolute URL based on the baseURL in search index
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""
    [params.search.fuse]
      # FixIt 0.2.17 | NEW https://fusejs.io/api/options.html
      isCaseSensitive = false
      minMatchCharLength = 2
      findAllMatches = false
      location = 0
      threshold = 0.3
      distance = 100
      ignoreLocation = false
      useExtendedSearch = false
      ignoreFieldNorm = false



[params.githubCorner]
    enable = true
    permalink = "https://github.com/hugo-fixit/FixIt"
    title = "在 GitHub 上查看源代码"
    position = "right" # ["left", "right"]

 [params.backToTop]
    enable = false
    # 在 b2t 按钮中显示滚动百分比
    scrollpercent = false

[params.readingProgress]
    enable = true
    # 可用值：["left", "right"]
    start = "left"
    # 可用值：["top", "bottom"]
    position = "top"
    reversed = false
    light = ""
    dark = ""
    height = "2px"

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    ################## 必要的配置 ##################
    # https://github.com/hugo-fixit/FixIt/issues/43
    codeFences = true
    lineNos = true
    lineNumbersInTable = true
    noClasses = false
    ################## 必要的配置 ##################
    guessSyntax = true
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

[author]
  name = "MoonOld"
  email = "MoonOld.h@gmail.com"
  #link = ""
  avatar = "/avatar.jpg"

[frontmatter]
  date = ["date", ":filename", "lastmod", ":git"]
  lastmod = [":git", "lastmod", ":fileModTime", ":default"]



[menu]
  [[menu.main]]
    identifier = "posts"
    name = "文章"
    url = "/posts/"
    weight = 3
  [[menu.main]]
    identifier = "categories"
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 1
        [[menu.main]]
        identifier = "music"
        name ="音乐"
        url = "/categories/音乐" 
        parent = "categories"
        [[menu.main]]
        identifier = "env"
        name ="环境搭建"
        url = "/categories/环境搭建" 
        parent = "categories"

  [[menu.main]]
    identifier = "tags"
    name = "标签"
    url = "/tags/"
    weight = 2
    [[menu.main]]
        identifier = "about"
        name = "关于 MoonOld"
        url = "/about"
        weight = 4
    [[menu.main]]
    identifier = "friends"
    name = "友链"
    url = "/friends/"
    title = "友情链接"
    weight = 21
    parent = "about"
        [menu.main.params]
        icon = "fa-solid fa-users"


[outputs]
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```

配置好之后可以直接通过 在site目录下执行```hugo new posts/posts_name.md```就可以生成名为posts_name的一篇文章，位于site/content/posts/posts_name.md 打开这个文件，可以看到里面已经有Hugo自动生成的一些属性配置了。这些属于Front Matter配置。其中```draft:true```选项直接标记了这篇文章是草稿，正式渲染的时候不会显示这篇文章。  
接下来在这篇文章中写一些你正在思考的东西。写完并保存后，可以在site目录下使用```hugo server -D```启动本地服务器。其中的```-D```选项表示这个服务器会渲染Draft（草稿）网页。  

如果没有什么别的问题的话，你至少正式迈出了Hugo网页搭建的第一步。恭喜你，拥有了可以让别人通过这万物互联的网络看见你的基础之一——静态网页。

## Front Matter选项
Front Matter是写在你的content目录下的markdown文件头的，可以通过这些配置非常简易地配置你的网页。类似这篇文章的Front Matter就通过 ```type```选项确定了网页模版，```tags``` ```categories```则分别确定了这篇文章的类型。
``` YAML
---
title: "使用Hugo搭建Fix it主题博客"
subtitle: "记录搭建这个博客过程的一些细节和坑点"
draft: true
descriprion: this is a test md
date: 2023-03-06T23:55:17+08:00
type : posts
keywords: abc
tags:
- Hugo
- Fix it
- 技术

categories:
- 环境搭建


lightgallery : true

---
```
具体配置可以查看[Hugo官网链接](https://gohugo.io/content-management/front-matter/)或者开头提到的FixIt文档，或者你自己选择的那个主题所提供的文档去详细参阅。我个人其实比较推荐，去看一下总体提供什么功能，自己用到的时候能知道有什么自己需要的，再去详细查看即可。以下收集一些自己用到的Option。

* lightgallery true可以在外面显示avatar
* hugo summary默认截取前70个字符进行摘要显示（在home），或者以 ***<\!--more-->*** 来标记之前的内容形成摘要。

## 评论系统
一般Hugo主题都会提供评论系统的支持。以我选择的Fixit主题为例，可以在```params.page.comment```中进行评论系统的配置。  

主流的评论系统giscus\utterancs\commento等。giscus通过github repo的discussions获取评论数据，所以在github page上可以非常简单的应用。下面以选择giscus进行配置为例，可以大致列出流程。
* 在Github Settings里中的Applications进行giscus的安装
* 在Settings中进入giscus应用的配置页面，将目标仓库的访问权限给予giscus
* 为目标仓库开启discussions模块
* 在[giscus网站](https://giscus.app/zh-CN)上获取配置，在这里可以顺便检测自己仓库是否可以用来使用giscus
* 将最后生成的配置填入自己主题的config选项中，包括mapping、repo、category等配置  

需要注意Front Matter中有```comment:```选项，此项为false时禁用评论系统。

## 踩坑点记录
* 用edge没有正确显示icon,调试了很多次才发现。最后决定一直用chrome了
* hugo的post目录下index文件是直接映射到文件目录的，可以直接引用目录下的文件。但是如果不是index的文档，就必须要从static里面引用了。这个很类似rust的crate设计。