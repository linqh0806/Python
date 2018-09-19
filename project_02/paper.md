
# 学习网站,膜拜大佬：http://blog.csdn.net/c406495762/article/details/59095864

# 一、urlopen的url参数 Agent

url不仅可以是一个字符串，例如:http://www.baidu.com。

url也可以是一个Request对象，这就需要我们先定义一个Request对象，然后将这个Request对象作为urlopen的参数使用，方法如下：
```
# -*- coding: UTF-8 -*-
from urllib import request

if __name__ == "__main__":
    req = request.Request("http://fanyi.baidu.com/")
    response = request.urlopen(req)
    html = response.read()
    html = html.decode("utf-8")
    print(html)
```

同样，运行这段代码同样可以得到网页信息。可以看一下这段代码和上个笔记中代码的不同，对比一下就明白了。

urlopen()返回的对象，可以使用read()进行读取，同样也可以使用geturl()方法、info()方法、getcode()方法。



* geturl()返回的是一个url的字符串；

* info()返回的是一些meta标记的元信息，包括一些服务器的信息；

* getcode()返回的是HTTP的状态码，如果返回200表示请求成功。

关于META标签和HTTP状态码的内容可以自行百度百科，里面有很详细的介绍。


* META标签


通常所说的META标签，是在HTML网页源代码中一个重要的html标签。META标签用来描述一个HTML网页文档的属性，例如作者、日期和时间、网页描述、关键词、页面刷新等。

META标签是HTML标记HEAD区的一个关键标签，它位于HTML文档的head和title之间（有些也不是在head和title之间）。它提供的信息虽然用户不可见，但却是文档的最基本的元信息。<meta>除了提供文档字符集、使用语言、作者等基本信息外，还涉及对关键词和网页等级的设定。


所以有关搜索引擎注册、搜索引擎优化排名等网络营销方法内容中，通常都要谈论META标签的作用，我们甚至可以说，META标签的内容设计对于搜索引擎营销来说是至关重要的一个因素，合理利用 Meta 标签的 Description 和Keywords 属性，加入网站的关键字或者网页的关键字，可使网站更加贴近用户体验。

从HTML代码实例中可以看到，一段代码中有3个含有meta的地方，并且meta并不是独立存在的，而是要在后面连接其他的属性，如description、Keywords、http-equiv等。下面简单介绍一些搜索引擎营销中常见的META标签的组成及其作用。

* HTTP状态码

HTTP状态码（HTTP Status Code）是用以表示网页服务器HTTP响应状态的3位数字代码。它由 RFC 2616 规范定义的，并得到RFC 2518、RFC 2817、RFC 2295、RFC 2774、RFC 4918等规范扩展。

中文名 HTTP状态码

外文名 HTTP Status Code

规范定义 RFC 2616

消息端 1字头，2字头，3字头

1 消息
▪ 100 Continue
▪ 101 Switching Protocols
▪ 102 Processing

2 成功
▪ 200 OK
▪ 201 Created
▪ 202 Accepted
▪ 203 Non-Authoritative Information
▪ 204 No Content
▪ 205 Reset Content
▪ 206 Partial Content
▪ 207 Multi-Status

3 重定向
▪ 300 Multiple Choices
▪ 301 Moved Permanently
▪ 302 Move temporarily
▪ 303 See Other
▪ 304 Not Modified
▪ 305 Use Proxy
▪ 306 Switch Proxy
▪ 307 Temporary Redirect

4 请求错误
▪ 400 Bad Request
▪ 401 Unauthorized
▪ 402 Payment Required
▪ 403 Forbidden
▪ 404 Not Found
▪ 405 Method Not Allowed
▪ 406 Not Acceptable
▪ 407 Proxy Authentication Required
▪ 408 Request Timeout
▪ 409 Conflict
▪ 410 Gone
▪ 411 Length Required
▪ 412 Precondition Failed
▪ 413 Request Entity Too Large
▪ 414 Request-URI Too Long
▪ 415 Unsupported Media Type
▪ 416 Requested Range Not Satisfiable
▪ 417 Expectation Failed
▪ 421 too many connections
▪ 422 Unprocessable Entity
▪ 423 Locked
▪ 424 Failed Dependency
▪ 425 Unordered Collection
▪ 426 Upgrade Required
▪ 449 Retry With
▪ 451Unavailable For Legal Reasons

5 服务器错误
▪ 500 Internal Server Error
▪ 501 Not Implemented
▪ 502 Bad Gateway
▪ 503 Service Unavailable
▪ 504 Gateway Timeout
▪ 505 HTTP Version Not Supported
▪ 506 Variant Also Negotiates
▪ 507 Insufficient Storage
▪ 509 Bandwidth Limit Exceeded
▪ 510 Not Extended
▪ 600 Unparseable Response Headers

了解到这些，我们就可以进行新一轮的测试，新建文件名urllib_test04.py，编写如下代码：
```
# -*- coding: UTF-8 -*-
from urllib import request

if __name__ == "__main__":
    req = request.Request("http://fanyi.baidu.com/")
    response = request.urlopen(req)
    print("geturl打印信息：%s"%(response.geturl()))
    print('**********************************************')
    print("info打印信息：%s"%(response.info()))
    print('**********************************************')
    print("getcode打印信息：%s"%(response.getcode()))
```

可以得到如下运行结果：

```
(learn) lqh@lqh-Inspiron-5577:~/git/Python/project_02$ python urllib_project01.py
geturl打印信息：https://fanyi.baidu.com/
**********************************************
info打印信息：Content-Type: text/html
Date: Wed, 19 Sep 2018 09:37:27 GMT
P3p: CP=" OTI DSP COR IVA OUR IND COM "
Server: Apache
Set-Cookie: locale=zh; expires=Tue, 16-Jul-2019 09:37:27 GMT; path=/; domain=.baidu.com
Set-Cookie: BAIDUID=2B47C70C9D0D023492A84581F320F21D:FG=1; expires=Thu, 19-Sep-19 09:37:27 GMT; max-age=31536000; path=/; domain=.baidu.com; version=1
Vary: Accept-Encoding
Connection: close
Transfer-Encoding: chunked


**********************************************
getcode打印信息：200

```

# 二、urlopen的data参数

我们可以使用data参数，向服务器发送数据。根据HTTP规范，GET用于信息获取，POST是向服务器提交数据的一种请求，再换句话说：

* 从客户端向服务器提交数据使用POST；

* 从服务器获得数据到客户端使用GET(GET也可以提交，暂不考虑)。

如果没有设置urlopen()函数的data参数，HTTP请求采用GET方式，也就是我们从服务器获取信息，如果我们设置data参数，HTTP请求采用POST方式，也就是我们向服务器传递数据。

data参数有自己的格式，它是一个基于application/x-www.form-urlencoded的格式，具体格式我们不用了解， 因为我们可以使用urllib.parse.urlencode()函数将字符串自动转换成上面所说的格式。

# 三、发送data实例

向有道翻译发送data，得到翻译结果。

1.打开有道翻译界面。

2.鼠标右键检查（F12），也就是审查元素。

3.选择右侧出现的Network。

4.在左侧输入翻译内容，输入Jack。

5.点击自动翻译按钮，我们就可以看到右侧出现的内容。

6.点击上图红框中的内容，查看它的信息，如下所示：

```
From Data

i: Jack
from: AUTO
to: AUTO
smartresult: dict
client: fanyideskweb
salt: 1537346833601
sign: 52e8a1d10c63bfaa2ec7af03145bdb37
doctype: json
version: 2.1
keyfrom: fanyi.web
action: FY_BY_CLICKBUTTION
typoResult: false
```

7.记住这些信息，这是我们一会儿写程序需要用到的。

新建文件translate_test.py，编写如下代码：

```
# -*- coding: UTF-8 -*-
from urllib import request
from urllib import parse
import json

if __name__ == "__main__":
    #对应上图的Request URL
    Request_URL = 'http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule&smartresult=ugc&sessionFrom=https://www.baidu.com/link'
    #创建Form_Data字典，存储上图的Form Data
    Form_Data = {}
    Form_Data['i'] = 'Jack'
    Form_Data['from'] = 'AUTO'
    Form_Data['to'] = 'AUTO'
    Form_Data['smartresult'] = 'dict'
    Form_Data['client'] = 'fanyideskweb'
    Form_Data['doctype'] = 'json'
    Form_Data['version'] = '2.1'
    Form_Data['keyfrom'] = 'fanyi.web'
    Form_Data['action'] = 'FY_BY_CLICKBUTTON'
    #使用urlencode方法转换标准格式
    data = parse.urlencode(Form_Data).encode('utf-8')
    #传递Request对象和转换完格式的数据
    response = request.urlopen(Request_URL,data)
    #读取信息并解码
    html = response.read().decode('utf-8')
    #使用JSON
    translate_results = json.loads(html)
    #找到翻译结果
    translate_results = translate_results['translateResult'][0][0]['tgt']
    #打印翻译信息
    print("翻译的结果是：%s" % translate_results)
```

这样我们就可以查看翻译的结果了，如下图所示：

```
(learn) lqh@lqh-Inspiron-5577:~/git/Python/project_02$ python urllib_project02.py
翻译的结果是：{'type': 'EN2ZH_CN', 'errorCode': 0, 'elapsedTime': 0, 'translateResult': [[{'src': 'Jack', 'tgt': '杰克'}]]}
```

JSON是一种轻量级的数据交换格式，我们需要从爬取到的内容中找到JSON格式的数据，这里面保存着我们想要的翻译结果，再将得到的JSON格式的翻译结果进行解析，得到我们最终想要的样子：杰克。
