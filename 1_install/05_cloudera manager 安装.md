## cloudera manager 安装 ##
- 参考网站
```
http://blog.csdn.net/tx_smile/article/details/78338110
https://segmentfault.com/a/1190000012540680
http://blog.csdn.net/eason_oracle/article/details/51818423
```
- 下载文件
```
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.14.0/RPMS/x86_64/cloudera-manager-agent-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.14.0/RPMS/x86_64/cloudera-manager-daemons-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.14.0/RPMS/x86_64/cloudera-manager-server-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm
http://archive.cloudera.com/cdh5/parcels/5.14.0/CDH-5.14.0-1.cdh5.14.0.p0.24-el7.parcel
http://archive.cloudera.com/cdh5/parcels/5.14.0/CDH-5.14.0-1.cdh5.14.0.p0.24-el7.parcel.sha1
http://archive.cloudera.com/cdh5/parcels/5.14.0/manifest.json
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
- 安装cloudear manager
```
$ yum --nogpgcheck localinstall cloudera-manager-daemons-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm
$ yum --nogpgcheck localinstall cloudera-manager-server-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm
$ yum --nogpgcheck localinstall cloudera-manager-agent-5.14.0-1.cm5140.p0.25.el7.x86_64.rpm cloudera-manager-daemons
```
- 复制安装包
```
# 修改CDH-5.14.0-1.cdh5.14.0.p0.24-el7.parcel.sha1为CDH-5.14.0-1.cdh5.14.0.p0.24-el7.parcel.sha
# 将3个cdh文件复制到/opt/cloudera/parcel-repo/
```
- 复制mysql驱动
```
# 驱动复制到/usr/share/cmf/lib/
```
- 设置数据库
```
$ /usr/share/cmf/schema/scm_prepare_database.sh mysql -hcentosd -P3306 cmf cmf 123456
```
- 配置agent
```
$ vi /etc/cloudera-scm-agent/config.ini
server_host=centos1
```
- 启动
```
# 启动agent（所有节点）
$ service cloudera-scm-agent start
# 启动server
$ service cloudera-scm-server start
# 查看server启动log
$ tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log
# 查看启动端口
$ netstat -ntlp
```
- 连接
```
http://192.168.1.201:7180/
admin/admin
```
- 复制mysql驱动
```
hive目录/opt/cloudera/parcels/CDH-5.14.0-1.cdh5.14.0.p0.24/lib/hive/lib
oozie目录/var/lib/oozie
```