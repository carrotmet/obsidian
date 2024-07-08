
- 线程必须依靠进程存在，至少依靠main进程

- linux中的第一个进程：init（PID为1）----> 被**systemd**接替
- 查看进程列表：
``` bash
ps -u $USER
ps aux\
ps aux|grep ping    对包含ping的过滤，利用管道

ps aux -aux
ps -ef
ps -efH

pstree   进程树，描写进程间的依赖关系
```

- 两种进程：守护进程、前台进程

- Linux进程设计思路：父进程、子进程继承、执行返回与等待

- systemd————LInux内核加载后，开启第一个线程就是systemd线程，负责启动配置

## ps、top、kill、nice

- top动态可交互查看进程信息，ps静态返回当前进程信息
- 日常最常用——systemd service类型 （系统服务管理）
- systemctl常用命令
![[Pasted image 20240517145226.png]]
```shell
systemctl启动原理：在目录 /usr/lib/systemd/system/
下保存有各种需要 systemctl启动的对用 .service文件

# 创建nginx启动文件
vim /usr/lib/systemd/system/nginx.service
```

- 杀进程

```shell
kill 9849  (按进程号kill)
kill 'pgrep ping'(pgrep--快速按一定策略查进程号，反引号命令会优先执行
kill -9 123456
pkill sshd (杀ssh登录进程)
pkill -t pts/2  杀终端，杀终端号为 pts/2的终端
w 查看目前终端
```

pkill 杀进程，不筛选
- 一般来说，查看+杀常见。不能处理则检查进程运行的代码本身的bug

- 进程优先级
	- nice值越小，优先级越高  [  -20  ,  19 ]
- 进程执行原理
	- 时间片技术——实现并行，一核处理多进程（快速切换）
	- [[Linux进程调度算法]]
	- 决定时间片大小的策略——优先级，nice值，进程密集度、状态、进程历史行为、系统负载......
	- 调整优先级可以让进程拥有更快的处理速度，cpu更优先处理这个进程
``` shell
nice -n 10 命令
```

- pri PR值——PR = PR + NICE
- pr是内核参数，可改更广阔的优先级


## 前台进程与后台进程

- 后台进程不一定是守护进程

- 前台进程：键盘相关，终端交互

``` shell
命令 +空格 +&  将进程改为后台进程（不接受键盘控制）
ping www.baidu.com &

暂停、恢复（恢复为前台或后台进程）

jobs   查看后命令、查看工作号（不能跨终端，用pstree看跨终端持续执行的后台进程）

将一个命令设计为后台执行，并且执行的内容不显示在前台，也不被保存
ping www.baidu.com > /dev/null &
```

- 后台进程：无需交互、继续运行（终端关闭也会运行）、不阻塞终端

- 用 & 赋予的后台进程——当终端关闭时，此后台进程也结束


- nohup——永久性后台运行进程
- 将命令加入  /etc/rc.local 系统只要启动，就会执行这个命令（但加入后需要重启服务器才能执行——太不方便）
- 使用系统定时任务
```shell
nohup 命令 &
本质将命令下挂到systemd进程下，保证只要系统在运行，则此命令运行
```


## 僵尸进程与孤儿进程