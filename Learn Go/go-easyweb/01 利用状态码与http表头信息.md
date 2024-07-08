- 熟记状态码，利用状态码做标准返回
- 利用表头信息做校验
	- "Accept"代表url信息

- gin使用
	- 注册Engine
	- 注册中间件
	- 注册路由 及 响应函数（传 gin.Context返回一些信息）


- 用v1 v2控制版本
	- 就是添加一个v1路由


- 用group写路由组
	- 如果一个路由有多个方法，还有中间件。用group吧！
```go
package main

import (
    "github.com/gin-gonic/gin"
)

func main() {
    router := gin.Default()

    // 创建一个分组，并设置中间件和路径前缀
    authGroup := router.Group("/api", AuthMiddleware)
    {
        // 所有这些路由都会应用 AuthMiddleware 中间件
        authGroup.GET("/users", GetAllUsers)
        authGroup.POST("/users", CreateUser)
        // 其他路由...
    }

    // 启动Gin服务器
    router.Run()
}

// AuthMiddleware 是一个示例中间件，用于鉴权
func AuthMiddleware(c *gin.Context) {
    // 鉴权逻辑...
    c.Next() // 调用链中的下一个处理函数（可能是其他中间件或最终的处理函数）
}

// GetAllUsers 是处理获取用户列表的路由的处理函数
func GetAllUsers(c *gin.Context) {
    // 处理逻辑...
}

// CreateUser 是处理创建用户的路由的处理函数
func CreateUser(c *gin.Context) {
    // 处理逻辑...
}
```


- 用context对象的shouldBind获取post、put、patch请求下的请求体数据(一般是json)吧
```go
type User struct {
    Name    string `json:"name"`
    Age     int    `json:"age"`
    Email   string `json:"email"`
}

func main() {
    r := gin.Default()

    r.POST("/user", func(c *gin.Context) {
        var user User
        if err := c.ShouldBindJSON(&user); err != nil {
            c.JSON(http.StatusBadRequest, gin.H{"error": "Invalid request"})
            return
        }
        // 处理 user 结构体
        c.JSON(http.StatusOK, gin.H{"message": "User created", "user": user})
   })

   r.Run()
}
```