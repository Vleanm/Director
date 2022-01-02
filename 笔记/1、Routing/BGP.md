# 工作机制

## 	报文类型

- Open

  ```
  用于建立BGP邻居，仅发送一次
  ```

- Keepalive

  ```
  用于保持BGP连接
  ```

- Update

  ```
  用于在对等体之间交换路由信息
  ```

- Notification

  ```
  在BGP交互过程中发生错误时，报告错误并终端BGP连接
  ```

- Route-refersh

  ```
  用于改变路由策略后请求对等体重新发送路由信息
  ```

## 状态机制

- idle（空闲）

  ```
  初始化状态，此状态下BGP拒绝邻居发送的连接请求
  在操作者配置或重启进程时，才开始尝试建立TCP连接，并转至Connect状态
  ```

- Connect（连接）

  ```
  在Connect状态下，BGP启动重传计时器，开始建立TCP连接
  若TCP连接成功，则开始发送open报文，转至opensent状态
  若TCP连接失败，则BGP转至Active状态
  若TCP连接超时，则一直处于Connect状态并进行重传
  ```

- Active

  ```
  此阶段不断尝试进行TCP连接
  若TCP连接成功，则发送open报文，转至opensent状态
  若TCP连接失败，则停留在Active状态，继续尝试
  若TCP连接超时，则转至Connect状态
  ```

- Opensent

  ```
  此阶段BGP等待对端发送来的open报文，并检查open报文中的参数
  	AS号、router-id、协议版本号、认证类型与认证信息
  若参数匹配成功，则转至openconfirm状态，并持续发送keepalive报文保活
  若参数匹配失败，则发送notification报文，转至idle状态
  ```

- OpenConfirm

  ```
  此状态下，BGP等待接收keepalive和notification报文
  若收到keepalive报文则转至establish状态，若收到notification报文则转至idle状态
  ```

- Establish

  ```
  此状态下，BGP等待对等体交换Update、Keepalive、route-refersh报文及notification报文，进行路由信息交换
  如果收到正确的update或keepalive报文，则继续保持BGP连接
  若收到错误的update或keepalive报文，则发送notification报文，并转至idle状态
  若收到notification报文，则转至idle状态
  若TCP连接断裂，BGP则断开连接并转至idle状态
  ```

## BGP属性

- AS_Path
- Next_Hop
- Origin
- Local_pref
- MED
- 团体属性
- 簇ID

## BGP选路原则

- 协议首选值Prefval最高
- 本地优先级最高
- 手动聚合、自动聚合、network引入、import引入、对等体学习到
- AS_Path最短
- 起源者属性为IGP、EGP、incomplete
- 同一AS中MED最小的
- EBGP、IBGP
- 优选Next_Hop度量值最小的
- 簇ID最短的
- router-id最小的