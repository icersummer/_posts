# Catsup 快速安装部署
- category: python
- tags: catsup, python

---

## 下载源码

你可以选择使用我修改过的或者原版的 catsup 代码，以我修改过的为例：

```bash
git clone https://github.com/messense/catsup.git
cd catsup
# 安装必需的 python 库
pip install -r requirements.txt
cp config-sample.py config.py
```

## catsup 配置

```python
# 网站名称
site_title = 'catsup'
# 网站描述
site_description = 'a blog'
# 网站地址，如要同时支持 http和https，请将网址中的 http://或https://替换成//
site_url = 'https://github.com/whtsky/catsup'

# 选择评论系统
comment_system = 'disqus'
# disqus 
disqus_shortname = 'catsup'
# 多说
duoshuo_shortname = 'catsup'

# 文章 url 是否包含日期
date_in_permalink = True

# 静态文件 url
static_url = '/static'
# 主题
theme_name = 'sealscript'
# Google Analytics ID
google_analytics = ''

#gzip = True

# 你的 Twitter 账号
twitter = 'whouz'
# 里的 Github 账号
github = 'whtsky'

# rss 地址
feed = 'feed.xml'
# 分页，每页显示多少篇文章
post_per_page = 3

# 友情链接
links = (
    ('whtsky', 'http://whouz.com', 'I write catsup'),
    ('catsup', 'https://github.com/whtsky/catsup', 'the source of this blog'),
)

```

参照如上配置，适当修改 config.py 即可

## 建立 _posts 仓库

使用 github 存储 posts 文件，去 github 新建一个仓库，命名为 _posts.复制仓库git地址，然后在 catsup 目录执行：

```bash
git clone /url/to/your/_posts.git
```

## 配置 webhook

在服务器上执行

```bash
nohup python catsup.py webhook --port=5555 &
```

或者使用 supervisord 管理 webhook，安装 supervisord 并修改配置文件 /etc/supervisord.conf，增加如下配置：

    [program:webhook]
    command=python /path/to/catsup.py webhook --port=5555
    user=your_username
    process_name=webhook
    numproc=1
    autostart=true
    autorestart=true

注意将其中的 /path/to/catsup.py 和 your_username 改成实际环境中的取值。

配置好 supervisord.conf 之后，执行 supervisord 启动 supervisord 守护进程或者执行 supervisorctl 后输入 reload 重新加载程序。你可以通过一些配置将 supervisord 设置为开机启动，具体操作方法请 Google 一下。

现在我们的 webhook 已经在运行之中了，假如你的网址是 <http://demo.com> ，那么你的 catsup webhook 地址便是 <http://demo.com:5555/webhook> 。打开你的 _posts 的 GitHub 项目地址，点击右边的 Admin ，打开后在左侧栏里找到 Service Hooks，点击 WebHook URLs，填写这个 Webhook 地址即可。

## 生成静态页面

你可以手动执行 `./catsup deploy` 或者 `python catsup.py deploy` 来生成静态页面，一般情况下不需要这么做，因为配置好 webhook 之后只要你的 _posts 仓库有更新，服务器会自动重新生成所有静态页面。

## Nginx 配置

为了让你的域名能正常访问catsup，添加Nginx配置文件catsup.conf

```nginx
server {
    listen 80;
    server_name demo.com;
    server_name blog.demo.com;
    index index.html index.htm;
    root /path/to/catsup/deploy/;
    access_log /path/to/access_log;
}
```

如上配置文件适当修改即可，弄好之后重启下 nginx：

```bash
/etc/init.d/nginx restart
# 有的系统上也可以如下
service nginx restart
```

## 本文参考

[Catsup部署教程](http://potatobox.org/2012-12-09-Catsup-Config.html)
