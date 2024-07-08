
- 重装系统
	- 准备一个启动盘 ventoy
	- 准备要启动的系统镜像
	- 通过wepe启动后，管理磁盘分区
	- 最后用windows镜像安装windows  ——— i tell you 下载镜像

- mbr和gpt 两种磁盘架构

## 分区管理fdisk
- 虚拟机新建硬盘
- 三种分区
	- parted 实时
	- fdisk
	- gdisk
- 一般使用fdisk分区
**建硬盘-->分区-->格式化-->挂载到某个目录**
- 挂载
	- mkfs.ext4 /dev/stb1
	- mkdir /disk1
	- mount /dev/sdb1/disk1  （挂载分区好的磁盘）
	- df -h  （查看挂载详情）
- 两种分区：临时分区，持久分区
- 格式化：ext4
- 挂载：临时挂载 mount（找到 /etc/fstab 进行挂载，使用mount）

## 硬盘监控 df du
- df 硬盘状态
- du 目录属性、磁盘分析
磁盘容量配额
- quota

## 交换分区
硬盘预先划分一定空间，然后把内存暂时不常用的数据临时存放到硬盘，以便腾出内存空间让更活跃的程序来使用。


## 目录管理
![[Pasted image 20240518143559.png]]

## 硬链接与软链接
![[Pasted image 20240519101931.png]]
硬：指向源文件inode的指针-->即使文件被删除，也能访问（不能跨磁盘，跨分区）
软：记录源文件的保存目录


## 几个实例

#### 磁盘配额

su 
su -   切换用户同时切换所有相关文件
#### 磁盘阵列——提升安全性
- mdadm
- 理解为一种联合挂载  将某个目录（大的虚拟硬盘）分配到在多块硬盘上，这个目录可以实现很多安全操作——比如容灾、并发读等

#### 模拟损坏修复——mdadm 硬盘移除与增补
mdadm -f 模拟
-D 删除

```shell
lablk 插硬盘挂载状态
mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sdb /dev/sdc /dev/sdd /dev/sde

## 带备份盘的硬盘阵列raid 5
```
模拟备份盘被替补
通过模拟，表现出sdb掉，sde顶上
![[Pasted image 20240519141843.png]]

## LVM逻辑卷管理器
解决硬盘在创建分区后不易修改分区大小的缺陷。
[11.LVM逻辑卷管理器 · 英格网络实验室 (atopos.cn)](http://book.atopos.cn/Linux/Linux%E5%9F%BA%E7%A1%80/11.LVM%E9%80%BB%E8%BE%91%E5%8D%B7%E7%AE%A1%E7%90%86%E5%99%A8.html)


PV：物理卷
VG：卷组——整合多个物理卷
PE：最小存储单元 一般是4mb

得到一个lv


- 逻辑卷快照
	- 大小不和原先一样
	- 只用一次


- 删除逻辑卷，最终
![[Pasted image 20240519162040.png]]
一切如初