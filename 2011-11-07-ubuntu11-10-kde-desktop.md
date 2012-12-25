# Ubuntu11.10安装KDE桌面
- category: 杂七杂八
- tags: linux

---

Ubuntu11.10的Unity桌面用着实在是不习惯就想换成KDE用用。打开终端，输入sudo apt-get install kUbuntu-desktop，在等待了N+1长时间后终于装好了，注销，登录时选择KDE桌面环境，进去发现TMD全是英语。虽说俺英语还行，但是看着挺不爽的，就想安装下kde的中文语言包。

Google一下发现命令：

    sudo apt-get install language-pack-kde-zh language-pack-kde-zh-base language-pack-zh language-pack-zh-base language-support-zh

在终端里面敲入提示出错，根据错误信息，发现新版本的KDE的语言包命名改变了，于是将上面的命令改成：

    sudo apt-get install language-pack-kde-zh-hans language-pack-kde-zh-hans-base language-pack-zh-hans language-pack-zh-hans-base

再运行成功安装中文语言包，又是注销/重新登录，可爱的中文出现鸟，哈哈。