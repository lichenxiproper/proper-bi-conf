## cloudera manager 安装 ##
- 参考网站
```
http://blog.csdn.net/tx_smile/article/details/78338110
https://segmentfault.com/a/1190000012540680
http://blog.csdn.net/eason_oracle/article/details/51818423
```
- 下载文件
```
http://archive.cloudera.com/cm5/cm/5/cloudera-manager-centos7-cm5.13.1_x86_64.tar.gz
http://archive.cloudera.com/cdh5/parcels/5.13.1/CDH-5.13.1-1.cdh5.13.1.p0.2-el7.parcel
http://archive.cloudera.com/cdh5/parcels/5.13.1/CDH-5.13.1-1.cdh5.13.1.p0.2-el7.parcel.sha1	
http://archive.cloudera.com/cdh5/parcels/5.13.1/manifest.json
https://dev.mysql.com/downloads/connector/j/
```
- 基本配置（所有节点）
```
$ vi /etc/hosts
192.168.1.201 centos1
192.168.1.202 centos2
192.168.1.203 centos3
$ vi /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=centos1
```
- SSH无密码登录
```
# centos1执行
$ mkdir /root/.ssh
$ ssh-keygen -t rsa
$ cat id_rsa.pub >> authorized_keys
$ chmod 600 authorized_keys
$ scp authorized_keys root@centos2:~/.ssh
# centos2执行
$ mkdir /root/.ssh
$ ssh-keygen -t rsa
$ cat id_rsa.pub >> authorized_keys
$ chmod 600 authorized_keys
$ scp authorized_keys root@centos3:~/.ssh
# centos3执行
$ mkdir /root/.ssh
$ ssh-keygen -t rsa
$ cat id_rsa.pub >> authorized_keys
$ chmod 600 authorized_keys
$ scp authorized_keys root@centos1:~/.ssh
$ scp authorized_keys root@centos2:~/.ssh
```
- 创建数据库
```
$ docker run --name mysql_hadoop -p 3307:3306 -v /opt/docker/mysql_hadoop/conf:/etc/mysql/conf.d -v /opt/docker/mysql_hadoop/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=cmf -e MYSQL_USER=cmf -e MYSQL_PASSWORD=123456 -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
- 关闭防火墙（所有节点）
```
$ systemctl stop firewalld.service
$ systemctl disable firewalld.service
```
- 关闭SELinux（所有节点）
```
$ vi /etc/selinux/config
SELINUX=disabled
```
- 配置NTP服务（所有节点）
```
$ yum install ntp
$ chkconfig ntpd on
$ service ntpd start
```
- 安装插件
```
$ yum -y install psmisc
```
- 解压cloudear manager
```
tar -zxvf cloudera-manager-centos7-cm5.13.1_x86_64.tar.gz
```
- 复制安装包
```
# 修改CDH-5.13.1-1.cdh5.13.1.p0.2-el7.parcel.sha1为CDH-5.13.1-1.cdh5.13.1.p0.2-el7.parcel.sha
# 将3个cdh文件复制到/opt/cloudera/parcel-repo/
```
- 复制mysql驱动
```
# 驱动复制到/opt/cm-5.13.1/share/cmf/lib/
# hive等安装时需要复制到指定软件内
```
- 配置agent
```
$ vi /opt/cm-5.13.1/etc/cloudera-scm-agent/config.ini
server_host=centos1
```
- 复制agent到其他节点
```
$ scp -r /opt/cm-5.13.1 centos2:/opt
$ scp -r /opt/cm-5.13.1 centos3:/opt
```
- 创建用户（所有节点）
```
$ useradd --system --home=/opt/cm-5.13.1/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
```
- 设置数据库
```
$ /opt/cm-5.13.1/share/cmf/schema/scm_prepare_database.sh mysql -hcentos1 -P3307 —scm-host centos1 cmf cmf 123456
$ cat cm-5.13.1/etc/cloudera-scm-server/db.properties
```
- 启动
```
# 启动server
$ /opt/cm-5.13.1/etc/init.d/cloudera-scm-server start
# 启动agent（所有节点）
$ /opt/cm-5.13.1/etc/init.d/cloudera-scm-agent start
```
- 连接
```
http://192.168.1.201:7180/
admin/admin
```
