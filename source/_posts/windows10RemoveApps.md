title: 卸载windows 10系统应用
date: 2015-07-18 17:04:01
categories: 工程配置
tags: [windows10]
---
最近Windows 10更新了10240版本，虽然官方没有说这就是RTM版，但从10166到10240变化这么大（虽然我没体会出除了版本号的变化之外的变化），大部分人倾向于认为这就是所谓的RTM了。由于从10166版本后系统自带的OneNote就会出现“很抱歉，出现问题，无法登录，请稍后尝试”的错误，延续到10240，花了一点时间来解决该问题。

<!-- more -->

# 卸载系统应用
网上没有找到现成的方法，只好尝试卸载系统应用，重新安装。参考[这篇文章](http://www.xitonghe.com/jiaocheng/Windows10-1075.html)卸载即可，第一步找到`WindowsPowerShell`，右击管理员运行，然后使用`get-appxpackage`模糊查找到OneNote的记录，最后remove。连起来就是
```
get-appxpackage *OneNote* | remove-appxpackage
```
卸载后使用应用商店重新安装，OneNote重建光明。
![Win10自带OneNote](http://i1.tietuku.com/6bd0b7565850e066.jpg "Win10自带OneNote")

# Windows 10已知bug

* 显卡问题从使用windows 10以来就没有解决过，我的机器是ThinkPad S1 Yoga，HD4600显卡，系统自带驱动版本是10.18.15.4235，显示不正常，需要使用10.18.14.4206版本。

* 使用[Clover](http://cn.ejie.me/)时，偶尔会无限创建新窗口，不确定是资源管理器问题还是兼容性问题。