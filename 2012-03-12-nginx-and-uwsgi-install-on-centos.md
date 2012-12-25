# CentOS配置nginx+uwsgi环境
- category: Python
- tags: python

---

用Python做web应用最麻烦的就是配置服务器环境了，它不像PHP那样简单配置下再把.php文件扔进web目录就可以了，而且当前配置python web环境的方法也有很多种，对于新手而言实在是折腾得难过。这里记录一下我在CentOS上编译、配置的nginx+uwsgi环境的情况。

## 准备

先更新系统，并安装编译环境等等。

{% highlight bash %}
yum update
yum install python python-devel libxml2 libxml2-devel python-setuptools zlib-devel wget openssl-devel pcre pcre-devel sudo gcc make autoconf automake
{% endhighlight %}

## 编译安装uwsgi

uwsgi的官方网站是[http://projects.unbit.it/uwsgi/](http://projects.unbit.it/uwsgi/)，这里我们下载它的当前稳定版本。

{% highlight bash %}
wget http://projects.unbit.it/downloads/uwsgi-1.0.4.tar.gz
tar -zxvf uwsgi-1.0.4.tar.gz # 解压
mv uwsgi-1.0.4 uwsgi # 重命名为uwsgi，仅仅为了方便
cd uwsgi # 切换到uwsgi目录
python setup.py build # 编译安装
make
{% endhighlight %}
下面将编译产生的可执行文件移动到/usr/bin里面去

{% highlight bash %}
mv uwsgi /usr/bin
{% endhighlight %}

## 编译安装nginx

nginx的官方网站是[http://nginx.org](http://nginx.org)，这里我们依然下载它的稳定版。

{% highlight bash %}
cd ~
wget http://nginx.org/download/nginx-1.0.13.tar.gz
tar -zxvf nginx.1.0.13.tar.gz
mv nginx-1.0.13 nginx
cd nginx
./configure --prefix=/usr/local/nginx # 编译选项配置，这里从简
make && make install # 编译安装
{% endhighlight %}

我们再将生成的nginx可执行文件在/usr/sbin里建立软链接

{% highlight bash %}
ln -s /usr/local/nginx/sbin/nginx /usr/sbin/nginx
{% endhighlight %}

## 配置nginx

使用vi /usr/local/nginx/conf/nginx.conf打开nginx配置文件，将以下内容加入/修改到server里(至于vi/vim的使用，呃，自己学吧)：

>location /{
>    uwsgi_pass 127.0.0.1:9001;
>    include uwsgi_params;
>}

ESC，输入:wq保存并退出。

## 测试

这里以 [Flask](http://flask.pocoo.org) 为例，安装Flask的简单方法是(前提是已安装了pip)：

    pip install flask


为了方便，这里在/usr/share里新建文件myapp.py

{% highlight python %}
from flask import Flask

app = Flask(__name__)
app.debug = True

@app.route('/')
def index():
    return 'Hello World!'
{% endhighlight %}

然后我们启动nginx和uwsgi：

{% highlight bash %}
nginx
cd /usr/share
uwsgi -s 127.0.0.1:9001 -w myapp:app -d /var/log/uwsgi.log
{% endhighlight %}

接着浏览器打开localhost，如果看到大大的邪恶的Hello World!即表示配置成功！