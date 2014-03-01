---
layout: docs
title: Pachter's comment on 2 nbt paper &bull; 翻译
prev_section: snf_code
permalink: /docs/discuz_grn/
---

> 此翻译尚未授权，Just pre-print as a draft, 我正在和Pachter教授联系 ... 

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

而使用米氏动力学模拟的一个问题是，虽然这么干对于酶动力学网络来说更有意义，但反过来说，对于调控网络这就显得没那么有意义了(了解更多请看Karlebach, G. & Shamir, R. 文章 <a href="http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CCoQFjAA&url=http%3A%2F%2Fwww.researchgate.net%2Fpublication%2F23263890_Modelling_and_analysis_of_gene_regulatory_networks%2Ffile%2F79e4150e41bf450e40.pdf&ei=XAz4UoPXOcnjoATk3oGoDw&usg=AFQjCNEtQSVc8z-HWilrI-C8_bVOGym2jg&sig2=20HdWqvwFrKc-SVdxTy8cg&bvm=bv.60983673,d.cGU">Modelling and analysis of gene regulatory networks</a>,Nat Rev Mol Cell Biol 9, 770–780 (2008)), 但是，无论在哪一个例子里面，这种动力学模拟都很难称得上是对(2)的有效验证。因为，本质上，他们就是之间可以被认为是无关的。那么，当一个人通过 Barzel-Biham模拟数据然后试图恢复参数的时候，究竟发生了什么？

<div align="center">
	<img src="{{site.baseurl}}/img/bb_res.jpg" width="500" height="450" ALT="方法对比分析AUROC">
	<p>对比图: 正则化偏相关标准方法 v.s. Barzel-Biham 模型准确推断。 随机稀疏图是通过 Erdös-Renyi 图模型G(5000, p)生成的，这里p作为不同的图密度，也是图中的X坐标，而y坐标则是描述了不同方法的<a href="http://en.wikipedia.org/wiki/Receiver_operating_characteristic">AUROC</a>, AUROC基于75轮的随机试验获得</p>
</div>

从上图我们可以看出，当我们检测Barzel-Biham模型对5000节点E.R 图的模拟结果时，我们惊讶地发现，Barzel-Biham模型的方法时对噪音高度敏感的: 当我们网E.R图中引入少量噪音的时候，“精确算法”(4)已经无法从全局响应矩阵G中恢复出局部响应矩阵S (我们同样方法也分析了方程(3), 发现他的表现比(4)还差)。而这个方法对于噪音之所以敏感，是因为其中的$$D(1/G^{-1})$$项的结果在当$$G^{-1}$$的对角阵趋于0时候是<b>有问题的</b>。

当我们研究$$G^{-1}$$的行为的时候，我们可以从观测$$S$$的等比数列是否是收敛的这个信息来研究之。我们注意到，$$G^{-1}$$的对角阵实际上满足
$$Diag(G^{-1}) = I + S + S^{2} + S^{3} + ... $$, 如果S有混合标识或者在网络里有重要的反馈的话，$$G^{-1}$$的对角阵就可能会接近0，同时在G检测
中的任何噪音都有可能会导致推断S中巨大的波动。这也就意味着，实际上Figure.1中的结果并不是依赖于图模型的选择,无论是ER model, 还是任何一种包含enhancer 和 repressor的转录调控网络，Barzel-Biham模型对于噪音的抵抗能力都比较弱。但是，在Barzel & Barabási文章中的Fig2a我们看到, Barabási在模拟S的时候，只排除了在网络中的负效果作用，这显然与转录调控网络的实际情况是不一致的。也就是说，他们的假设，在生物学意义上是不真实的。

然而，Barzel-Biham模型中的这个关于噪音的难题还会有更多的影响，首先，如果在实验中假设系统的偏差模型是一个为常数的信噪比，那么一个很重要的问题就是<em>没有一个实验可以直接检测G矩阵中的元素。</em> 获得$$G_{ij}$$的方法就是对基因$i$做一个大小为$e$扰动，然后观测基因$j$的变化，再对这个变化除以$e$, 最后这一步把$G_{ij}$检测中的噪音增大了$\frac{1}{e}$倍(这个对检测中固有噪音的放大程度还是有点大的)，对基因扰动程度的增长，会导致对G矩阵估计的错误也增加。所以，这也就是为什么Barzel-Biham模型要得到较低的G的错误估计需要原始数据的低噪音.而对于生物调控网络来说，这就意味着我们需要多次实验重复。然而，在Barzel & Barabási的文章中，甚至没有实验一次重复扰动实验。

更加重要的是，在发现 Barzel-Biham model 表现很差之外，我们还发现，一个被广泛运用的基于偏相关的岭估计方法(Schäfer, J. & Strimmer, K. <a href="http://uni-leipzig.de/~strimmer/lab/publications/journals/shrinkcov2005.pdf">A Shrinkage Approach to Large-Scale Covariance Matrix Estimation and Implications for Functional Genomics</a>, Statistical Applications in Genetics and Molecular Biology 4, (2005)) 在DIrect linkage predict方面的表现远远的好过方程(4)中的模型，这也暗示这里的方程(4)其实没啥用。。这个方法甚至不适合用来估计这个方法自己生成的数据。

我们继续都到文章的"结果"部分，为了说明他们这个“沉默者”方法的有效，网络皇帝仅仅跑了跑<a href="http://www.the-dream-project.org/category/challengesdream/dream5">DREAM DATA</a>三个数据集中的一个例子就结束了。。而且，他也只是把自己的silencer方法和DREAM 5的Benchmark中包含的35中方法中的3种方法进行了对比。还有，Barzel & Barabási文章做的是基于网络扰动实验的分析，但是作者们做方法比较的时候却是从互信息，相关系数等方面来比较这些方法的Performance。。结果如下图所示: ［P.S 这里我并不十分同意帕教授的说法，不同的域来描述network的恢复情况我觉得是可行的，但是如何做到扰动则是一个问题。］

<div align="center">
	<img src="{{site.baseurl}}/img/barzel-barabasi_figure.jpg" width="500" height="200" ALT="结果图">
	<p>Barzel & Barabási 结果图</p>
</div>

但是Barzel & Barabási 在文中进行比较的三个方法，他们在DREAM benchmark中各自在与实际结果相关系数，互信息的得分排名排名都不是很高，各自Pearson 和 Spearman 相关的排名在35种方法中只是占据 16 和 18名，也就是说，作者提出highlight的方法没有和最好的方法进行比较。这可能才是为什么本文的方法在与"这些方法"比较之后表现最好的原因: 实际上，如果全局扰动矩阵G是相关矩阵的话，本文给出的路径解释结合上Seawall Wright的路径参数的推导过程(ca 1920), 实际上就是偏相关。

另外，在互信息的检测种，作为一个在35种方法种排第19的方法，我们也认为没有什么统计显著性。如此一来，我们可以这么说，网络皇帝把一个在DREAM BENCHMARK中效果排名16/35的方法称为"顶级的"方法， 至少，这是不合适的。。

那么，我们不得不问：为啥Barabási就可以在NBT上发表一篇对仅仅是无聊的DREAM数据的网络预测都几乎没有任何提升的Paper呢？

把他们的文章放到我们的这个"帕三篇"中看的话，后面被我们骂的狗血喷头的Feizi et al那篇文章至少相比网络皇帝这篇文章还有一个可取之处：他们把自己的方法在3个数据集，与9种方法都进行了比较(设置还使用了基于社区的方法)。所以，我们认为，如果想要证明 Silencer这个方法在网络推断上确实有提高的话，至少还需要更多的验证实验。只有这样的评价标准才是。

所以, 到现在为止，我们也没有看到任何合适的关于Barzel-Biham model 的实际应用（可能除了这个子网络扰动实验之外)。与此同时，我们也不相信这个模型适用于那些需要容忍噪音问题的实验。

还有更不幸的事，伟大的作家们: Barzel, Baruch, and Albert-László Barabási 之后， 又在 Nature Physics 上在这个问题上继续钻牛角尖下去了(P.S. 译者认为，这是好事儿阿上`=_=`): "<a href="http://www.nature.com/nphys/journal/v9/n10/abs/nphys2741.html">Universality in Network Dynamics</a>"。在这个Paper里面，他们继续讨论了局部响应矩阵$$S$$, 同时，矩阵S种所有的元素都是正数。也可以理解为: 例如在转录调控网络里面，负调控就没有了。(P.S. 这里译者也有疑问，如帕教授所言的话，这里要求对应的响应矩阵必须是相关矩阵了，如果是其他类型的响应矩阵的话，是否可以保证矩阵中的所有元素为正？) 

而在Barabási的这篇文章中，实际上，这种约束直接影响了方法对应的结果。可是这样的结果，从生物学显著性上来说，毫无意义。更何况，如果是使用方程(5)来推算G的话，这个被加在S上的约束也是毫无必要的。考虑到这些直接原因，我们不得不怀疑在读“Universality in Network Dynamics”这篇文章还是否有意义，否则，这篇Blog就太长了。

当然，这也不是Barabási 第一次在 N/S 系列杂志上发表一篇包含大量无关和不一致的结果。事实上，Barabási干这方面倒是是很久,而且还总能投到顶级期刊。

Barabási 这几年被发现的问题：

*  其有名的 BA model, 但是其数学模型早就建立了。

*  对其 metabolic networks 是 scale-free 还是不是 scale－free的分析。

*  John Doyle 拒绝文章 Error and attack tolerance of complex networks。

*  “The origin of bursts and heavy tails in human dynamics” 这篇文章中，皇帝假装观测了人类行为爆发现象，但是基于的模型是人工数据，但是对于实际数据，是不满足power－law分布的。

*  (P.S. 我最爱的一篇文章，不过终究说来，体现的不过是我的愚蠢傀儡属性罢了），“Controllability of complex networks”, 作者说，稀疏的不均匀网络，难以被控制，繁殖，密集的均匀网络，易于控制。但是，实际上，不是这样的。。<a href="http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0038398">请看这篇漂亮而强有力的反驳(Nodal Dynamics, Not Degree Distributions, Determine the Structural Controllability of Complex Networks)</a>,  Carl Bergstrom 和他的同事们展示了在PDS(Power Dominating Set)问题上，单一控制输入就是大多数这类图的的所需的结构控制。而我之前，在"Finite decimal expansion, infinite time constants and structural controllability of networks"这篇博客里面，也详细的介绍了Carl Bergstrom 的工作，以及为什么他的工作让Barabási的文章显得是控制论的"耻辱"。

换句话说吧，我们都知道，Barabási的工作就是Nature and Science 期刊里面的一个专栏。。尽管我们也知道，许多的杰出科学家也已经证明了这位网络皇帝其实是在"裸奔"。。

最后一句：在这篇我们讨论的nbt paper中， Barzel and Barabási发表评论说：<a href="http://www.northeastern.edu/news/2013/07/barzel/">“这个工作让研究组更加的接近理解，预测和控制人类疾病”</a>, 现在看来，他们真应该是 <a href="http://www.michaeleisen.org/blog/?p=1516"> Michael Eisen’s pressies奖</a>的优秀候选人阿。。(毒舌毒死了。。。)







































