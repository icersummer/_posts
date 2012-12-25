# 叛离WordPress
- category: 吐槽
- tags: tornado, wordpress

---

这个博客从去年6月份建立，在今天(2012-05-17)之前一直都在使用WordPress作为博客系统，而今天我叛离了WordPress，换上了自己开发的YaBlog系统(YaBlog=Yet a Blog)。

其实WordPress很好，至少对一个新手来说，它是建立独立博客的不二之选，而且还那么耐折腾。但是，我还是觉得，用自己写出来的博客系统比较cool~一般来说，重复造轮子不是什么好事，不过也未尝是坏事，至少通过开发它，学会了很多东西。当然啦，开发期间也受够了各种bug的折腾。犯错不可怕，改正错误并吸取教训就好了。

主题什么的，这次是模仿的博客系统Jekyll的主题，使用了Twitter的Bootstrap(用起来很方便)。顺便说说这个YaBlog的架构吧，python写的，用了tornado web框架，数据库方面用的是SQLAlchemy配合一个DjangoQuery类，还用到了memcached缓存(其实挺想试试Redis的，呵呵)，使用markdown写作，pygments处理代码高亮，等等。目前还是有很多问题的，慢慢来咯。

文章是人肉搬过来的，删掉了很多以前写的无病呻吟的文章。呵呵，就这样。