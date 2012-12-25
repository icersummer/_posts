# spl_autoload在自定义session类中出错的解决办法
- category: PHP
- tags: php

---

做新项目session入库时遇到这个问题，google一下在php的bug库心理找到了它。09年提交的bug,10年11月才解决，难怪现在用的那个php5.3有问题。郁闷。。。据知问题出在write方法上。。。

bug地址： [http://bugs.php.net/bug.php?id=49867&amp;edit=2](http://bugs.php.net/bug.php?id=49867&amp;edit=2)

解决办法：

```php
< ?php register_shutdown_function("session_write_close");?>
```