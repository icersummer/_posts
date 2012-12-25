# 用腾讯微博做公告栏
- category: WordPress
- tags: wordpress

---

昨天在网上游荡，谷歌了一下“腾讯微博rss”，发现腾讯微博已经开放了rss输出广播和json输出广播功能([传送门](http://open.t.qq.com/resource.php?i=3,3))，于是就想用这个做一个公告栏，效果见本博客顶部。嘿嘿，说做就动手。
首先，在你的functions.php里面增加如下代码:

{% highlight php %}
<?php
    /**
     * 显示腾讯微博 by 乱了感觉
     * http://messense.me
     */
    function messense_show_weibo() {
        $num = 5; //显示的数量
        $url = 'http://v.t.qq.com/output/json.php?type=1&name=wapdevelop&sign=6f79f65316f78951be3acf106e97904423e67983'; //腾讯微博的json地址,改成你自己的，type=1为我的广播，=2为我的首页，建议1
        $expire = 300; //过期时间
        $cache_file = get_template_directory() . '/weibo.dat';  //缓存文件
        $arr = array();
        clearstatcache();
        if (file_exists($cache_file) && time() <= (filemtime($cache_file) + $expire)) {
            //缓存文件存在且未过期则直接读取
            $arr = unserialize(file_get_contents($cache_file));
        } else {
            //缓存文件不存在或已过期则读取json数据处理并缓存
            $content = file_get_contents($url);
            $content = str_replace(")", "", str_replace("weiboData(", "", $content));
            $weibo = json_decode($content, true);
            $arrsize = count($weibo['data']);
            if ($num > $arrsize) {
                $num = $arrsize;
            }
            $arr = array_slice($weibo['data'], 0, $num);
            file_put_contents($cache_file, serialize($arr)); //写入缓存文件
        }
        if ($arr) {
            echo '<div id="myweibo">';
            echo '<div id="weibo-data">';
            foreach ($arr as $weibo) {
                echo '<p class="weibo-content"><a href="http://t.qq.com/p/t/', $weibo['id'], '" target="_blank" title="点击进入详细阅读">', messense_cut_str($weibo['content'], 120), '</a><span class="weibo-time">(', date('m-d H:i', $weibo['timestamp']), ')</span></p>';
            }
            echo "</div></div>";
        } else {
            echo '<div id="myweibo><p class="weibo-error">微博数据读取出错鸟！腾讯微博抽风鸟！</p></div>';
        }
    }

    function messense_cut_str($string, $sublen, $start = 0, $code = 'UTF-8') { //中文截断专用函数
        if ($code == 'UTF-8') {
            $pa = "/[\x01-\x7f]|[\xc2-\xdf][\x80-\xbf]|\xe0[\xa0-\xbf][\x80-\xbf]|[\xe1-\xef][\x80-\xbf][\x80-\xbf]|\xf0[\x90-\xbf][\x80-\xbf][\x80-\xbf]|[\xf1-\xf7][\x80-\xbf][\x80-\xbf][\x80-\xbf]/";
            preg_match_all($pa, $string, $t_string);
            if (count($t_string [0]) - $start > $sublen)
                return join('', array_slice($t_string [0], $start, $sublen));
            return join('', array_slice($t_string [0], $start, $sublen));
        } else {
            $start = $start * 2;
            $sublen = $sublen * 2;
            $strlen = strlen($string);
            $tmpstr = '';
            for ($i = 0; $i < $strlen; $i++) {
                if ($i >= $start && $i < ($start + $sublen)) {
                    if (ord(substr($string, $i, 1)) > 129)
                        $tmpstr .= substr($string, $i, 2);
                    else
                        $tmpstr .= substr($string, $i, 1);
                }
                if (ord(substr($string, $i, 1)) > 129)
                    $i++;
            }
            return $tmpstr;
        }
    }
?>
{% endhighlight %}

上面的代码已经给出了注释，你可以更改$num的数量来决定输出地微博数量，我设置了5条，后面的jQuery代码中的一些数字与之相应，改这个数的话需要做出修改。

然后在适当的地方调用```<?php messense_show_weibo();?>```。

接着当然是用css来修饰一下啦，根据你的需要修改一下,我没做什么美化的。

{% highlight css %}
    #myweibo{float:left;height:65px;width:510px;margin: 2px 10px 0 10px;background:rgba(246,246,246,0.6);position:relative;overflow: hidden;}
    #weibo-data{position: absolute;width:510000px;height:65px;padding-left: 2px;}
    .weibo-content{display:inline-block;float:left;width:510px;line-height: 16px;text-align: left;}
    .weibo-time{display:inline-block;}
    .weibo-error{}
{% endhighlight %}

下面轮到jQuery轮显的代码上场了，撒花。。。我做的是左右滚动。

{% highlight javascript %}
jQuery(document).ready(function($){
    wbShowNum=1;
    function showWeibo(){
        if(wbShowNum<5){
            $('#weibo-data').animate({
                left:'-=510' 
            },{
                duration:400
            });
        }else{
            $('#weibo-data').animate({
                left:'+=510' 
            },{
                duration:400
            });
            if(wbShowNum==8) wbShowNum=0;
        }
        wbShowNum++;
    }
    wb=setInterval(showWeibo,3000);
    $('#myweibo').hover(function(){
        clearInterval(wb);
    },function(){
        wb=setInterval(showWeibo,3000);
    });
}
{% endhighlight %}

自己适当修改吧。

好了，到此为止。玩去了。