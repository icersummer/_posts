# 刷QQ空间留言php代码
- category: PHP
- tags: PHP

---

写了几篇生活抱怨文乐，换换口味，写写技术文。好久没动过php鸟，都忘得差不多鸟。无聊帮女女(你懂的)刷QQ空间留言，懒得手动去搞，就想到可以利用下3GQQ来刷留言。嘿嘿，配合Cron任务，让服务器跑代码给我刷，我自己一边凉快去，哈哈。这个是通过Post数据给手机QQ空间的留言板的add_msg.jsp实现的，你知道的，腾讯对留言频率是有限制的，而且一次最多留言10条就要输入验证码了(自动识别验证码？擦，说得容易，实现起来可不容易！)，所以最好多用几个sid刷。sid是什么？不解释，玩过3GQQ的都知道。代码如下：

{% highlight php %}
<?php
    $sids = array('AdPmYwx-I-12euFm8nFIxMXw','AWRe-dapASAyqybLgggUOBP_'); //这两个sid已经失效了哦，你懂的
    $B_UID = '69504439'; //这个QQ69504439木有开通QQ空间的撒，换成开了的哈，你懂的
    $msgs = array(
        '刷留言中，嘿嘿，我对你好吧？',
        '给你踩踩，加点人气撒！',
        '低调路过，高调留言！',
        '我们的口号是：进空间一定要留言！嘻嘻。',
        '欢迎回访我的博客,http://messense.me，嘿嘿！',
        '让哥的留言泛滥吧，无聊ing，我无聊，服务器也受罪，嘻嘻！',
        '留言一定要短，长了就懒得打字鸟，飘过！',
        '囧~~~囧~~~',
        '只能留言十次。。。鄙视腾讯，鄙视麻花藤',
        '嘿嘿，虽然手动留言更有诚意，不过这么麻烦的事还是交给机器吧，服务器跑代码比自己输入可快多了，嘻嘻~~~'
    );
    $success = 0;
    foreach ($sids as $sid) {
        for ($i = 0; $i <= 9; $i++) {
            $curl = curl_init('http://blog30.z.qq.com/mmsgb/add_msg.jsp?sid=' . $sid);
            curl_setopt($curl, CURLOPT_HEADER, false);
            curl_setopt($curl, CURLOPT_NOBODY, true);
            curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
            curl_setopt($curl, CURLOPT_REFERER, 'http://blog30.z.qq.com/mmsgb/msg_board.jsp?sid=' . $sid);
            curl_setopt($curl, CURLOPT_USERAGENT, 'Nokia1680c_CMCC/2.0 (05.61) Profile/MIDP-2.1 Configuration/CLDC-1.1 nokia1680c');
            curl_setopt($curl, CURLOPT_TIMEOUT, 20);
            curl_setopt($curl, CURLOPT_POST, true);
            curl_setopt($curl, CURLOPT_POSTFIELDS, 'msg=' . urlencode($msgs[$i]) . '&sign=1&B_UID=' . $B_UID . '&entry=board&super=0');
            $data = curl_exec($curl);
            curl_close($curl);
            unset($curl);
            if (strstr($data, '成功')) {
                $success++;
            }else{
                //break; //跳出循环，注释掉了哈
            }
        }
    }
    echo '成功发表留言', $success, '条!';
?>
{% endhighlight %}

怎么执行？擦，这也要问？傻一点的，自己浏览器访问这个php文件。偷懒的，传到服务器，配置Linux的Cron任务，30分钟执行一次，你懂的！