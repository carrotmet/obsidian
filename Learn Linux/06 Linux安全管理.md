- 防火墙、代理、封包过滤

- 公司网关有硬件防火墙——防DDOS攻击

 - ip 过滤

### firewall iptables


- 对于linux内核而言，两者只是管理工具。防火墙是内核的netfilter
- 端口管理、ip访问控制、（通行码）flag管理  允许分析mac地址提供服务
- 五连四表
![[Pasted image 20240526093925.png]]

### 规则链与规则表——Netfilter框架
- 链是进入流程（按照raw->mangle->nat->filter的顺序）
- 表记录规则（有四种表）
- 结合表的五链——
- FORWARD处理需要本机进行转发的数据，对此进行过滤（因为需要分段与负载均衡）
- INPUT处理本机接收的数据，对此进行过滤
![[Pasted image 20240526094242.png]]
### nat filter的几种action
- accept
- drop（转圈，吊着）
- reject（拒绝访问）
- snat 源地址转换
- redirect 端口映射
- log 记录日志，传给下一条规则
```shell
基本语法

# 增
iptables -t(加表) -A(加链) -s(加目标ip) -j(加action) -p(添加协议)

iptables -A input -s 192.168.2.1/130
# 删
iptables -D (加链) [nums](action编号)
# 改
iptables -R (加链) [nums](action编号) -j (action)
# 查 -L 必须写链  ，-t不加则默认filter
iptables -t -L (列出规则) -nv
iptables -vnL (加链)

# 相关文件——配置持久化
 /proc/sys/net/ipv4/ip_forward
 /etc/sysconfig/iptables-config

```
- icmp——ip数据传输的管理工具或管理策略
- [24 张图搞定 ICMP ：最常用的网络命令 ping 和 tracert-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1841044)
### 四表action对应职责
- raw跟踪数据包状态   有对应action
- mangle为数据包设置标记   有对应action
- nat修改数据包中的源、目标ip或端口
- filter确定是否放行
[[四表ACTION]]


### 自定义链

- 在五链之后或之间，增加一堆自己定义action的链
```shell
# 自定义链
iptables -A INPUT -j MY_CUSTOM_CHAIN (INPUT之后转到我们自定义的链)

# RETURN
iptables -A MY_CUSTOM_CHAIN -j RETURN
返回到调用链（在这个场景中是INPUT链），继续执行MY_CUSTOM_CHAIN规则之后的规则。
```

### iptables与httpd

iptables 一部分是在网络层面上工作的防火墙，它对进入和离开系统的所有网络流量进行控制。如果 iptables 设置了规则来禁止特定的IP地址、端口或协议类型的流量，那么这些流量在到达应用层（如HTTPd）之前就已经被阻止了。因此，即使 HTTPd 配置了允许这些流量的规则，这些规则也不会生效，因为流量已经被 iptables 在网络层面上拦截。

换句话说，iptables 充当了第一道防线，它决定了哪些流量可以进入系统。只有通过了 iptables 规则的流量才有机会到达应用层，如 HTTPd。如果 iptables 已经禁止了某些流量，那么这些流量根本不会触及到 HTTPd，无论 HTTPd 的配置如何。

这种分层的安全策略确保了系统的安全性，因为即使应用层配置有疏漏，网络层面的防火墙仍然可以提供保护。这也强调了在设计网络安全策略时，需要综合考虑网络层面和应用层面的安全措施。

iptables -m 工作在 运输层，可以操作tcp/utp协议进行流量控制

进而完全理解TCP/IP协议

### 模块 iptables -m
[[05 Linux网络管理#TCP/IP协议]]
- tcp/utp模块
- iprange
- limit模块
- state模块

### 利用NAT表做ip转换

#### 公网IP
- 有限的
- 一个区域共用一个公网ip
![[Pasted image 20240526140228.png]]
#### 内网IP
- 真实ip，通过一系列转发策略转发到路由器，再通过公网IP访问网站


#### 如何实现
- server1配置两个网卡ens33和ens37 ； ens33用NAT保证联网，ens33不写网关保证作为操作系统间通信的网卡。
- server2配置一个网卡，主机模式，改ip，改网关为server1中ens37的ip
- server1开启 允许作为数据转发系统
- server1添加POSTROUTING链的NAT策略实现ip转换
```shell
iptables -t nat -A POSTROUTING -s 192.168.12.0/24 -j SNAT --to-source 192.168.2.130
# -j 前是匹配条件 ， -j 之后是处理策略
```

### 利用NAT表做端口号映射


### firewalld
- 区域、服务
- 动态更新规则
- 区域是更高级别的抽象，将一系列命令打包成一组区域
![[Pasted image 20240601095533.png]]
- 紧急模式
	- firewalld采取一种严格策略
	- 紧急提高系统安全限制
```shell
firewall-cmd --panic-on
firewall-cmd --panic-off
```
- firewall 富规则
```shell
语法：firewall-cmd --permanent --add-rich-rule='<rich_rule_definition>'


# 允许某个ip访问某个端口
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.88.1" port port="80" protocol="tcp" accept'

# 限制某个区域的SSH连接次数
 firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.88.0/24" port port="22" protocol="tcp" limit value="3/m" accept'

```



### 服务访问控制——TCP Wrappers


### SELinux安全子系统
- 基于角色/类型 而非 用户/组
	- 场景：最大程度减少系统服务进程可访问资源（只允许查看使进程运行的最小资源）