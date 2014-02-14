---
layout: docs
title: SNF &bull; Introduction
prev_section: masterdissertation
next_section: snf_algo
permalink: /docs/snf_intro/
---

>**引子** . . .

今天看到 Williams 在 Twitter 抛了一篇文章： 

> <a href="http://dl.acm.org/citation.cfm?id=1743546.1743569">Conference paper selectivity and impact</a>

文中大约的总结到，CS口的人是不怎么看重N/S 的`:-P`,不过就我自己的喜好来说，因为CS口的conference还是太多了, 不过就我自己来说，我是不同意此文的观点的，自己也还是更倾向于看nbt和nmeth的技术类paper，这些文章更贴近生物学问题本身。

先继续留下<a href="http://xinchoubiology.github.io/macroEnsemble/docs/masterdissertation/">上篇文章</a>中留下的inderect link
erase 这个坑不提`:-)`,我今天主要说说最新一期的 Nature method 上的<a href="http://www.nature.com/nmeth/journal/vaop/ncurrent/pdf/nmeth.2810.pdf">Similarity network fusion for aggregating data types on a genomic scale</a>. 作者提出了一种有效的不同Type数据整合方式，让我们可以得到comprehensively landscape of different disease and biology process. 窃以为这才是所谓CS，所谓Big Data的真正意义。不管怎么说，从microarray开始，甚至从前人们做蛋白结构开始，生物就是一门‘Big Data’的学科嘛`:)`

下面进入主题:

1, 背景:<br>
&nbsp;&nbsp;以TCGA(The Cancer Genome Atlas)为例，现在的Biology Dataset 已经收集大量不同病人的 Genomics(基因组), Transcriptome(转路组), Epigenome(表观遗传组)的数据了。把这些数据整合起来，显然有利于我们对生物问题本身Heterogeneity 的区分。例如更好的区别不同的Cancer...But,目前这些数据整合分析方法存在的几个Key Barriers:

> 其实这本质上还是一个分类问题，我们不断的检测不同 type的 data,目的就是为了能够获得对不同sample的更好的category。

* 所谓的<a href="http://en.wikipedia.org/wiki/Curse_of_dimensionality">curse of dimensionality</a> : 只有少量的Patients样本的情况下，实际上的可监测量对应的measurement vector是高维的.
* 量纲问题［For instance: PCA 中的量纲问题］.
* 数据的噪音问题(Collection bias and nosie in dataset).

对于这个问题来说，最简单的想法就是把所有的measurement concatenate 起来。例如，对于我之前考虑过的一个Project: <em>Protein-Protein interaction</em>,基于Primary Sequence的统计学习方法就是把Differen Chain's Sequence concatenate 在一起，如此一来的话，我们得到一个简单的interction feature vector.

> 但是直接把检测值连接起来的 Negative effect是: 会降低信噪比(signal-to-noise ratio).虽然现在有一些preselect的方法来提高信号强度，但是这都要基于初始状态(补...).

SNF区别与之前这些Data Integration 方法最主要的地方在于，并不直接从measurement出发，而是基于不同measurement 下样本之间的关联矩阵(similarity matrix)出发。如此以来，至少就解决了之前纠结的量纲问题和度量高维的问题(直接对比):

<div align="center">
	<img src="{{site.baseurl}}/img/SNF_scheme.png" type="image/svg+xml" alt="Fig.1 SNF 流程图">
	<br>
	<p>Fig.1 SNF 流程图</p>
</div>

基于不同measurement的相似矩阵fused得到的融合网络就同时包含了来自不同dataset的数据信息(更全面)。因为我们最终的目的都是基于检测量对于samples进行分类即可，所以究竟不同的measurement的classification threshold是否明确似乎就不那么重要了，我们如果能通过一种类似 Network Structure Optimization的方式得到来源于数据的分类结果也等价于达到结果。

所以，SNF正是如此，其实话说回来这个和09年Science上的<a href="http://www.psi.toronto.edu/affinitypropagation/FreyDueckScience07.pdf">Clustering by Passing Messages Between Data Points</a>那篇文章有点类似。都是使用similarity matrix作为算法的入口的一个聚类方法，但是SNF的优点在于，它可以很好的整合不同类型的数据。同时，即使sample量小也可以进行很好的分类。

2, SNF method review:<br>
&nbsp;&nbsp;我们考虑两个or以上的(Fig.1)样本数据采样整合。SNF基于不同的数据类型创建相似矩阵(similarity matrix),然后再把它们融合成一个network:

* 对于只考虑mRNA expression(Fig.1a) 或者DNA methylation(Fig.1a),对不同的样本(sample)进行sample-sample相似性的度量[这里其实有文章可做啊？欧氏空间or非欧空间...] &rarr; 生产sample-by-sample similarities matrix(Fig.1b)。
* Similarity Matrix Based Method 可以对 patients 进行clustering。这些都基于从 Fig.1b 中得到的相似矩阵而来。Fig.1c 中的network是weighted network, 其 edge weight来自于不同的patient之间的similarity score。
* 网络fusion基于message-passing theory的非线性方法(见code),基于这个方法，来自不同dataset的相似网络不断的迭代，目标是让自身与other更加接近。如此SNF网络收敛到一个唯一的network(Fig.1d).

---

从最终的结果上看，SNF 整合方法实际上类似于: 

* 消除了弱相似性; 

* 保留了弱相似但是全局网络的Key Supported Node。


3, 基于SNF的特征选择:<br> 
&nbsp;&nbsp;SNF的好处就是在完成subtype assignment之前不进行特征选择(feature preselect)的工作。但是我们还是可以基于SNF分类后的网络结构, 逆向选择出能够表征不同subtype的feature。不同feature对于samples 分类的分类能力我们可以使用NMI(标准化互信息)来表示。我们定义使用SNF得到的分类结果是$g$, 而使用第i个 feature得到的分类结果是$l_{i}$.

$$
\begin{align*}
cs_{i} = NMI(l_{i}, g) = \frac{I(l_{i}, g)}{\sqrt{H(l_{i})H(g)}}
\end{align*}
$$

这里, I(.)表示互信息, for instance, $I(A,B)=H(A) - H(A|B) = H(B) - H(B|A)$, $H(.)$表示各自cluster自身的熵值(Entropy). NMI $\in [0, 1]$,
这里的cs只是一种<a href="http://en.wikipedia.org/wiki/Mutual_information#Normalized_variants">normailized MI</a>的方法，实际上NMI有很多种不同的matric 形式。

> <a color="orange">$$cs_{i} = 1$$</a>, 我们可以认为使用 <a color="orange">$$f_{i}$$</a>特征对病人分类的效果等价于使用SNF分类，也就是说，实际上我们做类似clinical diagnosis只需要检测这一维数据就可以。反之，<a color="orange">$$cs_{i} = 0$$</a>, 就说明特征 <a color="orange">$$f_{i}$$</a> 与实际上的fusion network之间没有关联。总结来说，cs越高，就说明实际上这一维特征在网络结构中越重要。我们也就可以基于SNF来对实际检测的不同 measurement进行基于重要性的rank。也是一种特征选择的方法.













