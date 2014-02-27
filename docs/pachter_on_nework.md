---
layout: docs
title: Pachter's comment on 2 nbt paper &bull; 翻译
prev_section: snf_code
permalink: /docs/discuz_grn/
---

> 此翻译尚未授权，Just pre-print as an draft, 我正在和Pachter教授联系 ... 

> 引子 :<br>
Pachter 教授就2013年8月份在nbt的一个关于 ' Network cleanup ' 的专题中报道的两篇文章做了一个评论，文中很多观点这两天都让我meditate
很久，自己似乎走在错误的道路上越来越远，如当头棒喝。Prof. Pachet 评论的两篇文章都是我今年最感兴趣的两篇NBT paper。出于自己的个人趣味，我还不断的把这两篇文章推荐给别人。。。But，I realized I am totally wrong and feel sorry about that.


### Post 1 &bull;  The network nonsense of Albert-László Barabási 

### Albert-László Barabási 的那篇网络文章是扯淡 !!

Nature Biotechnology 杂志，2013年8月刊, 以背靠背的形式发表了两篇网络领域的理论文章：
	
* 1, Baruch Barzel & Albert-László Barabási, <a href="http://www.nature.com/nbt/journal/v31/n8/abs/nbt.2601.html">Network link prediction by global silencing of indirect correlations</a>, Nature Biotechnology 31(8), 2013, p 720–725. doi:10.1038/nbt.2601.

* 2, Soheil Feizi, Daniel Marbach, Muriel Médard & Manolis Kellis, <a href="http://www.nature.com/nbt/journal/v31/n8/abs/nbt.2635.html">Network deconvolution as a general method to distinguish direct dependencies in networks</a>, Nature Biotechnology 31(8), 2013, p 726–733. doi:10.1038/nbt.2635.

我把我的这个系列命名为“帕三篇”，这个是“帕三篇”的第一篇，在我的“帕三篇”系列里，我会与我的学生，Nicolas Bray 讲述关于这两篇文章的一切，以及为何我们花时间阅读，批评他们。

我们以 Barzel-Barabási 的文章开始，这篇文章是关于对 Barzel 和他的PhD 导师, Ofer Biham 所提出的模型的应用（这里注意一下，虽然都姓B，但是Biham和Barabási 是不一样的`:)`)。

为了定量描述生物网络的联通性，Barzel 和 Biham 提出了一个实验扰动模型 <a href="http://arxiv.org/pdf/0910.3366.pdf">Quantifying the connectivity of a network: The network correlation function method</a>, 这篇PRE文章组成了Barzel-Barabási 这篇nbt文章的基础。在生物学问题的背景下，网络中的链接预测涉及从数据中获取对基因间功能链接的识别, 但是，这些从数据中预测的链接可能会被网络链接中的间接作用效应所混淆. 例如，在一个调控拓扑网络里面，基因A对基因B又抑制作用，于此同时，基因B有抑制基因C的表达，通过传递效应，我们可以推知，如果A的表达增加了，B的表达就会下降，那么C的表达就会增加。这样一来，即使实际上A与C之间没有实际上的基因调控的之间链接关系, 我们也会在数据观测中发现基因A和基因C的表达出现正相关。

Barzel-Biham 的模型以一个扰动实验为基础，他们假设了在调控网络中的每一个基因都是处于稳态，如此一来，一旦改变了这个稳态网络中某一基因的表达，在网络中的其他基因就会出现对此扰动的相应的小的扰动作为响应。

> 所以，帕教授一直都在强调这是 Barzel-Biham model，而网络皇帝Barabási只是应用了这个PRE上的工作。

在Barzel-Biham 模型中的参数是一个被叫做局部响应矩阵的矩阵 $S$, 在这个矩阵中，$S_{ii} = 0$, 然后，基于关于平衡态扰动的物理论证，我们可以得到方程：

$G=SG$( 非对角线) 同时，$G_{ii} = 1$ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; (1)

因为这里的"全局响应矩阵" $G$ 原则上是可以被观测的，同时也可以被用于局部响应矩阵$S$ 的推断，这也就是这篇文章中Barzel 和 Barabási的创新之处：给通过G恢复局部响应矩阵S提供了一个近似公式：

$S\approx(G-I+D((G-I)G)^{-1}$  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; (2)

这里D作为算符的作用是让M矩阵的非对角元素设置为0, 而Barzel-Barabási 这篇文章也花费了很大的篇幅来解释他们的这个逼近是good的。然后呢，他们也就进一步说他们通过(2)这个近似和从转录调控网络中收集的表达实验数据，可以推断转录调控网络之间的因果联系。与此同时，Barzel-Biham 他们声称，使用公式(2)是必要的，因为通过G矩阵来计算S矩阵计算需要纠结一个$n^{2}$个方程组成的系统：

$G=SG$( 非对角线) 同时，$S_{ii} = 0$ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; (3)

Barzel-Barabási 说，这是十分困难的。当然，他说的求解$n^{2}$个方程实际上是就是求解方程(3)而已阿。他们断言这个工作难以实现是基于他们说，其包含的方程数是coupled的。他们的推理过程是这样的，针对不同的局部矩阵S，都要进行(3)中的矩阵求逆的运算。如此，实际上一共$N^{2}$个S对应的方程需要计算，同时，由于矩阵求逆的复杂度可以被写作$O(n^{3})$, 那么实际上，最后的操作就一共需要$O(m^{6})$，因为有$n^{2}$个方程(3)。

但是实际上呢？当我们看一个系统的时候，我们的第一个想法应该是：尽管他很大，但是，他也是有结构的. 我们坐下来重新审视这个问题，并且以一个简单的例子来重新写下这个问题的描述的话：

一个矩阵S实际上可以被分解成$n$个方程的$n$个系统，因为对于 $$S_{ii}=0$$和 $$G_{ij} = \sum_{k}S_{ik}G_{kj}$$。其实每一个方程里面只有n个未知的参数:
$$S_{i1}, ..., S_{in}$$。 如此，实际上算法的复杂度也就只是$O(n^4)$(算n次逆就可以了阿)，而且每一个考虑的$$S_{i?}$$对应的i还各不相同，并行化之后复杂度只有$$O(n^{3})$$。换言之，网络皇帝是在危言耸听。。。哪里有那么高的复杂读阿？现在并行计算这么发达，$$O(n^{3})$$不是什么大问题嘛。。。

但是，当我看这篇文章的时候，我不得不先去洗手间一下`:)`,然后，当我回来的时候，我意识到，很有可能，网络皇帝实际上可以使用<a href="http://en.wikipedia.org/wiki/Sherman%E2%80%93Morrison_formula"> Sherman-Morrison 公式</a>来获得关于下面这个公式的准确解：

$S=I-D(1/G^{-1})G^{-1}$&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; (4)

这里，‘／’ 用来表示对矩阵中每一个元素的除法操作，这也说明了通过G推断S实际上不需要任何除了对G取逆之外的操作。这个公式(4)比公式(2)要更加简单和有效.[2/23补：Jordan Ellenberg 指出，显然。。因为$$G=SG$$(去对角线相等)实际上就是等价于$$SG=G-D$$, 这里的D是指关于G的对角线矩阵,  所以，$$S = I - DG^{-1}$$, 同时，又因为S的对角元素必须是0，所以我们可以知道D＝D(1/D^{-1}), 后一个D是Barzel-Barabási 之前在(2)中定义的只保留对角元的算符，这样就推出了方程(4)了。。=_=,很好，换言之，实际上我们连<a href="http://en.wikipedia.org/wiki/Sherman%E2%80%93Morrison_formula"> Sherman-Morrison 公式</a>也不是必要的了。]。如此一来，也就是说，这篇在nature biotechnology上发表的文章中的最主要的结果，可以被我们很快的使用一些小技巧来完美的取代掉了, 事实上, <em>这个完整的练习倒是很适合拿来做本科生的线性代数家庭作业~~</em>. 这就是我们的网络皇帝Barabási, <a href="http://edge.org/conversation/thinking-in-network-terms">虽然他很喜欢拿自己和伟大的物理学家和诺奖得主苏布拉马尼扬·钱德拉塞卡比较</a>,不过我很难想像伟大的钱德拉塞卡也会面临上面这样的"困难"。。。

在我们找到的公式(4)中其实还暗示这比解本身更重要的结论，(4) 和 (1)联合起来提供了一个对偶公式来通过S计算G，例如，从模型开始做模拟，我们用这个想法可以得到：

$G = (S-I)^{-1} D(1/(S-I)^{-1} )$&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; (5)

有了公式(5)的话，我们很容易从局部扰动矩阵S来推断全局扰动矩阵G，换句话说，我们<em>不需要像Barzel & Barabási那样借助米氏动力学方程的模拟来对他们研究方法的工作性能进行评估。。同时用(4)我们可以有效的模拟数据。</em>























