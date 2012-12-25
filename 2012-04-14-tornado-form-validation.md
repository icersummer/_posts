# tornado表单验证
- category: Python
- tags: python, tornado

---

最近在学习Python web框架tornado的使用，话说tornado的RequestHandler类的get_argument方法可以获得get/post传过来的值，要做表单验证的话，python也有其它的专门的类库。不过我的要求比较简单，就没必要动用那些库了，于是自己简单弄了个功能简单的表单验证。

整个思路就是让RequestHandler在调用self.render_string的时候自动注册一个显示表单验证提示的模板函数.上代码吧：

{% highlight python %}
from tornado.web import RequestHandler

class ObjectDict(object):

    def __getattr__(self, key):
        if key in self:
            return self[key]
        return None

    def __setattr__(self, key ,value):
        self[key] = value

class BaseHandler(RequestHandler):

    def prepare(self):
        self._prepare_context()
        self._prepare_filters()

    def _prepare_context(self):
        self._context = ObjectDict()
        self._context.form = []

    def _prepare_filters(self):
        self._filters = ObjectDict()
        self._filters.form_error = self._form_error

    def _form_error(self, field, msg, error_class='form-error'):
        if field and hasattr(self._context.form, field):
            ret = self.locale.translate(getattr(self._context.form, field))
            if ret:
                return '<span class="' + error_class + '">' + ret + '</span>'
        return ''
    
    def form_error(self, key, msg):
        setattr(self._context.form, field, msg)

    def render_string(self, template_name, **kwargs):
        kwargs.update(self._filters) # 注册模板函数
        assert "context" not in kwargs, "context is a reserved keyword."
        kwargs["context"] = self._context
        return super(BaseHandler, self).render_string(template_name,**kwargs)
{% endhighlight %}

之后在定义的其它handlers的时候只要继承 BaseHandler ，当表单的某个选项验证出错的时候即可调用 self.form_error(field,msg) 来注册验证错误信息，比如 username 选项不能为空，则可以调用 self.form_error('username','Username can not be empty') ，然后在模板中用 {{ form_error('username') }} 来显示信息，并且不影响tornado的多语言国际化。