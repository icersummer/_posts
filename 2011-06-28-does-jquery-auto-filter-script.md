# jQuery自动过滤script?
- category: Web前端
- tags: jQuery

---

这些天折腾WordPress主题的全站ajax化，一直很想把加载文章全文也给ajax化。于是动手做了，又为了兼容ajax评论，需要加载comments-ajax.js，想直接用jQuery的getScript方法加载，结果不知怎么地就是不能成功，郁闷死了。

然后又尝试用jQuery给body节点增加一个script元素，把它的src设为comments-ajax.js的地址，使用了下面的代码：

{% highlight javascript %}
$('body').append('<script type="javascript" src="/wp-content/themes/messense/js/comments-ajax.js"></script>');
{% endhighlight %}

结果却发现并没有增加这个script，真晕了。又尝试了用$.html等方法，发现都不行。

于是我开始想，是不是jQuery的这些方法都自动过滤掉了script?求助于Google大神，在网上发现了有人提出的同样的问题。可以确信jQuery确实过滤了script。我日，真的费解了，jQuery干嘛自作主张过滤掉script?为了安全？可是这个不用你来考虑吧，开发者自己会处理的啊！好歹你也给个选项让我们自己选择是否过滤script嘛，真没劲！

最后用原生的javascript增加了那个script，代码如下：

{% highlight javascript %}
var script=document.createElement('script');
script.setAttribute('type','text/javascript');
script.setAttribute('src','/wp-content/themes/messense/js/comments-ajax.js');
var bodyTags=document.getElementsByTagName('body');
var thebody=bodyTags[0];
thebody.appendChild(script);
{% endhighlight %}

郁闷的是，最后为了功能的完整，又把这个蛋疼的功能给去掉了，狂汗。。。