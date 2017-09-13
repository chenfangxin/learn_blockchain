# P2P通信

P2P通讯协议主要处理`NAT穿越`的问题。

## NAT介绍

内网地址范围： `10.0.0.0~10.255.255.255`, `172.16.0.0~172.31.255.255.255`, `192.168.0.0~192.168.255.255`

网络地址转换(NAT, Network Address Translation)是`RFC3022`定义，用于内网地址向公网地址的转换，以解决公有IP地址稀缺的问题，目前NAT的应用不限于此。

根据`RFC3022`，NAT分为两大类：`基本NAT`和`网络地址/端口转换`(NAPT, Network Address/Port Translation)

`基本NAT`仅仅将内网地址转换为公网地址，并不修改TCP/UDP端口信息。基本NAT又分为`静态NAT(Static NAT)`和`动态NAT(Dynamic NAT)`。

+ 静态NAT：内网地址与公网地址一一对应。静态NAT实际上是双向的，在使用静态NAT场景下，内网主机也可以对外提供服务。
+ 动态NAT：内网地址与公网地址的对应关系是动态的

`网络地址/端口转换(NAPT)`是指将内部地址映射到外网地址的不同端口上，采用端口复用的方式，实现内网所有主机共用*一个*合法的外网IP，实现对外网的访问。

NAPT可分为`锥形NAT(Cone NAT)`和`对称NAT(Symmetric NAT)`。`锥形NAT`是指同一个内网地址和端口发出的包，经过NAT之后会转换为同一个外部地址和端口。

锥形NAT又分为：`完全锥形NAT(Full Cone NAT)`, `受限制锥形NAT(Restricted Cone NAT)`和`端口受限制锥形NAT(Port Restricted Cone NAT)`。


## NAT穿越

目前常用的`NAT穿越`技术如下：

+ STUN: Session Traversal Utilities for NAT/Simple Traversal of UDP Through NAT
+ TURN: Traversal Using Relays around NAT
+ ICE: Iteractive Connectivity Establishment

#### STUN介绍

有两个协议的缩写是STUN，分别是`RFC3489(Simple Traversal of UDP Through NAT)`和`RFC5389(Session Traversal Utilities for NAT)`。


#### TURN介绍

`RFC57766(Traversal Using Relays around NAT)`

#### ICE介绍

`RFC5245(Iteractive Connectivity Establishment)`

