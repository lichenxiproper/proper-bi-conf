## hadoop安装 ##
- 解压
```
$ tar -zxvf hadoop-2.8.3.tar.gz
```
- 配置环境变量
```
$ vi /etc/profile
export HADOOP_HOME=/opt/hadoop-2.8.3
export PATH="$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH"
$ source /etc/profile
```
- 