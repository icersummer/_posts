# 让主题支持mailtocommenter的评论样式
- category: WordPress
- tags: wordpress

---

mailtocommenter这个插件，想必玩wp的童鞋都应该比较熟悉。不知道你注意过没有，在WP后台的Mail To Commenter Option这个设置里有个“自定义按钮显示”和“按钮样式”的选项。想必很多童鞋都选择了显示图片，就是那个@的符号的图片吧。然而当我们满怀希望地去刷新日志时却发现根本没有显示成后台设置的那种样式，于是很失望很失望。

不知道你注意到没有，在它的后台设置里有句话是这么说的：

>自定义按钮显示：
>
><small>这部分用来设定mailtocommenter_button()函数的输出内容和格式。请在模板文件中插入&lt;?php if(function_exists('mailtocommenter_button')) mailtocommenter_button();?&gt;代码用于生成按钮。</small>
>
>**按钮样式**：
>
><small>自定义mailtocommenter_button()函数的输出。支持html代码。默认输出显示插件目录下的notify.png图片。您可自定义采用其它图片或使用文本，如[回复]</small>

这说明什么？说明要显示后台设置的这种样式必须在主题模板的适当位置调用mailtocommenter_button这个函数。于是看了下自己主题的回复链接的生成代码：

```php
    <?php comment_reply_link(array_merge($args, array('depth' => $depth, 'max_depth' => $args ['max_depth']))); ?>
```

又查了查wp函数comment_reply_link的用法，了解到可以用一个reply_text参数设置链接显示的文字。可是mailtocommenter这个插件提供的那个mailtocommenter_button函数会直接输出链接，没法将值传给reply_text作为参数啊！于是又去看mailtocommenter的源代码，发现还有个mailtocommenter_button_html函数只会返回后台设置的那种样式，这样就好办了。

于是我把上面的代码改成了:

```php
<?php comment_reply_link(array_merge($args, array('depth' => $depth, 'max_depth' => $args ['max_depth'], 'reply_text' => messense_reply_text()))); ?>
```

然后是这个messense_reply_text函数的实现:

```php
<?php 
    //兼容mailtocommenter的button,by 乱了感觉(http://messense.me)
    function messense_reply_text() {
        if(function_exists('mailtocommenter_button_html')) {
            return mailtocommenter_button_html();
        } else {
            return __('[Reply]');
        }
    }
?>
```

如此便实现了这个简单的兼容，好了，到此为止吧。