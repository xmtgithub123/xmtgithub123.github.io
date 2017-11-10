---
layout: post
title: "让元素垂直居中的几种方法!"
author: "MengTing Xu"
categories: css
tags: [documentation,sample]
image:
  feature: mountains.jpg
  teaser: mountains-teaser.jpg
  credit: Death to Stock Photo
  creditlink: ""
---

不管在大大小小的项目中，多多少少会遇到要让元素各种的居中对齐，垂直对齐，水平对齐，垂直水平对齐啦，所以就在各种搜查中总结了以下几种方法，以便日后更快捷的完成对齐任务！让我们开始吧......

# HTML

定义一组代码，父类元素parent,子类元素child

{% highlight html %}
<div class=”parent”>
  <div class=”child”>Test</div>
<div>
{% endhighlight %}

图片居中
{% highlight html %}
<div class=”parent”>
  <img src="#" alt="">
<div>
{% endhighlight %}

{% highlight html %}
<div class=”parent”>
  <div class="floater"></div>
  <div class="child">
        测试代码
  </div>
<div>
{% endhighlight %}

# CSS

### Method 1:

最常用的方法：定义父类元素的高度后，将子类元素line-height设置成200px,代码如下：

{% highlight css %}
.parent{
  height:200px;
}
.parent .child{
  line-height:200px;
}
{% endhighlight %}

  若要想将图片垂直居中,方法是将父类元素设置为200px,line-height设置为200px;再将img设置为vertical-align:middle;即可。代码如下：

{% highlight css %}
.parent{
  height:200px;
  line-height:200px;
}
.parent img{
  vertical-align:middle;
}
{% endhighlight %}

### Method 2:

最常用的方法:将父类元素设置为table属性,再将子类元素设置为table-cell，vertical-algin:middle;即可，代码如下：

{% highlight css %}
.parent{
  display:table;
  height:200px;
}
.parent .child{
  display:table-cell;
  vertical-align:middle;
}
{% endhighlight %}

### Method 3:

原始用法：将父类元素设置为相对定位，子类元素设置为绝对定位后，分别设置top与left,代码如下：

{% highlight css %}
.parent{
  position:relative;
  height:200px;
}
.parent .child{
  position:absolute;
  top:50%;
  left:50%;
}
{% endhighlight %}

### Method 4:

{% highlight css %}
.parent{
  position:relative;
  height:200px;
}
.parent .child{
  position:absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 50%;
  height: 50%;
  margin: auto;
}
{% endhighlight %}

### Method 5:

  将父类设置内边距padding:x 0，则子类设置内边距为 padding:2x 0;代码如下：
{% highlight css %}
.parent{
  padding:2% 0;
}
.parent .child{
  padding:4% 0;
}
{% endhighlight %}

### Method 6:

  定义父类高度，将floater设置height为50%；margin-bottom设置为-y/2 px,child设置height属性为y px即可，代码如下：

{% highlight css %}
.parent{
  height: 350px;
}
.parent .floater{
  height: 50%;
  width: 100%;
  margin-bottom: -50px;
}
.parent .child{
  clear:both;
  height: 100px;
}
{% endhighlight %}