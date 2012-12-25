# WP短代码之评论可见
- category: WordPress
- tags: wordpress

---

昨天给大家分享了[《WP短代码之登录可见》](http://messense.me/wp-shortcode-of-login-to-read.html)，大家大多认为没什么用处，也是。不过，如果你的博客经常分享一些文件下载的话，倒是可以用它来防止恶意盗链，呵呵。

今天继续给大家分享《WP短代码之评论可见》，这个可就实用多了吧，比如说你的文章里面给出了一个115的附件下载链接又不希望看文章的人直接去下载了而不对文章发表评论，这时候评论可见就可以起作用了。好了，下面给出实现这个功能的代码，加到你的主题的functions.php中。代码虽然很短，但是我可是查阅了很多WP函数参考的资料才终于写好的呢。

{% highlight php %}
<?php
    /**
    * 短代码之评论可见
    * @author 乱了感觉(http://messense.me)
    */
    function reply_to_read($atts,$content=null){
        extract(shortcode_atts(array("notice"=>'<span class="reply-to-read">此处内容需要<a href="'. get_permalink().'#respond" title="评论本文">评论本文</a>后<a href="javascript:window.location.reload();" title="刷新">刷新本页</a>才能查看.</span>'),$atts));
        $email=null;
        $user_ID=(int)wp_get_current_user()->ID;
        if($user_ID>0){
            $email =  get_userdata($user_ID)->user_email; //如果用户已登录,从登录信息中获取email
        }else if(isset($_COOKIE['comment_author_email_'.COOKIEHASH])){
            $email=str_replace('%40','@',$_COOKIE['comment_author_email_'.COOKIEHASH]); //如果用户未登录但电脑上有本站的Cookie信息，从Cookie里读取email
        }else{
            return $notice; //无法获取email，直接返回提示信息
        }
        if(empty($email)){
            return $notice;
        }
        global $wpdb;
        $post_id=get_the_ID(); //文章的ID
        $query="SELECT `comment_ID` FROM {$wpdb->comments} WHERE `comment_post_ID`={$post_id} and `comment_approved`='1' and `comment_author_email`='{$email}' LIMIT 1";
        if($wpdb->get_results($query)){
            return $content; //查询到对应的已经审核通过的评论则返回内容
        }else{
            return $notice; //否则返回提示信息
        }
    }
    
    add_shortcode('reply', 'reply_to_read');
?>
{% endhighlight %}

用法依然很简单，即：[@reply]评论可见的内容[@/reply]  (去掉@)，同时支持自定义提示信息，使用[@reply notice="自定义的提示信息"]评论可见的内容[@/reply]  (同样要去掉@)。ok,写完了，玩去了。