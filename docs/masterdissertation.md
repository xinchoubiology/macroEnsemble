---
layout: docs
title: ShwinGen Framework
prev_section: home
permalink: /docs/masterdissertation/
---

这个其实就是我的Master Degree Dissertation的工作。只是说，在我实际上的Final Paper中，原文的题目是作为: 

`From local to global: a new perspective of Hepatitis B Virus Genot- 
yping Frameowrk`

本质上，Dissertation的题目更能够表现我的实际工作的属性和重心，只是由于Blog排版的需要，我还是用与之相关的 Tools Developed BY me 作为 Document的
Title. 原文链接如下：<a href="{{site.baseurl}}/assets/MasterDissertation.pdf">Dissertation of ShwinGen Framework</a>.

ShwinGen [*Short window-based Genotyping webtool*],是我们在Research中使用局部采样的方法来完成基因组全局特征描述的一个新框架。这个工作初期的需求本
来只是想要做一个简单的HBV[Hepatitis B Virus] RT Region的测序分型工作基于HBV的RT Region。但是，在初步分析之后，我发现一个实际上的HBV基因组序列存在着序列结构上的保守性:
<div>
	<img src="{{site.baseurl}}/img/hbv_circos.svg" width="300" height="300" style="float:left;" type="image/svg+xml">
	基于NCBI的<a href="http://www.ncbi.nlm.nih.gov/nuccore">nuccore</a>数据库得到了我们做HBV特征分析的原始文本。我们基于获得的HBV全基因组序列和
	基于全长的<a href="http://newbioafrica.mrc.ac.za/rega-genotype/">REGA</a> Genotyping 方法. 我获得了对HBV不同subtype代表的全基因组上的各个<a href="http://en.wikipedia.org/wiki/Open_reading_frame">ORF(Open Reading Framework)</a>的具体定位信息: 从<a style="color:#FFCC00">黄</a>到<a style="color:#FF66FF">红</a>代表了HBV circular Genome上,从Polymerase Region 到 core Region 不同ORF的相互发生overlap的具体区域。
</div>

上面的HBV <a href="http://circos.ca">circos</a>在描述不同ORF定位的同时，还使用了KL-Entropy来计算了我们MSA之后的每一个HBV Genome Loci对应的保守性$D_{i}^{a}$；通过上图的circle 外侧 Histogram 来描述。

$$
\begin{equation}
D_i^{(a)}=f_i^{(a)}\ln\frac{f_i^{(a)}}{q^{(a)}}+(1-f_i^{(a)})\ln\frac{1-f_i^{(a)}}{1-q^{(a)}}      a\in (A,T,C,G,-)
\end{equation}
$$

$D_i^{(a)}$也被看作是序列对齐坐标上每一个位置的碱基频率相对于背景碱基频率分布$q^{(a)}$的偏差程度，如果位点趋向于保守，他的自身的熵值就会有下降，D就会上升。而只有在亚型之间不保守的序列位点，才具有作为分型的表征位点的可能性.

所以我们可以看出，实际上整个HBV Genome的大部分位点都是"Conservatives",考虑到实际上又存在着大量的ORF overlapped Region，这才开始了我的Genome Compressed Comprehension的方法。

除去掉对NCBI数据预处理的细节`:)`，我们可以直接来讨论，基于NCBI获得的HBV全长分型数据，再考虑这个MSA matrix本身蕴含的inter-loci corrlation.
以此，基于这种方法我们找到了合适的描述整个基因组位点间相关性的方法；我也叫他，位点间的耦合度量 $C_{ij}$。

<div align="center">
$$
\begin{equation}
C_{ij}^{ab} = f_{ij}^{ab} - f_{i}^{(a)}f_{j}^{(b)} 
\end{equation}
$$
</div>

这里 a,b 表示不同的 nucleotide，而 i, j 表示不同的 position. 又考虑到，不同的为点之间的相关性表现出来实际上会受到他们实际对应位点自身的保守性所约束，不同保守程度的位点表现出来相近的 similarity 则暗示着我们实际上他们之间有很大的不一样。

所以，我再在 $C^{ab}_{ij}$ 上乘以其中每一个位点关于这个碱基的 KL 散度的一阶导数,相当于说额外的 bit 数都是有一致的相关性,越保守的位点之间,存在的相关就越可靠可以认为(因为 KL 散度的一阶导数越大,该位置碱基发生变换的概率也就减小,同时发生的概率就更小了, 所以,位点相关的显著性上升).

<div>
$$
\begin{equation}
\hat{C_{ij}^{ab}} = \phi(D_i^{(a)})\phi(D_j^{(b)})C_{ij}^{ab}
\end{equation}
$$
其中，$\phi(D)=\frac{\partial{D}}{\partial{f}}$, 表示位点保守性(Conservation)的梯度。
</div>

最后，考虑到碱基我不需要碱基维信息，所以，把原本的(base, position, sequence)3维张量，压缩到$C_{ij}$表示不同的位点之间的Correlation measurement.


既然都到这里了，也就是说，实际上，我们这些Genome Sequence上的每个位点，终归是会有一些位点再约束着他的。我下面要做的事情就是对这些各个相关的位点做一个Filter，这个Filter的作用简单来说就是把最相关的各个Locus subset归纳出来，不同的subset实际上只是内部选上几个代表基本上就可以做出表征(represent)这个集合了，如此一来，我不是就可以用少量的信号来表示整个HBV Genome的全基因组信号了吗?`:)`

所以，在上面Correlation matrix among different locus的基础上，再通过清洗Transive Effect之后。实际上，我觉得我可以想办法来寻找实际的位点相关子集合了。首先我们先来分析实际上的HBV Genome Correlation Heatmap，如下`;)`
<div>
	<img src="{{site.baseurl}}/img/ND_Cleanup_Positiona.png" width="200" height="200" style="float:right">
</div>

在使用Transive-Effect Erase [Method Origin:<a href="http://www.nature.com/nbt/journal/v31/n8/full/nbt.2657.html">Network Cleanup</a>] 之后的位点相关矩阵仍然表明，大部分位点是显著Correlated的，如此以来，我们的焦点就被聚焦在了剩下的这10% 看似独立的Locus上了，只需要考虑这352个没有被完全包含到强相关联的大色块里面的位点，应当就可以完全的表征出不同的subtype之间的Difference了，也就可以完成分型工作。

既然如此，我们找的这些位点总结起来就是：一些 Locus Subset，内部自然是抢关联，越强越好，越强我们用的也就越少； 不同的Locus subset之间的关联则是越小约好，尽可能的independent。所以，我们不妨就把实际上的不同的,’独立的‘这些subset当作是Genome Evolution过程中，产生的独立信源，如此一来，问题就被可以模型化成标准的<a href="http://en.wikipedia.org/wiki/Independent_component_analysis">ICA问题</a>，不同的位点带来的正交的进化信息被记录在了Genome的不同Locus上，于此同时，还可以发现，实际上不同的信源，应该就是代表者不同的HBV subtype，这样一来，我们不仅可以通过少量位点确定出来实际的HBV subtype，同时，还可以完成对不同subtype发生测序过程中 recombination chimera error的检测。因为不同的Eigen-segment表征的subtype是独立的。

<div align="center">
	<img src="{{site.baseurl}}/img/ICA_Classification.png" width="290" height="200" style="float:center" ALT="ICA decompose matrix's icvalue to each icvector">
</div>

小区间可以表征不同的subtype之间的差异，同时这个差异足够显著来帮助我们确定HBV 病毒血清对应的亚型归属。也就证明了我们确实可以通过少量的位点信息来对病人的血清样本进行分型工作了。`:)`

This article aims to briefly introduce the main idea generated my `ShwinGen Framework`. I have hide a lot of details in our theorism anlysis and the actual implementation of CHB patients' serum virus subtype classification. In addition, you can read my master dissertation form these and here is the silde of my final defence. `:)` <a href="{{site.baseurl}}/assets/Final_Defence.pdf">[The slide of final defence...]</a>























