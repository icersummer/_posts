# Gentoo 快速安装
- category: 杂七杂八
- tags: linux

---

## 官方文档

* 英文：[http://www.gentoo.org/doc/en/gentoo-x86-quickinstall.xml](http://www.gentoo.org/doc/en/gentoo-x86-quickinstall.xml)
* 中文：[http://www.gentoo.org/doc/zh_cn/gentoo-x86-quickinstall.xml](http://www.gentoo.org/doc/zh_cn/gentoo-x86-quickinstall.xml)

P.S. 推荐看英文的，中文的没英文的新。

## 安装过程

{% highlight bash %}
# 配置网络
livecd root # net-setup eth0
# 开启 livecd sshd 服务
livecd root # /etc/init.d/sshd start
# 设置 livecd root 密码
livecd root # passwd
# 查看 dhcp 分配到的 ip 地址
livecd root # ifconfig
# 现在可以从另外一台机子上 ssh 连接过去安装，也可以不
# 分区，通常分三个区 boot root 和 swap，boot 100M 够了，swap 跟实际内存大小一样，root 根据需要配置大小，假设 boot=/dev/sda1，root=/dev/sda3，swap=/dev/sda2
livecd root # cfdisk
# 格式化分区并启用 swap 分区
livecd root # mke2fs /dev/sda1
livecd root # mke2fs -j /dev/sda3
livecd root # mkswap /dev/sda2 && swapon /dev/sda2
# 挂载分区
livecd root # mount /dev/sda3 /mnt/gentoo
livecd root # mkdir /mnt/gentoo/boot
livecd root # mount /dev/sda1 /mnt/gentoo/boot
livecd root # cd /mnt/gentoo
# 安装 stage3 包，可以在 livecd 里面 wget 下来，也可以另外下载下来再 scp 到 livecd 里面
livecd gentoo # wget http://mirrors.163.com/gentoo/releases/x86/current-iso/stage3-i686-20121213.tar.bz2
# 解压 stage3 包
livecd gentoo # time tar xjpf stage3*
livecd gentoo # cd /mnt/gentoo/usr
# 安装 portage 包
livecd usr # wget http://mirrors.163.com/gentoo/snapshots/portage-latest.tar.bz2
# 解压 portage 包
livecd usr # time tar xjf portage*
# 切换系统
livecd usr # cd /
livecd / # mount -t proc proc /mnt/gentoo/proc
livecd / # mount --rbind /dev /mnt/gentoo/dev
livecd / # cp -L /etc/resolv.conf /mnt/gentoo/etc
livecd / # chroot /mnt/gentoo /bin/bash
livecd / # env-update && source /etc/profile
# 配置时区
livecd / # cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
livecd / # echo "Asia/Shanghai" > /etc/timezone
# 设置 hostname
livecd / # cd /etc
livecd etc # echo "127.0.0.1 gentoo.local gentoo localhost" > hosts
livecd etc # sed -i -e 's/hostname.*/hostname="gentoo"/' conf.d/hostname
livecd etc # hostname gentoo
livecd etc # hostname -f
# 配置内核，这里使用 genkernel 自动配置，免去手动配置的麻烦
livecd etc # time emerge gentoo-sources
livecd etc # time emerge genkernel
livecd etc # genkernel all
livecd etc # cd /usr/src/linux
livecd linux # cp arch/i386/boot/bzImage /boot/kernel
# 配置 fstab，使用 nano 编辑，不习惯的话，可以先 emerge vim 然后用 vim 编辑
# 把 fstab 里的 BOOT 改成 /dev/sda1，ROOT 改成 /dev/sda3，SWAP 改成 /dev/sda2
livecd linux # nano -w /etc/fstab
# 配置网络
livecd linux # cd /etc/init.d
livecd init.d # ln -s net.lo net.eth0
livecd init.d # cd ../conf.d
livecd conf.d # echo 'config_eth0=( "dhcp" )' >> net
livecd conf.d # echo 'hostname=gentoo' > hostname
# 开机自动获取 dhcp 获取 ip
livecd conf.d # rc-update add net.eth0 default
# 开机自动运行 sshd 服务
livecd conf.d # rc-update add sshd default
# 修改 root 密码
livecd conf.d # passwd
# 安装系统工具
livecd conf.d # time emerge syslog-ng vixie-cron
livecd conf.d # rc-update add syslog-ng default
livecd conf.d # rc-update add vixie-cron default
# 安装网络工具
livecd conf.d # time emerge dhcpcd
livecd conf.d # time emerge ppp
# 安装 grub 引导器
livecd conf.d # time emerge grub
# 编辑 grub.conf
livecd conf.d # nano -w /boot/grub/grub.conf
# grub.conf 示例
default 0
timeout 10

title Gentoo
root (hd0,0)
kernel /boot/kernel root=/dev/sda3
initrd /boot/initramfs # 注意路径要正确
# 安装 grub 到硬盘
livecd conf.d # grub
grub> root (hd0,0)
grub> setup (hd0)
grub> quit
# 完成并重启
livecd conf.d # exit
livecd / # umount -l /mnt/gentoo/dev{/shm,/pts,}
livecd / # umount -l /mnt/gentoo{/proc,/boot,}
livecd / # reboot
{% endhighlight %}