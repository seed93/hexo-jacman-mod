title: ctc
date: 2015-08-06 09:54:45
tags: [CNN, RNN]
---

半个多月没更新博客，因为真的很忙，就做了一件事，都还没做好。现在有点空余时间赶紧记录一下。

# 任务

最开始决定要做的任务是车牌识别，这算是文字识别中的一个特殊情况，此处所说的文字识别不是OCR，希望能做一个比较好的通用的文字识别算法，包括街景文字、书面文字、车牌等。假定已经通过检测算法得到了比较好的bounding box，只需要从中识别出正确的字符串。如下图：

{% raw %}
<img src="http://i3.tietuku.com/d545e6fa2552313d.jpg" style="display:block;margin:auto"/>
{% endraw %}
从图中得到"tickets"即可。

<!-- more -->
# 方法

用什么方法比较好呢？和图片相关的显然是DeepLearning大法好，但是由于图片尺寸变化较大，且字符串长度不一，CNN并不能直接应用。传统的OCR做法是检测、分割、单字符分类，每一步都会损失信息，最终结果不好。使用DL的有几个做法，一种是基于词库，就是有一个已知的字典，假设要识别的图片文字都来自于词库中的单词，只需要做分类任务即可，因此和Imagenet的任务很相似，在数据量很大的条件下，准确率可以做得很高，因为大大限制了输出的空间。还有一类做法是用RNN（Recurrent Neural Network），这类算法把图片和字符串认为是一个序列，通过增加时间上的联系来提高准确率。所谓时间联系指的是相邻字符的关联，更准确一点，指的是图片相邻区域对最终字符串的不同影响。

我们采用了后一种方法，RNN中比较常用的方法是LSTM（Long Short Term Memory），可以避免梯度在传递过程中的不稳定。本文不打算介绍LSTM，仅挂出一张自制的示意图供参考。

{% raw %}
<img src="http://i1.tietuku.com/20fbb55c61fcfc06.png" style="display:block;margin:auto"/>
{% endraw %}

本文想着重介绍的是CTC（Connectionist Temporal Classification），这个是整个框架中的重要一环，用于将LSTM的输出映射到最终的结果，主要解决的是alignment的问题。比如在测试的时候，假如groundtruth是"good"，而LSTM的输出是"-gg-o---oo--d-"，"-"代表非字符，用"-"分成若干区域，每个区域中取最大概率的字符，最后连在一起就是最终结果。

# 现有CTC实现

大部分CTC都是python版本，基于[theano](https://github.com/gwtaylor/theano-rnn)实现，例如[mohammadpz][1]、[rnn_ctc][2]，由于依赖theano，函数体并不是很清楚地展现CTC实现过程，移植起来比较麻烦。[stanford-ctc][3]写的比较清楚，是论文的完整实现。

# 参考链接
[1] [mohammadpz][1]
[2] [rnn_ctc][2]
[3] [stanford-ctc][3]
[4] [patyork][4]

[1]: https://github.com/mohammadpz/CTC-Connectionist-Temporal-Classification/tree/master "mohammadpz"
[2]: https://github.com/rakeshvar/rnn_ctc "rnn_ctc"
[3]: https://github.com/amaas/stanford-ctc "stanford-ctc"
[4]: https://github.com/patyork/connectionist-temporal-classification