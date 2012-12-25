# WordPress密码保护文章相关
- category: WordPress
- tags: wordpress

---

简单整理一下，方便他人。

## 去除标题前的“密码保护:”等前缀

将如下代码扔进主题的functions.php里面即可：

{% highlight php %}
<?php
    add_filter('protected_title_format', 'no_title_prefix');
    add_filter('private_title_format', 'no_title_prefix');

    function no_title_prefix( $prefix ) {
        return '%s';
    }
?>
{% endhighlight %}

## 首页显示输入密码表单

默认情况下，输入密码的表单只会在文章页显示，而其他页面比如首页、分类页等则是显示一条提示语句，尤其是最后有个“密码：”，然后后面又没有输入框，很别扭。于是想到不如直接显示出输入密码的表单好了。依然把如下代码扔进functions.php即可：

{% highlight php %}
<?php
    function index_password_form($content) {
        global $post;
        if(post_password_required( $post )) {
            return '<div class="password-form">' . get_the_password_form() . '</div>';
        }
        return $content;
    }

    if(!is_singular()) {
        add_filter('the_content','index_password_form');
    }
?>
{% endhighlight %}

## 自定义密码输入表单

如果觉得WP自动生成的表单不好看也不好用CSS渲染，想自行生成密码表单的HTML结构，把如下代码丢进functions.php里面可以做到：

{% highlight php %}
<?php
    function custom_password_form() {
        global $post;
        $label = 'pwbox-'.(empty($post->ID) ? rand() : $post->ID);
        $o = '<form class="protected-post-form" action="' . get_option('siteurl') . '/wp-pass.php" method="post">' . __( "This post is password protected and this is what I want to say about that. To view it please enter your password below:" ) . '<label for="' . $label . '">' . __( "Password:" ) . ' </label><input name="post_password" id="' . $label . '" type="password" size="20" /><input type="submit" name="Submit" value="' . esc_attr__("Submit") . '" /></form>';
        return $o;
    }

    add_filter('the_password_form', 'custom_password_form');
?>
{% endhighlight %}

你可以在custom_password_form函数里面自由发挥，创造一个个性的表单。