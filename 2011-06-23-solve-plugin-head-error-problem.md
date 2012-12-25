# 解决WP插件头部错误的问题
- category: WordPress
- tags: wordpress

---

昨天，在某WP大虾的博客里发现了wp-content-index这个不错的自动文章内索引的插件，就去WP后台准备装下这个插件，习惯性地搜索wp-content-index，第一个结果就是，然后就鼠标猛击“现在安装”，没想到WordPress竟然提示“插件的头部错误”。

真晕，我就把插件下载下来，在本地的WordPress里测试安装，一下子就成功了。郁闷。但郁闷归郁闷，问题还是要解决的，求助Google大神，在某博客(忘了名字和网址了，真抱歉)里找到了解决办法。

原来是Object-Cache的原因。因为之前为了给博客提速，就开启了object-cache(用来减少数据库查询数的一个东东，具体请Google一下)，它会在wp-content/cache/文件夹里生成一个object-cache什么.lock格式的文件。于是就ftp上去把这个.lock文件给删了，再进WP后台，发现wp-content-index插件已经安装好了。嘿，直接启用，再经过点小设置插件就OK了。