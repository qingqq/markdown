# Thrift安装教程

标签（空格分隔）： thrift python rpc

---

### 1. thrift简介以及下载
thrift是一个跨语言的服务部署框架，最初由facebook开发，之后进入apache的开源项目。
目前可以从apache官网的thrift项目下下载安装包，[下载地址](http://thrift.apache.org/download)
### 2. thrift安装
目前最新版本到0.9.3，这里演示0.9.2的安装过程
（1）复制 thrift-0.9.2.tar.gz 一个目录，这个目录不一定是安装目录
（2）解压文件，然后进入文件夹
（3）执行configure命令，configure命令可以指定参数，常用参数有一下
--prefix参数配置安装的路径,如果不配置该选项，安装后可执行文件默认放在/usr/local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc，其它的资源文件放在/usr/local/share，比较凌乱；
--with-PACKAGE[=ARG]来指定已有软件包的安装目录；
--with-PACKAGE=no和without禁止你的软件包与系统已有的软件包交互（就是不编译指定的软件包）。
（4）编译和安装命令make、make install命令执行。
（5）具体过程如下：
```
[root@h1 ~]# tar zxvf thrift-0.9.2.tar.gz 
[root@h1 ~]# cd  thrift-0.9.2
[root@h1 thrift-0.9.2]# ./configure --prefix=/usr/local/thrift-0.9.2 --with-lua=no
[root@h1 thrift-0.9.2]# make
[root@h1 thrift-0.9.2]# make install
```
（6）在环境变量中配置thrift的安装目录下的bin目录：
```
vim .bash_profile
THRIFT_HOME=/usr/local/thrift-0.9.2
PATH=$PATH:$THRIFT_HOME/bin
export PATH
```
（7）测试安装是否正确
```
[root@h1 ~]$ thrift -version
Thrift version 0.9.2
```
### 3. python安装thrift插件
（1）进入thrift的安装目录下的lib下的py目录，执行python setup.py install
执行后thrift插件会自动安装到python的site-package目录下，然后通过python命令测试是否安装成功
（2）安装过程：
```
[root@h1 ~]# cd /usr/local/thrift-0.9.2/lib/py
[root@h1 ~]# python setup.py install
```
（3）验证：没有异常信息则说明安装插件成功
```
[root@h1 ~]# python
Python 2.7.5 (default, Jun 24 2015, 00:41:19) 
[GCC 4.8.3 20140911 (Red Hat 4.8.3-9)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> from thrift import Thrift
>>> 
```
### 4. 常见错误解决方案
（1）src/protocol/fastbinary.c:20:20:致命错误：Python.h：没有那个文件或目录
>解决方案：缺少python-devel包，执行yum install python-devel，安装完成后再执行thrift的安装

（2）src/luasocket.c:20:17: fatal error: lua.h: No such file or directory
>解决方案：缺少lua包，lua是一个脚本语言，目前tas中不会用到可以通过在thrift的安装阶段执行参数不编译lua包即可解决，如下命令：
```
[root@h1 thrift-0.9.2]# ./configure --with-lua=no
```


