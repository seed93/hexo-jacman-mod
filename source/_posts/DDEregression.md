title: DDEregression
date: 2015-08-14 14:09:47
tags: 3D
---

这是接下来要做的一项工作，会花一些精力来做，可能需要一位帮手一起。Paper的名字叫[Displaced Dynamic Expression Regression for Real-time Facial Tracking and Animation](http://gaps-zju.org/DDE/DDEregression.pdf)，是浙江大学[zhou kun](http://kunzhou.net/)组的工作，第一完成人Cao Chen围绕这个课题做了很多出色的工作，将face 3D alignment and animation在RGB camera上做到了极致，这一点我在CVPR 2015上也从[Li Hao](http://www.hao-li.com)处得到了证实。这项工作效果非常棒，Demo很炫酷，可以参见下面的视频

{% video '<iframe height=498 width=510 src="http://player.youku.com/player.php/sid/XMTMwOTQ0NzE4MA==/v.swf" frameborder=0 allowfullscreen></iframe>' %}

<!-- more -->

# 项目介绍

如上面的视频介绍，这项工作仅使用单个彩色相机就可以实时获得人物的三维面部模型，该模型可以较精确地描绘人物表情，还可以利用模型驱动虚拟3D模型，让avatar实时保持和你相同的表情。这项应用intel也在做，叫[pocket avatar](http://newsroom.intel.com/docs/DOC-5482)，个人认为intel在CV领域和学界、业界差距还比较大，不过多评论。这个技术能用来做什么呢？一个是像intel那样，提供一种新的社交方式，用表情传递消息，还有人提出可以用这个技术来匿名聊天、交友。当然还应该有更多的应用，比如增强[我是表情帝](https://itunes.apple.com/app/id883273541)，可以用来辅助人脸活体检测等等。技术很好，应用也很多，但不知道为什么还没有推广开。我先来做一做吧。

# 技术思路

![framework](http://i1.tietuku.com/55961e8ea4d07fe4.png  "framework")

技术框架见上图，大部分操作都是在训练regression model，需要的外部数据：三维模型数据库，二维alignment数据库。文章使用的是他们组自己做的FaceWareHouse数据库，这个库很厉害，是非商业化的，有150个人，每人46个表情的blendshape，所有模型都是拓扑一致的，据说拓扑一致还是比较难做的。

DDE Regression是核心，用FaceWareHouse数据库的blendshape作为一组基，假设任何人的任何表情都是其线性表示，用两个归一化的向量e、u来描述表情、identity。除了要估计这两个向量，还需要相机的外参，也就是旋转矩阵R和平移向量t。Q是相机内参，相机不变的情况下Q保持不变。由于三维点有偏差，需要用二维alignment来修正，修正量称为D，真实的二维点投影是S。以上所有变量组成DDE模型核心公式

$$
\text{DDE}(\mathbf{Q}, \mathbf{u}; \mathbf{e}, \mathbf{R}, \mathbf{t}, \mathbf{D}) = \mathbf{S}
$$

其中$\mathbf{Q}$只与相机有关，$\mathbf{u}$只与identity有关，因此在多数情况下是不变的。$\mathbf{e}, \mathbf{R}, \mathbf{t}, \mathbf{D}$构成了shape vector
$$
\mathbf{P} = (\mathbf{e}, \mathbf{R}, \mathbf{t}, \mathbf{D})
$$

至于DDE模型是怎么计算的，其实也很简单

$$
\begin{cases}
\mathbf{F} = \mathbf{R}(\mathbf{Be^T}) + \mathbf{t} \\\
\mathbf{B} = \mathbf{Cu^T} \\\
\mathbf{S} = \mathbf{V} + \mathbf{D} 
\end{cases}
$$

至于怎么由$\mathbf{Q}$怎么算，以及如何用$\mathbf{F}$和$\mathbf{Q}$得到二维点$\mathbf{V}$，是最简单的计算摄像学，不再赘述。

DDE训练的时候先根据二维点拟合得到最优$(\mathbf{Q}, \mathbf{u}; \mathbf{P})$，然后对每个变量进行绕动训练得到绕动前和绕动后的回归映射。

CPR我还没仔细看，这个是在另一篇[3D Shape Regression for Real-timeFacial Animation](http://www.ccf.org.cn/resources/1190201776262/digital/SIGGRAPH/vface2013-10-24-02_01_36.pdf)里介绍的。核心是

$$ \text{CPR}(I, \mathbf{Q}, \mathbf{u}; \mathbf{P}^{in}) = \mathbf{P}^{out} $$

从CPR输出后在经过一步后处理，做一个联合优化

$$
{% raw %}
\begin{cases}
E_{fit}= & \sum_{k=1}^{m}\left\Vert \prod_{\mathbf{Q}}\left(\hat{\mathbf{R}}^{t}\left(\mathbf{B}\hat{\mathbf{e}}^{tT}\right)+\hat{\mathbf{t}}^{t}\right)-\mathbf{s}_{k}^{t}\right\Vert ^{2}\\
E_{reg}= & \left\Vert \hat{\mathbf{P}}^{t}-\bar{\mathbf{P}}^{t}\right\Vert ^{2}\\
E_{tm}= & \left\Vert \hat{\mathbf{P}}^{t-2}-2\hat{\mathbf{P}}^{t-1}+\hat{\mathbf{P}}^{t}\right\Vert ^{2}
\end{cases}
{% endraw %}
$$

$${% raw %}
E_{tot} = E_{fit} + \omega_{reg}E_{reg} + \omega_{tm}E_{tm}
{% endraw %}$$


由于$\mathbf{Q}$和$\mathbf{u}$不需要经常更新，因此需要一个动态的更新过程，维护一个frame集合，将初始几帧默认放进去，后面的帧如果表情向量PCA投影和与原空间偏离过多，则加入该帧。最后一起做一个简单的优化即可。

实现的思路大致是这样，有空就来做一做。