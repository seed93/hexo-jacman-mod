title: sublime
date: 2015-12-15 13:17:42
tags: sublime
---

最近把sublime好好配置了一下，更好用啦，在这里整理一下我装的插件。

<!-- more -->

# package control
首先肯定是要装[package control](https://packagecontrol.io/)，这个很早就装过了，以至于我都不记得是怎么装得，反正装完以后是很好用的，所有的安装包只要点击Preferences->Package control就可以用了，点install来安装包，很方便。

# Alignment

这个包是很早之前装的，一直没用过，那个default快捷键是ctrl+alt+A，然而好像和我系统冲突掉了，于是我就改成了ctrl+shift+S，这个包的功能是对=号等字符对齐，功能不是很强。顺便提一下，一直不知道怎么用sublime自动对齐，原来就在Edit->Line->Reindent，可以设置快捷键，我设置了ctrl+shift+A，对齐效果还可以吧，好像对于caffe来说不太好，因为有namespace么。。

# Bracket Highlighter

这个是用来高亮括号等匹配的，各种配对符号都可以，甚至包括#ifdef和#endif，可以设置各种匹配的颜色和风格，我都设置成solid了，感觉比underline要明显一些。
附一张效果图
![highlight效果图](http://i4.tietuku.com/af605eb91f438b19.png  "highlight效果图")

# Ctags

这个也是比较早的时候装的，当时为了自动补全，还需要手动生成tags，不是很好用，而且也不能跨文件补全。不太爽，可以被[SublimeClang](#SublimeClang)替代了。

# DocBlockr

用于自动生成函数和类的注释，使用方法是在函数上文输入/**后加tab，可以自动生成如下图所示的说明，然后自己修改。而在caffe的项目里这个不好使。。
![DocBlockr效果图](http://i12.tietuku.com/4bdeecea3304c70d.png)

# SublimeClang

这个是最麻烦的，当然也是极屌的，用于C/C++自动补全。正常安装完会出现错误
```
Unfortunately ctypes can't be imported, so SublimeClang will not work.
```
按照<a href="http://blog.chinaunix.net/uid-28894229-id-3839961.html">博文</a>的操作后就可以搞定，然而不知道可不可以用其他python版本，反正我没试好。
然后需要配置路径。
1. `"additional_language_options"`下的`"c++" `添加`["-std=c++11"]`以支持C11
2. `"options"`下的路径按实际位置改好，我的是
```
"options":
[
    "-isystem", "/usr/include",
    "-isystem", "/usr/include/c++/4.9",
    "-isystem", "/usr/include/x86_64-linux-gnu/c++/4.9",
    "-Wall"
]
```
3. `"diagnostic_ignore_dirs"`这里最好把C++的目录加进来，否则会有一对红色的错，很烦。
```
"diagnostic_ignore_dirs":
[
    "/usr/include/c++/4.9/"
]
```
4. 配置project里的路径，以caffe为例，新建一个sublime project，假设叫caffe.sublime-project，在caffe根目录下，按如下配置
```
{
	"folders":
	[
		{
			"path": "/home/liangd/text_recog/caffe-master-parallel"
		}
	],
	"settings":
	{
		"sublimeclang_additional_language_options":
		{
			"c++" : ["-std=c++11"],
			"c": [],
			"objc": [],
			"objc++": []
		},
		"sublimeclang_options":
		[
			"-I${folder:${project_path:caffe.sublime-project}}/include/",
			"-I/usr/local/cuda/include",
			"-I${folder:${project_path:caffe.sublime-project}}/build/src/",
			"-I/usr/include/hdf5/serial/",
			"-isystem", "/usr/include",
			"-isystem", "/usr/include/c++/4.9",
			"-isystem", "/usr/include/x86_64-linux-gnu/c++/4.9",
			"-Wall"
		]
	}
}

```
这里的选项会覆盖默认设置。
![SublimeClang效果图](http://i12.tietuku.com/9782eca1e1e40c63.png)

# SublimeCodeIntel

很早之前装的，据说对python自动补全有作用，然而我不太熟python，每次都是去直接百度的。。略过

# Trailing Spaces

这个对处女座很棒，可以把多余的空格和tab标红，还可以一键去掉。
![Trailing Spaces效果图](http://i12.tietuku.com/80c605692e5f6379.png)

# SublimeAStyleFormatter

立威推荐了这个对齐工具，比sublime自带的好很多。

# 关于中文输入

参考<a href="http://my.oschina.net/tsl0922/blog/113495">这一篇博客</a>就可以搞定，但是也不是很好用，有时候中文还是会失效，重开一下就可以了。
