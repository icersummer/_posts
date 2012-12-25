# jQuery的mouseenter和mouseleave事件
- category: Web前端
- tags: jQuery

---

最近把主题chromeStyle首页的文章摘要显示从感应光标自动下拉改成了淡入淡出(fadeIn &amp; fadeOut)，因为涉及到ajax加载文章，所以不能简单地用hover(fn1,fn2)来绑定事件，必须得用$.live方法分别绑定光标移入和移除这两个事件。

开始我是这样写的:

{% highlight javascript %}
$('.postlist .post').live('mouseover',function(){
    $(this).find('.summary').fadeIn();
});
$('.postlist .post').live('mouseout',function(){
    $(this).find('.summary').fadeOut('fast');
});
{% endhighlight %}

结果发现鼠标放在首页的文章缩略图上时，文章摘要(就是那个有点透明的黑色遮罩)不停地闪啊闪，闪得我眼都花了，我了个去！果断Google一下寻求解决办法，找到一篇qiqiboy!的[《优化javascript中mouseover和mouseout事件》](http://www.qiqiboy.com/2011/01/19/javascript-mouseover-and-mouseout.html),纯JavaScript，表示压力很大没看懂。再次寻找，又找到一篇愚人码头的[《jQuery中的mouseenter和mouseleave事件》](http://www.css88.com/archives/2297)，里面提到了jQuery的mouseenter和mouseleave事件，我擦，手册里面没看到，晕晕。用这两个事件成功解决了问题，修改后的代码是：

{% highlight javascript %}
    $('.postlist .post').live('mouseenter',function(){
        $(this).find('.summary').fadeIn();
    });
    $('.postlist .post').live('mouseleave',function(){
        $(this).find('.summary').fadeOut('fast');
    });
{% endhighlight %}

最后去查了下mouseenter和mouseleave的官方api：

**mouseenter**: [http://api.jquery.com/mouseenter/](http://api.jquery.com/mouseenter/)（jQuery官方，英文）
**mouseenter**: [http://www.w3school.com.cn/jquery/event_mouseenter.asp](http://www.w3school.com.cn/jquery/event_mouseenter.asp)（w3school，中文）
**mouseleave**: [http://api.jquery.com/mouseleave/](http://api.jquery.com/mouseleave/)（jQuery官方，英文）
**mouseleave**: [http://www.w3school.com.cn/jquery/event_mouseleave.asp](http://www.w3school.com.cn/jquery/event_mouseleave.asp)（w3school，中文）