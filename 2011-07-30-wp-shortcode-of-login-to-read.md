# WP短代码之登录可见
- category: WordPress
- tags: wordpress

---

很多Discuz!或者PHPWind搭建的论坛都有帖子内容登录可见的功能，那么咱的WordPress博客可不可以实现这样的功能呢？虽然WordPress核心程序没有设计这样的功能，但得益于WordPress优秀的扩展能力，我们完全可以用小小的一段代码来实现这个小小的功能。

相信喜欢折腾WordPress的童鞋一定听说过短代码(shortcode)吧？什么？没听说过？我汗、、、下面我就用这个一个shortcode来实现这个功能。把下面的代码加入到你的主题的functions.php中：

{% highlight php %}
<?php
    /**
    * 短代码之登录可见
    * @author 乱了感觉(http://messense.me)
    */
    function login_to_read($atts,$content=null) {
        extract(shortcode_atts(array("notice"=>'<span class="login-to-read">此处内容需要<a href="'. wp_login_url(get_permalink()).'" title="登录">登录</a>后才能查看.</span>'),$atts));
        if(is_user_logged_in()){
            return $content;
        }else{
            return $notice;
        }
    }
    
    add_shortcode('login', 'login_to_read');
?>
{% endhighlight %}

肯定又有童鞋要问了，这个要怎么用呢？其实很简单，在你写文章的时候，把你想让它登录可见的内容用“[@login]”和“[@/login]”(这里为了防止这段文字“被登录可见”，我特意添加了@来防止，使用时请去掉@)包围起来即可。