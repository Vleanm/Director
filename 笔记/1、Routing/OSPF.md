链路状态路由协议

支持VLSM与CIDR

支持区域认证与链路认证

域内路由协议（IGP）

# 介绍

- OSPF（open shortest path first  |  开放最短路径优先协议）
- 采用SPF最短路径算法，基于节点接口状态（带宽、延迟、利用率）运行SPF算法，进行最短路径计算
- 协议有两个版本，V2用于IPV4，V3用于IPV6，CISCO中V3以支持双栈
- OSPF工作过程分为：邻居发现，路由信息交换，路由计算，路由维护

# 工作过程详解

## 	状态机制

- 失效状态（DOWN）

  ```
  当一台路由器在最近的一个RouterDeadInterval时间内未收到hello数据包时，处于当前状态
  除了NBMA网络，其他网络类型中不会向失效路由器发送hello数据包
  当一台路由器转到失效状态时，会清除链路状态重传列表、数据库摘要列表和链路状态请求列表
  ```

- 尝试状态（Attempt）

  ```
  仅会出现在NBMA网络中
  NBMA网络中，邻居关系手工指定
  在NBMA网络中，被指定为DR或具备DR选举资格的路由器会将其他路由器置位Attempt状态
  ```

- 初始化状态（init）

  ```
  在接收到对端路由器发送的hello数据包，但还未进入two-way状态时，处于当前状态
  处于当前状态的路由器会在发送的hello数据包中填充邻居字段
  会将所有处于当前状态或更高状态的路由器ID封装到邻居字段中
  ```

- 双向通信（2-way）

  ```
  路由器已经收到来自邻居路由器的hello数据包，且数据包中的邻居字段包含自己的路由器ID
  若处于init状态的路由器在收到邻居路由器发送的DBD数据包后，会直接转至2-way状态
  在此状态下会进行DR/BDR选举
  ```

- 开始交换（ExStart）

  ```
  处于当前状态的路由器会进行主从选举
  注意区分主从选举与DR/BDR选举是不同的
  DR/BDR决定MA网络中由那台路由器与其他路由器交换路由信息（多个路由器之间），通过Hello数据包进行选举
  主从选举是选举一个路由器来控制信息交换（两个路由器之间），通过DBD数据包进行选举
  ```

- 信息交换（Exchange）

  ```
  处于当前状态的路由器向其邻居路由器发送可以描述自己链路状态的DBD报文
  在此状态下，若收到了来自邻居路由器的DBD数据包，也会发起LSR请求，有别于下一个状态，在此状态下未能发送LSU
  ```

- 信息加载（Loading）

  ```
  本地路由器会向他的邻居路由器发送LSR、LSU及LSACK来进行LSA加载
  ```

- 临街状态（Full）

  ```
  双方路由器完成LSA数据包交互，路由收敛完成
  ```

## 邻居关系建立条件

```
1、Area区域ID一致
2、router-id不一致
3、hello、Dead时间一致
4、特殊区域标识一致
5、认证类型与认证信息一致
6、MA网络中掩码长度一致
7、同为单播或组播
```

##  数据包

- Hello

  ```
  周期发送，用来发现邻居、维持邻居关系、选举DR/BDR
  ```

- DBD（Database Description packet）

  ```
  描述本地LSDB摘要信息，用于两台路由器同步链路状态数据库，同时还用与主从关系选举
  ```

- LSR（Link State Request packet）

  ```
  用与向对方请求所需的LSA，只有在双方成功交换DBD数据包后才会发送LSR
  ```

- LSU（Link State Update packet）

  ```
  用于回复LSR请求，向对方发送其所需要的LSA
  ```

- LSACK（Link State Acknowledgment packet）

  ```
  用来对收到LSU后对收到的LSA信息进行确认
  ```

##   LSA类型

- Router-LSA（Type1）

  ```
  在每个设备上都会产生，用来通告本设备上的路由信息（末节网络）与拓扑信息（MA网络或虚链路）
  ```

- Network-LSA（Type2）

  ```
  由DR路由器产生，用来描述MA网络中的路由信息
  ```

- Network-summary-LSA（Type3）

  ```
  由ABR产生，用来向另一个区域传递本区域内的路由信息
  ```

- ASBR-summary-LSA（Type4）

  ```
  由ABR产生，用于向除了自己所在区域之外的所有区域宣告到达ASBR的路由信息
  ```

- AS-external-LSA（Type5）

  ```
  由ASBR产生，描述AS外部的路由信息，用于将外部路由导入本AS
  ```

- NSSA LSA（Type 7）

  ```
  由ASBR产生，描述AS外部的路由信息，此LSA只在NSSA区域内传输
  ```

- Opaque LSA（Type9/Type10/Type11）

  ```
  用于OSPF扩展
  ```

##   网络类型

- 广播（Broadcast）

  ```
  hello时间10s
  ```

- P2P

  ```
  hello时间10s
  ```

- NBMA 

  ```
  需要手工指定邻居关系
  hello时间30s
  ```

- P2MP

  ```
  hello时间30s
  ```

##   更新方式

```
OSPF采用触发更新机制，同时OSPF会进行30分钟周期LSA状态刷新，以确保LSA序列号不至于太大，导致无法判断LSA新旧程度
OSPF采用组播或单播更新，组播地址224.0.0.5在所有网络中都会使用，组播地址224.0.0.6只在MA网络中使用
```

# 特殊区域

## 	Stub

```
区域内抑制4、5类LSA的传播，生成一条3类缺省LSA指向ABR
此区域不能进行路由重发布，不能建立虚链路
处于此区域得到路由器其hello数据包中特殊区域标识E位置0
```

## 	Totally Stub

```
区域内抑制4、5、3类LSA，生成一条3类缺省LSA指向ABR
```

## 	NSSA

```
允许外部路由通告
外部区域通过NSSA区域与OSPF进行路由信息交换
NSSA区域内路由器之间以7类LSA的形式传递域外路由信息
在NSSA区域的ABR上会将7类LSA转为5类LSA（携带FA地址），并产生一条4类LSA
```

## 	Totally NSSA

```
在NSSA的基础上，过滤3类LSA，产生3类缺省LSA
```

