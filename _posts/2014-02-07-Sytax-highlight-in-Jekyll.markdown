---
layout: news_item
title: 'About the sytax highlight in my Blog'
date: 2014-02-07 17:43:32 -0800
permalink: /news/Sytax-Highlight
author: xinchoubiology
version: About the sytax highlight in my Blog
prev_section: LaTex-plugin
categories: [release]
---

我们基于Mac OSX的brew库，获得对Jekyll的语法高亮支持：

{% highlight bash %}
~ $ gem install pygments
{% endhighlight %}

<!-- more -->

这样我们就可以针对不同的语言进行相应的语法高亮了。
In addition，the language highlighter `pygments` support `Bash`,`Python`,`Html`,`Ruby` and so on.

{% highlight python %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}


如此我们可以在此博客中加入相关的语法高亮支持
