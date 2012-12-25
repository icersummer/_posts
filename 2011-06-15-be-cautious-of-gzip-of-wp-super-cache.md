# 慎用WP Super Cache的GZIP压缩
- category: WordPress
- tags: wordpress

---

最近在折腾自己的主题，搞得还算可以的时候传到了服务器上，之后就一直有网友反应博客打不开，出现乱码，知道后我就一直在找原因。谷歌了一下，很多人说乱码是因为主题文件编码的原因，于是我就尝试转换编码，从utf-8到gb2312到gbk,从utf-8无BOM到有BOM，各种方法都用尽了，乱码的情况却还是没有解决，我狂晕。

看着被自己改来改去的主题文件，我都想放弃了，但我不能。

我又去谷歌求助了，这次在博客[八万光年](http://zhaolianlin.com)找到了解决办法，原文地址请看[《wordpress博客页面乱码的问题》](http://zhaolianlin.com/2010/03/wordpress-garbage-characters/)。原来是启用了WP Super Cache的gzip压缩造成的，想了想这很奇怪啊，gzip怎么会导致乱码呢？但是我用Chrome12打开又不会乱码，其它的浏览器就乱码。

于是我就停用了WP Super Cache的gzip压缩功能，结果果然没乱码，Firefox4完美打开。因为觉得没了gzip可能会影响页面载入速度，我就想不通过WP Super Cache来开启gzip，无奈Cpanel后台竟然没有优化网站这个按钮。。。然后我就心血来潮想看看没gzip怎么样，就去了站长工具的[网页GZIP压缩检测](http://tool.chinaz.com/Gzips/)测试了下。这不测不知道，一测吓一跳，明明我没有手工/插件开启gzip，检测结果却已经是启用了gzip压缩了，压缩率还达到了71%，我狂汗！！！难道服务器已经默认开启了GZIP?如此来看，乱码的原因便明了了，两次GZIP压缩导致的。貌似Chrome对gzip很友好，这样也能正常显示。

虽然说WP Super Cache是一个非常好的WP插件，但当我们使用时还是应该注意，希望这篇文章能解决同样被这样的乱码困扰的童鞋们的问题。其实只要我的小站不乱码，我就要烧高香了，呵呵。