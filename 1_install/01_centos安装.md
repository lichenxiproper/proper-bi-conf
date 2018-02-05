## centos安装 ##
- 安装centos7：http://www.osyunwei.com/archives/7829.html
- 修改主机名
```
$ vi /etc/hostname
centos1
```
- 配置固定 IP
```
$ vi /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="a56c8196-b2c8-4926-99b1-f0c5d240628c"
DEVICE="ens33"
ONBOOT="yes"
IPADDR="192.168.1.201"
NETMASK="255.255.255.0"
GATEWAY="192.168.1.1"
DNS1="219.148.204.66"
$ service network restart
```

