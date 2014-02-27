---
layout: docs
title: SNF &bull; Code
prev_section: snf_algo
next_section: discuz_grn
permalink: /docs/snf_code/
---
## Code Archive . . .

可以用 R function package.skeleton 生成
{% highlight bash %}
SNFtool code/
 data/
 man/		---> 帮助文档
	***.Rd
 R/		---> R 原文件
 	***.R
 	interal.R       	#  Hidden in ls("package:SNFtool")
 	affinityMatrix.R 		#  affinityMatrix 函数
 	calNMI.R			#  calNMI 函数
 	chiDist.R 			#  chiDist 函数（计算Chi-Square Distance A&B）
 	concordanceNetworkNMI.R 	# 
 	displayClusters.R      	#  聚类结果的画图
 	dist2.R 			#  计算两个feature vector 之间的distance
 	groupPredict.R 		#  针对新引入的sample来预测他的实际group `:)`这个很重要
 	SNF.R                      	#  执行similarity network fusion 工作
 	spectralClustering.R 	 	#  谱聚类
 	standardNormalization	#  标准化相似矩阵
 demo/		---> 演示代码
 data/		---> save() 生成的 .Rda
 NAMESPACE	---> 那些R对象是用户可见的，隐藏函数
 DESCRIPTION	---> basic information about package. (描述文件，包括包名、版本号、标题、描述、依赖关系等)

{% endhighlight %}   

安装 R package ：`R CMD INSTALL pkg` <br>

我又重新写了SNFtools 的python 版本: <a href="https://github.com/xinchoubiology/SNFtools">SNFtools</a>
具体代码细节见我的github repository `:)`





