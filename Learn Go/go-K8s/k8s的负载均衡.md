- kube-proxy
	- 做服务发现、服务注册、兼职负载均衡
	- 性能压力大


- iptables
	- 默认方式
	- 操作系统层复杂均衡
	- 但负载均衡策略有限（随机、轮询）


- ipvs
	- 超大规模集群的负载均衡
	- 扩展性强，性能好

### 前三种都是按node节点完成资源配置与规则转发
- ingress
	- 这是个七层网关（应用层网关）
	- 性能不强但功能多



- 集群外调用集群内的服务：node-pod ， ingress  ， loadbalancer服务、服务网格等
- 集群内调用：DNS发现（域名解析） 、ip+端口
nodeip 节点（一组物理机）
podip  服务（服务的后端实例）
clusterip（一组服务的维护ip，通过cluserip转发到podip）

- **服务网格————无侵入式（很大特色）**

[Istio Gateway与Kubernetes Ingress Controller对比-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/636511)

[Istio与Ingress灰度发布对比_istio ingress-CSDN博客](https://blog.csdn.net/u013499394/article/details/120436195)

服务注册、服务发现、负载均衡、（服务治理、可观测性、认证管理、安全管理——istio）

[[istio和ingress]]
