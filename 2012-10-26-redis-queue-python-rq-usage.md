# 简单任务队列 python-rq 使用
- category: Python
- tags: python

---

RQ (Redis Queue) is a simple Python library for queueing jobs and processing them in the background with workers. It is backed by Redis and it is designed to have a low barrier to entry. It should be integrated in your web stack easily.

官网：[http://python-rq.org](http://python-rq.org)

## 安装

    pip install rq

## 任务队列守护队列(task_server.py)

{% highlight python %}
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from __future__ import with_statement
import os
import redis
from rq import Worker
from rq import Queue
from rq import Connection

listen = ['high', 'default', 'low']

redis_url = os.getenv('REDISTOGO_URL', 'redis://localhost:6379')

conn = redis.from_url(redis_url)

if __name__ == '__main__':
    with Connection(conn):
        worker = Worker(map(Queue, listen))
        worker.work()
{% endhighlight %}

使用 `./task_server.py` 或者 `python task_server.py` 启动 worker

## 任务示例

{% highlight python %}
import requests

def count_words_at_url(url):
    resp = requests.get(url)
    return len(resp.text.split())
{% endhighlight %}

## 添加任务

{% highlight python %}
# -*- coding:utf-8 -*-
from rq import Queue
from rq import use_connection

redis_url = os.getenv('REDISTOGO_URL', 'redis://localhost:6379')
conn = redis.from_url(redis_url)
use_connection(conn)

task_queue=Queue()

task_queue.enqueue(count_words_at_url, 'http://google.com')
{% endhighlight %}

## Tips

可以将如下代码独立成一个 .py 文件，如 client.py：

{% highlight python %}
# -*- coding:utf-8 -*-
from rq import Queue
from rq import use_connection

redis_url = os.getenv('REDISTOGO_URL', 'redis://localhost:6379')
conn = redis.from_url(redis_url)
use_connection(conn)

task_queue=Queue()
{% endhighlight %}

然后在需要添加任务的时候 import 进来，调用 `task_queue.enqueue(task,params)` 即可

