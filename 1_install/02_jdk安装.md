## jdk安装 ##
- 解压
```
$ tar -zxvf jdk-8u161-linux-x64.tar.gz
```
- 配置环境变量
```
$ vi /etc/profile
export JAVA_HOME=/opt/jdk1.8.0_161
export PATH="$JAVA_HOME/bin:$PATH"
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
$ source /etc/profile
```