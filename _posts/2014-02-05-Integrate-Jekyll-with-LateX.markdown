---
layout: news_item
title: 'Integrate Jekyll with $\LaTeX$'
date: 2014-02-05 17:43:32 -0800
permalink: /news/LaTex-plugin
author: xinchoubiology
version: LaTex plugin
prev_section: Sytax-Highlight
categories: [Webdev & Tools]
---

如果可以的话，最佳的方案应该是自己写相关的公式渲染插件，例如我们知道<a href="https://github.com/be5invis/eido">Eido</a>这个工具。但是，由于原作者自己几年过去了，仍然没有补充合适的`README.md`或者`Example`.这让我不得不在 "让网站开始工作的有效性" 和 
"网站本身渲染的有效性" 两者之间权衡，我选择了使用前者。所以，这里仍然使用广泛运用的，但依旧没有很高的排版效率的<a href="http://www.mathjax.org">mathJax</a>来进行工作。

<!-- more -->

WTF`｛＝_＝｝`所以说给跪了。。不能总这么依赖别人阿。。。还是要争取尽早写出自己的排版系统阿。。。又一次不得不 `ORZ` 高纳德大神了。。。

###mathJax‘s configuration
我们使用了script定义的mathjax 的CDN, 在default.html的文件头加上一下mathjax的声明。
{% highlight html %}
<!-- mathjax config similar to math.stackexchange -->

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

{% endhighlight %}

然后，我们就可以使用mathjax来渲染公式了。注意到

{% highlight html %}
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>
{% endhighlight %}
用法就与我们之前写$\LaTeX$是一样的了。如下:

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

公式大概就是这个样子了。。。