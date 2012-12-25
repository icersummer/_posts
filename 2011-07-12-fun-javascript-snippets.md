# 分享5个有趣的 JavaScript 代码片段
category: Web前端
tags: JavaScript

---

很多人认为编程语言只是用于工作，没有什么乐趣，其实，只要我们发挥奇思妙想，再死板的东西也有有趣的一面。这篇文章告诉大家：使用JavaScript，可以做很多很多有趣的事情。以下代码拷贝到地址栏回车即可运行，赶紧试试吧。

## 1. 网页射击游戏
这个游戏可以在任何网页里面玩，把下面代码粘贴到地址栏回车，按空格键进行射击，W键可前进，A、D键或者方向键可改变射击方向。

{% highlight javascript %}
    javascript:var%20s%20=%20document.createElement('script');s.type='text/javascript';document.body.appendChild(s);s.src='http://erkie.github.com/asteroids.min.js';void(0);
{% endhighlight %}

## 2. 让图片飞起来

只要把下面的代码贴到浏览器的地址栏里然后按Enter键，当前网页的所有图片都将动起来。

{% highlight javascript %}
javascript:R=0; x1=.1; y1=.05; x2=.25; y2=.24; x3=1.6; y3=.24; x4=300; y4=200; x5=300; y5=200; var DI= document.getElementsByTagName("img"); DIL=DI.length; function A(){for(i=0; i<DIL; i++){DIS=DI[ i ].style; DIS.position='absolute'; DIS.left=Math.sin(R*x1+i*x2+x3)*x4+x5+"px"; DIS.top=Math.cos(R*y1+i*y2+y3)*y4+y5+"px"}R++}tag=setInterval('A()',5 );document.onmousedown=function(){clearInterval(tag);for(i=0; i<DIL; i++){DI[i].style.position="static";}}; void(0)
{% endhighlight %}

## 3. 让网页可编辑

此JavaScript代码，可以让你实时修改任何的网页，在Firefox中，你甚至可以把修改的网页保存到起来，对于网页设计者来说，这个功能可以辅助完善页面效果。

{% highlight javascript %}
javascript:document.body.contentEditable='true'; document.designMode='on'; void(0);
{% endhighlight %}

## 4. 让浏览器抖起来
改变浏览器窗口尺寸到普通模式，可能半屏的效果是最好的。把下面的代码贴到地址栏，按Enter键（貌似只有IE有效果）。

{% highlight javascript %}
javascript:function Shw(n) {if (self.moveBy) {for (i = 35; i > 0; i--) {for (j = n; j > 0; j--) {self.moveBy(1,i);self.moveBy(i,0);self.moveBy(0,-i);self.moveBy(-i,0); }}}} Shw(6);
{% endhighlight %}

## 5. 地址栏计算器

哈哈，这个以前还真不会想到，地址栏就是个计算器嘛。

{% highlight javascript %}
javascript: alert(4+5+6+7+(3*10));
{% endhighlight %}

本文转载自 [梦想天空](http://www.cnblogs.com/lhb25/)

原文地址: [http://www.cnblogs.com/lhb25/archive/2011/07/10/fun-javascript-snippets.html](http://www.cnblogs.com/lhb25/archive/2011/07/10/fun-javascript-snippets.html)