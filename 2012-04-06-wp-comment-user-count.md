# WP获取文章评论人数
- category: WordPress
- tags: wordpress

---

早上在Google Reader里看到 [@Zww](http://zww.me) 大大的新博文[《 WordPress 变态需求: 获取文章的评论人数》](http://zww.me/archives/25613)，简单看了下，@zww 给出的代码如下：

{% highlight php %}
<?php
    /* 获取文章的评论人数 by zwwooooo | zww.me */
    function zfunc_comments_users($postid=0,$which=0) {
	   $comments = get_comments('status=approve&post_id='.$postid); //获取文章的所有评论
	   if ($comments) {
		  $i=0; $j=0; $commentusers=array();
		  foreach ($comments as $comment) {
		      ++$i;
			  if ($i==1) { $commentusers[] = $comment->comment_author; ++$j; }
			  if ( !in_array($comment->comment_author, $commentusers) ) {
			     $commentusers[] = $comment->comment_author;
				 ++$j;
			 }
		  }
		  $output = array($j,$i);
		  $which = ($which == 0) ? 0 : 1;
		  return $output[$which]; //返回评论人数
	   }
	   return 0; //没有评论返回0
    }
?>
{% endhighlight %}

这是用循环来统计不同的用户评论数，实在是有些麻烦了，也没有这个必要。正如 @zww 所说的，其实可以用SQL直接查询出来，我就小小写了一下咯，很简单的。把下面的代码扔进主题的functions.php里面：

{% highlight php %}
<?php
    function comment_users($post_id = NULL) {
        if(!$post_id)
            $post_id = get_the_ID();
        global $wpdb;
        $sql = "SELECT COUNT(DISTINCT  `comment_author_email`) FROM {$wpdb->comments} WHERE `comment_post_ID`={$post_id} and `comment_approved`='1'";
        $count = $wpdb->get_var($wpdb->prepare($sql));
        return $count;
    }
?>
{% endhighlight %}

调用方法：
{% highlight php %}
<?php echo comment_users(); ?> //或者<?php comment_users(get_the_ID()); ?>
{% endhighlight %}

就是这么简单咯。