# 插件WirelessWordPress发布
- category: WordPress
- tags: wordpress

---

### 2011.08.26 update

新增一款jQuery Mobile手机版主题，详情请看 [http://messense.me/theme-wireless-jm.html](http://messense.me/theme-wireless-jm.html)

### 2011.08.23 update

插件已提交至WordPress官方插件目录，官方地址：[http://wordpress.org/extend/plugins/wireless-wordpress/](http://wordpress.org/extend/plugins/wireless-wordpress/)

## 吼吼,Wireless WordPress插件正式发布!

这是本人写的第一个WordPress插件，对我来说意义非凡，哈哈，撒花。下面对这个插件做一些介绍。

**插件名称:** Wireless WordPress(中文即无线WordPress，也就是WP的wap版的意思)

**插件版本:** 1.0

**插件功能:** 

1. 自动检测手机访问
2. 支持设置wap版默认主题
3. 插件自带一款名为wireless的WordPress主题，此主题专为手机版制作，也可以直接作为WordPress普通主题在控制面板启用
4. 支持用户自定义的手机版主题，只需在主题控制面板里面上传主题并在Wireless WordPress插件的设置里面将其设置为默认手机主题即可
5. 支持调整文章内容中的图片的大小以适应手机浏览器，可以选择调整或不调整，并可以自定义图片显示的高度和宽度
6. 可以使用GET请求参数 mobile=1 让PC浏览器显示手机版主题，同样可以使用mobile=0强制显示Web版主题，这样比较方便预览和开发手机版主题
7. 支持定义是否允许手机版搜索引擎(Googlebot-Mobile & Yahoo!)收录手机版页面

**插件增加的WordPress选项:** wireless_wordpress_options

## 为什么写这样一个插件?

WordPress并不原生支持手机访问，因而产生了很多wap插件，比如MobilePress、WP Mobile Edition、WP-T-WAP、WPTouch等。我也试用过了很多这种插件，MobilePress和WP-T-WAP的界面基本相同，我不是很喜欢那种风格；WP Mobile Edition个人感觉太大、太复杂了点，也不是很喜欢;而WPTouch的显示效果很棒，只可惜这里是中国，并不是每个人都拥有iPhone/iPad/Andriod，大多数人的手机都比较低端，那种效果在目前的客户端环境下并不适用。另外，这些插件大多只能使用它所提供的主题，而我想要的是一个可以自定义手机版主题的插件。基于以上原因，我花了一点时间写了Wireless WordPress这个插件，实现了我想要的功能。

## 关于默认主题wireless的说明

这个主题是我原创的WP主题，比较适合手机版。这个主题支持WordPress3.0及其以上版本的菜单功能。要为手机版设置独立于web版的菜单(用于顶部导航)，您需要在控制面板启用主题wireless，然后进入外观选项的菜单设置中，新建菜单，菜单名 **必须为wap**，然后保存即可。

该主题使用XHTML Mobile Profile标记语言，电脑浏览器同样可以访问。主题内置邮件回复提醒、自定义keywords和description、数字分页、登录可见&评论可见短代码、嵌套评论等功能。

该主题在手机自带的浏览器及Opera Mobile等对标准支持较好的浏览器中显示效果比较好，由于UC、QQ等手机浏览器使用了wap中转压缩/适应屏幕等东西，显示效果并不如手机自带浏览器。有个小问题是UC的wap中转压缩是经过UC的服务器请求的页面 **可能导致** 无法自动识别出是手机访问。由于没有使用浮动(float)布局，该主题在各种电脑浏览器中显示效果比较一致。

由于是手机版主题，wireless并未引入任何javascript,还默认禁用了WP-Postviews等插件的js(无觅相关文章的js暂时还好像无法禁用，郁闷!)。我不能保证你的其它插件不会自动引入js。

前面说过wireless主题可以直接作为WordPress普通主题在控制面板启用。该主题也会不定期更新，使用者请关注本博客。

后续更新也会在本文列出，如果您喜欢本插件，请收藏本博客以便获得插件更新信息。同时，本插件也将提交到WordPress插件目录。如果您在使用的过程中发现任何Bug，请在本文留言或者联系本人提交Bug及解决办法(如果您找到了解决办法的话)。同时，如果您有什么意见或建议，也请在本文留言告诉我。