[[API接口专讲]]

- web1 只读网站
- web2 读写网络网站
- web3 区块链->协议认证

- http0.9 只支持get，仅能访问html
- http1.0 增加post和head，支持cache，tcp短连接，**但使用了connection:keep-alive请求不要关闭连接。**


- http1.1 支持持久连接，一个tcp允许多个请求。新增put，patch，delete，options等
- http2.0 性能大幅提升，多路复用，header压缩,持久化链接，服务端推送


- http3.0 google基于udp协议的quic协议。

- 一个URL
``` 
http://baidu.com/abd?someelse#1
? 传参
# 锚点——用于页面内定位
```


## 请求
- 无连接（一次渲染）、媒体独立、无状态（无记忆）
- 请求——响应 结构
	- 请求行，请求头
	- 响应式布局——根据页面变化而进行请求——改变格式
![[Pasted image 20240521151349.png]]

### GET
- 请求服务器资源——幂等
- 请求附着在url之后，？分割url并传输数据，&连接多个参数
- 安全性低
### POST
- 表单请求，也不安全，需要加密
- 大小理论上无限
### PUT
- 将指定资源状态改为请求中，主要用于文件上传等
![[Pasted image 20240521151452.png]]


## 响应
![[Pasted image 20240521151611.png]]
### 响应码
![[Pasted image 20240521152341.png]]
[HTTP状态码大全 - 常用参考表对照表 - 脚本之家在线工具 (jb51.net)](https://tools.jb51.net/table/http_status_code)
重要的响应头属性——set cookie

session 存在服务器端——检查用户存的coockie是否对应
location 做跳转
- 1，继续处理
- 2，成功
- 3，重定向
- 4，客户端错误
- 5，服务端错误

### http工作原理
![[Pasted image 20240521160044.png]]
长连接+超时时间




## 重点
- http是什么
- 版本，区别
- 消息结构（请求，响应  请求方法，状态码）
- http长连接和短连接
- 在地址栏输入一个url之后，发生了什么？
- 如何用http传自己想传的数据？