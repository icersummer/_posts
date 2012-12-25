# PHP仿WordPress式参数传递
- category: PHP
- tags: PHP

---

做过WordPress主题或者插件开发的童鞋应该了解，WordPress的一些函数的调用传递参数时既可以用数组又可以用以&amp;连接的字符串。比如wp_get_archives函数，你使用数组式传参调用：<blockquote>&lt;?php wp_get_archives(array('type' =&gt; 'monthly','limit' =&gt; 12)); ?&gt;</blockquote>和使用字符串传参：<blockquote>&lt;?php wp_get_archives('type=monthly&amp;limit=12'); ?&gt;</blockquote>的效果是完全一样的，这说明WordPress可以正确解析这两种传参方式。那么，这是如何实现的呢？

得益于PHP的灵活性，这种传参方式的实现还是比较容易的。php有个函数parse_str可以实现将类似"type=monthly&amp;limit=12"的字符串解析成对应的array('type' =&gt; 'monthly','limit' =&gt; 12)数组，如此一来我们要做的仅仅是判断下传入的是数组还是字符串，如果是字符串则使用parse_str函数解析成数组。

上代码解释一切：

{% highlight php %}
<?php
    /**
    * @author 乱了感觉
    * @param array|string $option
    * @param array|string $arrowed 允许的参数名(可选)
    * @param array|string $default 参数默认值(可选)
    * @return array
    */
    function parse_option($option, $arrowed = null, $default = null) {
        $output = array();
        if($option && is_string($option)) {
            parse_str($option,$output); //解析字符串为数组
        } else if(is_array($option)) {
            $output = $option;
        }
        if(!is_null($arrowed) && is_string($arrowed)) {
            $arrowed = parse_str($arrowed);
        }
        if(!is_null($default) && is_string($default)) {
            $default = parse_str($default);
        }
        foreach($output as $key => $value) {
            if($arrowed && is_array($arrowed)) {
                if(!in_array($key,$arrowed)) {
                    unset($output[$key]); //去除不被允许的参数
                }
            }
        }
        if($default && is_array($default)) {
            $output = array_merge($default,$output); //覆盖、合并默认参数
        }
        return $output;
    }
?>
{% endhighlight %}

如此便可以实现类似WordPress式的传递参数方式，当然，上面的代码做了一些另外的扩展。