- go通过返回两个值（目标值和error）实现错误处理
- panic——终止程序
```go
func main() {
    if someCondition {
        panic("unexpected error")
    }
}
```
- panic后会执行所有defer，一般用defer关闭数据库或文件或网络连接等
```go
f, err := os.Open("file.txt")
if err != nil {
    return err // 如果打开文件失败，直接返回错误
}
defer f.Close() // 注册文件关闭操作，无论函数如何退出都会执行

// 函数的其余部分
```

