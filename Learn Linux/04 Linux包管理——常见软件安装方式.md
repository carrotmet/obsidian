
# 有问题，先重装



[Linux 软件安装方式汇总与详解-CSDN博客](https://blog.csdn.net/cooldream2009/article/details/135041437)

- 记得配置环境变量
[linux安装protobuf3.6.1编译安装 c++ 及安装时遇到的错误_recipe for target 'all-recursive' failed-CSDN博客](https://blog.csdn.net/usstmiracle/article/details/103401948)

![[Pasted image 20240512163201.png]]
![[Pasted image 20240512163316.png]]
- 通过软连接配置环境变量——将编译所在目录软连接到 /usr/bin/ 目录下
```shell
ln -s /apps/nginx/sbin/nginx /usr/bin/
```

- 一个编译安装proto3的实例
```shell
sudo yum install autoconf automake libtool curl make g++ unzip
git clone -b v3.6.1 https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh
./configure
make
make check
sudo make install
sudo ldconfig

```


### yum
- 编译安装
- 二进制安装

- 换源
```shell
 ls /etc/yum.repos.d/  # 查看源文件
 curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo   # 安装阿里源 for centos7
```

- 常用命令
``` shell
yum -y install
yum reinstall
yum update httpd
yum updata
yum remove httpd
```


### 提供内网服务器软件包更新
- 配置一台能够连接外网的主机 下载但不安装需要的软件包——通过公网
- 利用creatrepo 软件包索引
- 部署ftp服务，关闭防火墙

- 在server2编写新repo，来源server1