# 安装完 Gentoo 后要做的事
- category: 杂七杂八
- tags: gentoo, linux

---

Gentoo 的安装并不难，一般只要按照官方文档走下去，基本没什么大问题。（[Gentoo 安装参考](http://messense.me/gentoo-quick-install.html)） 不过 Gentoo 安装的重点其实不是前面的安装过程，而是后续的配置过程。这里记录下我的心得。

## 安装实用工具

```bash
$ emerge gentoolkit
$ emerge eix
```

## 更新软件源

```bash
$ emerge --sync
```

## 更新 GCC

先 `emerge -uDNpv gcc` ，没有问题就 `emerge -uDNv gcc` ，把 GCC 更新到最新先，之后可以用 `gcc-config` 来选定新的gcc版本

优先更新GCC的好处是，这样后面的重编译都可以选择最新的编译器，这样最新编译器会给系统带来或多或少的好处

这个过程是很漫长的，建议趁此机会休息一下。:-)

## 更新 world

如果整个系统的软件改动较大，就用 `emerge -ev world` 来更新，否则就用 `emerge -uDNv world` 来更新。至于怎么叫改动大，就看个人了，一般来说，没必要就不要 `emerge -ev world` ，优先考虑 `emerge -uDNv world` 。因为前者是不管软件的版本是什么都强制重编译的。

此过程也可能很漫长。。。

## 更新完之后

```bash
$ emerge --depclean # 清理无用的包
$ revdep-rebuild # 重建依赖关系
$ eclean-dist # 清理旧包
```

务必执行 `revdep-rebuild` 修复一些软件间的依赖关系，否则系统可能启动不了。

## 安装常用软件

```bash
$ emerge git
$ emerge subversion
$ emerge nginx
```

Gentoo 的默认 python 版本是 3.x 的，而我们常用的却是 2.7.x ，于是更改下：

```bash
$ eselect python list
$ 1 python2.7
  2 python3.2 *
$ eselect python set 1
``` 

大概就是如此了