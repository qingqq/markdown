[TOC]
# Python 环境安装
-----
###1. python的简介

一般情况下，Linux都会预装Python了，但是这个预装的Python版本一般都非常低，很多 Python的新特性都没有，必须重新安装新一点的版本，从下边的截图，可以看到我的 linux下，预装Python 的版本非常低，古老的 2.4.3版本。这里准备安装新版本 2.6.6。

--------------
###2. 安装前的准备工作

####2.1. 下载软件包：
 下载地址：https://www.python.org/downloads/，下载版本如图：Gzipped source tar ball(2.6.6)(sig)
####2.2. 具备gcc环境：
 验证方法：
```
[root@tas-oracle ~]# rpm -qa|grep gcc
libgcc-4.4.7-4.el6.x86_64
gcc-4.4.7-4.el6.x86_64
gcc-c++-4.4.7-4.el6.x86_64
libgcc-4.4.7-4.el6.i686
```
如上，敲入命名后可看到gcc版本信息，就说明已具备gcc环境

-----
###3. 安装过程
> * [注]在root用户下安装，否则会报权限不足

####3.1. 上传安装包至指定的目录下(具体目录可自己指定)：
####3.2. 解压安装包（可解压在指定目录下）：
```
[root@tas-oracle home]# tar -xzf Python-2.6.6.tgz -C /opt/  (解压到/opt目录下)
[root@tas-oracle home]# cd /opt/Python-2.6.6   (查看解压文件)
[root@tas-oracle Python-2.6.6]# ls
```
####3.3.编译：
```
[root@tas-oracle Python-2.6.6]# ./configure --prefix=/opt
``` 
configure 命令执行完之后，会生成一个 Makefile 文件，这个 Makefile主要是被下一步的 make 命令所使用。打开 Makefile你就会发现，里边制定了构建的顺序 Linux 需要按照Makefile 所指定的顺序来构建 (build) 程序组件。
####3.4. 安装：
```
[root@tas-oracle Python-2.6.6]# make
```
make实际上编译你的源代码，并生成执行文件。
####3.5. 再执行make install 命令
```
[root@tas-oracle Python-2.6.6]# make install
```
make install 实际上是把生成的执行文件拷贝到 linux系统中必要的目录下，比如拷贝到 /usr/local/bin 目录下，这样所有 user就都能运行这个程序了。

-------
###4. 验证

####4.1. 验证python版本
```
[root@tas-oracle Python-2.6.6]# python -V
Python 2.4.3
```
此时显示的版本还是2.4.3，但已经安装了2.6.6版本，解决办法参考“第五步常见错误解决方案”
2.验证python是否正确安装
```
[root@tas-oracle Python-2.6.6]# python
>>>print(1+2+3)
>>>6
```
如果在执行print语句后输出结果正确（为6），则验证成功。

---------
### 5. 常见错误解决方案
#### 5.1 正确安装python后，版本不对
在正确安装python后，查看python版本时，显示的版本还是2.4.3，但已经安装了2.6.6版本。原因是系统先搜到的是/usr/bin/里面的python，而不是我们安装的python，这里只要做个软连接就好了：
```
[root@tas-oracle ~]# cd /usr/bin
[root@tas-oracle bin]# ll |grep python //查看该目录下python
[root@tas-oracle bin]# rm -rf python
[root@tas-oracle bin]# ln -s opt/Python-2.6.6/python ./python  //opt为你解压python的目录，这一步主要是创建软连接
#python -V
```
####5.2 不具备gcc环境，会在“3.3编译”过程报错
报错信息如下（方框中的错误信息）：
```
[root@d95f611369c5 Python-2.6.6]# ./configure --prefix=/opt/home/python2.66
checking for --enable-universalsdk... no
checking for --with-universal-archs... 32-bit
checking MACHDEP... linux3
checking EXTRAPLATDIR... 
checking machine type as reported by uname -m... x86_64
-------------------------------------
| checking for --without-gcc... no  |
| checking for gcc... no            |
| checking for cc... no             |
| checking for cl.exe... no         |
-------------------------------------
```
解决办法：
①安装gcc环境：可参考下文“Python插件--Mysql安装”--1.4 是否具备gcc环境
②编译：重新执行3.3.编译
③安装：重新执行3.4和3.5
④验证：验证python环境是否正确安装

---------
# Python插件--Mysql安装
----------
### 1.安装前的准备工作
#### 1.1 软件包的下载
下载地址：https://pypi.python.org/pypi/MySQL-python/1.2.5，下载软件包：MySQL-python-1.2.5.zip
#### 1.2 是否存在python-devel驱动
验证是否具备：
```
[root@tas-oracle MySQL-python-1.2.5]# rpm -qa|grep python-devel
python-devel-2.6.6-64.el6.x86_64
```
若输出有python-devel信息则具备，否则需要安装，安装过程：
```
[root@tas-oracle /]# sudo yum install python-devel.x86_64
```
再次验证：
```
[root@tas-oracle /]# rpm -qa|grep python-devel
python-devel-2.6.6-64.el6.x86_64
```
成功，已具备python-devel驱动
#### 1.3 是否存在mysql-devel驱动
验证是否具备：
```
[root@tas-oracle MySQL-python-1.2.5]# rpm -qa|grep mysql-devel
mysql-devel-5.1.73-5.el6_6.x86_64
```
若输出有python-devel信息则具备，否则需要安装，安装过程：
```
[root@tas-oracle /]# sudo yum install mysql-devel.x86_64
```
再次验证：
```
[root@tas-oracle /]# rpm -qa|grep mysql-devel
mysql-devel-5.1.73-5.el6_6.x86_64
```
成功，已具备 mysql-devel驱动
#### 1.4 是否具备gcc环境
验证是否具备：
```
[root@tas-oracle /]# rpm -qa|grep gcc
```
若输出以下信息则具备：
```
libgcc-4.4.7-4.el6.x86_64
gcc-4.4.7-4.el6.x86_64
gcc-c++-4.4.7-4.el6.x86_64
libgcc-4.4.7-4.el6.i686
```
否则需要重新安装gcc，安装方法:
```
[root@tosd01 ~]# yum list|grep -i gcc
[root@tosd01 ~]# yum install gcc.x86_64
[root@tosd01 ~]# yum install gcc-c++.x86_64
```

---------

### 2 安装过程
#### 2.1 解压：
```
[root@tas-oracle home]# unzip -n MySQL-python-1.2.5.zip -d /opt
[root@tas-oracle home]# cd /opt/MySQL-python-1.2.5   (去目录下看是否解压成功)
```
#### 2.3 编译配置文件：
```
#python setup.py build
```
#### 2.4 安装：
```
#python setup.py install
```
#### 2.5 验证：
```
#import MySQLdb
```
若果没报错就安装成功

-----
### 3. 常见错误问题总结
#### 3.1 EnvironmentError: mysql_config not found
这是因为mysql的配置文件未找到，这是因为mysql不是开发版，缺少相应的配置文件，解决办法：
修改site.cfg文件
 ①找到mysql的安装目录，目录为：/usr/lib64/mysql/mysql_config
```
#find / -name mysql_config
```
②修改site.cfg文件
```
#cd /opt/MySQL-python-1.2.5
#ls(找到site.cfg文件)
#vi site.cfg (修改)
```
删除mysql_config=XXX这行的注释(即行首的#)，并将mysql_config=xxx修改为
mysql_config=/usr/lib64/mysql/mysql_config（上面find找出的路径）
修改完以后，返回安装过程，重新操作编译(2.3)，安装(2.4)，验证(2.5)
#### 3.2 缺少插件setuptools，错误信息如下：
```
[root@tas-oracle MySQL-python-1.2.5]# python setup.py build
Traceback (most recent call last):
  File "setup.py",line 7, in <module>
    import setuptools
importError:No module named setuptools
```
解决办法：
①下载软件包
下载安装包：setuptools-0.6c11.tar.gz，下载地址：http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
②解压：
```
[root@tas-oracle home]# tar -zxvf setuptools-0.6c11.tar.gz -C /opt
```
③编译安装：
```
[root@tas-oracle home]# cd /opt/setuptools-0.6c11/
[root@tas-oracle setuptools-0.6c11]# python setup.py build 
[root@tas-oracle setuptools-0.6c11]# python setup.py install
```
④安装完后在重新返回安装过程执行编译(2.3),安装(2.4),验证(2.5)

----------
# Python插件---DB2安装
-----
## DB2客户端的安装
-----
### 1.简介
本次操作我们是用db2_v9.7_server的安装包安装客户端，安装包是v9.7_linuxx64_server.tar.gz，系统版本是CentOS release 6.5 (Final)

-----
### 2. 安装过程
#### 2.1 将安装包上传到服务器
#### 2.2 创建db2inst目录，将安装包移动到此目录，然后开始解压
```
[root@tas-oracle db2inst]# tar -zxvf v9.7_linuxx64_server.tar.gz -C /opt
```
#### 2.3 解压完成后进入server目录，运行db2prereqcheck，检查db2安装环境
```
[root@tas-oracle server]# ./db2prereqcheck
```
这时会遇到错误，暂时忽略：
```
WARNING:
   The 32 bit library file libstdc++.so.6 is not found on the system. 
   32-bit applications may be affected.  
ERROR: 
   The required library file libaio.so.1 is not found on the system. 
   Check the following web site for the up-to-date system requirements
   of IBM DB2 9.7
```
警告暂时忽略，我们看错误，说找不到必须文件libaio.so.1 ，这个百度下看看需要安装 libaio-0.3.107-10.el6.i686.rpm
   我们用yum命令安装:
```
[root@tas-oracle server]#  yum install libaio
```
#### 2.4 安装
在没有错误的时候运行：
```
[root@tas-oracle server]#  ./db2_install
```
在开始安装的时候会问是否设置安装目录，选择no，安装到默认目录下，默认目录是 /opt/ibm/db2/。在选择产品的时候选择CLIENT，等待安装完成。
#### 2.5 创建用户  db2inst
```
[root@tas-oracle server]# useradd db2inst
[root@tas-oracle server]# passwd db2inst
```
#### 2.6 创建实例
```
[root@tas-oracle server]# cd /opt/ibm/db2/V9.7/instance
[root@tas-oracle instance]# ./db2icrt -u db2inst //在db2inst用户下创建db2inst实例，当然实例名称也可以自己命名写到后面如：./db2icrt -u db2inst tas
[root@tas-oracle instance]# ./db2icrt -u inst2Fence db2inst 
```
#### 2.7 切换到用户db2inst下   
```
[root@tas-oracle instance]# su db2inst
```
#### 2.8 在客户端的机器上能够把远程的服务器能够识别出来
```
[root@tas-oracle instance]# db2 catalog tcpip node db2inst1  remote 192.168.0.201 server 50001  //db2inst1 节点名称自己命名
```
#### 2.9 把该实例下的数据库catalog到本地
```
[root@tas-oracle instance]# db2 catalog db tosd at node db2inst1
```
#### 2.10 连接到远程数据库
```
[root@tas-oracle instance]# db2 connect to tosd user tosd using tosd201
db2 => 
```
#### 2.11 测试
```
db2 => select * from BASE.TEST

NAME                          
------------------------------
lili                          
mal                           

  2 record(s) selected.
```
此时若能正确查出表中数据，则表明安装成功

----------
## python连接DB2插件安装--PyDB2_1.1.1
----------
### 1.安装前的准备工作
#### 1.1 是否存在python-devel驱动
验证是否具备：
```
[root@tas-oracle MySQL-python-1.2.5]# rpm -qa|grep python-devel
python-devel-2.6.6-64.el6.x86_64
```
若输出有python-devel信息则具备，否则需要安装，安装过程：
```
[root@tas-oracle /]# sudo yum install python-devel
```
再次验证：
```
[root@tas-oracle /]# rpm -qa|grep python-devel
python-devel-2.6.6-64.el6.x86_64
```
成功，已具备python-devel驱动
#### 1.2 是否存在mysql-devel驱动
验证是否具备：
```
[root@tas-oracle MySQL-python-1.2.5]# rpm -qa|grep mysql-devel
mysql-devel-5.1.73-5.el6_6.x86_64
```
若输出有python-devel信息则具备，否则需要安装，安装过程：
```
[root@tas-oracle /]# sudo yum install mysql-devel
```
再次验证：
```
[root@tas-oracle /]# rpm -qa|grep mysql-devel
mysql-devel-5.1.73-5.el6_6.x86_64
```
成功，已具备 mysql-devel驱动
#### 1.3 是否具备gcc环境
验证是否具备：
```
[root@tas-oracle /]# rpm -qa|grep gcc
```
若输出以下信息则具备：
```
libgcc-4.4.7-4.el6.x86_64
gcc-4.4.7-4.el6.x86_64
gcc-c++-4.4.7-4.el6.x86_64
libgcc-4.4.7-4.el6.i686
```
否则需要重新安装gcc，安装方法参考文档“R环境安装手册”中“3.安装基础库:gcc和gcc-c++” 

---------
### 2. 安装过程
#### 2.1 上传软件包至指定目录
#### 2.2 解压
```
[root@tas-oracle home]# tar -zxvf  PyDB2_1.1.1-1.tar.gz -C /opt
```
#### 2.3 进入目录  PyDB2_1.1.1
```
[root@tas-oracle /]# cd opt/PyDB2_1.1.1
```
#### 2.4 执行编译安装
```
[root@tas-oracle /]# python setup.py install
```
#### 2.5 验证是否安装正确
```
[root@tas-oracle PyDB2_1.1.1]# python
Python 2.6.6 (r266:84292, Jan 28 2016, 18:22:41) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import DB2
>>> conn=DB2.connect(dsn='tosd',uid='tosd',pwd='tosd201')
>>> conn=DB2.connect(dsn='tosd',uid='tosd',pwd='tosd201')
>>> cursor=conn.cursor()
>>> cursor.execute("select * from BASE.TEST")
>>> row =cursor.fetchone()
>>> print row
('lili',)
```
此时若没报错则安装正确。

--------
### 3 常见错误解决方法
#### 3.1 找不到ibdb2.so.1 文件 
当我们正常安装后，在验证安装是否正确时会报错：ImportError: libdb2.so.1: cannot open shared object file: No such file or directory，这时需要做个软连接，解决办法：
```
[root@tas-oracle PyDB2_1.1.1]# find / -name libdb2.so.1 //全局搜索 libdb2.so.1 文件,搜索的目录为：
/usr/lib/libdb2.so.1
/opt/ibm/db2/V9.7/lib64/libdb2.so.1
/opt/ibm/db2/V9.7/lib32/libdb2.so.1
[root@tas-oracle PyDB2_1.1.1]# ln -s /opt/ibm/db2/V9.7/lib64/libdb2.so.1 /usr/lib/libdb2.so.1 //建立软连接，注意32位和64位，不要建立错了
[root@tas-oracle PyDB2_1.1.1]# ldconfig  //刷新下ld命名配置
```
到此这个问题已经解决完，然后可继续导入db2模块验证是否正确导入，即执行2.5验证是否安装正确
#### 3.2 缺少MAX函数
当我们正确当我们正常安装后，在验证安装是否正确时会报错：ImportError: /usr/local/lib/python2.6/site-packages/_db2.so: undefined symbol: max。这时因为调用的setup.py脚本需要通过_db2_module.c文件来编译，是_db2_module.c文件里缺少个函数，需要添加上加max函数，解决办法，找到文件db2_module.c，在43行添加函数
```
int max(int a,int b)
{
  return a>b?a:b;
};
```
# python连接hive库

##1.python连接hiveserver(hive-0.11版本)
###1.1安装thrift-0.9.0
上传安装包：thrift-0.9.0.tar.gz到/home/ocdp/目录下
```
$cd /home/ocdp/
$tar zxvf thrift-0.9.0.tar.gz
$mv thrift-0.9.0 thrift
$cd thrift
$python setup.py install
```
###1.2启动Hive服务(如果hive服务起了，不需要这步)
```
$nohup hive --service  hiveserver &
```
###1.3拷贝hive文件到python目录下	
```
ImportError: No module named hive_service
cp /home/ocdc/app/hive/lib/py/* /usr/lib/python2.6/site-packages/
```
###1.4测试
```
$python
>>> from thrift.transport import TTransport
```
执行python脚本正常则python连接hive库成功
*############################################################*
##2.python连接hiveserver2(hive-1.2.1版本)
###2.1安装thrift 0.9.3
上传安装包thrift-0.9.3.tar.gz到主机上
```
$tar zxvf thrift-0.9.3.tar.gz
$cd thrift-0.9.3 thrift
$python setup.py install
cd /usr/lib/python2.7/site-packages
vi .pth
    输入setings目录，适配变量：/home/ocdp/tas/setings
保存退出   
```
###2.2安装pyhs2
可能会报错，sasl安装需要连网,如果认证方式不是sasl，则可以修改/usr/lib/python2.6/site-packages/pyhs2-0.6.0-py2.6.egg/pyhs2/connections.py文件，注释掉import sasl,from cloudera.thrift_sasl import TSaslClientTransport
下面介绍认证方式不是sasl的安装方法：
####2.2.1上传安装包pyhs2-master.zip到主机上
####2.2.2解压缩，修改配置文件
```
[root@YSHD5 tools]# unzip pyhs2-master.zip
[root@YSHD5 tools]# cd pyhs2-master
[root@YSHD5 pyhs2-master]# vi setup.py
```
   *注释掉“sasl”，这一行前面输入#即可*
保存退出
```
[root@YSHD5 pyhs2-master]# cd pyhs2
[root@YSHD5 pyhs2]# vi connections.py
```
*需要注释两行代码，注释代码如下，注释方法依然是前面输入#即可：*
```
import sasl
from cloudera.thrift_sasl import TSaslClientTransport
```
保存退出
####2.2.3 执行安装
```
[root@YSHD5 pyhs2-master]# python setup.py install
```
####2.2.4修改hive配置文件
切换到ocdp用户（ocdp为hive安装用户，根据本地而定），修改hive配置：
```
cd /home/ocdp/app/hive-ocdp3.5/conf
vi hive-site.xml
```
*注：a,上面安装pyhs2时的认证方式不是sasl，所以需要修改hive.server2.authentication的值为NOSASL*
```
markdown写法，引入代码但是预览的时候不显示大于小于号里面的内容：
<name>hive.server2.authentication</name>
<value>NOSASL</value>
以下是使用HTML转义字符的写法，预览时显示正常：
&lt;name&gt;hive.server2.authentication&lt;/name&gt;
&lt;name&gt;NOSASL&lt;/value&gt;
```
*注：b,修改hive.server2.thrift.bind.host主机名称为安装thrift的名称（本机名称）*
```
markdown写法：
<name>hive.server2.thrift.bind.host</name>
  <value>yshd5</value>
使用HTML转义字符的写法：
&lt;name&gt;hive.server2.thrift.bind.host&lt;/name&gt;
  &lt;value&gt;yshd5&lt;/value&gt;
```
###2.3准备连接hive的python代码
在使用Python连接hive之前需要将hive中的文件拷贝到python的sys.path中
```
cp -r $HIVE_PATH/lib/py     /usr/local/lib/python2.7/site-packages
或者拷贝py目录下的文件
cp -r /home/ocdp/app/hive-ocdp3.5/lib/py/* /usr/lib/python2.7/site-packages/.
```
###2.4检查hiveserver2是否启动
执行如下代码检查进程是否启动，如果存在Hiveserver2进程则已启动：
```
ps -ef|grep Hiveserver2
```
如果Hiveserver2未启动，执行如下命令：
```
cd /home/ocdp/app/hive-ocdp3.5/bin
nohup ./hiveserver2 &
```
###2.5测试连接
输入如下代码测试python连接hive是否成功
```
python
import sys  
import pyhs2
conn=pyhs2.connect(host='10.4.56.3',port=10000,authMechanism="NOSASL",user='ocdp',password='',database='default')
cur=conn.cursor()
print cur.getDatabases()
all=cur.execute("select * from hivetas.dim_app limit 5")
print cur.fetch()
```
*注：执行conn=pyhs2.connect(host='10.4.56.3',port=10000,authMechanism="NOSASL",user='ocdp',password='',database='default')的时候，user必须填写*






 

