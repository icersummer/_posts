# 开源YaBlog
- category: Python
- tags: tornado,  python

---

虽然几乎还是个半成品，不过已经把源代码托管到Github上去了。

Fork it: [https://github.com/messense/YaBlog](https://github.com/messense/YaBlog "YaBlog on Github")

### 使用到的技术

1. Python(2.7+)
2. Tornado
3. memcached
4. SQLAlchemy
5. MySQL

后台全部是基于RESTful架构的，所以全部操作都是基于ajax的。

注意，此代码只是合个人学习和折腾，并不太适合新手使用。

推荐使用supervisior进行部署，建议阅读：[http://www.idndx.com/posts/ways-to-deploy-tornado-under-production-environment-using-supervisor.html](http://www.idndx.com/posts/ways-to-deploy-tornado-under-production-environment-using-supervisor.html)