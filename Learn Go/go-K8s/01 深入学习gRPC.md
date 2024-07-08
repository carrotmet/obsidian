- rpc框架
[[API接口专讲]]

- 三个组成部分
	- 网络传输层使用http2
	- 数据包序列化协议使用Protocol Buffers而不是传统的json
	- 代码生成——通过protobud自动生成多种编程语言客户端或服务器代码

## 利用gRPC实现跨语言通信的实例：一个go服务端和一个java客户端
### 第一步：定义服务接口

首先，您需要定义一个 `.proto` 文件，这个文件描述了 RPC 服务和消息类型。

```protobuf
// greet.proto
syntax = "proto3";

package greet;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings.
message HelloReply {
  string message = 1;
}
```

### 第二步：生成 Go 语言的服务器代码

使用 Protocol Buffers 编译器 `protoc` 生成 Go 语言的代码。

```bash
protoc --go_out=. --go-grpc_out=. -I=. greet.proto
```

这将生成 `greet.pb.go` 和 `greet_grpc.pb.go` 文件，包含 Go 语言的消息和 RPC 服务代码。

### 第三步：编写 Go 语言的服务器

使用生成的代码来编写服务器。

```go
// server.go
package main

import (
    "context"
    "log"
    "net"

    "google.golang.org/grpc"
    pb "path/to/your/generated/go/package"
)

const (
    port = ":50051"
)

type server struct {
    pb.UnimplementedGreeterServer
}

func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
    log.Printf("Received: %v", in.GetName())
    return &pb.HelloReply{Message: "Hello " + in.GetName()}, nil
}

func main() {
    lis, err := net.Listen("tcp", port)
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }
    s := grpc.NewServer()
    pb.RegisterGreeterServer(s, &server{})
    log.Printf("server listening at %v", lis.Addr())
    if err := s.Serve(lis); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}
```

### 第四步：生成 Java 客户端代码

使用 Protocol Buffers 编译器 `protoc` 和 Java 插件生成 Java 客户端代码。

```bash
protoc --java_out=. --grpc-java_out=. -I=. greet.proto
```

这将生成 Java 消息和 RPC 服务的代码。

### 第五步：编写 Java 客户端

使用生成的代码来编写 Java 客户端。

```java
// GreeterClient.java
package io.grpc.examples;

import greet.GreeterGrpc;
import greet.HelloReply;
import greet.HelloRequest;

import io.grpc.ManagedChannel;
import io.grpc.ManagedChannelBuilder;

public class GreeterClient {
    public static void main(String[] args) throws Exception {
        // The port number(50051) is set as per the server setup
        ManagedChannel channel = ManagedChannelBuilder.forAddress("localhost", 50051)
                .usePlaintext()
                .build();
        GreeterGrpc.GreeterBlockingStub blockingStub = GreeterGrpc.newBlockingStub(channel);

        // Create a simple request
        HelloRequest request = HelloRequest.newBuilder().setName("world").build();

        // Make the request
        HelloReply response = blockingStub.sayHello(request);

        System.out.println(response.getMessage());

        // Shut down the channel
        channel.shutdownNow().awaitTermination(5, TimeUnit.SECONDS);
    }
}
```

确保您已经在本地运行了 Go 语言服务器，然后运行 Java 客户端。客户端将向服务器发送一个 RPC 请求，服务器将响应，客户端将打印出响应消息。

这个例子展示了 gRPC 和 Protocol Buffers 如何配合使用，以允许不同语言编写的客户端和服务器之间进行通信。您需要确保服务器和客户端使用的是相同的 `.proto` 文件定义。





## gRPC基本流程
- gRPC hello world
	- 见 Learn go/src/grep-go-master/helloworld
- 四步骤
	- 定义pb文件（.proto文件） 
	- 利用命令行生成两个pb文件
	- 实现服务端
	- 实现客户端


- 设计模式——代理模式
	- 在k8s架构中，服务间通信基于gRPC
	- gRPC将各种client请求首先转发给虚拟proxy，由proxy处理连接到real subject，再返回
	- 中介隔离，开闭原则（对拓展开放，对修改关闭）
	- 分类：静态代理，动态代理（动态代理 real subject）

- proto3介绍
	- 数据类型
	- 编解码原理
	- 自定义插件：比如proto-gen-go-grpc
	- 通信流程

- proto3如何编码数据
	- ![[Pasted image 20240427224412.png]]
	- https://developers.google.com/protocol-buffers/docs/encoding
	- 这六种数据类型是所有proto3支持数据类型的编码方式
	- 可变字节长度编码
	- 消息编码——最后3位是数据类型
	- 符号整形，使用ZigZag算法


- gRPC通信流程
	- 服务端实现RPC接口，监听TCP端口，注册服务，启动服务。
	- 客户端创建TCP连接，创建Client，指向RPC调用，得到返回消息

- API Server
	- restful  API ->认证鉴权->K8S资源对象->Etcd


- proto文件创建
- 定义rpc（定义服务）
- 定义message（定义消息）——请求和响应消息、数据表模型消息
- 因为grpc是根据数据类型进行序列化的API接口，所以需要明确数据类型