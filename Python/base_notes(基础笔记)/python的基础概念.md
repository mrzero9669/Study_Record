
- [python 解释器](#python-解释器)
- [python版本介绍](#python版本介绍)
- [python版本控制](#python版本控制)
    - [pyenv 简介](#pyenv-简介)
        - [virtualenv](#virtualenv)
        - [导出项目依赖包](#导出项目依赖包)
        - [pyenv 相关路径](#pyenv-相关路径)
    - [pip命令](#pip命令)
        - [pip 常用命令操作](#pip-常用命令操作)
        - [Pypi源地址更改](#pypi源地址更改)


## python 解释器
> 
* 官方Cpython
```
c语言开发，最广泛的Python解释器
```
* Ipython
```
一个交互式，功能增强的Cpython
```
* Pypy
```
Python语言写的Python解释器，JIT技术，动态编译Python代码
```
* Jypython
```
python的源代码编译成java字节码，泡在jvm上
```
* IronPython
```
与Jypthon类似，运行在.NET平台上的解释器，Python代码被编译成.Net的字节码
```
__注__:目前主流的还是官方提供的Cpython，pypon是基于python语言编写的，针对性的进行了许多优化，官方速度可以达到Cpython的5倍.

## python版本介绍
&ensp;&ensp;&ensp;&ensp;Python是很多Linux系统默认安装的语言，以Centos为例由于其yum包管理工具使用的是Python开发，所以其内置了Python2.x版本，但是Python目前已经发展到了3.7版本了，并且Python官方对2.x的支持也快到期，所以建议学习ython的3.x版本。
&ensp;&ensp;&ensp;&ensp;Python 3.x的在本质上和Python 2.x有很大的变化，2.x的程序是不能直接在3.x的版本上运行的，它们的主要区别有：
> 
* 语句函数化。例如print的打印，在3.x中是个函数，要打印的内容会被当作参数传递进入，而2.x中的含义是print语句打印元祖整除。在3.x中，/为自然除，//为整除。2.x中/和//都为整除。
* input函数。3.x中把2.x中的raw_input舍去，功能合并到input函数中去。
* round函数。在3.x中的取整变为距离最近的偶数
* 字符串统一使用unicode。2.x中如果想要输入中文，还需要在文件头显示声明(_*_coding:utf-8 _*_)
* 异常的捕获、抛出的语法改变

## python版本控制
&ensp;&ensp;&ensp;&ensp;在工作场景中由于许多老项目的python代码采用的是python2.x运行，而新增的功能代码又是基于python3.x版本开发的，这时候就涉及到一台linux服务器上多个python环境共存问题，通常我们的解决办法有：
*  编译安装新版本至某一个路径
*  多版本python管理工具pyenv
### pyenv 简介
&ensp;&ensp;&ensp;&ensp;pyenv是一个多版本Python管理工具，它可以帮我们安装想要的Python版本，并且可以一键切换，属于现在比较流行的工具。pyenv是一个开源的项目，其代码托管在github上，我们可以访问它的github站点来根据install的步骤进行安装。pyenv的github地址:`https://github.com/pyenv/pyenv` pyenv官方还提供了便捷的安装方式，它的项目地址是:`https://github.com/pyenv/pyenv-installer`,仅需要简单几步就可以完成安装,下面是操作步骤：
> * 1、安装依赖的包
```
yum install -y gcc make patch gdbm-devel openssl-devel sqlite-devel readline-devel zlib-devel bzip2-devel
```
* 2、安装GIT(git使用clone的方式从github拉取pyenv的代码)
```
yum  install git -y
```
* 3、创建用户python
```
useradd python  
passwd  python
vim  /etc/sudoers.d/python
python    ALL=(ALL)    NOPASSWD: ALL  (sudo授权视情况需求而定)
```
* 4、使用python 用户登陆后安装pyenv
```
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
## PS:这里可能遭遇报错： curl: (35) SSL connect error 解决如下：更新nss，如果问题依旧，尝试更新curl
yum update nss
yum update curl 
```
* 5、将pyenv 添加到环境变量
vim ~/.bash_profile
```
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
source ~/.bash_profile
* 6、查看状态
```
[python@node03 ~]$ pyenv version
system (set by /home/python/.pyenv/version) # system 表示当前使用的版本是系统自带的版本
```
* 7、pyenv 常用命令
```
pyenv -h                # 即可列出命令信息
pyenv install -l        # 列出安装的版本信息
pyenv install 3.6.7     # 在线安装安装 3.6.7
pyenv versions          # 查看系统python版本（pyenv install 安装的版本都可以在这里看到）
pyenv global 3.6.7     # 切换Python默认版本为  3.6.7
pyenv local 3.6.7      # 切换当前目录下的Python版本为  3.6.7(和目录绑定,子目录继承环境设定)
pyenv shell 3.6.7      # 仅仅针对当前shell环境(会话级别)
```
* 8、pyenv 离线安装python版本
&ensp;&ensp;&ensp;&ensp;通过 pyenv install  3.6.7 进行安装时，它会联网下载 Python  3.6.7 的源码包，如果机器不能上网的话，可以采用离线的方式,预先下载号Python 要安装的Python的版本包（注意需要gz,xz,bz，三种格式都下载) 在pyenv的安装目录下，一般在用户的家目录下~/.pyenv目录中，mkdir   ~/.pyenv/cache/ && cd  ~/.pyenv/cache/，然后把三个包拷贝到 ~/.pyenv/cache/，然后再次执行 pyenv install  3.6.7 即可
```
[python@node03 ~]$ mkdir  ~/.pyenv/cache/
[python@node03 ~]$ cd   ~/.pyenv/cache/
[python@node03 ~]$ rz    # 上传三个下载好的Python包
[python@node03 ~]$ pyenv install  3.6.7
```
#### virtualenv
&ensp;&ensp;&ensp;&ensp; 如上2方案虽然解决了多版本共存管理问题，但是不能解决不同项目见依赖问题（pip 依赖包引用);那么这个时候我们就需要用到virtualenv来构建python虚拟环境。这个环境是基于pyenv中管理的某个主环境，派生出来的独立子环境，你对virtualenv进行的操作，和其他的virtualenv没有任何关联。
```
# 创建一个virtualenv环境，名字为web367，基于pyenv管理的3.6.7版本
[python@node03 ]$ pyenv virtualenv 3.6.7 web367
[python@node03 .pip]$ pyenv versions
*system (set by /home/python/.pyenv/version)
3.6.7
3.6.7/envs/web367
web367
#这时，我们再对某个项目进行切换时，如下:
[python@node03 ~]$ mkdir web && cd web
[python@node03 cmdb]$ pyenv local web367
(web367) [python@node03 web]$ ls        # 最前面多了个virtualenv环境的名称(web367)，只要进入这个目录我们就进入了web37这个虚拟环境；可以操作下载自己需要的依赖了
```
#### 导出项目依赖包
&ensp;&ensp;&ensp;&ensp;当我们在自己的环境跑通代码后要移植到测试、生产服务器上，所以需要将当前环境安装的所有依赖包导出给运维人员部署环境，该怎么办呢？Python已经提供了一个工具，pip命令(python 3.x中已经内置该命令),使用它的freeze参数完成导出。
```
(web367) [python@node03 web]$ pip freeze > requirement.txt 
(web367) [python@node03 web]$ cat requirement.txt
attrs==19.1.0
backcall==0.1.0
bleach==3.1.0
decorator==4.3.2
defusedxml==0.5.0
entrypoints==0.3
ipykernel==5.1.0
ipython==7.3.0
ipython-genutils==0.2.0
ipywidgets==7.4.2
jedi==0.13.3
Jinja2==2.10
jsonschema==3.0.1
jupyter==1.0.0
jupyter-client==5.2.4
jupyter-console==6.0.0
jupyter-core==4.4.0
MarkupSafe==1.1.1
mistune==0.8.4
nbconvert==5.4.1
nbformat==4.4.0
notebook==5.7.6
pandocfilters==1.4.2
parso==0.3.4
pexpect==4.6.0
pickleshare==0.7.5
prometheus-client==0.6.0
prompt-toolkit==2.0.9
ptyprocess==0.6.0
```
&ensp;&ensp;&ensp;&ensp;我们在另一台服务器上相同虚拟环境中就只需要用 pip install -r requirement.txt ，即可让pip按照requirement.txt文件中标识的包和版本进行安装。

#### pyenv 相关路径
* versions 管理路径
 &ensp;&ensp;&ensp;&ensp; virtualenv创建的虚拟环境都存放在`pyenv`安装目录的`versions`下
```
(web367) [python@node03 web]$ ls -l ~/.pyenv/versions/
total 0
drwxr-xr-x. 7 python python  68 Mar 15 22:39 3.6.7
lrwxrwxrwx. 1 python python  47 Mar 15 22:39 test367 -> /home/python/.pyenv/versions/3.6.7/envs/test367
lrwxrwxrwx. 1 python python  46 Mar 15 23:05 web367 -> /home/python/.pyenv/versions/3.6.7/envs/web367
```
* virtualenv中安装依赖包
 &ensp;&ensp;&ensp;&ensp;virtualenv中安装的包，存放在virtualenv环境中对应的site-package目录下
```python
[python@node03 versions] ls /home/python/.pyenv/versions/3.6.7/envs/web367/lib/python3.6/site-packages/|xargs -l5
attr attrs-19.1.0.dist-info backcall backcall-0.1.0-py3.6.egg-info bleach
bleach-3.1.0.dist-info dateutil decorator-4.3.2.dist-info decorator.py defusedxml
defusedxml-0.5.0.dist-info easy_install.py entrypoints-0.3.dist-info entrypoints.py ipykernel
ipykernel-5.1.0.dist-info ipykernel_launcher.py IPython ipython-7.3.0.dist-info ipython_genutils
ipython_genutils-0.2.0.dist-info ipywidgets ipywidgets-7.4.2.dist-info jedi jedi-0.13.3.dist-info
jinja2 Jinja2-2.10.dist-info jsonschema jsonschema-3.0.1.dist-info jupyter-1.0.0.dist-info
jupyter_client jupyter_client-5.2.4.dist-info jupyter_console jupyter_console-6.0.0.dist-info jupyter_core
jupyter_core-4.4.0.dist-info jupyter.py markupsafe MarkupSafe-1.1.1.dist-info mistune-0.8.4.dist-info
mistune.py nbconvert nbconvert-5.4.1.dist-info nbformat nbformat-4.4.0.dist-info
notebook notebook-5.7.6.dist-info pandocfilters-1.4.2-py3.6.egg-info pandocfilters.py parso
parso-0.3.4.dist-info pexpect pexpect-4.6.0.dist-info pickleshare-0.7.5.dist-info pickleshare.py
pip pip-19.0.3.dist-info pkg_resources prometheus_client prometheus_client-0.6.0-py3.6.egg-info
prompt_toolkit prompt_toolkit-2.0.9.dist-info ptyprocess ptyprocess-0.6.0.dist-info pvectorc.cpython-36m-x86_64-linux-gnu.so
__pycache__ pygments Pygments-2.3.1.dist-info pyrsistent pyrsistent-0.14.11-py3.6.egg-info
```
### pip命令
 &ensp;&ensp;&ensp;&ensp; pip它是Python的包管理工具，类似于Yum和CentOS的关系，通常可以使用pip命令安装几乎所有的Python第三方包。
#### pip 常用命令操作
```
[python@node03 web367]$ pip -h
Usage:   
  pip <command> [options]
Commands:
  install                    # 安装第三方包
  download                   # 下载第三方包
  uninstall                  # 卸载第三方包
  freeze                     # 输出包的名称还版本信息，可以重定向到文件中去
  list                       # 显示已安装的第三方包
  show                       # 显示安装包信息
  search                     # 在Pypi库中查找第三方包
  help                       # 查看帮助信息
General Options:
  -h, --help                 # 显示帮助
  -v, --verbose              # 显示详细信息
  -V, --version              # 显示版本信息
  -q, --quiet                # 安静模式,不输出任何提示信息
  --log <path>               # 把输出信息追加log文件中
  --proxy <proxy>            # 使用代理，格式为： [user:passwd@]proxy.server:port.
  --retries <retries>        # 最大连接失败重试次数，默认5次
  --timeout <sec>            # 设置最大超时时间，默认是15秒
  --cache-dir <dir>          # 指定缓存目录
  --no-cache-dir             # 禁用缓存
```
#### Pypi源地址更改
 &ensp;&ensp;&ensp;&ensp; pip命名默认是从Python官方提供的Pypi仓库进行第三方软件包，由于官方源在国外，访问速度受限，这时候可以把Pypi源换成国内的阿里源，操作是：新增pip的配置文件，指定源为阿里源。
```
[python@node03 ~]$ mkdir ~/.pip
[python@node03 ~]$ cd ~/.pip
[python@node03 .pip]$ vim pip.conf
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com 
```