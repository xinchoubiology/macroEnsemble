---
layout: docs
title: SNF &bull; Algorithm
prev_section: snf_intro
permalink: /docs/snf_algo/
---

Similarity Network Fusion Algorithm & Methods:

上一篇文章SNF‘s introduction 主要介绍了基于类似矩阵网络融合方法。这一篇文章则是主要结合R code分析如何实现最后的SNF algorithm package。

###Network Fusion Procedure：

&nbsp;&nbsp; 从TCGA上获得5种癌症类型的相关数据，他们分别是: GBM(神经母细胞瘤), BIC(乳腺浸润性癌), LSCC(喉鳞癌), KRCCC(肾透明细胞癌) and COAD(结肠癌)。对每一种癌症来说，我们都获得了其3种类型的数据: gene expression; miRNA expression; DNA methylation information. 

####数据预处理：

>  1, outlier remove。<br>
    2, >20% 数据缺失需要删除。当然，对于missing data我们也使用 SMOTE来对数据进行恢复。<br>
    3, 对于构建network之前的每一种feature，我们都进行normalization:  $\tilde{f} = \frac{f - E(f)}{\sqrt{Var(f)}}$。

* 判定Metrics:
	* sihouette : $s(i) = \frac{b(i) - a(i)}{max(a(i), b(i))}$; a(i)表示的是cluster内最大不相似measurement；b(i)表示cluster外部的最大不相似的 measurement。 由于dissimilarity $\in [0， 1]$ , 所以，我们认为实际上我们得到的分类方法约好，$s(i) \sim 1$

####Similarity Matrix Fusion:

&nbsp;&nbsp; Supposed 我们有 n 个sample，对应m 种测量值。 作为sample network来说，可以被表示做图G=(V,E). 其中V表示病人$$( x_{1}, x_{2}, ..., x_{n} )$$, 而E本身是带weight的，作为边表示的是连接的两个node的相似性。所以边权我们可以表示做，W(i, j)。W 是一个N x N 的 Matrix。

####如何计算W(i, j)?

<div color="orange">
$$W(i, j) = exp(-\frac{\rho^{2}(x_{i}, x_{j})}{\mu\epsilon_{i,j}})$$
</div>
&nbsp;&nbsp; 这里，$$\rho(x_{i}, x_{j})$$表示的是sample i, j 之间的基于某一个measurement的欧式距离: $$(\sum_{k=1}^{N}(f_{i,k} - f_{j, k})^{2})^{\frac{1}{2}}$$。

在上面的公式中，我们定义$$\mu$$是超参数(基于经验设定)， $$\epsilon_{i,j}$$是一个伸缩因子:

$$\epsilon_{i,j} = \frac{mean(\rho(x_{i}, N_{i})) + mean(\rho(x_{j}, N_{j})) + \rho(x_{i}, x_{j})}{3}$$

这里，$$mean(\rho(x_{i}, N_{i}))$$ 表示的是第i个sample和其kNN &rarr; $$N_{i}$$ 之间的平均距离。K作为kNN的参数用户自定(10 ~ 30)。

####标准化W(i, j)矩阵:

&nbsp;&nbsp; 设D是对角阵, $$D(i, i) = \sum_{j}{W(i, j)}$$, 那么标准化矩阵 $$P＝D^{-1}W$$。如此，我们保证了rowsum &rarr; $$\sum_{j}{P(i, j)} = 1$$。

同时，为了消除自相似带来的<a href="http://en.wikipedia.org/wiki/Numerical_stability">数值不稳定性</a>, 我们对P进行稍微的修改。防止了如果self-similariity $$\gg$$ other similarity的情况。

$$
P(i, j) = \left\{
	\begin{array}{c}
	\frac{W(i, j)}{2\sum_{k \neq i}W(i, k)}, j \neq i \\
	\frac{1}{2}, j = i
	\end{array}
       \right.
$$

然后，让$$N_{i}$$表示$$x_{i}$$的K个最近邻，K作为我们pre-defined hyperparameter。如此，我们可以得到local affinity matrix:

$$
S(i, j) = \left\{
	\begin{array}{c}
	\frac{W(i, j)}{\sum_{k \in N_{i}} W(i,k)}, j\in N_{i} \\
	0, j \in others
	\end{array}
        \right.
$$

####SNF calculation:

对比$P$矩阵和$S$矩阵我们可以发现，P 保留了样本之间相似性的全部信息，而S 则只是保留了样本样本之间相似性的局部信息。这里，我们把P当作SNF的初始矩阵，而把S当作SNF操作的Kernel矩阵。然后迭代更新由不同data type产生的status matrix P:

对于每一个data type对应 $P^{(v)}$，他的更新都依赖于其他所有data type matrix。对于 data type number > 2的数据来说。设#type_number = m


<a color="yellow">Loop i in m data_type</a> :

> 
calculated each $$P$$ &rarr; 
$$ P^{(v)} = S^{(v)} \times \frac{\sum_{k \neq v}P^{k}}{m - 1} \times (S^{(v)})^{t}$$ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (MP algorithm)<br>

$$\frac{\sum_{k \in N_{d}}P^{(v)}}{N_{d}}$$ &rarr; $$P$$; 由此计算出最后的<a color="pink">converage similarity matrix</a>。

####基于SNF matrix 的聚类算法:

这里我就直接介绍<a href="http://en.wikipedia.org/wiki/Spectral_clustering">Spectral Clustering</a>算法就可以了。

* 为什么spectral clustering合适？
	* 好处之一在于，谱聚类只需要相似性矩阵就可以。这样的要求对于我们使用SNF计算出来的P(similarity matrix)天然耦合了。Output of SNF is input of Spectral Clustering.
	* Spectral Clustering比普通的K-means更加robust一些。
	* 速度更快。 实现细节可以参考<a href="http://www.kyb.mpg.de/fileadmin/user_upload/files/publications/attachments/Luxburg06_TR_%5b0%5d.pdf">A Tutorial on Spectral Clustering</a>

之前SNF通过迭代已经收敛得到similarity matrix fused all type network。W=(V, E); V: 不同的patients, E: 他们之间的收敛的relationship。

我们可以把W 看做是samples之间的邻接矩阵(<em> adjacency matrix </em>)。那么，对于$$v_{i} \in V$$, 我们定义其度(Degree) = $$d_{i}$$

$$
d_{i} = \sum_{j=1}^{n}W(i, j);
$$

基于$$d_{i}$$, 可以说，*degree matrix* &rarr; D 就是一个对角阵，$$diag(d_{1}, d_{2}, ..., d_{n})$$。


定义Laplacian Matrix: <br>
W 是indirected的; $$L = D - W$$, D matrix就是之前定义的度matrics。<br>

>如此, Laplacian matrix 有如下性质:

1. 对于每一个 $$f \in R$$: 
	$$f^{'}Lf = \sum_{i \in V, k \in V}(f_{k} - f_{i})^{2}W_{ik}$$
2. 所以, 由1可知，L是一个半正定的矩阵，同时，其eigen-value是非负的。

&nbsp;&nbsp; 如果(1) 中的f是L的特征向量，我们可以&rarr; $$f^{'}Lf = f^{'} \lambda f = \lambda(f^{'}f)$$. 所以，如果$f$ 对应的是eigenvector = 0 的特征向量的话，就意味着 $$f^{'}Lf = 0$$, 这么说的话，我们只有一种可能，要么 $$W(i, j) = 0$$, 要么 $$(f_{k} - f_{i}) = 0$$。所以，我们可以说，在一个similarity network里面，对于每一个connected component来说(也就是component内任意$$i, j$$的$$ W(i, j) \neq 0$$)。他们对应的 0-特征值的特征向量中的$$f$$是一个constant。

所以，我们得到,实际上的cluster就是实现:

$$
min_{Q\in R^{n \times C}} Trace(Q^{T}L^{+}Q). \\
s.t. Q^{T}Q = I
$$

对于一个要划分标签的N label system来说，我们定义 N个 labels。每一列表示的是: $$y_{i} \in {0, 1}$$. 如果sample $$k \in $$ label i，那么我们就说$$y_{i}(k) = 1$$, 反之就是0, 如此，$$Y = (y_{1}^{T}, y_{2}^{T}, ..., y_{N}^{T})$$。

$$
Q = Y(Y^{T}Y)^{-1/2}；
$$

Q 表示的是实际上的划分矩阵了就是，我们只需要最小化上面的Laplacian Matrix就可以了。。P.S. laplacian matrix不一定是固定的形式，他只需要形式上满足前面所说的几点定义就可以。所以实际上我们在使用的时候可以定义$$L$$, 也可以定义 $$L_{sym}$$或者$$L_{+}$$。
















