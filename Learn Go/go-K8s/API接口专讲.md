# API接口的本质
- 序列化
- 反序列化
- 服务操作定义
- 服务通信流程（网络传输协议）

常见：restful、grpc、websocket、graphql、webhooks




# http超文本传输协议

- 一次请求和应答——Http连接（一旦三次握手建立，就可以开始一次请求，这是应用层实现）
- 流式请求和应答——三次握手 TCP连接（TCP层实现）
- 先三次握手，再一次请求和应答
- 详解三次握手
	- [一篇文章帮你拿下面试八股文之网络三次握手四次挥手， HTTP超文本传输协议重点理论刨析到实现简单的HTTP服务, 思考着图解着学习网络 (咱不死记硬背)_三次握手 四次挥手 八股文-CSDN博客](https://blog.csdn.net/weixin_53695360/article/details/123189672)


[流行的几种API接口模式：RESTful、GraphQL、gRPC、WebSocket、Webhook-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1448316)
[[restful接口]]
# https
 - 在http基础上再tcp连接建立后增加 SSL/TLS握手
 - SSL/TLS握手
	 - **客户端发起**：客户端发送一个“开始SSL握手”的消息到服务器，表明它要使用SSL/TLS加密。
	- **服务器响应**：服务器响应其SSL证书，包含了公钥和证书信息。
	- **客户端验证**：客户端验证服务器的SSL证书的有效性（如证书是否由受信任的证书颁发机构签发）。
	- **密钥交换**：使用Diffie-Hellman密钥交换算法在客户端和服务器之间生成一个唯一的会话密钥。
	- **握手完成**：一旦密钥交换完成，SSL/TLS握手结束，建立了一个加密的通信通道。
- 使用https原因
	- **数据加密**：HTTPS使用SSL/TLS协议加密数据，可以防止数据在传输过程中被窃听。
	- **数据完整性**：SSL/TLS协议确保传输的数据在传输过程中没有被篡改。
	- **身份验证**：通过SSL证书，HTTPS可以验证服务器的身份，防止中间人攻击。
	- **提高信任**：使用HTTPS可以增加用户对网站的信任，特别是在处理敏感信息（如密码、支付信息）时。
	- **搜索引擎优化**：Google等搜索引擎优先索引使用HTTPS的网站，因为它们认为这些网站更安全。
	- **合规性**：某些行业法规要求必须使用HTTPS来保护用户数据。
# rpc

### RPC调用示例

假设我们有一个简单的RPC服务，它提供了一个`Greet`方法，接受一个`name`参数，并返回一个问候语。

**服务端代码（RPC Server）**:

```go
package main

import (
	"fmt"
	"net"
	"net/rpc"
)

type Args struct {
	Name string
}

type GreetingService struct{}

func (t *GreetingService) Greet(args *Args, reply *string) error {
	*reply = "Hello, " + args.Name + "!"
	return nil
}

func main() {
	server := rpc.NewServer()
	server.Register(new(GreetingService))

	listener, _ := net.Listen("tcp", ":1234")
	fmt.Println("Listening on :1234")
	server.Accept(listener)
}
```

**客户端代码（RPC Client）**:

```go
package main

import (
	"fmt"
	"net/rpc"
	"log"
)

func main() {
	client, err := rpc.Dial("tcp", ":1234")
	if err != nil {
		log.Fatal("dialing:", err)
	}

	args := Args{Name: "Alice"}
	var reply string
	err = client.Call("GreetingService.Greet", &args, &reply)
	if err != nil {
		log.Fatal("calling:", err)
	}

	fmt.Println(reply) // Should print: Hello, Alice!
}
```

### HTTP调用示例

假设我们有一个HTTP服务，它提供了一个API端点`/greet`，接受一个`name`查询参数，并返回一个问候语。

**服务端代码（HTTP Server）**:

```go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
)

type GreetingResponse struct {
	Message string `json:"message"`
}

func Greet(w http.ResponseWriter, r *http.Request) {
	name := r.URL.Query().Get("name")
	if name == "" {
		http.Error(w, "Name not provided", http.StatusBadRequest)
		return
	}
	response := GreetingResponse{Message: "Hello, " + name + "!"}
	json.NewEncoder(w).Encode(response)
}

func main() {
	http.HandleFunc("/greet", Greet)
	fmt.Println("Server listening on :8080")
	http.ListenAndServe(":8080", nil)
}
```

**客户端代码（HTTP Client）**:

```go
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"io"
	"net/http"
	"os"
)

func main() {
	url := "http://localhost:8080/greet?name=Bob"
	resp, err := http.Get(url)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer resp.Body.Close()

	var response GreetingResponse
	err = json.NewDecoder(resp.Body).Decode(&response)
	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Println(response.Message) // Should print: Hello, Bob!
}
```

### 区别
1. **协议**：RPC通常使用自定义协议（如gRPC使用的Protocol Buffers），而HTTP调用使用HTTP协议。
2. **传输层**：RPC可以基于TCP、HTTP/2等，而HTTP调用基于HTTP。
3. **语义**：RPC调用更接近直接函数调用，具有强类型和面向对象特性；HTTP通常以资源为中心，使用CRUD操作。
4. **数据格式**：RPC可以自定义数据序列化方式，而HTTP常用JSON或XML。
5. **安全性**：RPC和HTTP都可以实现安全性，但HTTP安全性实现更为普遍和标准化。
6. **发现和负载均衡**：HTTP调用可以通过URL直接访问，而RPC可能需要服务发现机制。
7. **跨语言**：HTTP接口更容易跨语言实现，而RPC可能需要特定语言的客户端库。

在实际应用中，选择RPC还是HTTP调用取决于具体的业务需求、性能要求、开发资源和团队熟悉度。


# SSH远程登录网络协议

# FTP文件传输协议

# TCP/UDP
