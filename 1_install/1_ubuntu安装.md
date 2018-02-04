## ubuntu安装 ##
- 安装ubuntu server 16：http://jingyan.baidu.com/article/3ea51489fa1ba252e71bba79.html (注：安装时要选择SSH插件，硬盘分区选择LVM)
- 如果安装时忘记选择SSH插件（此步骤服务器必须联网）
```
# 更新apt-get
$ sudo apt-get update
# 安装openssh-server
$ sudo apt-get install openssh-server
# 启动ssh
$ sudo service ssh start
```
- 设置SSH可使用root用户登录
```
# 设置root密码
$ sudo passwd
# 跳转到root用户
$ su root
# 打开ssh配置文件
$ vi /etc/ssh/sshd_config
# 修改文件内[PermitRootLogin without-password]->[PermitRootLogin yes]，保存并退出
# 重启ssh
$ service ssh restart
```
- 新版本openssh-server修改加密方式，SSH client无法连接解决方案
```
# 打开ssh配置文件
$ vi /etc/ssh/sshd_config
# 文件最后添加下列三行代码，保存并退出文件
# Ciphers aes128-cbc,aes192-cbc,aes256-cbc,aes128-ctr,aes192-ctr,aes256-ctr,3des-cbc,arcfour128,arcfour256,arcfour,blowfish-cbc,cast128-cbc
# MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160,hmac-sha1-96,hmac-md5-96
# KexAlgorithms diffie-hellman-group1-sha1,diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group-exchange-sha256,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group1-sha1,curve25519-sha256@libssh.org
# 重启ssh
$ service ssh restart
```
- 配置 DNS
```
# 查看 dns 配置
$ cat /etc/resolv.conf
# 添加 dns 设置
$ sudo vi /etc/resolvconf/resolv.conf.d/base
# 添加域名服务器，如：
# nameserver 219.148.204.66
# nameserver 223.5.5.5
# nameserver 8.8.8.8
# 重启令配置生效
$ sudo resolvconf -u
```
- 配置固定 IP
```
$ sudo vi /etc/network/interfaces
# 修改 The primary network interface 部分
# iface 行 dhcp 修改为 static，并增加如下内容
# address 192.168.1.201
# gateway 192.168.1.1
# netmask 255.255.255.0
# 保存后使设置生效
$ sudo /etc/init.d/networking restart
```
