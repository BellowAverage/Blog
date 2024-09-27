# Hadoop3.x Basics

Created: September 12, 2024 1:48 PM
Labels & Language: CentOS, Hadoop, Java
Source: https://www.bilibili.com/video/BV1Qp4y1n7EN/

## 引言

Hadoop是大数据技术中重要的框架之一。在Hadoop的基础上，能够更容易地开发和运行其他处理大规模数据的框架。作为学习笔记，本文包括Hadoop的理论和在CentOS上的应用实践，包括配置和基本命令等。

## Hadoop3.x 学习笔记 - 基础篇

### 使用VMware虚拟机进行CentOS的配置和安装

1. 在本机中安装CentOS 7镜像的虚拟机，命名为hadoop100
2. 在虚拟网络编辑器中，设置NAT网络，VMnet8，子网IP设置为10端口
3. 在网络适配器控制面板中设置该适配器默认网关为192.168.10.2
4. 将Hadoop100设置为静态IP地址，以确保本机能够找到该服务器，并添加其他相关配置。修改主机名称，配置主机名称映射等。

```bash
[root@hadoop100 chriswang]# vim /etc/sysconfig/network-scripts/ifcfg-ens33
--------------------------------------------------------------------------
# BOOTPROTO="dhcp"
# 修改为：
BOOTPROTO="static"
# 添加IP地址、网关、域名解析器
IPADDR=192.168.10.100
GATEWAY=192.168.10.2
DNS1=192.168.10.2
--------------------------------------------------------------------------
# 修改主机名称为hadoop100
[root@hadoop100 chriswang]# vim /etc/hostname
# 修改主机名称映射
[root@hadoop100 chriswang]# vim /etc/hosts
--------------------------------------------------------------------------
# 添加映射给至多8台服务器
......
192.168.10.100 hadoop100
192.168.10.101 hadoop101
......
192.168.10.108 hadoop108
--------------------------------------------------------------------------
# 重启以更新配置
[root@hadoop100 chriswang]# reboot
```

1. 使用本机PowerShell通过ssh协议模拟远程连接，成功。

```powershell
PS C:\Users\10131> ssh chriswang@192.168.10.100
chriswang@192.168.10.100's password:
Last login: Tue Jul  4 19:05:39 2023
```

1. 配置Windows地址映像，地址为`C:\Windows\System32\drivers\etc\hosts`
2. CentOS环境配置 - 基本工具&环境

```bash
[chriswang@hadoop100 ~]$ sudo yum install -y epel-release
[chriswang@hadoop100 ~]$ sudo systemctl stop firewalld
[chriswang@hadoop100 ~]$ sudo systemctl disable firewalld.service
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
# 清理预装java工具
[root@hadoop100 chriswang]# rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps
```

### 克隆多台虚拟机以模拟分布式集群

- 使用VMware内置克隆工具，选择完全克隆即可
- 对克隆后的服务器进行配置修改，即`(100 —> 102, 103, 104)`

### 安装并配置Java JDK和Hadoop

- 安装Java JDK后，配置全局声明：

```java
#JAVA_HOME
export JAVA_HOME=/home/chriswang/Applications/jdk1.8.0_212
export PATH=$PATH:$JAVA_HOME/bin
```

- 安装Hadoop后，添加声明：

```java
#HADOOP_HOME
export HADOOP_HOME=/home/chriswang/Applications/hadoop-3.1.3
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

- 随后使用`source /etc/profile`即可开启该位置注册过的所有服务

### 完全分布式的运行模式

1. 编写集群分发脚本`xsync`
- 使用`scp`命令拷贝工具和环境至其他服务器

```bash
# 在hadoop102上执行：
scp -r /home/chriswang/Applications/jdk1.8.0_212/ root@hadoop104:/home/chriswang/Applications
scp -r /home/chriswang/Applications/jdk1.8.0_212/ root@hadoop103:/home/chriswang/Applications
```

- 或使用以`rsync`为基础的`xsync`集群分发脚本

a. 在`/home/chriswang/bin`下创建`bash`命令文件`xsync`：

```bash
#!/bin/bash
#1. 判断参数个数
if [ $# -lt 1 ]
then
 echo Not Enough Arguement!
 exit;
fi
#2. 遍历集群所有机器
for host in hadoop102 hadoop103 hadoop104
do
 echo ==================== $host ====================
 #3. 遍历所有目录，挨个发送
 for file in $@
 do
 #4. 判断文件是否存在
 if [ -e $file ]
 then
 #5. 获取父目录
 pdir=$(cd -P $(dirname $file); pwd)
 #6. 获取当前文件的名称
 fname=$(basename $file)
 ssh $host "mkdir -p $pdir"
 rsync -av $pdir/$fname $host:$pdir
 else
 echo $file does not exists!
 fi
 done
done
```

b. 修改`xsync`脚本权限，并将脚本复制到`/bin`中，以便全局调用

```bash
chmod +x xsync
sudo cp xsync /bin/
```

c. 使用`xsync`脚本将`kafka`部署至其他服务器

```bash
xsync ../Applications/kafka/
```

## Other References

- 中文教程 - 尚硅谷Bilibili
    
    [尚硅谷大数据Hadoop教程，hadoop3.x搭建到集群调优，百万播放_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Qp4y1n7EN/)
    
- 官方文档 - Apache Hadoop 3.3.6
    
    [Apache Hadoop 3.3.6 – Hadoop: Setting up a Single Node Cluster.](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)
    
- 建议学习路线图 - 尚硅谷Bilibili
    
    [2023最新大数据学习路线图](https://www.bilibili.com/read/cv5213600)