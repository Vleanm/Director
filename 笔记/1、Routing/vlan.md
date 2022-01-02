### 交换机基础配置：

```
查看接口信息
	display interface G0/0/1
进入接口
	interface G0/0/1
关闭接口自动协商模式
	undo negotiation auto
更改接口带宽
	speed 100
禁止终端显示信息
	undo terminal monitor 
更改接口双工模式
	duplex half(半双工)	duplex full(全双工)
```

MAC相关

```
显示MAC地址表
	display mac-address
添加静态MAC地址条目(静态条目不会老化)
	mac-address static 0001-2233-4455 E0/0/3 vlan1
显示MAC地址表老化时间
	display mac-address aging-time
更改MAC地址表老化时间(交换机默认300s)
	mac-addtess aging-time 200
```

MAC洪泛攻击

```
攻击方在同一时间大量的向目标交换机发送大量MAC地址不相同的数据包，从而导致交换机在短时间内学习到过多MAC条目，
当交换机无法再添加新的MAC地址条目时，再次发送来的数据包中目的MAC未知，
则该数据包会被进行洪泛，从而完成网络监听的目的
```

VLAN

```
作用：限制广播域，同一VLAN的主机之间可以互相通信
帧格式：
	在源MAC后添加TAG标签，tag标签中含有以下字段：
		TPID(Tag Protocol Identifier)2字节,固定值0x8100标志这是802.1q封装
		TCI(Tag Control Information)2字节
			PRI(Priority)3位，帧优先级
			CFI(Canonical Format Indicator)1位，为0表示以太网，
				为1表示FDDI或令牌环网
			VLAN ID(VLAN Identifier)12位，1-4094
VLAN划分：
	基于port、MAC、IP、协议、策略进行VLAN划分
交换机端口类型：
	Access：
		终端接入端口，只能属于一个VLAN，只有一个PVID，只能传输一个VLAN的数据
		数据包进接口：
			若不存在标签，则添加本接口PVID标签
			若存在标签，并和本地不同，则丢弃，相同则接收
		数据包出接口：
			若和本接口PVID相同，则去标签，否则丢弃
	Trunk：
		只有一个PVID，可以通过多个VLAN的数据包
		数据包进接口：
			若不存在标签，则插入本地PVID再处理
			若存在标签，则判断本地是否允许此VLAN，允许则接收，否则丢弃
		数据包出接口：
			判断标签是否与接口PVID相同，相同则去标签转发，不同则保留标签
	Hybrid：
		数据包进接口：
			若不存在标签，则插入本地PVID再处理
			若存在标签，则判断本地是否允许此VLAN，允许则接收，否则丢弃
		数据包出接口：
        	判断端口是否允许该VLAN通过，不允许则丢弃
        	
        	
VLAN配置：
	创建VLAN：
		vlan 2
	配置接口模式并划分VLAN：
		interface e0/0/1
		access:
			port link-type access
			port default vlan 2
		trunk:
			port link-type trunk
			port trunk allow-pass vlan 2
			port trunk pvid vlan 2
		hybrid:
			port link-type hybrid
			port hybrid pvid vlan 2
			port hybrid untagged vlan 1 2 3
			port hybrid tagged vlan 1
	查看VLAN信息：
		查看所有vlan：
			display vlan
			display vlan summary
		查看接口模式，及接口标签列表
			display port vlan 
			display port vlan active
```
