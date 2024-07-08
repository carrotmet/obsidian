## 负载均衡定义
- 面对海量请求，对一组后端web server集群进行流量控制、请求调度的过程。
- 分为四层和七层[[负载均衡]]
- 常用负载均衡器：Nginx、LVS、Haproxy


## Nginx负载均衡
- 使用proxy_pass模块
- 标准语法：
```python
Syntax: upstream name{}
Default:http

upstream backend{
   server backend1.example.com    weight=5;
   server backend2.example.com:8080;
   server unix:/tmp/backend3;
   server backup1.example.com:80800 backup;
}
server{
  location / {
      proxy_pass http://backend;
      }
}
```


## 轮询配置
- 负载均衡算法的选择、backup和down状态的设计（多少次下机）
- 轮询、weight、least_conn(最少连接)、ip_hash/url_hash
## 健康模块/会话保持
- 一般会设计为30-3600s的会话保持（四层）、30-86400s(七层)
- 