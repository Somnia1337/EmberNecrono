DHCP 提供 ~ 确认的 `yiaddr` 均为客户端选中的候选 IP。

![[MSS 与 MTU.png|500]]

路由选择协议：

- *RIP* 基于 距离向量(DV) 算法
- *OSPF* 基于 链路状态(LS) 算法

> [!tip] 计算某子网的网络地址、广播地址、主机地址范围、主机数量
> 
> 已知任一接口的 IP 地址 `ip` 和子网掩码 `mask`：
> 
> - `网络地址 = ip & mask`
> - `广播地址 = network_addr | (!mask)`（`网络地址` 的 *主机位* 全变为 `1`）
> - 主机地址范围为 `(网络地址 + 1)..=(广播地址 - 1)`
> - 主机数量为 `pow(2, 主机位数) - 2`

报文首部开销：

```text
- UDP:   8 Bytes
- TCP:  20 Bytes
- IPv4: 20 Bytes (addr  32 bits)
- IPv6: 40 Bytes (addr 128 bits)
```

*MSS* 不包含 TCP 头部长度。

$\rm EstimatedRTT_{new} = (1 - \alpha) \cdot EstimatedRTT_{old} + \alpha \cdot SampleRTT$

$\rm rwnd = rbuf - (LBRcvd - LBRead)$

发送 $P$ 个分组的端到端时延：$d = (N + P - 1) {\large \frac{L}{R}}$