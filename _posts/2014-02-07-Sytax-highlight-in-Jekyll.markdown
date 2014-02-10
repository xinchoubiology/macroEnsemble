---
layout: news_item
title: 'About the sytax highlight in my Blog'
date: 2014-02-07 17:43:32 -0800
permalink: /news/Sytax-Highlight
author: xinchoubiology
version: About the sytax highlight in my Blog
next_section: LaTex-plugin
prev_section: ShwinGen
categories: [Webdev & Tools]
---

我们基于Mac OSX的brew库，获得对Jekyll的语法高亮支持：

{% highlight bash %}
~ $ gem install pygments
{% endhighlight %}

<!-- more -->

这样我们就可以针对不同的语言进行相应的语法高亮了。
In addition，the language highlighter `pygments` support `Bash`,`Python`,`Html`,`Ruby` and so on.

> 例如，这是一个关于Rcpp的求卷积(convolution)的Rcpp操作：
	$y[n]=f[n]*g[n]=\sum^{M-1}_{m=0}{f[n-m]g[m]}$
	得到的R code 如下: ...

{% highlight cpp%}
src <- '
  Rcpp::NumericVector xa(a);
  Rcpp::NumericVector xb(b);
  int n_xa = xa.size(), n_xb = xb.size();
  
  Rcpp::NumericVector xab(n_xa + n_xb - 1);
  for(int i = 0; i < n_xa; i++)
    for(int j = 0; j < n_xb; j++)
      xab[i+j] += xa[i] * xb[j];
  
  return(xab); 
'
suppressMessages(require(inline))
f <- cxxfunction(signature(a="numeric", b="numeric"), src, plugin="Rcpp", verbose=TRUE)

{% endhighlight %}


如此我们可以在此博客中加入相关的语法高亮支持
