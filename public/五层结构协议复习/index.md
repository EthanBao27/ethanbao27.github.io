# 🛜五层体系结构协议复习


# 🛜 五层体系结构协议复习

首先我们需要了解为什么计算机网络要学习**五层结构模型（TCP/IP 模型）**，因为这更简化，便于我们了解实际网络协议，并且更贴近现实的网络组成。

而七层模型（OSI）则是由**国际标准化组织 ISO**制定的，为网络设计提供了框架，这其实是一个概念模型，虽然在实际上不常用，但为网络设计提供了参考。

## 层

让我们来看一下五层和七层有哪些层次，七层实际上在五层基础上加了两层：

- 物理层 Physical
- 数据链路层 Data Link
- 网络层 Network
- 传输层 Transport

  - 会话层 Session
  - 表示层 Presentation

- 应用层 Application

对于学过计网的人来说，夹在第四和第五之间的两个层或许不太熟悉，我们来介绍一下。**会话层**负责建立、管理和终止应用程序之间的会话。可见主要是 Socket 的建立和关闭操作。因为**传输层**只负责端到端的可靠传输，而给上层提供接口靠的是 Session 层；再看看**表示层**，它负责处理数据表示形式，包括加密、解密、压缩和格式化，确保不同系统之间的数据能正确解释，例如 SSL/TLS 加密、JPEG 格式转换。可见表示层更切近应用层的具体应用。

让我们回归正题，复习一下五层结构的各类协议/技术。

## 物理层

实现设备之间的比特传输，定义物理连接（相邻的主机到主机）的标准。

- Ethernet（以太网）：有限网络的电气和信号标准。
- IEEE 802.11（Wi-Fi）：定义无线局域网的通信方式。
- 光纤：高速骨干网传输。
- Bluetooth（蓝牙）：短距离无线通信协议。

## 数据链路层

实现同一局域网中的节点（主机和主机）间通信，负责帧的封装、差错检测和流量控制。

- **Ethernet（以太网）**：最广泛的局域网数据传输协议，实现以太帧（Frame）格式的定义和介质访问控制。
- **PPP（Point-to-Point Protocal）**：提供点到点的链路传输，如电话拨号。_（也有特殊格式的帧）_
- **HDLC（High-Level Data Link Control）**：面向比特的链路控制协议，用来实现**时钟同步**。_（也有特殊格式的帧）_

- **ARP（Address Resolution Protocol）**：将 IP 地址解析为 MAC 地址，用于局域网通信，实现从三层到二层转换的方式。
- VLAN（Virtual LAN）技术：虚拟局域网，使得相同交换机下的主机可以分配到不同子网。

## 网络层

实现不同网络（局域网）之间的数据传输，负责路径选择和路由转发，实现 Internet 上主机到主机的连接。

- **IP（Internet Protocol）**：网络层核心，定义数据包的封装与路由，它的任务只是根据**数据包标头的 ip 地址，将数据包从源主机传送到目标主机**。
  - IPv4:广泛使用的协议，32 位 IP 地址，使用 ARP 协议地址解析。
  - IPv6：128 位地址，采用 NDP 协议。
- **ICMP（Internet Control Message Protocol）**：用于传递网络层错误消息和状态消息，广为流传的 ping 指令。它是依靠 IP 协议的协议，IP 数据包中有它。利用 TTL 将路由诊断信息返回到源。
- IGMP（Internel Group Management Protocol）：实现 IP 多播。

![路由协议分类](/images/routing.png)

### 路由协议

#### 距离向量路由协议 DVRP

距离向量路由协议（DVRP）也被称为“按**跳数**计算的路由算法”，其原理是：**每个节点都维护到达目的节点所需的距离，每次更新将本节点到所有其他节点的距离向量发送给相邻节点，相邻节点再将其发给相邻节点……直到所有节点的距离向量被更新。最终每个节点都得到了到达目的节点的最短距离。**

常见的距离向量路由协议有

- RIP（Routing Information Protocol）
- IGRP（Interior Gateway Routing Protocol）

#### 链路状态路由协议 LSRP

链路状态路由协议（LSRP）也被称为“基于**状态**的路由算法”，其原理是**每个节点都把自己的链路状态信息发给相邻节点，相邻节点保存下来并传递给其它相邻节点。当所有节点都交换完成链路状态信息之后，每个节点通过计算最短路径算法得到网络的最短路径。**

常见的链路状态路由协议有

- OSPF（Open Shortest Path First）
- IS-IS（Intermediate System to Intermediate System）

#### 静态路由协议

在静态路由协议中，网络管理员**手动配置路由表**，然后**路由器依据配置的路由表进行数据包的转发**。

静态路由协议的**缺点**是不灵活，不能及时响应网络拓扑结构的变化。

#### 动态路由协议

动态路由协议可以根据网络拓扑结构的变化自动调整路由表，路由表的计算是通过运行路由协议来完成的。动态路由协议虽然比静态路由协议更复杂，但是具有灵活、自适应、可靠的优点。

常见的动态路由协议有

- BGP
- OSPF
- IS-IS
- RIP
- IGRP
- EIGRP
- OSPFv3

#### 单播、多播、组播路由协议

单播路由协议是指进行单播转发的路由协议。多播路由协议是指进行多播转发的路由协议。组播路由协议是一种组播数据包传输的路由协议，与多播路由协议类似。

#### 内部网关协议和外部网关协议

内部网关协议（IGP）是指在一个企业或组织内部部署、用于内部路由器之间通信的协议，如 RIP、IGRP、EIGRP、OSPF 和 IS-IS 等。外部网关协议（EGP）是指在不同的自治系统之间进行路由选择的协议，如 BGP,各大 ISP 进行连接使用的是 BSP 协议（使用 TCP 连接）。

#### 工作原理

路由协议的工作原理可以分为四个步骤：

- 邻居发现
- 路由表建立
- 路由表维护
- 路由表选择

在选择适合特定网络环境的路由协议时，需要综合考虑网络规模、复杂性、性能需求和管理能力。通常，大型企业网络和互联网使用链路状态协议（如 OSPF 和 IS-IS），而小型网络可能会选择距离向量协议（如 RIP）。同时，BGP 在连接自治系统之间的路由选择方面具有广泛的应用。

> 以上路由协议部分内容均为[这篇华为云社区文章](https://bbs.huaweicloud.com/blogs/399479)的原创内容，本博客仅作为个人学习使用，无抄袭意图，了解详情请点击超链接进入此博客观看。

## 传输层

提供端到端（port）的数据传输服务，确保数据包按顺序、无误地传输到目的主机。

- **TCP（Transmission Control Protocol）**：面向连接、可靠的协议，提供可靠的数据传输和流量控制。适用于需要保证数据完整性的应用，如 HTTP、FTP。
- **UDP（User Datagram Protocol）**：无连接协议，不保证数据的可靠传输，但传输速度快。适用于实时应用，如视频流、在线游戏，DNS 解析也用 UDP。

![握手过程](/images/transmission.jpg)

- SCTP（Stream Control Transmission Protocol）：同时支持多流传输的协议，适用于 VoIP 等应用。

TCP 与 UDP 的区别之一是**重传丢失的数据**。在 TCP 协议中，每个数据包都被赋予一个**唯一的序列号**。数据包发送者仔细跟踪发送了哪些数据包。作为响应，接收系统发出一个 ACK 数据包（代表“确认”），其中包含确认收到的数据包的序列号。如果序列号不匹配或丢失，发送机器将重新发送数据包。这个过程会持续下去，直到匹配的 ACK 确认传输成功。

第二大区别是通过**三向握手建立持久化连接**。在 TCP 中，三向握手是一种通信机制，以确保所有数据的发送和正确接收。简而言之，这发生在三个部分：

- 初始化(SYN)：SYN 是想要建立通信的设备发出的初始数据包。该数据包包含同步标志(SYN)和接收者的 IP 地址。
- **确认启动(SYN-ACK)：**接下来，接收者发回 SYN-ACK 数据包，假设它已准备好并愿意进行通信。
- **最终确认(ACK)：**一旦发送方收到 SYN-ACK，就会发送最终 ACK 以确认有效连接。

TCP 面向连接是依赖于它的**错误检测和流量控制**等其他功能*（在后文）*，这些特性成为 TCP 面向连接的本质的支柱。

#### TCP 错误检测和流量控制简述

应用需要传输的数据可能会非常大，如果直接传输就不好控制，因此当传输层的数据包大小超过 MSS（TCP 最大报文段长度） ，就要将数据包分块，这样即使中途有一个分块丢失或损坏了，只需要重新发送这一个分块，而不用重新发送整个数据包。在 TCP 协议中，我们把每个分块称为一个 TCP 段（TCP Segment）（IP 是超过 MTU 1500 Bytes，即以太网帧的载荷后分片，⚠️ 注意分片标识、偏移量、MF 更多分片标识、DF 禁止分片标识这些概念）。TCP 段对应标识有 Seq 序列号（如何组装在一起）、确认号（希望接受的下一个字节的序号，累积确认机制标识以及成功接受的数据，未确认的字节等待重传）、FIN 关闭连接等。

在 TCP 报文段的头部中，有一个 **16 位窗口字段**，用于表示接收方的缓冲区可用空间大小。**窗口大小值** 告诉发送方当前允许发送的未确认数据量。通过该字段，发送方可以动态调整发送速度，确保不会导致接收方的缓冲区溢出。如果接收方缓冲区剩余空间减少，它会将窗口大小缩小；如果可用空间增大，它会增大窗口大小。

## 应用层

为应用程序提供网络服务接口，用户可以直接使用这些协议访问网络资源。

- **HTTP/HTTPS（Hypertext Transfer Protocol）**：用于传输网页数据，HTTPS 在 HTTP 基础上添加了加密（TLS/SSL）。

  ![ssl1](/images/ssl1.png)

  ![ssl2](/images/ssl2.png)

- **FTP（File Transfer Protocol）**：用于文件传输，支持上传和下载功能。

- **SMTP（Simple Mail Transfer Protocol）**：用于电子邮件发送。

- **POP3（Post Office Protocol 3）** 和 **IMAP（Internet Message Access Protocol）**：用于电子邮件接收，IMAP 支持邮件同步。

- **DNS（Domain Name System）**：将域名解析为 IP 地址。

![ssh](/images/ssh.png)

- **Telnet** 和 **SSH**：用于远程登录，SSH 提供了加密的传输。

- **DHCP（Dynamic Host Configuration Protocol）**：用于自动分配 IP 地址。

#### URL（uniform resource identifier，统一资源定位符）

在 HTTP 协议中，用来标识唯一的资源。

Web 上可用的每种资源如 HTML 文档、图像、视频片段、程序等都是一个来 URI 来定位的

##### URL 组成

1. 访问资源的命名机制
2. 存放资源的主机名
3. 资源自身的名称，由路径表示，着重强调于资源。

#### HTTP Request

![http request](/images/http1.webp)

```http
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host    img.mukewang.com
User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept    image/webp,image/*,*/*;q=0.8
Referer    http://www.imooc.com/
Accept-Encoding    gzip, deflate, sdch
Accept-Language    zh-CN,zh;q=0.8
```

```http
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```

#### HTTP Response

![http response](/images/http2.webp)

```tex
GET     请求指定的页面信息，并返回实体主体。
HEAD     类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
POST     向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
PUT     从客户端向服务器传送的数据取代指定的文档的内容。
DELETE      请求服务器删除指定的页面。
CONNECT     HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
OPTIONS     允许客户端查看服务器的性能。
TRACE     回显服务器收到的请求，主要用于测试或诊断。
```

关于 HTTP 协议，详情可参考[这篇博客](https://www.cnblogs.com/ranyonsue/p/5984001.html)

#### SSL&SSH

SSH 和 SSL(都是网络安全协议，通过加密和认证提升两台设备间传输数据的安全性。但 SSH 和 SSL 的生效方式和服务目标存在差异。

SSH 在两台设备间创建**安全隧道**，使这两台设备间可以安全地发送命令、传输数据等。例如，客户端通过 SSH 远程登录到一台服务器上，就可以安全地远程管理这台服务器，在服务器上执行想要的命令。

SSL 则是使用**SSL 证书**保证两台设备间安全地传输数据，而不是像 SSH 那样可以执行命令。例如，用户通过浏览器访问某安装了 SSL 证书且启用了 HTTPS 的服务器，浏览器和服务器之间可以安全地传输数据。

SSH 就像一辆汽车，我们看不到这辆封闭的汽车里装载的是什么。而 SSL 就像一个封闭的集装箱，我们可以用不同的交通工具运输它，但看不到集装箱里装的是什么。

