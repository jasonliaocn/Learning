Centos7下Zookeeper安装及使用
<1> 安装Zookeeper
从清华镜像下载安装包
[root@localhost ~]# wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/stable/zookeeper-3.4.10.tar.gz
解压缩
[root@localhost ~]# tar -zxvf zookeeper-3.4.10.tar.gz
删除压缩包
[root@localhost ~]# rm zookeeper-3.4.10.tar.gz

<2> 配置Zookeeper
[root@localhost ~]# cp /opt/zookeeper-3.4.10/conf/zoo_sample.cfg        /opt/zookeeper-3.4.10/conf/zoo.cfg

<3> 启动Zookeeper
[root@localhost ~]# /opt/zookeeper-3.4.10/bin/zkServer.sh start

<4> 查看Zookeeper状态
[root@localhost ~]# /opt/zookeeper-3.4.10/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/zookeeper-3.4.10/bin/../conf/zoo.cfg
Mode: standalone

<5> 设置Zookeeper开机自动启动
[root@localhost ~]# cd /etc/rc.d/init.d
[root@localhost init.d]# touch zookeeper
[root@localhost init.d]# vi zookeeper
#!/bin/bash  
#chkconfig: 2345 10 90  
#description: service zookeeper  
export JAVA_HOME=/opt/jdk
export  ZOO_LOG_DIR=/opt/zookeeper-3.4.10/log
export  ZOOKEEPER_HOME=/opt/zookeeper-3.4.10
su      root    ${ZOOKEEPER_HOME}/bin/zkServer.sh       "$1"

为zookeeper文件添加可执行权限
[root@localhost init.d]# chmod +x zookeeper
将脚本添加到启动项
[root@localhost init.d]# chkconfig --add zookeeper
查看是否添加成功
[root@localhost init.d]# chkconfig  --list
netconsole     	0:off	1:off	2:off	3:off	4:off	5:off	6:off
network        	0:off	1:off	2:on	3:on	4:on	5:on	6:off
zookeeper      	0:off	1:off	2:on	3:on	4:on	5:on	6:off

电脑重启后，使用service  zookeeper   status可查看服务运行状态
[root@localhost init.d]# service  zookeeper   status
ZooKeeper JMX enabled by default
Using config: /opt/zookeeper-3.4.10/bin/../conf/zoo.cfg
Mode: standalone

<6> 遇到的问题：
执行service  zookeeper   status后，Error contacting service. It is probably not running
原因：
1. 没有配置export  JAVA_HOME
2. JDK路径不对，centos默认安装的jdk路径不正确，卸载重新安装。
