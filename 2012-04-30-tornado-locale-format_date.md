# tornado.locale中format_date的几点注意
- category: Python
- tags: tornado, python

---

tornado.locale中有个format_date方法，可以用来将python的datetime格式化为相对时间，即几分钟前、几小时前这种格式。通过在RequestHandler的子类中调用self.locale.format_date可实现。

format_date方法的源代码如下：

{% highlight python %}
def format_date(self, date, gmt_offset=0, relative=True, shorter=False,full_format=False):
    if self.code.startswith("ru"):
        relative = False
    if type(date) in (int, long, float):
        date = datetime.datetime.utcfromtimestamp(date)
    now = datetime.datetime.utcnow()
    if date > now:
        if relative and (date - now).seconds < 60:
            # Due to click skew, things are some things slightly
            # in the future. Round timestamps in the immediate
            # future down to now in relative mode.
            date = now
        else:
            # Otherwise, future dates always use the full format.
            full_format = True
    local_date = date - datetime.timedelta(minutes=gmt_offset)
    local_now = now - datetime.timedelta(minutes=gmt_offset)
    local_yesterday = local_now - datetime.timedelta(hours=24)
    difference = now - date
    seconds = difference.seconds
    days = difference.days

    _ = self.translate
    format = None
    if not full_format:
        if relative and days == 0:
            if seconds < 50:
                return _("1 second ago", "%(seconds)d seconds ago",
                    seconds) % {"seconds": seconds}

            if seconds < 50 * 60:
                minutes = round(seconds / 60.0)
                return _("1 minute ago", "%(minutes)d minutes ago",
                    minutes) % {"minutes": minutes}

            hours = round(seconds / (60.0 * 60))
            return _("1 hour ago", "%(hours)d hours ago",
                hours) % {"hours": hours}

        if days == 0:
            format = _("%(time)s")
        elif days == 1 and local_date.day == local_yesterday.day and \
            relative:
            format = _("yesterday") if shorter else \
                _("yesterday at %(time)s")
        elif days < 5:
            format = _("%(weekday)s") if shorter else \
                _("%(weekday)s at %(time)s")
        elif days < 334:  # 11mo, since confusing for same month last year
            format = _("%(month_name)s %(day)s") if shorter else \
                _("%(month_name)s %(day)s at %(time)s")

    if format is None:
        format = _("%(month_name)s %(day)s, %(year)s") if shorter else \
            _("%(month_name)s %(day)s, %(year)s at %(time)s")

    tfhour_clock = self.code not in ("en", "en_US", "zh_CN")
    if tfhour_clock:
        str_time = "%d:%02d" % (local_date.hour, local_date.minute)
    elif self.code == "zh_CN":
        str_time = "%s%d:%02d" % (
            (u'\u4e0a\u5348', u'\u4e0b\u5348')[local_date.hour >= 12],
            local_date.hour % 12 or 12, local_date.minute)
    else:
        str_time = "%d:%02d %s" % (
            local_date.hour % 12 or 12, local_date.minute,
            ("am", "pm")[local_date.hour >= 12])

    return format % {
        "month_name": self._months[local_date.month - 1],
        "weekday": self._weekdays[local_date.weekday()],
        "day": str(local_date.day),
        "year": str(local_date.year),
        "time": str_time
    }
{% endhighlight %}

由源代码可以看出这个方法使用是需要注意的一些东西：

1. 参数date应为UTC时间
2. 参数gm_offset为GMT时区的分钟数，且GMT+的要传入负值，因为它计算是用的是减号运算。比如UTC+8的就得是 format_date(date,-480)
3. 通过在csv/mo翻译文件中加入月份、星期等翻译可以实现本地化。

That is all.