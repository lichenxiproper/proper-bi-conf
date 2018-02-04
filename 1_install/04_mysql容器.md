## mysql容器 ##
- 启动mysql容器
```
$ docker run --name mysql_oa -p 3306:3306 -v /opt/docker/mysql_oa/conf:/etc/mysql/conf.d -v /opt/docker/mysql_oa/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=oa -e MYSQL_USER=oa -e MYSQL_PASSWORD=123456 -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```
- 启动binlog
```
$ vi /opt/docker/mysql_oa/conf/my.cnf
[mysqld]
log-bin = mysql-bin
server-id = 1
$ docker restart mysql_oa
```
- 查看log-bin状态
```
show global variables like '%LOG%';
```
