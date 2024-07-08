
# 文件、目录、权限
### vim、bash、cat、touch、split、tar

- less、head、tail
- grep...
- 管道 |  重定向（输出，输入）
- vim
	- ctrl+f   ctrl+b 
	- 0  (移动到首)
	- $ (移动到尾)
	- gg 移动到首  G 移动到尾
	- pp 复制本行   y粘贴到下一行
	- u 重复上一个动作
	- ctrl + r 重做
	- . (重做上一个)

- AMC time  、 which
- 文件种类
![[Pasted image 20240602112959.png]]
- find
	- -name -size -maxdepth -a/m/ctime
	- - 属主、属组
	- -a 与 -o 或 -not 取反
- ![[Pasted image 20240602113401.png]]

- ![[Pasted image 20240602113419.png]]

- 软连接与硬链接 ln
	- 软式快捷方式，硬是指向持久文件的第二个指针
	- ln -s 源文件  链接文件   （软）
	- ln（硬）
	- 软连接配用户的环境变量

### cd、(cp、mv、rm)、mkdir、rmdir、pwd、ls
- ls -l 查权限  rwx

### 文件权限
- ls -l 列出包括权限的详细信息
- chmod 修改权限    文件
- chown 改所有者    用户
- chgrp 改所属组
- find 查找修改大量权限


- /etc/passwd 用户信息
- /etc/group 组
- /etc/shadow 用户密码
- /etc/gshadow 组密码
- /etc/sudoers/ 谁可以用sudo
- /etc/fstab/  持久化挂载，挂载常用文件
- .bashrc 环境配置文件
- /etc/profile 非命令行配置文件
- /proc/sys/fs 包含文件


- ugo a  rwx
	- 属主，属组，其他用户，三者 ，读写执行
- 421 读写执行（三位出现代表三类用户）
- 修改
	- +-=
	- 属主可chown自己的文件权限
	- root可改所有

- ACL权限
### 用户权限
- useradd 创建新用户
- usermod 修改用户信息
- passwd
- su
- sudo
- visudo
- groups
- newgrp
- id 显示用户和组
- last 显示用户登录记录 


/var/log 记录日志，包括用户登录日志
/etc/login.defs 包含用户和组的默认配置，如密码策略、UID和GID范围等

### 文本三剑客grep、sed、awk
# 内存、进程

### 内存
- free 显示内存使用
- vmstat 显示虚拟内存统计信息
- top 动态交互进程、内存界面  --> htop
- meminfo 详细内存
- smem 比free提供更多信息
- pmap 进程内存映射
- /proc/meminfo 文件
### 进程
- 时间片轮转与进程调度算法
- ps、top、kill、nice、ionice pr值
- 前台进程与后台进程  &    持久后台：nohup 
	- /etc/rc.locl
- 孤儿进程与僵尸进程
![[Pasted image 20240602115309.png]]

# 磁盘
### 建硬盘-->分区-->格式化-->挂载到某个目录
- lsblk 查看挂载情况
- fdisk /dev/sdb 进入fdisk操作界面
- mkfs.ext4 /dev/stb1  (常用rxt4)
- mount /dev/sdb1/ 目标目录

- 持久化挂载 /etc/fstab
- df 硬盘使用情况

- 磁盘配额quota

### madam 磁盘阵列（几种模式） mdadm
- 0加读写，无备份  条带  2个起
- 1做1:1容灾  2 个起
- 5加读，写在多个上（奇偶校验做信息统一）  3个起
- 10 加读写，有备份  镜像条带   4个起

-  mdadm -Cv /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde
### lvm逻辑卷管理器
- PV 总逻辑卷空间  pvcreate /dev/sdb /dev/sdc/
- VG 卷组 （storage）vgcreate storage /dev/sdb /dev/sdc/
- PE 扩容缩容基本单位 4mb
- 逻辑卷扩容、缩容、快照、删除


# 安装包
### 源码编译
- .tar.gz   或   .zip 开始
- ./autogen.sh
- ./configure
- make
### 二进制编译
- RedHat/Centos
	- rpm
	- yum
- Ubuntu/Debain
	- dpkg
	- apt

- 配阿里源
	- curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
	- yum -y install epel-release

- yum自建源——内网传软件包（应用层工作）

### 五链四表

# 网络、通信协议
[[05 Linux网络管理]]
[linux基础——linux进程间通信（IPC）机制总结_isp retime 缓冲区-CSDN博客](https://blog.csdn.net/a987073381/article/details/52006729)
nmtui
# 安全
iptables 五链四表
# 计划任务、日志
at、log
# 语法、管道、重定向

- 输出重定向、文件描述符、输出重定向
- 管道 
- 文本三剑客 grep
### 干净系统
- 关防火墙
- 配置固定yum源
- 固定ip


