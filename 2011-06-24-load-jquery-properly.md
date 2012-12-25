# 按需加载jQuery
- category: WordPress
- tags: wordpress

---

相信喜欢折腾wordpress的童鞋们的主题模板一定都加载了jQuery，不知道大家有没有觉得，当我们设计、编码自己的主题时，我们往往都是在本地的php环境做的。而我们一般都习惯使用Google Api里面的jQuery库，这样一来，在本地测试时jQuery也要从Google那请求、下载过来，这和加载本地WordPress自带的jQuery库的速度根本没法比，尽管Google的服务器很快。况且，有时候我们身边没有网络接入，就用不了Google的jQuery库了。所以，为了方便本地测试也不影响服务器上时使用GG的jQuery库，我们可以对加载jQuery代码的地方做一点小小的改变，省的老是要改来改去。代码如下：

```php
    <?php if (get_bloginfo('url') == 'http://localhost')://测试环境jQuery路径 ?>
    <script type="text/javascript" src="/wp-includes/js/jquery/jquery.js"></script>
    <?php else://服务器环境jQuery路径,自行替换成所需的jQuery库 ?>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
    <?php endif; ?> 
```

小技巧一个，似乎都不值一提，呵呵。