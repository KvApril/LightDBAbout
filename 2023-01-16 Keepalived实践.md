### Keepalived实践

- #### 相关术语
| 缩写 | 全称 | 描述 |  
| --- | --- | --- |
| LB  | Load Balancer | 负载均衡 |
| HA  | High Available | 高可用 |
| Failover  | Failover | 失败切换 |
| Cluster  | Cluster | 集群 |
| LVS  | Linux Virtual Server Linux | 虚拟服务器 |
| DS  | Director Server | 前端负载均衡器节点 |
| RS  | Real Server | 后端真实的工作服务器 |
| VIP  | Virtual IP | 虚拟IP地址，直接面向用户请求，作为用户请求的目标IP地址 |
| DIP  | Director IP | 主要用于和内部主机通讯的 IP 地址 |
| RIP  | Real Server IP | 后端服务器的 IP 地址 |
| CIP  | Client IP | 访问客户端的 IP 地址 |
| VRRP | Virtual Router Redundancy Protocol | 虚拟路由冗余协议 |



- #### Keepalived简介 
```sql
Keepalived一个基于VRRP协议来实现的LVS服务高可用方案，主要实现真是机器的故障隔离及负载均衡器间的失败切换，提高系统的可用性
```

- #### Keepalived运行原理
```sql
Keepalived 是以 VRRP 协议为基础实现。  
VRRP协议中，将N台提供相同功能的路由器组成一个路由器组，组里有一个master和多个backup，  
在master上有一个对外提供服务的VIP,外部程序可以通过该IP访问这台服务器。如果master出现故障(断网，重启，keepalived crash)  
keepalived会其他backup机器上选出一台作为新的master，新的master分配同样的VIP。
```

- #### 选举策略  
```sql
根据VRRP协议，根据priority和weight来进行master的选举。  
会触发选举的条件有：
1.keepalived启动
2.master服务出现故障(断网，重启，master上的keepalived crash)
3.有新的备份服务器加入且权重更大
```

- #### 实践TODO
