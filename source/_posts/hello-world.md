title: 博客开通，欢迎交流
date: 2015-07-04 22:31:10
categories: hexo
tags: [建站]
---
一直觉得应该有这么个东西，把日常遇到的技术问题和发现记录下来，和大家分享，但又觉得以前错过了太多太多，现在再开始已经来不及，其实什么时候开始都不算晚。花了一天时间建好了本站，下面写一些在建站时经历和遇到的问题。

本站基于[Github Pages](pages.github.com)和[Hexo](http://hexo.io/)，使用了[jacman](https://github.com/wuchong/jacman)模板，参考[Zipperary](http://zipperary.com/categories/hexo/)完成了绝大部分配置。下面按先后顺序简要介绍配置过程。

<!-- more -->

# Hello World
建立一个`hello world`并没想象容易。按先后顺序安装*nvm*、*nodejs*、*npm*、*hexo*，遇到一些小问题，主要是顺序要搞对。安装完就可以在本地生成预览页面了。之后关联到[Github](https://github.com/)即可拥有一个[*.github.io](http://seed93.github.io)的主页。

# 配置主题
先对[Hexo](http://hexo.io/)本身的`_config.yml`进行配置，然后选择了[jacman](https://github.com/wuchong/jacman) 主题，对该主题进行进一步定制。大部分时间花在了这里。

## 添加社交网站超链接
本来是一件很简单的事，[jacman](https://github.com/wuchong/jacman) 主题已经支持很多社交网站，但我想加入[人人](http://www.renren.com)和[图虫](http://tuchong.com)，所以首先需要改掉`footer.ejs`文件，很容易加入了链接，但是要选择对应的icon，这是个麻烦事。该主题的icon放在字体文件里，每个icon有一个序号索引，需要在字体文件中找到对应索引，如果没有需要的图标还需要自己手动添加到字体文件中。我选择了[typetool](http://www.fontlab.com/font-editor/typetool/)软件来编辑字体文件，打开`fontdiao.ttf`即可看到各种图标，其中包含了[人人](http://www.renren.com)的，只需要读取索引即可。而[图虫](http://tuchong.com)这个网站并不多见，网上也没有找到合适的logo，最后决定自己添加一个`图`字作为图标。下载好字体，由于不会搜索，翻了好久才在几千个汉字中找到它，复制粘贴到`fontdiao.ttf`中，另存为，大功告成。加载使用后发现没有生效，可能是除了`.ttf`要修改外，其他格式的字体也需要同步修改。没有一个合适的离线软件，在线网站[font squirrel](http://www.fontsquirrel.com/tools/webfont-generator)生成的字体只支持常用标点字符，最后使用[font2web](http://www.font2web.com/)得到完整的字体包。后来还遇到了浏览器缓存的问题，需要清除缓存才可以重新加载。至此，网页上链接了8个个人社交网站，欢迎访问。

### 添加多说评论系统
该主题对[多说](http://duoshuo.com/)支持很好，多说本身也比较简单易用，配置过程中没有花费太多经历，一次成功。
后来手动添加了最近访客和最新评论，这两个widget在原主题是没有的，自己从[多说](http://duoshuo.com/)上找到了相应代码，照猫画虎添加了这两个widget。其中最近访客没有边框，于是手动添加了一个`div`搞定。

## 添加访问量统计
先是在[bruce](http://service.ibruce.info/)看到了可以统计访问量，简单添加后真的work了，只不过自己加的位置比较糟糕。后来看到该主题支持百度统计，于是注册了后者，百度统计已经更新代码，主题还没有同步更新。使用百度统计后发现`hexo g`以后`index.html`会出现无法解析`analytics.ejs`文件的情况。`hexo clean`后删除该文件，`index.html`居然不会变，看上去和`analytics.ejs`文件毫无关系，但如果改一下`<%- partial('analytics') %>`的文件名就会有变化。~~最后解决办法是改文件名，原因未知。~~已经发现原因，使用gedit编辑文件时产生了`~`，在读取时候读的是旧文件。PS：百度统计只是静默统计，用于主页所有者后台分析，不会实时显示访问量。

## 添加搜索
参考[coney](http://gengbiao.me/hexo/hexo%E6%B7%BB%E5%8A%A0%E7%99%BE%E5%BA%A6%E7%AB%99%E5%86%85%E6%90%9C%E7%B4%A2/)的文章，一步步艰难做完，发现到最后[Github](https://github.com/coneycode/hexo-generator-baidu-sitemap)项目上的时候，被告知[baidu](http://www.baidu.com)无法抓取[Github](https://github.com/)内容了，于是放弃。
转而使用[tinysou](http://tinysou.com/)，这个很简单，有一个短短的视频，边看边做几分钟搞定，效果还很不错。

# 购买域名
大部分博客推荐在[Godadday](https://www.godaddy.com)网站购买，我看了一下并没有想要的，而且稍贵，最后选择在[万网](http://www.net.cn/)购买，`seed93`的域名大部分都有，而且也不贵，但是名字略长，还是想买个短一点的，就还是选了`ld99`，`.com`和`.cn`都被注册了，`.net`不是很喜欢，最后选择了`.top`，这个国际顶级域名是去年年底开放注册的，`top`意味着最顶级，正好我的名也是`ding`这个谐音，域名一共7个字符，也比较简单，就这样决定。接下来就是关联[DNSPOD](https://www.dnspod.cn)，一开始总是无法访问，原因是我绑定了`*.ld99.top`的二级域名，一级域名没有绑定。~~目前的问题是我自己`ubuntu`系统用firefox无法访问自己主页，会被铁通DNS解析错，但是ping的时候却可以显示解析到了[Github](https://github.com/)，该问题待解。虚拟机没有问题。~~过了一天这个问题自己好了，可能是铁通DNS 更新延迟。