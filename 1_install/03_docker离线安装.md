## docker离线安装 ##
- 下载离线安装包：https://get.docker.com/builds/Linux/x86_64/docker-latest.tgz
- 解压
```
$ tar -zxvf docker-latest.tgz
```
- 安装docker-compose
```
# docker-compose下载命令
$ curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```
- 配置环境变量
```
$ vi /etc/profile
export PATH="/opt/docker/docker:$PATH"
$ source /etc/profile
```
- 修改Linux进程最大打开文件数
```
$ vi /etc/profile
ulimit -n 65539
$ source /etc/profile
```
- ubuntu调整swap
```
$ vi /etc/default/grub
# 修改文件内[GRUB_CMDLINE_LINUX=""]->[GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"]，保存并退出
# 更新 GRUB
$ update-grub
# 重启系统
```
- 启动docker
```
$ dockerd &
```
- 导出镜像
```
docker save hello-world > /home/user/hello-world_image.tar
```
- 导入镜像
```
docker load < /home/user/hello-world_image.tar
```
