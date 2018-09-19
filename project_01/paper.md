## 一、网络爬虫的定义

网络爬虫，也叫网络蜘蛛(Web Spider)，如果把互联网比喻成一个蜘蛛网，Spider就是一只在网上爬来爬去的蜘蛛。网络爬虫就是根据网页的地址来寻找网页的，也就是URL。举一个简单的例子，我们在浏览器的地址栏中输入的字符串就是URL，例如：https://www.baidu.com/

URL就是同意资源定位符(Uniform Resource Locator)，它的一般格式如下(带方括号[]的为可选项)：

```
protocol :// hostname[:port] / path / [;parameters][?query]#fragment
```

URL的格式由三部分组成：

(1)protocol：第一部分就是协议，例如百度使用的就是https协议；

(2)hostname[:port]：第二部分就是主机名(还有端口号为可选参数)，一般网站默认的端口号为80，例如百度的主机名就是www.baidu.com，这个就是服务器的地址;

(3)path：第三部分就是主机资源的具体地址，如目录和文件名等。

网络爬虫就是根据这个URL来获取网页信息的。

## 二、简单爬虫实例

在Python3.x中，我们可以使用urlib这个组件抓取网页，urllib是一个URL处理包，这个包中集合了一些处理URL的模块，如下：

1.urllib.request模块是用来打开和读取URLs的；

2.urllib.error模块包含一些有urllib.request产生的错误，可以使用try进行捕捉处理；

3.urllib.parse模块包含了一些解析URLs的方法；

4.urllib.robotparser模块用来解析robots.txt文本文件.它提供了一个单独的RobotFileParser类，通过该类提供的can_fetch()方法测试爬虫是否可以下载一个页面。

我们使用urllib.request.urlopen()这个接口函数就可以很轻松的打开一个网站，读取并打印信息。


urlopen有一些可选参数，具体信息可以查阅Python自带的documentation。

了解到这些，我们就可以写一个最简单的程序，文件名为urllib_test01.py，感受一个urllib库的魅力：

```
# -*- coding: UTF-8 -*-
from urllib import request

if __name__ == "__main__":
    response = request.urlopen("http://fanyi.baidu.com")
    html = response.read()
    print(html)
```

urllib使用使用request.urlopen()打开和读取URLs信息，返回的对象response如同一个文本对象，我们可以调用read()，进行读取。再通过print()，将读到的信息打印出来。

虽然我们已经成功获取了信息，但是显然他们都是二进制的乱码，看起来很不方便。我们怎么办呢？

我们可以通过简单的decode()命令将网页的信息进行解码，并显示出来，我们新创建一个文件，命名为urllib_test02.py，编写如下代码(还是以百度翻译网站fanyi.baidu.com为例)：

```
# -*- coding: UTF-8 -*-
from urllib import request

if __name__ == "__main__":
    response = request.urlopen("http://www.fanyi.baidu.com/")
    html = response.read()
    html = html.decode("utf-8")
    print(html)
```
这样我们就可以得到这样的结果，显然解码后的信息看起来工整和舒服多了。

## 三、自动获取网页编码方式的方法

获取网页编码的方式有很多，个人更喜欢用第三方库的方式。

首先我们需要安装第三方库chardet，它是用来判断编码的模块，安装方法如下图所示，只需要输入指令：
```
pip install chardet
```

安装好后，我们就可以使用chardet.detect()方法，判断网页的编码方式了。至此，我们就可以编写一个小程序判断网页的编码方式了，新建文件名为chardet_test01.py：

```
# -*- coding: UTF-8 -*-
from urllib import request
import chardet

if __name__ == "__main__":
    response = request.urlopen("http://fanyi.baidu.com/")
    html = response.read()
    charset = chardet.detect(html)
    print(charset)
```    
运行程序，查看输出结果如下：
```
{‘confidence’:0.99,'encoding':'utf-8'}
[Finished in 0.7s]
```

瞧，返回的是一个字典，这样我们就知道网页的编码方式了，通过获得的信息，采用不同的解码方式即可
