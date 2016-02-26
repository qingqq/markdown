# R环境安装手册

☑ [TOC]

软件包名称：R-3.1.2.tar.gz
##1.上传R安装包到服务器目录下：/home/ocdp/R
##2.修改环境变量：
标签：需用root用户安装
1.登录root用户
2.修改环境变量：```vi /etc/profile```
  增加以下内容
```
JAVA_HOME=/home/ocdp/app/ibm-java-ppc64-71
export JAVA_HOME
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=./:$JAVA_HOME/lib:$JRE_HOME/lib:$JRE_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

export HADOOP_HOME=/home/ocdp/app/hadoop-ocdp3.5
export HADOOP_CMD=$HADOOP_HOME/bin/hadoop
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-2.6.0-cdh5.4.4.jar
```
3.环境变量生效：```source /etc/profile```
##3.安装JQ
你可以理解成jq是用js编写的一个为了方便快速开发的库，虽然它实际上比以往所谓的库函数更有组织，是一个有机的类对象，包括很多强大的方法，通过它们，你可以使平时需要很多代码完成的js工作简化成几行，不仅方便，在行为意图上也会一目了然。
```
cd /home/ocdp/R
chmod +x ./jq
sudo cp jq /usr/bin
```
##4.安装R基础包
```
tar zxvf R-3.1.2.tar.gz
cd R-3.1.2
./configure --with-x=no
make
make install
```
***注：如果这一步执行./configure报错，提示“C++ preprocessor "/lib/cpp" fails sanity check”或者“cannot compile a simple Fortran program”，执行下面第5步、第6步操作，再执行第4步、第7步...，硬件环境搭建的时候可能没有把基础的包安装全；如果不报错执行第7步***
>##5.安装基础库:gcc和gcc-c++
```
yum list|grep -i gcc
yum install gcc.ppc64
yum install gcc-c++.ppc64
```
##6.安装系统依赖包:ncurses-devel、readline-devel、fortran
```
yum list|grep -i ncurses-devel
yum install ncurses-devel.ppc64
yum list|grep -i readline-devel
yum install readline-devel.ppc64
yum list|grep -i fortran
yum install gcc-gfortran.ppc64
```

##7.安装Rhadoop基础包
```
cd /home/ocdp/R/RHadoop
R CMD INSTALL digest_0.6.8.tar.gz
R CMD INSTALL functional_0.6.tar.gz
R CMD INSTALL stringr_0.6.2.tar.gz
R CMD INSTALL Rcpp_0.11.4.tar.gz
R CMD INSTALL plyr_1.8.1.tar.gz
R CMD INSTALL reshape2_1.4.1.tar.gz
R CMD INSTALL bitops_1.0-6.tar.gz
R CMD INSTALL caTools_1.17.1.tar.gz
R CMD INSTALL RJSONIO_1.3-0.tar.gz
R CMD INSTALL rmr2_3.3.1.tar.gz
```
##8.环境注册到R里
```
R CMD javareconf
```
##9.安装rjava
```
R CMD INSTALL rJava_0.9-6.tar.gz
R CMD INSTALL rhdfs_1.0.8.tar.gz
```
##10.安装算法包
###10.1安装决策树算法包
```
cd /home/ocdp/R/tools
R CMD INSTALL C50_0.1.0-21.tar.gz
```
###10.2安装协同过滤算法
```
R CMD INSTALL reshape_0.8.5.tar.gz
R CMD INSTALL arules_1.1-6.tar.gz
R CMD INSTALL registry_0.2.tar.gz
R CMD INSTALL proxy_0.4-14.tar.gz
R CMD INSTALL recommenderlab_0.1-5.tar.gz
R CMD INSTALL Matrix_1.2-0.tar.gz
R CMD INSTALL hashFunction_1.0.tar.gz
```
##11.R环境验证
输入R进入R环境，依次执行
```
library(rmr2)
library(rhdfs)
from.dfs(to.dfs(keyval(1,1:10)))
```

验证结果如下：
```
[root@tas-214 ~]# R

R version 3.1.2 (2014-10-31) -- "Pumpkin Helmet"
Copyright (C) 2014 The R Foundation for Statistical Computing
Platform: x86_64-unknown-linux-gnu (64-bit)

R是自由软件，不带任何担保。
在某些条件下你可以将其自由散布。
用'license()'或'licence()'来看散布的详细条件。

R是个合作计划，有许多人为之做出了贡献.
用'contributors()'来看合作者的详细情况
用'citation()'会告诉你如何在出版物中正确地引用R或R程序包。

用'demo()'来看一些示范程序，用'help()'来阅读在线帮助文件，或
用'help.start()'通过HTML浏览器来看帮助文件。
用'q()'退出R.

[原来保存的工作空间已还原]

> library(rmr2)
Please review your hadoop settings. See help(hadoop.settings)
警告信息：
S3方法‘gorder.default’, ‘gorder.factor’, ‘gorder.data.frame’, ‘gorder.matrix’, ‘gorder.raw’在NAMESPACE里有声明但不存在 
> library(rhdfs)
载入需要的程辑包：rJava

HADOOP_CMD=/home/jiangl/hadoop/hadoop-1.0.3/bin/hadoop

Be sure to run hdfs.init()
> from.dfs(to.dfs(keyval(1,1:10)))
$key
[1]1 1 1 1 1 1 1 1 1

$val
[1]1 2 3 4 5 6 7 8 9 10

> 
```