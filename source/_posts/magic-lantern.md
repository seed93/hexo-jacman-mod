title: magic lantern
date: 2016-07-25 16:14:26
tags:
---
最近看到了一个自称是[中文魔灯官方网站](http://www.tecdee.com/)，做了汉化版[magic lantern](http://magiclantern.fm/)，本来ML是开源的，仅仅是汉化后就要收费128，而且版本也不更新，挺气愤的，就想自己汉化一下，然而失败了。。真的很难。

首先遇到的困难是编译ML，需要用arm的交叉编译，按官方的contrib/toolchain/里的脚本来是不好使的，要安装gcc4.6.2，就困难重重，后来偶然看到这个[帖子](http://www.magiclantern.fm/forum/index.php?topic=14725.0)，简单下载现成的编译器就可以，果然，很快搞定。

之后遇到的问题是没法显示汉字，发现是字体的问题，没有汉字字体，我下载了tecdee网站提供的包，提取出rbf中文字体（[rbf](http://chdk.wikia.com/wiki/RBF_fonts)字体是嵌入式设备专用的字体），然而一载入字体机器就挂了。经过论坛上询问，版主建议用QEMU调试。QEMU倒也好配置，就是中间少了一步make clean，一直没出来图像。有了QEMU以后调试方便了很多，后来发现汉化是一件很难的事，有以下几个问题：

* 中文字体太大，占用内存
* rbf字体不支持中文的codepage
* 代码里用的都是char来描述字符，只能表示256个字符，中文需要改成wchar

具体可见[帖子](http://www.magiclantern.fm/forum/index.php?topic=17581.0)，最终放弃。
