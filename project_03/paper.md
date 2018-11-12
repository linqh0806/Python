# urllib.error异常

# 一.urllib.error

urllib.error可以接收有urllib.request产生的异常。urllib.error有两个方法，URLError和HTTPError。

* urllib.error.URLError

让我们先看下URLError的异常，编写如下代码：
```
# -*- coding: UTF-8 -*-
from urllib import request
from urllib import error

if __name__ == "__main__":
    #一个不存在的连接
    url = "http://www.iloveyou.com/"
    req = request.Request(url)
    try:
        response = request.urlopen(req)
        html = response.read().decode('utf-8')
        print(html)
    except error.URLError as e:
        print(e.reason)
```

我们可以看到如下运行结果:
```
(learn) lqh@lqh-Inspiron-5577:~/git/Python/project_03$ python urllib_project01.py
[Errno -2] Name or service not known
```
* urllib.error.HTTPError

再看下HTTPError异常，编写如下代码：
```
# -*- coding: UTF-8 -*-
from urllib import request
from urllib import error

if __name__ == "__main__":
    #一个不存在的连接
    url = "http://www.douyu.com/Jack_Cui.html"
    req = request.Request(url)
    try:
        responese = request.urlopen(req)
        # html = responese.read()
    except error.HTTPError as e:
        print(e.code)
```
运行之后，我们可以看到404，这说明请求的资源没有在服务器上找到，www.douyu.com这个服务器是存在的，但是我们要查找的Jack_Cui.html资源是没有的，所以抛出404异常。

# 二.URLError和HTTPError混合使用

最后值得注意的一点是，如果想用HTTPError和URLError一起捕获异常，那么需要将HTTPError放在URLError的前面，因为HTTPError是URLError的一个子类。如果URLError放在前面，出现HTTP异常会先响应URLError，这样HTTPError就捕获不到错误信息了。

如果不用上面的方法，也可以使用hasattr函数判断URLError含有的属性，如果含有reason属性表明是URLError，如果含有code属性表明是HTTPError。编写代码如下：
```
# -*- coding: UTF-8 -*-
from urllib import request
from urllib import error

if __name__ == "__main__":
    #一个不存在的连接
    url = "http://www.douyu.com/Jack_Cui.html"
    req = request.Request(url)
    try:
        responese = request.urlopen(req)
    except error.URLError as e:
        if hasattr(e, 'code')
            print("HTTPError")
            print(e.code)
        elif hasattr(e, 'reason')
            print("URLError")
            print(e.reason)
```
