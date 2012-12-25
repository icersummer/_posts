# 好用的PHP调试函数
- category: PHP
- tags: PHP

---

像php这样的动态语言，调试起来还是挺麻烦的。我们经常需要打印运行中的一些变量来判断程序是否有错误，然而变量是有很多类型的。一般的字符串、数字等可以直接echo/print，数组就print_r，对象呢？其实print_r也是可以打印对象的。为了方便，我常常会定义一个简单地函数来处理这些事情：

{% highlight php %}
<?php
    function p($data1 = null) {
        $args = func_get_args();
        foreach ($args as $arg) {
            echo '<pre>';
            if (is_null($arg)) {
                $arg = "NULL";
            } else if (is_bool($arg)) {
                $arg = $arg ? "TRUE" : "FALSE";
            }
            print_r($arg);
            echo '</pre>';
        }
    }
?>
{% endhighlight %}

然后每次打印变量只需p(变量)即可(也可以接受多个参数，如p(变量1,变量2))，是不是很方便呢？