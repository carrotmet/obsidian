- gRPC协议转restful接口


- go get -u github.com/gin-gonic/gin
![[Pasted image 20240506161915.png]]
![[Pasted image 20240506162114.png]]
路由组——中间件——渲染
应用：设置http参数、设置与获取cookie


# gRPC连接池

gRPC依赖http2进行数据通信，本身是长连接而非tcp/utp的短连接，支持上万并发。更高并发才考虑连接池
- sync.Pool自动扩缩容，无锁操作（CAS——比较交换原则）
[[CAS比较交换原则]]

# 反射简化gRPC调用
- gRPC url  （list、方法清单、describe获取源码）
- go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
- ![[Pasted image 20240506163758.png]]
- 对服务的反射（不新建对象的情况下获得类信息）（运行时检查数据类型是常见的反射应用）


# 实现Gin框架对gRPC服务的路由


# 使用grpc-gateway