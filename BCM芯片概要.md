[TOC]

# BCM芯片概要

## 系统逻辑视图

-[ ] Ingress Chip 

1. 解析报文128字节的头部（MMU cell 的最小单位）
2. 隧道终结 
   1. 报文头分类，以决定VRF 
3. 通过VRF与报文头的信息，进行L2/L3/MPLS查找 
4. 入口ACL处理：基于ACL进行计数与统计
5. 报文缓存、准入控制与调度
6. 修改报文（如基于报文类型进行修改）

-[ ] Switch Fabric

1. 基于HiGig头部信息进行报文的交换选路
2. 多播处理
3. 支持基于服务的流控制

-[ ] Egress Chip

1. 解析HiGig报文头部
2. 根据HiGig头部信息决定出端口
3. 报文缓存、准入控制与调度
4. 修改报文
5. 出口ACL处理

![1.0](D:\RGOS_group4\学习文档\1.0.PNG)

## 芯片逻辑视图

![1.1](D:\RGOS_group4\学习文档\1.1.png)

TCAM是三态查找，其表内的所有条目都可以并行访问，比如，如果你有100条ACL，TCAM能够一次性就对着100条ACL进行对比操作，过去如果有100条ACL的话，需要第一条ACL对比完后才能对第二条，第三条，直至N条，效率很面向没有TCAM高。

## 交换架构

- [ ] 采用模块化、高性能的管道式报文交换处理结构。在管道上的每一个模块都有各自的处理功能。并把处理结果提交给下一个模块进行处理。

      * Intelligent Parser ：包括两个独立的解析器：全解析器和HiGig解析器。全解析器负责来自端口与CMIC的报文（面板口与CPU口），需要的信息都可以在头128字节获得，全解析器必须要保存所有的解析信息。以备各种搜索引擎时候，HiGig解析器负责解析来自HiGig口的报文。
      * Sercurity Engine: 早期的硬件安全检测机制，防止DoS攻击。
      * L2 Switch: 分配VLAN、优先级、源MAC学习，目的MAC查找。
      * L3 Routing: 源/目的IP查找。
      * ContentAware Processing： CAP用来提供ACL、差分服务、QoS类型的应用。图中的IFP、EFP即是CAP。
      * Buffer Management： 控制端口的传输行为与流量整形。
      * Modification： 根据搜索引擎的结果，进行VLAN转换、隧道分装与L3路由变更。 

      ​









1. ###### ​,,​,,​,,​



