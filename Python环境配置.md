# Python环境配置
* update-alternatives命令切换
* anaconda进行python包管理
* virtualenv进行虚拟环境创建
* docker容器介绍
# update-alternatives
我们可以使用 update-alternatives 来为整个系统更改 Python 版本。以 root 身份登录，首先罗列出所有可用的 python 替代版本信息：
```
update-alternatives --list python
update-alternatives: error:no alternatives for python
```
如果出现以上所示的错误信息，则表示 Python 的替代版本尚未被 update-alternatives 命令识别。想解决这个问题，我们需要更新一下替代列表，将 python2.7 和 python3.4 放入其中。
```
update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
update-alternatives:using/usr/bin/python2.7 to provide /usr/bin/python (python)inauto mode
update-alternatives --install /usr/bin/python python /usr/bin/python3.4 2
update-alternatives:using/usr/bin/python3.4 to provide /usr/bin/python (python)inauto mode
```
--install 选项使用了多个参数用于创建符号链接。最后一个参数指定了此选项的优先级，如果我们没有手动来设置替代选项，那么具有最高优先级的选项就会被选中。这个例子中，我们为 /usr/bin/python3.4 设置的优先级为2，所以 update-alternatives 命令会自动将它设置为默认 Python 版本。
```
python --version
Python3.4.2
```
接下来，我们再次列出可用的 Python 替代版本。
```
update-alternatives --list python
/usr/bin/python2.7
/usr/bin/python3.4
```
现在开始，我们就可以使用下方的命令随时在列出的 Python 替代版本中任意切换了。
```
update-alternatives --config python
# python --version
Python2.7.8
```
移除替代版本
一旦我们的系统中不再存在某个 Python 的替代版本时，我们可以将其从 update-alternatives 列表中删除掉。例如，我们可以将列表中的 python2.7 版本移除掉。
```
# update-alternatives --remove python /usr/bin/python2.7
update-alternatives: removing manually selected alternative - switching python to auto mode
update-alternatives:using/usr/bin/python3.4 to provide /usr/bin/python (python)inauto mode
```

# anaconda的使用(recommmend)
参考网站：
* https://www.jianshu.com/p/62f155eb6ac5
* http://python.jobbole.com/87522/
## 1.下载anaconda
下载链接：https://www.continuum.io/downloads#linux

Anaconda和Python版本是对应的，所以需要选择安装对应Python2.7版本的还是Python3.6版本的，根据自己的需要下载合适的安装包。**如果ubuntu系统是64bit，记得下载64bit的。**

## 2.安装anaconda
其实安装方式很简单，官网的下载页面也给出了安装命令。下载好的文件在Downloads文件夹下面，所以打开终端执行下列命令：
```
cd Downloads
bash Anaconda2-4.4.0-Linux-x86_64.sh
```
之后会出现欢迎信息，告诉你要阅读许可文件：根据提示按回车键阅读，注意按一次回车之后左下角会显示一个“--More--”，意思是许可信息还没显示完，一直按回车，知道最后许可信息显示完出现下面提示：

问你是否接受许可文件，输入yes继续安装即可。

之后就提示你要将Anaconda安装在目录/home/yourname/anaconda2下面：（建议使用此目录）

直接按回车键表示使用此目录，此时就进行安装过程了，等待安装完之后会询问是否把anaconda的bin添加到用户的环境变量中，选择yes.

## 3.检测anaconda是否安装成功

上面的安装过程执行完成之后关闭那个终端，重新打开一个，在终端输入“python”，如果出现下面的信息，说明安装成功：
```
lqh@lqh-Inspiron-5577:~$ python -V
Python 3.6.5 :: Anaconda, Inc.
lqh@lqh-Inspiron-5577:~$ python
Python 3.6.5 |Anaconda, Inc.| (default, Apr 29 2018, 16:14:56)
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
```
介绍了python的版本信息，但是后面带了anaconda的标识，这就说明Anaconda安装成功了，此时输入exit()退出python环境，输入：
```
conda list
lqh@lqh-Inspiron-5577:~$ conda list
# packages in environment at /home/lqh/anaconda3:
#
# Name                    Version                   Build  Channel
_ipyw_jlab_nb_ext_conf    0.1.0            py36he11e457_0  
alabaster                 0.7.10           py36h306e16b_0  
anaconda                  5.2.0                    py36_3  
anaconda-client           1.6.14                   py36_0  
anaconda-navigator        1.8.7                    py36_0  
anaconda-project          0.8.2            py36h44fb852_0  
asn1crypto                0.24.0                   py36_0  
astroid                   1.6.3                    py36_0  
astropy                   3.0.2            py36h3010b51_1  
attrs                     18.1.0                   py36_0  
babel                     2.5.3                    py36_0  
backcall                  0.1.0                    py36_0  
backports                 1.0              py36hfa02d7e_1  
backports.shutil_get_terminal_size 1.0.0            py36hfea85ff_2  
beautifulsoup4            4.6.0            py36h49b8c8c_1  
bitarray                  0.8.1            py36h14c3975_1  
bkcharts                  0.2              py36h735825a_0  
blas                      1.0                         mkl  
blaze                     0.11.3           py36h4e06776_0  
bleach                    2.1.3                    py36_0  
blosc                     1.14.3               hdbcaa40_0  
bokeh                     0.12.16                  py36_0  
boto                      2.48.0           py36h6e4cd66_1  
bottleneck                1.2.1            py36haac1ea0_0  
bzip2                     1.0.6                h14c3975_5
...
```
会显示一大堆可用的packages，说明我们的安装是没有问题的。

## 4.anaconda的使用
1.创建新环境
```
conda create --name <env_name> <package_names>
```
注意：<env_name>即创建的环境名。建议以英文命名，且不加空格，名称两边不加尖括号“<>”。<package_names>即安装在环境中的包名。名称两边不加尖括号“<>”。

如果要安装指定的版本号，则只需要在包名后面以=和版本号的形式执行。如：
```
conda create --name python2 python=2.7
```
即创建一个名为“python2”的环境，环境中安装版本为2.7的python。

如果要在新创建的环境中创建多个包，则直接在<package_names>后以空格隔开，添加多个包名即可。如：c
```
onda create -n python3 python=3.5 numpy pandas
```
即创建一个名为“python3”的环境，环境中安装版本为3.5的python，同时也安装了numpy和pandas。--name同样可以替换为-n。

**提示：默认情况下，新创建的环境将会被保存在/Users/<user_name>/anaconda3/env目录下，其中，<user_name>为当前用户的用户名。**

2.切换环境
① Linux 或 macOS
```
source activate <env_name>
```
② Windows
```
activate <env_name>
```
③ 提示
如果创建环境后安装Python时没有指定Python的版本，那么将会安装与Anaconda版本相同的Python版本，即如果安装Anaconda第2版，则会自动安装Python 2.x；如果安装Anaconda第3版，则会自动安装Python 3.x。

当成功切换环境之后，在该行行首将以“(env_name)”或“[env_name]”开头。其中，“env_name”为切换到的环境名。如：在macOS系统中执行source active python2，即切换至名为“python2”的环境，则行首将会以(python2)开头。

3.退出环境至root
① Linux 或 macOS
```
source deactivate
```
② Windows
```
deactivate
```
③ 提示
当执行退出当前环境，回到root环境命令后，原本行首以“(env_name)”或“[env_name]”开头的字符将不再显示。

4.显示已创建环境
```
conda info --envs
或
conda info -e
或
conda env list
```
5.复制环境
```
conda create --name <new_env_name> --clone <copied_env_name>
```
注意：<copied_env_name>即为被复制/克隆环境名。环境名两边不加尖括号“<>”。<new_env_name>即为复制之后新环境的名称。环境名两边不加尖括号“<>”。如：
```
conda create --name py2 --clone python2
```
即为克隆名为“python2”的环境，克隆后的新环境名为“py2”。此时，环境中将同时存在“python2”和“py2”环境，且两个环境的配置相同。

6.删除环境
```
conda remove --name <env_name> --all
```
注意：<env_name>为被删除环境的名称。环境名两边不加尖括号“<>”。
7.管理包

1. 查找可供安装的包版本

① 精确查找
```
conda search --full-name <package_full_name>
```
注意：--full-name为精确查找的参数。<package_full_name>是被查找包的全名。包名两边不加尖括号“<>”。例如：
```
conda search --full-name python即查找全名为“python”的包有哪些版本可供安装。
```

② 模糊查找
```
conda search <text>
```
注意：<text>是查找含有此字段的包名。此字段两边不加尖括号“<>”。例如：
```
conda search py即查找含有“py”字段的包，有哪些版本可供安装。
```

2. 获取当前环境中已安装的包信息

```
conda list
```
执行上述命令后将在终端显示当前环境已安装包的包名及其版本号。

3. 安装包

① 在指定环境中安装包
```
conda install --name <env_name> <package_name>
```
注意：<env_name>即将包安装的指定环境名。环境名两边不加尖括号“<>”。<package_name>即要安装的包名。包名两边不加尖括号“<>”。例如：
```
conda install --name python2 pandas即在名为“python2”的环境中安装pandas包。
```
② 在当前环境中安装包
```
conda install <package_name>
```
注意：<package_name>即要安装的包名。包名两边不加尖括号“<>”。执行命令后在当前环境中安装包。例如：

```
conda install pandas即在当前环境中安装pandas包。
```

③ 使用pip安装包
使用场景
当使用conda install无法进行安装时，可以使用pip进行安装。例如：see包。

命令
```
pip install <package_name>
```
注意：<package_name>为指定安装包的名称。包名两边不加尖括号“<>”。如：
```
pip install see即安装see包。
```
注意：
pip只是包管理器，无法对环境进行管理。

因此如果想在指定环境中使用pip进行安装包，则需要先切换到指定环境中，再使用pip命令安装包。

pip无法更新python，因为pip并不将python视为包。

pip可以安装一些conda无法安装的包；conda也可以安装一些pip无法安装的包。因此当使用一种命令无法安装包时，可以尝试用另一种命令。

4. 卸载包

① 卸载指定环境中的包
```
conda remove --name <env_name> <package_name>
```
注意：

<env_name>即卸载包所在指定环境的名称。环境名两边不加尖括号“<>”。

<package_name>即要卸载包的名称。包名两边不加尖括号“<>”。

例如：
```
conda remove --name python2 pandas即卸载名为“python2”中的pandas包。
```

② 卸载当前环境中的包
conda remove <package_name>
注意：

<package_name>即要卸载包的名称。包名两边不加尖括号“<>”。

执行命令后即在当前环境中卸载指定包。

例如：
```
conda remove pandas即在当前环境中卸载pandas包。
```

5. 更新包

① 更新所有包
```
conda update --all
或
conda upgrade --all
```

建议：在安装Anaconda之后执行上述命令更新Anaconda中的所有包至最新版本，便于使用。

② 更新指定包
```
conda update <package_name>
或
conda upgrade <package_name>
```

注意：
<package_name>为指定更新的包名。包名两边不加尖括号“<>”。

更新多个指定包，则包名以空格隔开，向后排列。如：conda update pandas numpy matplotlib即更新pandas、numpy、matplotlib包。


# virtualenv的使用--[推荐网站](http://www.cnblogs.com/freely/p/8022923.html)

virtualenv------用来建立一个虚拟的python环境，一个专属于项目的python环境。用virtualenv来保持一个干净的环境非常有用

测试环境：linux下

**1.基本使用**

通过pip安装virtualenv：
```
pip install virtualenv
```
测试安装:
```
virtualenv --version
```
为一个工程项目搭建一个虚拟环境:
```
cd my_project
virtualenv my_project_env
```
另外，如果存在多个python解释器，可以选择指定一个Python解释器（比如``python2.7``），没有指定则由系统默认的解释器来搭建：
```
virtualenv -p /usr/bin/python2.7 my_project_env
```
将会在当前的目录中创建一个名my_project_env的文件夹，这是一个独立的python运行环境，包含了Python可执行文件， 以及 pip 库的一份拷贝，这样就能安装其他包了，不过已经安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境来。

要开始使用虚拟环境，其需要被激活：
```
source my_project_env/bin/activate
```
停用虚拟环境：
```
deactivate
```
停用后将回到系统默认的Python解释器

**2.其他**

用pip freeze查看当前安装版本
```
pip freeze
```
另外：
```
pip freeze > requirements.txt
```
这将会创建一个 requirements.txt 文件，其中包含了当前环境中所有包及 各自的版本的简单列表。您可以使用 “pip list”在不产生requirements文件的情况下， 查看已安装包的列表。这将会使另一个不同的开发者（或者是您，如果您需要重新创建这样的环境） 在以后安装相同版本的相同包变得容易。
```
pip install -r requirements.txt
```
这能帮助确保安装、部署和开发者之间的一致性。

## virtualenvwrapper

提供了一系列命令使得和虚拟环境工作变得愉快许多。它把您所有的虚拟环境都放在一个地方。

将您的所有虚拟环境在一个地方。

包装用于管理虚拟环境（创建，删除，复制）。

使用一个命令来环境之间进行切换。

1、安装

安装（确保 virtualenv 已经安装了）：
```
pip install virtualenvwrapper
export WORKON_HOME=~/Envs  #设置环境变量
mkdir -p $WORKON_HOME #创建虚拟环境管理目录
find / -name virtualenvwrapper.sh #找到virtualenvwrapper.sh的路径
source 路径 #激活virtualenvwrapper.sh
```
默认virtualenvwrapper安装在下面python解释器中的site-packages，实际上需要运行virtualenvwrapper.sh文件才行；所以需要先进行配置一下：
```
找到virtualenvwrapper.sh的路径：find / -name virtualenvwrapper.sh
运行virtualenvwrapper.sh文件：source 路径
```
ps：每次要想使用virtualenvwrapper 工具时，都必须先激活virtualenvwrapper.sh,另外，如果创建前要将即将的环境保存到Envs中，就要先设置一下环境变量：export WORKON_HOME=~/Envs，再搭建



对于Windows，您可以使用 virtualenvwrapper-win

安装（确保 virtualenv 已经安装了）：
```
pip install virtualenvwrapper-win
```
在Windows中，WORKON_HOME默认的路径是 %USERPROFILE%Envs 。

2、基本使用

1、创建一个虚拟环境：
```
mkvirtualenv project_env
```
这会在Envs 中创建 project_env虚拟环境

选择一个python解释器来搭建：
```
mkvirtualenv env  --python=python2.7
```

2、在虚拟环境上工作：
```
workon project_env
```
或者，您可以创建一个项目，它会创建虚拟环境，并在 $WORKON_HOME 中创建一个项目目录。 当您使用 workon project_env 时，会 cd -ed 到项目目录中。
```
mkproject project_env
```
virtualenvwrapper 提供环境名字的tab补全功能。当您有很多环境， 并且很难记住它们的名字时，这就显得很有用。

workon 也能停止您当前所在的环境，所以您可以在环境之间快速的切换。

3、停止虚拟环境
```
deactivate
```
4、删除：
```
 rmvirtualenv project_env
```
5、其他有用的命令
复制代码
```
lsvirtualenv    #列举所有的环境。
cdvirtualenv    #导航到当前激活的虚拟环境的目录中，比如说这样您就能够浏览它的 site-packages。
cdsitepackages   # 和上面的类似，但是是直接进入到 site-packages 目录中。
lssitepackages     #显示 site-packages 目录中的内容
virtualenvwrapper 命令的完全列表 。
```
# docker容器介绍

该部分后续有时间再补充
