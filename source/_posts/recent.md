title: recent
date: 2016-01-17 19:50:26
tags:
---

又有一个月没有更新博客了，首先要说的是上次的重要进展被跳票了，其实还是有一些结果的，只是不如预期，以及重点还是要放在中文文字识别上，英文的东西后来没仔细管。

最近很忙，实验室有两个项目，一个是数据堂的采集，在实时生成视频时遇到了性能瓶颈，还在想办法解决。另一个是要做多gopro的控制，用python写了一个脚本，有GUI，可以读入json生成不同的UI，感觉还不错。

![multi gro](http://i4.tietuku.com/bb840f6edbb6c704.jpg  "gopro")

<!-- more -->

在windows下用命令行连接wifi的命令如下

```
# delete all profile
netsh wlan delete profile *
# list all wlans
netsh wlan show interface
# list all ssid
netsh wlan show networks
# loop to add profile
netsh wlan add profile filename="GoproNumberX.xml" interface="WLAN name"
# loop to connect
netsh wlan connect name=GoproNumberX interface="WLAN name"
```

其实最近还做了一些事，比如nonobank。发现了一个大bug，可以登录任意用户，太神奇了，我报了乌云，然而总是被拒绝，太不友好了，放弃。给网站客服打了电话，根本不会有人理会。之前爬到了一些数据，大概是12.16的时候爬到了30万左右的交易，现在开始处理，然后用logistic和随机森林做了分类，随机森林完爆。接下来貌似还要重做一遍。。