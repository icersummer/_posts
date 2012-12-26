# Catsup 写作指南
- category: catsup
- tags: catsup, markdown

---

## 基本结构

    # 标题
    - title: 这样定义标题也可以
    - category: 分类
    - tags: 标签1, 标签2
    - format: regular
    - date: 2012-12-26
    - comment: open
    ---

    正文部分

属性前的 `- ` 是可选的。

* title 标题有以上两种写法，选择自己喜欢的一种即可。
* category 属性暂时还没有使用，只是保留属性而已，可不定义。
* tags 用来定义文章的标签，多个标签用半角逗号隔开即可。
* format 用来定义文章形式，没错，就是从 WordPress 的文章形式里借鉴来的，合法的 format 值有：regular(普通文章)、aside(日志)、status(状态)、image(一张图片)、link(链接)、gallery(相册)、video(视频)、audio(音频)、quote(引用)、chat(聊天)，除此之外的值都会被重定义为 regular 。format 是可选属性，且需要主题支持才会真正有效果。
* date 属性用来重定义从 .md 文件名获取到的日期
* comment 属性用来定义是否开启这篇文章的评论，默认是开启的，你可以将其定义为 close 或 no 或 0 或 false 来关闭评论
* 正文前的 `---` 是必须的，**否则会导致程序产生死循环**，务必注意
* 原版 catsup 正文是不支持 html 的，我的修改版去掉了这个限制，可以使用 html 写作

## 文章摘要

你可以通过在正文里添加 &lt;!--more--&gt; 标签来定义文章摘要，所有在此标签之前的正文内容将会被视为文章摘要。
你可以通过修改 config.py 里的 excerpt_index 来决定是否默认在首页显示摘要。当然这需要主题的支持，不过不用担心，内置的两个主题均支持此功能。

## Markdown

关于 markdown 语法之类的，请看 [http://wowubuntu.com/markdown/](http://wowubuntu.com/markdown/)

## 代码高亮

你可以使用 [Github 式的代码高亮语法](http://github.github.com/github-flavored-markdown/) 或者 liquid 式的代码高亮语法(jekyll)。提供 liquid 式的代码高亮语法是为了方便从 jekyll 转到 catsup 的人使用，比如我，:-)