# 博客宠物修正版
- category: WordPress
- tags: wordpress

---

这几天有不少网友问我关于我博客上的那个飞来飞去的小人怎么制作的问题。这里说明一下，这个东西叫博客宠物，是博客[捣鼓笔记](http://www.dao-gu.com)的博主木木(不是林木木)的作品，我只是拿来用用，原文地址是[《博客页面浮动交互小人宠物发布》](http://www.dao-gu.com/web/blog-spig-jqurey.html)。

捣鼓那里提供下载的脚本spig.js里面有一些小错误，我把它修正了，另外也修改了很多代码(纯属蛋疼，修改成自己习惯的模式)，这里把我修改后的js脚本源码发出来,下载地址: [Google Code](http://messense.googlecode.com/files/spig.js)

请先下载捣鼓那里的博客宠物附件后用我提供的js替换里面的spig.js，这个东西需要jQuery库，必须先加载jQuery库。

您可能需要修改spig.js里面的一些东西，比如里面的一些域名网址等

## 写给新手的使用方法,高手灰过

第一步，在你的footer.php(其实其他文件也是可以的)的&lt;/body&gt;前加入如下HTML代码：

{% highlight html %}
<div id="spig" class="spig">
    <div id="message">加载中……</div>
    <div id="mumu" class="mumu"></div>
</div>
{% endhighlight %}

接着在你的style.css里面添加如下css代码：

{% highlight css %}
/* 博客宠物 */
    .spig {display:block;width:150px;height:190px;position:absolute;top: -200px;left: 160px;z-index:9999;}
    #message{color :#191919;border: 1px solid #c4c4c4;background:#ddd;-moz-border-radius:5px;-webkit-border-radius:5px;border-radius:5px;min-height:1em;padding:5px;top:-30px;position:absolute;text-align:center;width:auto !important;z-index:10000;-moz-box-shadow:0 0 15px #eeeeee;-webkit-box-shadow:0 0 15px #eeeeee;border-color:#eeeeee;box-shadow:0 0 15px #eeeeee;outline:none;}
    .mumu{width:150px;height:190px;cursor: move;background:url(images/spig.png) no-repeat;}
{% endhighlight %}

注意要把下载得到的spig.png放在主题的images文件夹里，当然了，修改路径也是可以的。

然后，加载jQuery库，如果你已经加载过直接无视！在你的&lt;/body&gt;前添加代码:

{% highlight html %}
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>
{% endhighlight %}

对于WordPress用户，在上面的代码下面添加如下代码，非WordPress版的我不清楚:

{% highlight php %}
<script type="text/javascript">
<?php if(is_home()) echo 'var isindex=true;var title="";';else echo 'var isindex=false;var title="',  get_the_title(),'";'; ?>
<?php if((($display_name = wp_get_current_user()->display_name) != null)) echo 'var visitor="',$display_name,'";'; else if(isset($_COOKIE['comment_author_'.COOKIEHASH])) echo 'var visitor="',$_COOKIE['comment_author_'.COOKIEHASH],'";';else echo 'var visitor="游客";';echo "\n"; ?>
</script>
{% endhighlight %}

然后在上面代码下面添加代码即可：
{% highlight html %}
<script type="text/javascript" src="你的spig.js的路径"></script>
{% endhighlight %}

关于怎么自定义显示的消息，用jQuery选择器加上 showMessage('信息') 即可