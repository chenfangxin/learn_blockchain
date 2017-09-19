# P2P通信

P2P通讯协议主要处理`NAT穿越`的问题。

## NAT介绍

内网地址范围： `10.0.0.0~10.255.255.255`, `172.16.0.0~172.31.255.255.255`, `192.168.0.0~192.168.255.255`

网络地址转换(NAT, Network Address Translation)是`RFC3022`定义，用于内网地址向公网地址的转换，以解决公有IP地址稀缺的问题，目前NAT的应用不限于此。

根据`RFC3022`，NAT分为两大类：`基本NAT`和`网络地址/端口转换`(NAPT, Network Address/Port Translation)

`基本NAT`仅仅将内网地址转换为公网地址，并不修改TCP/UDP端口信息。基本NAT又分为`静态NAT(Static NAT)`和`动态NAT(Dynamic NAT)`。

+ 静态NAT：内网地址与公网地址静态一一对应。静态NAT实际上是双向的，在使用静态NAT场景下，内网主机也可以对外提供服务。
+ 动态NAT：内网地址与公网地址的对应关系是动态的。

在运行时，`基本NAT`只修改数据包的IP地址，而且内网地址与外网地址是对应的，并没有节省公网地址，所以现在很少被使用。绝大多数NAT都是`NAPT`。

`网络地址/端口转换(NAPT)`是指将内部地址映射到外网地址的不同端口上，采用端口复用的方式，实现内网所有主机共用*一个*合法的外网IP，实现对外网的访问。

NAPT可分为`锥形NAT(Cone NAT)`和`对称NAT(Symmetric NAT)`。`锥形NAT`是指同一个内网地址和端口发出的包，经过NAT之后会转换为同一个外部地址和端口，不具备这一特性的NAPT，就被称为`对称NAT`。

锥形NAT又分为：`完全锥形NAT(Full Cone NAT)`, `受限制锥形NAT(Restricted Cone NAT)`和`端口受限制锥形NAT(Port Restricted Cone NAT)`。举个例子来说明这三种锥形NAT的区别：假设内网主机(192.168.0.8:4000)发往外网的报文，经过NAT以后源地址和端口映射为(1.2.3.4:6200)，那么：

+ 完全锥形NAT(Full Cone NAT): 任何NAT网关收到的目的地址为(1.2.3.4:6200)的报文，都会被转发给内网主机(192.168.0.8)
+ 受限锥形NAT(Restricted Cone NAT): 只有内网主机(192.168.0.8)先给外网服务器(5.6.7.8)发送一个报文后，NAT网关收到的来自外网服务器(5.6.7.8)的，且目的地址为(1.2.3.4:6200)的报文，才会被转发给内网主机(192.168.0.8)。也就是限制了回包的源地址。
+ 端口受限锥形NAT(Port Restricted Cone NAT): 只有内网主机(192.168.0.8)先给外网服务器(5.6.7.8:8000)发送一个报文后，NAT网关收到的来自外网服务器(5.6.7.8:8000)，且目的地址为(1.2.3.4:6200)的报文，才会被转发给内网主机(192.168.0.8)。也就是回包的源地址和端口都受限。

## NAT穿越(NAT Traversal)

  在实际场景中，绝大多数用的是`NAPT`，也就是会NAT网关会改变报文的源地址和端口。而某些应用层协议需要使用传输层的端口信息，一些应用层协议甚至会会携带下层的地址和端口信息，这样如果经过NAT后，修改了报文的源地址和端口，但是没有修改应用层里的信息，应用层协议就会失败。为了让应用层协议穿过NAT运作，需要NAT穿越(NAT Traversal)技术。

目前常用的`NAT穿越`技术如下：

+ UPnP: Universal Plug an Play
+ ALG: Application Layer Gateway
+ STUN: Session Traversal Utilities for NAT/Simple Traversal of UDP Through NAT
+ TURN: Traversal Using Relays around NAT
+ ICE: Iteractive Connectivity Establishment

#### STUN介绍

有两个协议的缩写是STUN，分别是`RFC3489(Simple Traversal of UDP Through NAT)`和`RFC5389(Session Traversal Utilities for NAT)`。

#### TURN介绍

`RFC5766(Traversal Using Relays around NAT)`

#### ICE介绍

`RFC5245(Iteractive Connectivity Establishment)`

