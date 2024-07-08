在iptables中，不同的表（table）用于处理不同的网络包处理任务，每个表都有其特定的action（动作）。以下是一些常用表及其常用的action：

1. **raw表**：
   - `DROP`：丢弃数据包，不生成任何回应。
   - `ACCEPT`：接受数据包，允许其继续处理。
   - `LOG`：记录数据包信息，通常用于调试。
   - `RETURN`：结束当前链的处理，将数据包传递给内核的下一个处理阶段。

2. **mangle表**：
   - `MARK`：给数据包打标记，用于后续处理或跟踪。
   - `MASQUERADE`：源地址转换，用于NAT。
   - `NAT`：网络地址转换。
   - `REDIRECT`：重定向数据包到本地服务。
   - `DNAT`：目标网络地址转换。
   - `SNAT`：源网络地址转换。
   - `CHECKSUM`：修改数据包的校验和。
   - `LIMIT`：限制数据包的传输速率。
   - `TPROXY`：透明代理，允许iptables在网络层进行代理。

3. **nat表**：
   - `SNAT`（Source NAT）：改变数据包的源地址。
   - `DNAT`（Destination NAT）：改变数据包的目标地址。
   - `MASQUERADE`：自动选择源地址转换。
   - `REDIRECT`：将进入的数据包重定向到另一个端口。
   - `NETMAP`：将整个网络映射到另一个网络。

这些action可以用于不同的策略和配置，以实现网络流量的控制、转换和监控。例如，`mangle`表的`MARK` action常用于基于标记的路由选择或流量分类，而`nat`表的`SNAT`和`DNAT` action则用于实现网络地址转换，这对于访问控制和网络地址的隐藏非常有用。