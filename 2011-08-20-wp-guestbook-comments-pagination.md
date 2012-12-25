# WP留言本评论分页
- category: WordPress
- tags: wordpress

---

曾经看过万戈大湿的[《WordPress 免插件实现评论分页导航》](http://wange.im/paginate-comments-without-plugins-in-wordpress.html)和[《WordPress 指定页面评论分页功能》](http://wange.im/paginate-comments-in-wordpress.html)以及[《WordPress 不同页面不同评论分页功能》](http://wange.im/per-page-of-wp-list-comments-in-wordpress.html)这三篇文章，都是讨论WordPress评论分页的。参考他所写的方法，我也给我的留言本添加了评论分页功能，还特地做了个ajax加载评论。这里分享下折腾过程。

首先，要想使用数字导航的评论分页，必须现在控制面板的设置->讨论里面开启评论分页功能，因为我只想对留言本的评论分页，所以这里的每页显示XX条评论我填了个大数字——200，然后默认显示最后页。这样一来一般的文章的评论就不会分页了，毕竟大多数人博客每篇文章的评论数都不会过百的。

然后在主题目录新建文件guestbook.php，复制page.php的所有内容到里面，并在最前面加上如下内容：
{% highlight php %}
<?php
/*
    Template Name: Guestbook
*/
?>
{% endhighlight %}

找到代码 ```&lt;?php comments_template(); ?&gt;``` ，将其替换成```&lt;?php comments_template('/guestbook_comments.php',TRUE); ?&gt;```。接着新建文件guestbook_comments.php，复制comments.php的所以内容到里面，找到代码 ```&lt;?php wp_list_comments('type=comment&callback=mytheme_comment'); ?&gt;```（参数不一定是这样），将其替换成 ```&lt;?php wp_list_comments("type=comment&callback=mytheme_comment&per_page=10"); ?&gt;```(callback的值换成你的主题的自定义评论回调函数名)，显然这里的per_page控制了每页显示的评论数。最后，查找代码 ```&lt;?php paginate_comments_links(); ?&gt;``` ，如果没有，在wp_list_comments之后添加上，如果被注释了请去掉它的注释(php or html)。

如果不出问题，去控制面板给你的留言本设置默认模板为Guestbook，然后你的留言本已经是数字导航评论分页了，至于CSS的美化我就不管了。

如果你使用了zww的评论楼层数代码，然后发现分页后楼层数不正确，请参考我的文章[《评论分页楼层数错误解决办法》](http://messense.me/solution-to-wrong-comment-floor.html)解决。

关于ajax加载评论的部分，这里不想写了，因为我的方法比较麻烦，修改的地方太多了，实在不好写。有兴趣的可以参考winy大湿的[《WordPress评论非插件实现ajax翻页》](http://winysky.com/wordpress-plug-ins-to-achieve-non-ajax-flip-comment),这个比较容易折腾一点。