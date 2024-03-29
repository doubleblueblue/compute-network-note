## 计算机网络-自顶向下方法

### 1 计算机网络和因特网

#### 1.1 因特网

​		与因特网连接的设备称为**主机**^【因为容纳/运行应用程序】^或**端系统**^【因为位于因特网的边缘】^。

​		主机分为**客户端**和**服务器**。

​		端系统彼此交换**报文**。端系统通过**通信链路**和**分组交换机**连接到一起。

​		通信链路的**传输速度**的单位是bit/s。一台端系统向另一台端系统发送报文时，发送端将报文分段并为每段加上首部字节，由此形成的信息包称为**分组**。

​		交换机主要包括**路由器**和**链路层交换机**。链路层交换机通常用于接入网中，而路由器通常用于网络核心中。从发送端系统到接收端系统，一个分组所经历的一系列通信链路和分组交换机称为通过该网络的**路径**。

​		端系统通过**因特网服务提供商**接入因特网。

​		端系统、分组交换机和其他因特网部件都要运行一系列**协议**，这些协议控制因特网中信息的接收和发送。**IP**协议定义了在路由器和端系统之间发送和接收的分组格式。因特网的主要协议统称为**TCP/IP**。

​		**协议**定义了两个或多个通信实体之间交换的报文的格式和顺序，以及发送/接收一条报文或其他事件所采取的动作。

​		**因特网标准**由**因特网工程任务组**研发，其标准文档称为**请求评论**。

​		应用程序涉及多个相互交换数据的端系统称为**分布式应用程序**。

​		与因特网相连的端系统提供了一个**套接字接口**，该接口规定了运行在端系统上的程序请求因特网基础设施向运行在另一个端系统上的特定目的地程序交付数据的方式。

#### 1.2 网络边缘

​		**接入网**指将端系统物理连接到其边缘路由器的网络。**边缘路由器**是端系统到任何其他远程端系统的路径上的第一台路由器。

​		家庭接入：DSL、电缆、FTTH、拨号和卫星

​		企业/家庭接入：以太网和WiFi

​		广域无线接入：3G/4G/5G和LTE

​		**物理媒体**包括**导引型媒体**^【电波沿着固态媒体传播】^和**非导引型媒体**^【电波在空气或外层空间传播】^。

#### 1.3 网络核心

​		通过网络链路和交换机移动数据有两种基本方法：**分组交换**和**电路交换**。

​		在电路交换的网络中，端系统间通信会话期间，==预留==了端系统间沿路径通信所需要的资源^【缓存和链路传输速度】^，而在分组交换的网络中==不会预留==这些资源。

##### 1.3.1 分组交换

​		多数分组交换机在链路的输入的使用**存储转发传输**机制。存储转发传输是指在交换机能够开始向输出链路传输该分组的第一个比特之前，必须接收到整个分组。

![store-and-forward_packet_switching](img/store-and-forward_packet_switching.png)

​		通过有`N`条速度均为`R`的链路组成的路径，其中有`N-1`台路由器，则端到端时延：$d_{端到端}=N\frac{L}{R}$。

​		对于每条相连的链路，分组交换机有一个用来存储路由器准备发往该链路的分组的**输出缓存/队列**。

![packet_switching](img/packet_switching.png)

​		分组需要承受输出缓存的**排队时延**。

​		由于缓存空间是有限的，一个到达的分组可能发现该缓存已被其他待传输的分组完全占满，此时刚到达的分组或已经排队的分组其中之一将被丢弃，称为**分组丢失/丢包**。

​		每个端系统都有IP地址，分组的首部中包含了目的地的IP地址。

​		每台路由器具有一个将目的IP地址(一部分)映射成输出链路的**转发表**。

##### 1.3.2 电路交换

​		链路中的电路是通过**频分复用**或**时分复用**来实现的。

​		对于频分复用，链路的频谱由跨越链路创建的所有连接共享。在连接期间链路为每条连接专用一个频段，该频段的宽度称为**带宽**。

​		对于时分复用，时间被划分为固定期间的帧，并且每个帧又被划分为固定数量的时隙。当网络跨越一条链路创建一条连接时，网络在每个帧中为该连接指定一个时隙。

![fdm_and_tdm](img/fdm_and_tdm.png)

##### 1.3.3 网络的网络

​		因为接入ISP向全球传输ISP付费，故接入ISP被认为是**客户**，而全球传输ISP被认为是**提供商**。

​		在任何给定的区域，可能有一个**区域ISP**，每个区域ISP则与**第一层ISP连接**。

​		任何ISP(除了第一层ISP)可以选择**多宿**。

​		位于相同等级结构层次的邻近一对ISP能够**对等**。

​		**因特网交换点**是一个汇合点，多个ISP能够在这里一起对等。

![interconnection_of_isps](img/interconnection_of_isps.png)

#### 1.4 分组交换网

​		当分组从一个节点/主机/路由器沿着这条路劲到后继节点/主机/路由器，该节点在沿途的每个节点经受了几种不同类型的时延，其中最重要的是**节点处理时延**、**排队时延**、**传输时延**和**传播时延**，这些时延的总和是**节点总时延**。若用$d_{proc}$、$d_{queue}$、$d_{trans}$、$d_{prop}$、$d_{nodal}$分别表示处理时延、排队时延、传输时延、传播时延和节点总时延，则$d_{nodal}=d_{proc}+d_{queue}+d_{trans}+d_{prop}$。

![total_nodal_delay_of_router](img/total_nodal_delay_of_router.png)

​		检查分组首部和决定将该分组导向何处所需要的时间是**处理时延**的一部分。

​		在队列中，当分组在链路上等待传输时，需要经受**排队时延**。

​		**传输时延**是路由器推出分组所需的时间，可表示为$\frac{L}{R}$，$L$表示分组的长度，$R(b/s)$表示链路的传输速度，即从队列中推出1bit的速度。		

​		$\frac{L\alpha}{R}$是**流量强度**，其中$\alpha(pkt/s)$表示分组到达队列的平均速度。流量强度主要用于衡量排队时延，==设计系统时流量强度不能大于1==。

​		1bit从一个路由器到另一个路由器所需的时间是**传播时延**。

​		源主机和目的主机之间有$N-1$台路由器，网络通畅^【排队时延可以忽略】^，节点时延累加起来，得到端到端时延：
$$
\begin{align}
d_{end-end}&=N(d_{proc}+d_{trans}+d_{prop})\\&=N(d_{proc}+\frac{L}{R}+d_{prop})
\end{align}
$$

​		**吞吐量**是进程交互比特的速度。

#### 1.5 协议层次及其服务模型

​		某层的**服务模型**是该层向上一层提供的服务。

​		各层的所有协议被称为**协议栈**。

![internet_protocol_stack_and_osi_reference_model](img/internet_protocol_stack_and_osi_reference_model.png)

##### 1.5.1 因特网协议栈

|        | 功能                                           | 主要协议                                  | 分组名称 |
| ------ | ---------------------------------------------- | ----------------------------------------- | -------- |
| 应用层 | 存留网络应用程序及它们的应用层协议             | HTTP、SMTP、FTP和DNS等                    | 报文     |
| 传输层 | 应用程序端点之间传输应用层报文                 | TCP、UDP、DCCP、DCTCP、TRFC、SCTP和QUIC等 | 报文段   |
| 网络层 | 也称为IP层，将数据报从一台主机移动到另一台主机 | IP                                        | 数据报   |
| 链路层 | 沿着路劲将数据包传递给下一个节点               | 以太网、WiFi和DOCSIS                      | 帧       |
| 物理层 | 将帧中的一个个比特从一个节点移动到下一个节点   |                                           | 比特     |

|      应用      | 应用层协议 |  传输层协议和端口   |
| :------------: | :--------: | :-----------------: |
|    电子邮件    |    SMTP    | TCP:25/465/587/2525 |
|  远程终端访问  |   Telnet   |       TCP:23        |
|      Web       |    HTTP    |       TCP:80        |
|    文件传输    |    FTP     |      TCP:20/21      |
| 远程文件服务器 |    NFS     |      UDP:2049       |
|   流式多媒体   |  通常专用  |       TCP/UDP       |
|   因特网电话   |  通常专用  |       TCP/UDP       |
|    网络管理    |    SNMP    |     UDP:161/162     |
|    域名系统    |    DNS     |       UDP:53        |
|    SSH隧道     |    SSH     |       TCP:22        |

##### 1.5.2 OSI模型

​		**网络体系结构**是通信系统的整体设计，其广泛采用OSI模型。

​		相比因特网协议栈，OSI模型多出表示层和会话层。

​		表示层的作用是使通信的应用程序能够解释交换数据的含义。这些服务包括数据压缩、数据加密和数据描述。

​		会话层提供了数据交换的定界和同步功能，包括了建立检查点和恢复方案的方法。

##### 1.5.3 封装

![encapsulation](img/encapsulation.png)

​		在每一层，分组包括首部字段和**有效载荷字段**^【通常是上一层的分组】^。

#### 1.6 网络安全

​		**病毒**是一种需要某种形式的用户交互来感染用户设备的恶意软件。

​		**蠕虫**是一种无须任何明显用户交互就能进入设备的恶意软件。

​		**Dos攻击**包括==弱点攻击==^【发送特殊的报文来控制或宕机】^、==带宽洪泛==^【发送大量分组】^和==连接洪泛==^【创建大量TCP连接】^。

​		用来观察执行协议实体之间交换的报文的基本工具被称为**分组嗅探器**。

​		**IP哄骗**指将具有虚假源地址的分组注入因特网。

### 第二章 应用层

#### 2.1 应用层协议原理

​		**套接字**是应用程序进程和传输层协议之间的接口。一个进程可以有多个套接字。

​		应用程序开发者可以通过套接字控制应用层的一切，但是对传输层的控制仅限于选择协议和设定几个传输层参数。应用程序体系结构通常使用客户端/服务器体系结构和对等体系结构。

​		当进程向另一台主机的进程发送分组时需要定义目的主机的地址和目的主机中接收进程的标识符。		

​		传输层为应用层提供的服务可分为四类：==可靠数据传输==、==吞吐量==、==定时==和==安全性==。

​		**带宽敏感的应用**具有吞吐量要求，而**弹性应用**能够根据可用的带宽。

​		**应用层协议**定义了运行在不同端系统上的应用程序进程如何相互传递报文：

​		﹡交互的报文类型

​		﹡各种报文类型的语法

​		﹡字段的语义

​		﹡确定一个进程何时以及如何发送报文，对报文进行响应的规则

#### 2.2 HTTP

​		Web的应用层协议是**HTTP**。

​		HTTP服务器不保存关于客户端的任何信息，故HTTP是一个**无状态协议**。为了识别客户端，HTTP使用了cookie。

​		**持续连接**指一个TCP可以传输多个HTTP请求和响应。

​		**非持续连接**指一个TCP只能传输一个HTTP请求/响应对。

​		**Web缓存器**，也称为**代理服务器**，能够代表Web服务器来满足HTTP请求的网络实体。

​		HTTP的**条件GET**机制可以解决Web缓存器缓存的数据陈旧的问题。

​		HTTP主要是一个**拉协议**，TCP连接是由接收端发起。

##### 2.2.1 HTTP请求报文

```http
GET /somedir/page.html HTTP/2
Host: www.test.com
User-Agent: Mozilla/5.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
```

​		HTTP请求报文的第一行是**请求行**。请求行包括方法字段、URL字段和HTTP版本字段。

​		HTTP请求报文第一行之后的行是**首部行**。

![http_request_message_format](img/http_request_message_format.png)

##### 2.2.2 HTTP响应报文

```http
HTTP/2 200 OK
content-type: text/javascript; charset=utf-8
content-encoding: br
last-modified: Wed, 03 Nov 2021 01:12:45 GMT
server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
```

​		HTTP响应报文的第一行是**状态行**。请求行包括协议版本字段、状态码字段和相应状态字段。

​		HTTP响应报文第一行之后的行是**首部行**。

![http_response_message_format](img/http_response_message_format.png)

##### 2.2.3 条件GET

​		如果请求报文是GET方法且请求报文的首部行包括`If-modified-since`，该请求报文就是条件GET请求报文。

![conditional_get](img/conditional_get.png)

​		第三步中`If-modified-Since`的值等于第二步中`last-modified`的值，这表示Web服务器仅当指定日期之后该对象修改后才发送该对象，假设该对象在指定日期后没被修改，于是第四步中Web服务器向Web缓存器发送的响应报文中状态码为304且没有对象，表示Web缓存器可以转发缓存的该对象的副本。

#### 2.3 电子邮件

​		因特网电子邮件系统由**用户代理**、**邮件服务器**和**简单邮件传输协议**组成。

![e-mail_protocols_and_their_communicating_entities](img/e-mail_protocols_and_their_communicating_entities.png)

​		1）发件方调用用户代理撰写内容并发送邮件。

​		2）发件方的用户代理把报文发给发件方的邮件服务器，在这里报文被放在报文队列里。

​		3）发件方邮件服务器上的SMTP客户端创建一个到收件方邮件服务器上的SMTP服务器的TCP连接。

​		4）经过初始SMTP握手后，SMTP客户端通过TCP连接发送报文。

​		5）收件方邮件服务器的SMTP服务器接收到报文并将报文投入到收件方的邮箱。

​		6）收件方调用用户代理查看邮件。

##### 2.3.1 SMTP

​		SMTP是因特网电子邮件中主要的应用层协议。它限制了邮件的报文(不只是首部)只能采用7-bit ASCII表示，若报文包含非7-bit ASCII字符或二进制数据，需要进行7-bit ASCII编码。

​		SMTP是**推协议**，TCP连接是发送端发起的。

```ini
S: 220 client
C: EHLO server
S: 250 from | 250 PIPELINING | 250 SIZE 12345
C: AUTH XOAUTH2 oauth
S: 235 2.7.0 Accepted
C: MAIL FROM: <client@email.com> 
S: 250 OK
C: RCPT TO: <server@email.com>
S: 250 OK
C: DATA
S: 354 End data with <CR><LF>.<CR><LF>
C: DATA fragment,content
S: 250 OK: queued as.
C: QUIT
S: 221 Bye
```

​		SMTP协议中客户端发送了5条命令：HELO/EHLO^【ESMTP版本】^(HELLO缩写)、MAIL FROM、RCPT TO、DATA以及QUIT。

​		ESMTP相比SMTP，在发送邮件时==需要验证用户账户==。

##### 2.3.2 电子邮件

```ini
From: from@email.com
To: to@email.com
Subject: subject
```

​		邮件报文的首部行包括环境信息，必须包含一个`From`和一个`To`，可能包含`Subject`以及其他可选首部行。

​		在用户代理建立一个到邮件服务器110端口上的TCP连接后，POP3按照3个阶段进行工作：授权、事务处理以及更新。

​		﹡授权阶段需要用户代理以明文形式发送用户名和密码来认证，主要指令有`user <username>`和`pass <password>`。

​		﹡事务处理阶段中用户代理可以取回报文，也可以添加/取消报文删除标记，主要指令有`list`、`retr`和`dele`。

​		﹡更新阶段在用户代理发出`quit`指令并结束会话后，邮件服务器会删除那些被标记的报文。

​		POP3会话期间，邮件服务器会保留一些状态信息，但是在会话中不会携带这些信息。

​		IMAP使用了TCP连接的143端口。相比POP3，IMAP把报文和文件夹联系起来并且提供了创建/修改/删除文件夹和获取报文某些部分的指令。

#### 2.4 DNS

​		主机可以用**主机名**和**IP地址**来进行识别。

​		一台名为a.com的主机，可能还有别名b.com和c.com，此时a.com是**规范主机名**。

​		**域名系统**是一个由分层的**DNS服务器**实现的分布式数据库，一个让主机查询分布式数据库的应用层协议。主要用于将主机名解析为IP地址，也提供**主机别名**、**邮件服务器别名**和**负载分配**服务。

##### 2.4.1 DNS工作原理

​		DNS服务器主要分为**根DNS服务器**、**顶级域DNS服务器**、**权威DNS服务器**和**本地DNS服务器**。

​		﹡根DNS服务器提供顶级域DNS服务器的IP地址。

​		﹡每个顶级域^【如com、org、net、edu和gov等】^和国家的顶级域^【如uk、fr、ca和cn等】^都有顶级域DNS服务器(集群)。顶级域DNS服务器提供权威DNS服务器的IP地址。 

​		﹡因特网上具有公共可访问主机的每个组织机构必须提供公共可访问的DNS记录，这些记录将这些主机的名称映射为IP地址。

​		﹡每个ISP都有一个本地DNS服务器/默认名称服务器

![interaction_of_the_various_dns_servsers](img/interaction_of_the_various_dns_servsers.png)

​		一般情况下，从请求主机到本地DNS服务器的查询是**递归查询**，其余的查询是**迭代查询**。

​		为了降低时延并减少报文数量，**DNS缓存**广泛使用，因此在大部分DNS查询中根DNS服务器都被绕过。

##### 2.4.2 DNS报文

​		**资源记录**提供了主机名到IP地址的映射，格式为`(Name, Value, Type, TTL)`。

​		﹡`Type = A`时，`Name`是主机名，`Value`是主机名对应的IP地址。

​		﹡`Type = NS `时，`Name`是域名，`Value`是能够获取该域名中主机IP地址的权威DNS服务器的主机名。

​		﹡`Type = CNAME `时，`Value`是别名为`Name`的主机的规范主机名。

​		﹡`Type = MX`时，`Value`是别名为`Name`的邮件服务器的规范主机名。

​		对于某个主机名，若DNS服务器是它的权威DNS服务器，则该DNS服务器会有一条包含该主机名的A型记录。若DNS服务器不是它的权威服务器，则该DNS服务器会有一条该主机名所属域名的NS型记录，还会有一条包含该NS型记录中`Value`的A型记录，还可能会有一条包含该主机名的A型记录。

![dns_message_format](img/dns_message_format.png)

​		DNS报文分为查询和应答报文，报文格式相同。

​		﹡在首部区域，第一个字段占16位，用于标识该查询，该字段会被复制到应答报文中来匹配请求。第二个字段有若干1 位的标志位。0/1标识查询/应答报文。若请求的是权威DNS服务器则应答报文会设置[权威]标志位。若客户端在DNS服务器没有资源记录时希望它执行递归查询则会设置[希望递归]标志位。若DNS服务器支持递归查询则会在应答报文设置[递归可用]标志位。剩下四个字段表示后四个区域数据的数量。

​		﹡问题区域包名称字段和类型字段，名称字段是待查询的主机名称，类型字段对应资源记录中的类型字段。

​		﹡应答区域可以包含多条资源记录。		

​		﹡权威区域包含其他权威DNS服务器的资源记录。

​		﹡附加信息区域包含其他有用的资源记录。

#### 2.5 P2P文件分发

##### 2.5.1 P2P体系结构

​		**分发时间**是所有$n$个对等方得到$f(bit)$文件的副本所需时间。

![file_distribution](img/file_distribution.png)

​		$D_{cs}$表示C/S体系结构的分发时间，其中服务器需要上传$n$个文件的副本，则
$$
\begin{align}
D_{cs}&\geqslant max\{D_c,D_s\}\\&\geqslant max\{\frac{f}{d_{min}},\frac{nf}{u_s}\}
\end{align}
$$
​		$D_{P2P}$表示P2P体系结构的分发时间，分发开始时只有服务器有文件，分发一次后可由对等方来分发，则
$$
\begin{align}
D_{P2P}&\geqslant max\{D_c,D_s,\frac{nf}{u_{total}}\}\\&\geqslant max\{\frac{f}{d_{min}},\frac{f}{u_s},\frac{nf}{u_s+\sum_{i=1}^{n}u_i}\}
\end{align}
$$

##### 2.5.2 BitTorrent

​		参与文件分发的所有对等方的集合称为**洪流**。在一个洪流中的对等方彼此下载等大小的文件块，通常是256KB。每个洪流具有一个基础设施节点，称为**追踪器**。当有对等方加入洪流时需要向追踪器注册并周期性地通知是否在洪流中。

![file_distribution_with_bittorrent](img/file_distribution_with_bittorrent.png)

​		与对等方建立TCP连接的其他对等方称为该对等方的**邻居/邻近对等方**。

​		**最稀缺优先**即优先请求本身没有且邻居中副本最少的块。这样可以大致地均衡每个块在洪流中的副本量。

​		对等方优先响应那些当前能够以==最高速度==提供副本的邻居。对等方会每十秒进行速度测量并确定速度前四的邻居，这四个邻居会被**疏通**。此外，每三十秒会随机再选择一个邻居进行交换，如果彼此都能满足，则继续交换。除了这五个邻居，其他邻居将被阻塞，即无法从该对等方获取块。这种机制被称为**一报还一报**。

#### 2.6 CDN

##### 2.6.1 视频流

​		一张未压缩/数字编码的图像有像素阵列组成，每个像素由一些比特编码来表示亮度或颜色。

​		视频是一系列图像以每秒二十四/三十张图像来展现。视频能被压缩，故可以比特率来权衡视频质量。

​		在DASH中，视频编码为比特率不同的多个版本，每个版本都有一个不同的URL，每个版本的URL和比特率都存在HTTP服务器中的**告示文件**中。DASH运行客户端自由地切换版本。

##### 2.6.2 CDN

​		**CDN**分布在多个地理位置的服务器上，并且将用户请求重定向到一个时延更低的CDN。

​		CDN分为**专用CDN**和**第三方CDN**，专用CDN由内容提供商自己拥有，第三方CDN分发多个内容提供商的内容。

​		CDN通常采用**深入**和**客邀**这两种安置原则。

​		﹡深入由Akamai首创，该原则通过在遍及全球的接入ISP中部署服务器集群来深入到ISP的接入网中。其目标是靠近端系统，通过减少端系统和CDN集群之间的链路和路由器来减少时延和提供吞吐量，但也带来了较高的维护管理成本。

​		﹡客邀被Limelight和很多其他CDN公司采用，该原则通过在少量的关键位置(通常是因特网交换点)建立大量集群来客邀ISP。相比深入，客邀的维护管理成本更低，但时延较高而且吞吐量较低。

![dns_redirects_a_request_to_a_cdn_server](img/dns_redirects_a_request_to_a_cdn_server.png)

​		大多数CDN利用DNS来截获和重定向请求。

​		1）客户端访问某Web网页。

​		2）客户端访问该Web下的某个资源时，发送了对应的DNS请求。

​		3）本地DNS服务器将DNS请求中继到该Web的权威DNS服务器，权威DNS服务器返回了CDN域下的主机名。

​		4）本地DNS服务器通过CDN域下的主机名向CDN权威DNS服务器发送DNS请求，CDN权威DNS服务器返回了CDN服务器的IP地址。

​		5）本地DNS服务器将IP地址返回给客户端。

​		6）客户端通过IP地址与CDN服务器建立TCP连接并发送HTTP请求。

​		CDN部署的核心都是**集群选择策略**，即动态地将请求重定向到CDN中的某个服务器集群/数据中心。

​		较简单的选择策略是将请求重定向到(距离DNS服务器)**地理上最近**的集群。另一种选择策略是对DNS服务器和集群之间进行周期性的时延以及丢包**实时测量**来选择。

### 第三章 传输层

​		网络层提供了主机之间的逻辑通信，传输层提供不同主机的进程之间的逻辑通信。

​		运输层的**多路复用**与**多路分解**指将主机间交付扩展到进程间交换。

​		﹡多路复用指在不同套接字中收集数据块并为每个数据块封装上首部信息从而生成报文段再将报文段传递到网络层。

​		﹡多路分解指将传输层报文段中的数据交付到正确的套接字。

​		多路复用需要**源端口号字段**^【套接字的唯一标识符】^和**目的端口号字段**^【报文段中标识目标套接字的字段】^。

​		端口号是一个16位的数。0~1023之间的端口称为**周知端口号**。

​		**序号**用于为从发送端流向接收端的分组按序编号。

​		**校验和**用于分组的差错检测。

​		在信道中分组重新排序是可能的，可以表现为序号不存在于发送端和接收端的窗口中的分组/ACK的副本可能会出现。信道可以看成用来缓存分组，并在==任意时刻==释放这些分组。

​		通过设置分组的TTL来实现序号在发送端确定不会出现之前不会再被使用。

#### 3.1 UDP

​		UDP在发送报文段前传输层实体间没有握手，故UDP被称为==无连接==的。

![udp_segment_structure](img/udp_segment_structure.png)

​		UDP首部有四个字段，每个字段由2字节构成。

​		﹡长度字段即报文段中的字节数(首部+应用数据)。

![checksum_of_udp](img/checksum_of_udp.png)

​		伪首部包括源IP地址、目的IP地址、填充0的保留字段、传输层协议号以及报文长度。TCP的传输层协议号是6，UDP的传输层协议号是17。

​		发送端在计算校验和时需要先加上伪首部并将校验和字段置零，将伪首部、首部和应用数据转换成16位二进制(不足部分填充零)并求和，求和时需要回卷(如果进位到第17位则将结果加一)，将和取反得到校验和，发送端将设置校验和并去掉伪首部。接收端计算校验和方式类似于发送端(不需要将校验和置零)，最后结果全为一则说明数据无误，否则警告。

​		由于无法确保链路的可靠和内存中的差错检测，UDP在端到端基础上的传输层进行差错检测，这种设计被称为**端到端原则**，即同样功能的实现成本在较低级别相比较高级别可能较高。

#### 3.2 可靠数据传输

![reliable_data_transfer](img/reliable_data_transfer.png)

##### 3.2.1 rdt1.0

![fsm_of_rdt1.0](img/fsm_of_rdt1.0.png)

​		rdt1.0协议指经完全可靠信道的可靠数据传输，故接收端就不需要提供任何反馈信息给发送端。

​		FSM的初始状态用虚线表示。发送端和接收端的FSM都只有一个状态，故变迁必定是从一个状态返回到本身。

​		上层应用调用发送端的过程中，发送端只通过`rdt_send(data)`接收数据，经由`make_pkt(data)`产生一个包含该数据的分组，并将分组发送到信道中。

​		较低层协议调用接收端的过程中，接收端通过`rdt_rcv(packet)`从底层信道接收一个分组，经由`extract(packet,data)`取出数据，并通过`deliver_data(data)`将数据传输给上层。

##### 3.2.2 rdt2.0

​		通过**肯定确认**或**否定确认**来让发送端知道那些内容被正确接收或接收有误需要重传的可靠传输协议称为**自动重传请求**协议。自动重传请求协议还需要==差错检测==、==接收端反馈==^【用1位来表示，0是NAK，1是ACK】^和==重传==来处理比特差错的情况。

![fsm_of_rdt2.0.0](img/fsm_of_rdt2.0.png)

​		rdt2.0相比rdt1.0，加入了差错检测和肯定/否定确认。

​		当发送端等待ACK/NCK时不能从上层获取数据或发送分组，故rdt2.0被称为**停等**协议。

​		发送端有两个状态。在左边的状态中，发送端正在等待上层调用。当出现`rdt_send(data)`时，发送端将通过`make_pkt(data,checksum)`产生一个包含数据和校验和的分组，经由`udt_send(sndpkt)`发送该分组。在右边的状态中，发送端正在等待接收端回传的ACK/NAK。若收到ACK分组，即`rdt_rcv(rcvpkt) && isACK(rcvpkt)`，发送端会回到等待上层调用的状态。若收到NAK分组，即`rdt_rcv(rcvpkt) && isNAK(rcvpkt)`，发送端会重传分组并等待接收回传的ACK/NAK。

​		接收端只有一个状态。当分组到达时，接收端回传ACK/NAK，即`rdt_rcv(rcvpkt) && notcorrupt(rcvpkt)`或`rdt_rcv(rcvpkt) && corrupt(rcvpkt)`。

​		但是rdt2.0忽视了ACK/NAK分组受损的情况，解决这一问题的简单方法就是添加一个新字段来表示发送数据分组的序号。

![fsm_of_rdt2.1.png](img/fsm_of_rdt2.1.png)

​		rdt2.1是rdt2.0的修订版，rdt2.1的发送端和接收端FSM的状态数都是以前的两倍，因为需要反映出目前分组的序号。rdt2.1使用了接收端到发送端的ACK/NAK。当收到乱序的分组时，接收端回传ACK。当收到受损的分组时，接收端回传NAK。

![fsm_of_rdt2.2](img/fsm_of_rdt2.2.png)

​		rdt2.2相比rdt2.1，rdt2.2无NAK，而是对上一次正确接收的分组回传ACK。发送端收**冗余ACK**后，就知道了接收端没有正确接收到冗余ACK对应的分组后的分组。因此，ACK报文需要一个序号字段。

##### 3.2.3 rdt3.0

![fsm_of_rdt3.0_sender](img/fsm_of_rdt3.0_sender.png)

​		rdt3.0是用于具有比特差错的丢包信道的协议。通过在发送端中加入**倒数计时器**来解决超时/丢包问题，接收端与rdt2.2相同。

​		因为分组序号在0和1之间交替，rdt3.0也被称为**比特交替协议**。

##### 3.2.4 流水线可靠数据传输协议

![stop-and-wait_and_pipelined_sending](img/stop-and-wait_and_pipelined_sending.png)

​		停等协议存在一定的性能问题，简单的解决方式就是不使用停等，允许发送方发送多个分组而无须等待。因为许多从发送端到接收端的分组可以被看出是填充到一条流水线，故这种技术被称为**流水线**。

​		流水线需要可靠数据传输协议增加序号的范围和发送/接收端缓存分组，而这些取决于差错恢复。流水线的差错恢复包括**回退N步**和**选择重传**。

##### 3.2.5 回退N步

![sender_view_of_sequence_numbers_in_gbn](img/sender_view_of_sequence_numbers_in_gbn.png)

​		随着协议的运行，该窗口的序号空间向前滑动，故$N$被称为窗口长度，GBN也被称为滑动窗口协议。

​		$base$表示最早待确认的分组的序号，$nextseqnum$表示最早的待发送的分组的序号。

​		$[0,base-1]$表示已被确认的分组，$[base,nextseqnum-1]$表示待确认的分组，$[nextseqnum,nextnum+N-1]$表示待发送的分组，$[nextnum+N,+\infty)$表示不可用的分组，直到当前流水线中待确认的分组确认。

![extended_fsm_of_gbn](img/extended_fsm_of_gbn.png)

​		发送端必须响应==上层的调用==、==接收ACK==和==处理超时==。

​		当上层调用`rdt_send()`时，发送端先检测发送窗口是否已满。若窗口未满则产生一个分组发送并更新对应的变量，反之则反馈上层。不存在待确认分组的情况下首次发送分组时会设置一个计时器。当收到ACK但仍存在待确认的分组时，计时器将重置。当不存在待确认的分组时停止计时器。

​		接收端用$expectedseqnum$来表示按序待接收的分组的序号。

​		接收端对序号为$n$的分组使用**累积确认**的方式，表明已正确收到序号在$n$之前且包括$n$的所有分组。

​		当接收端收到序号为$n$的分组且上次交付给上层的分组的序号是$n-1$时会对分组$n$回传ACK，否则丢弃该分组并对最近交付给上层的分组回传ACK。

​		接收端会丢失所有乱序分组，因为这些分组还会重传。

![gbn_operation](img/gbn_operation.png)

##### 3.2.6 选择重传

![sender_and_receiver_views_of_sequence_numbers_in_sr](img/sender_and_receiver_views_of_sequence_numbers_in_sr.png)

​		发送端仅重传那些在接收方可能丢失/受损的分组，也可能收到窗口内某些分组的ACK。

​		接收端将确认一个正确接收的分组而不管是否按序，乱序的分组将被缓存，当所有的乱序分组都被接收后再一起交付给上层。

​		当上层调用`rdt_send()`时，发送端检测可用于分组的序号是否在窗口内。若在窗口内则产生一个分组发送，反之则反馈上层。每个分组都有一个计时器。当收到ACK时，若ACK对应的分组序号在窗口内则将该分组标记为已确认，若ACK对应的分组序号等于$send\_base$则将窗口移动至待确认的最小序号处。窗口移动后且窗口内存在待发送的分组，这些分组将被发送。

​		对接收端，==序号在$[rcv\_base-N,rcv\_base+N-1]$内的分组将被正确接收==。若接收分组的序号在窗口内将回传ACK，分组首次接收时缓存该分组。若接收分组的序号等于$rcv\_base$，则该分组以及缓存中序号始于该分组的序号且连续的所有分组将交付给上层。若接收分组的序号在$[rcv\_base-N,rcv\_base-1]$，则==必须==回传一个ACK，无论该分组是否已被确认。

![sr_operation](img/sr_operation.png)

​		SR问题在于发送端/接收端之间缺乏同步，唯一能确定的是只有信道中收到的分组/ACK。

![problem_of_sr](img/problem_of_sr.png)

​		SR的窗口长度必须小于或等于序号去重后的数量的一半。

#### 3.3 拥塞控制原理

##### 3.3.1 拥塞原因与代价

![two_senders_and_a_router_with_infinite_buffers](img/two_senders_and_a_router_with_infinite_buffers.png)

​		$\lambda_{in} (B/s)$表示应用层通过套接字发送初始报文到传输层的速度，$\lambda_{out} (B/s)$表示应用层接收报文的速度。

​		分组通过一台路由器在容量为$R$的共享式输出链路上传输，忽略添加底层首部信息的开销、差错恢复、流量控制和拥塞控制，显然$\lambda_{out} \leqslant \frac{R}{2}$ 。时延的增长率随着$\lambda_{in}$增长，$\lambda_{in}$达到$\frac{R}{2}$时时延无穷大。这里体现了==拥塞的代价之一：当分组的到达速度接近链路容量时，分组承受巨大的排队时延==。

![two_senders_with_retransmissions_and_a_router_with_finite_buffers](img/two_senders_with_retransmissions_and_a_router_with_finite_buffers.png)

​		$\lambda^{'}_{in} (B/s)$表示传输层发送初始报文段和重传报文段到网络层的速度，也称为**供给载荷**。

​		假设发送端能确定路由器的可用缓存并根据情况发送分组，所以不会丢包，$\lambda_{in}=\lambda^{'}_{in}$，连接的吞吐量为$\lambda^{'}_{in}$。在这种情况下，平均主机发送速度不能超过$\frac{R}{2}$。

​		假设发送端仅确定分组丢失后下才重传，当$\lambda_{in}^{'}=\frac{R}{2}$且平均时 $\lambda_{in}=\lambda_{out}=\frac{R}{3}$。这里体现了==拥塞的代价之一：发送端必须重传因缓存溢出而丢失的分组==。

​		假设发送端重传因排队时延超时但未丢失的分组，当$\lambda_{in}^{'}=\frac{R}{2}$且平均(转发两次)时$\lambda_{in}=\lambda_{out}=\frac{R}{4}$。这里体现了==拥塞的代价之一：发送方因较大的时延进行了不必要的重传来占用路由器额外的链路带宽==。

![four_senders_with_retransmissions,routers_with_finite_buffers](img/four_senders_with_retransmissions,routers_with_finite_buffers.png)

​		当$\lambda_{in}$较小时，路由器缓存比较充足，吞吐量大致等于供给载荷，$\lambda_{out}$随着$\lambda_{in}$增大而增大。当$\lambda_{in}$较大时，对于R2，主机B发送的分组的到达速度高于主机A发送的分组的到达速度，这会导致主机A发送的分组因缓存溢出而丢失。

​		若考虑网络资源的浪费，当分组在第二跳及之后的路由器丢失时，之前路由器的转发工作毫无意义。这里体现==拥塞的代价之一：分组被丢弃时转发过该分组的上游路由器因转发而使用的传输容量被浪费==。

##### 3.3.2 拥塞控制方法

​		根据网络层是否为传输层提供了显示支持可将拥塞控制分为==端到端拥塞控制==和==网络辅助拥塞控制==。

​		在端到端拥塞控制中，网络层==没有==为传输层提供显示支持。即使网络中存在拥塞，端系统也必须通过对丢包和时延等网络行为的观察来确定是否拥塞。

​		在网络辅助的拥塞控制中，路由器向发送端提供关于的拥塞状态的显示反馈信息。拥塞信息反馈到发送端通常有两种方式，一种是路由器直接发送关于拥塞状态的分组给发送端，此时该分组称为**抑制分组**。另一种方式更通用，TCP、DCCP和DCTCP都有使用。路由器更新==由发送端到接收端的数据报头部中的ECN拥塞标志位==来表示出现拥塞。若接收端收到分组的标志位表示出现拥塞则会通知发送端，故这种方式至少需要一个完整的往返周期。

#### 3.4 TCP

​		在进程发送数据之前，进程间必须握手，即相互发送一些预备报文段来设置确保数据传输的参数，故TCP是**面向连接的**。

​		TCP连接只能有一个客户端和一个服务端，故TCP是**点对点**的。

​		进程间建立TCP连接后，双方都可以发送/接收报文段，故TCP是**全双工服务**。

​		TCP根据ACK到达的速度来调节拥塞窗口，故TCP是**自计时**的。

​		客户端先发送一个特殊的报文段，服务端用另一个特殊报文段来响应，最后，客户端用第三个特殊报文段作为响应，这种建立连接的过程被称为**三次握手**。前2个报文段不承载有效载荷，第三个报文段可以承载有效载荷。

​		TCP的双方都由一个接收缓存、一个发送缓存和几个变量组成。

​		TCP会使用**重传计时器**、**坚持计时器**、**保活计时器**和**时间等待计时器**这四种计时器。重传计时器用于报文段重传。坚持计时器用于防止双方的死锁。保活计时器用于在长连接中断开无响应的连接。时间等待计时器用于四次握手断开连接前的等待。

​		**最大传输单元**指从源到目的地所有链路上发送的最大链路层帧。**最大报文段长度**是报文段中有效载荷的最大长度。最大传输单元一般是1500字节，TCP/IP首部的长度一般是40字节，故最大报文段长度一般是1460字节。

​		当收到按序报文段时，若序号在按序报文段之前的报文段都已经确认，则等待下一个按序报文段最多500ms，超时则发送ACK。若还存在另一个按序报文段待确认，则立即发送单个累积ACK来确认这两个报文段。

​		当收到序号在按序报文段之后的报文段时则立即发送冗余ACK。

​		一旦收到3个冗余ACK，TCP就执行**快速重传**。

​		当==超时或收到3个冗余ACK==时，发送端出现了丢包。

​		TCP的差错恢复机制是**选择性确认**，即有选择地确认乱序报文段。

##### 3.4.1 TCP报文段结构

![tcp_segment_structure](img/tcp_segment_structure.png)

​		TCP报文段包括16位**源端口**、16位**目的端口**、32位**序号**、32位**确认序号**、4位**首部长度**、3位保留字段、9个标志位、16位**窗口长度**、16位**校验和**、16位**紧急指针**以及最多40字节的的选项字段。

​		TCP将数据看成一个无结构且有序的字节流，通过字节流确定序号，序号是==报文段首字节的编号==。初始序号一般随机。

​		确认号是==下一次按序应接收报文段首字节的编号==。TCP只确认报文段有效载荷中到第一个丢失字节为止的字节，故TCP提供**累积确认**。当有效载荷为空时吗，确认号被**捎带**在报文段中。

​		9个标志位中第2、3位标志位用于显式拥塞控制，若发送端至接收端链路中的某个路由器出现拥塞，当数据报达到该路由器后，路由器将数据报头部中的ECN标识为置1，接收端收到数据报后将ACK报文段头部中的`ECE`置1来通知发送端链路出现拥塞，发送端像快速重传一样对`ECE`为1的ACK回应ACK，发送端在下一个报文段中将`CWR`置1来通知接收端拥塞窗口已缩减。后6个标志是控制位。

​		﹡**NS(N)**通常用于防止标记数据包被意外或恶意地隐藏。

​		﹡**CWR(C)**为1时通知对方拥塞窗口已缩减。

​		﹡**ECE(E)**为1时通知对方链路出现拥塞。

​		﹡**Urgent/URG(U)**为1时表示高优先级报文段，紧急指针生效。

​		﹡**ACK(A)**为1时表示确认序号生效，报文段成功接收。

​		﹡**Push/PSH(P)**为1时表示应该立即将该报文段交给应用层而不用等待缓存区填满。

​		﹡**Reset/RST(R)**为1时表示重置连接。

​		﹡**Synchronization/SYN(S)**为1时表示三次握手中建立连接。

​		﹡**Finish/FIN(F)**为1时表示四次挥手中断开连接。

​		选项字段包括8位Kind、可变的Length以及可变的Info。

| Kind字段值 | Length字段值 | Info字段长度 | 名称                       | 含义           |
| ---------- | ------------ | ------------ | -------------------------- | -------------- |
| 0          | 1            |              | 选项表结束(EOP)            |                |
| 1          | 1            |              | 空操作(NOP)                |                |
| 2          | 4            | 2字节        | 最大报文段长度(MSS)        |                |
| 3          | 3            | 1字节        | 窗口扩大系数(WSOPT)        | 窗口长度扩展   |
| 4          | 2            |              | 选择性确认(SACK-Premitted) | 表示支持SACK   |
| 5          | 可变         |              | 选择性确认(SACK)           | 收到的乱序数据 |
| 8          | 10           |              | TSPOT                      | 时间戳         |
| 19         | 18           |              | TCP-MD5                    | MD5认证        |
| 28         | 4            |              | User Timeout(UTO)          | 超时时间       |
| 29         | 可变         |              | TCP-AO                     | 认证算法       |
| 253/254    | 可变         |              | Experimental               | 保留           |

​		窗口长度用于流量控制服务。

​		校验和的计算与UDP相同。

##### 3.4.2 连接管理

![three-way_handshake](img/three-way_handshake.png)

​		1）客户端向服务端发送**SYN(报文段)**，即报文段的有效载荷为空，`SYN`为1，客户端进入`SYN_SENT`。

​		2）服务端收到SYN后为该连接分配TCP缓存和变量，再向客户端发送**SYNACK(报文段)**，即ACK报文段的`SYN`为1，最后服务端进入`SYN_RCVD`。

​		3）客户端收到SYNACK后该连接分配TCP缓存和变量，再向服务端发送ACK，此时连接已建立，`SYN`置为0，最后服务端进入`ESTABLISHED`。服务端收到ACK后进入`ESTABLISHED`。

![four-way_handshake](img/four-way_handshake.png)

​		1）客户端向服务端发送**FIN(报文段)**，即报文段的有效载荷为空，`FIN`为1，客户端进入`FIN_WAIT_1`。

​		2）服务端收到FIN后发送ACK并进入`CLOSE_WAIT`。客户端收到ACK后进入`FIN_WAIT_2`。

​		3）服务端向客户端发送FIN并进入`LAST_ACK`。

​		4）客户端收到FIN后发送ACK并进入`TIME_wAIT`，同时设置时间等待计时器，到时后释放资源(包括端口号)并进入`CLOSED`。服务端收到ACK后释放资源并进入`CLOSED`。

![tcp_state](img/tcp_state.png)

​		在TCP连接的生命周期中，运行在每台主机的TCP协议会在各种**TCP状态**间变迁。

##### 3.4.3 超时

​		SampleRTT表示报文段从发送(交付给IP)到收到该报文段的确认所需时间。TCP不会为重传的报文段测量SampleRTT，仅仅为只需要传输一次的报文段测量。

​		EstimatedRTT表示SampleRTT的平均值。
$$
EstimatedRTT=(1-\alpha)\times EstimatedRTT+ \alpha \times SampleRTT(\alpha =\frac{1}{8})
$$
​		DevRTT表示SampleRTT偏离EstimatedRTT的程度。
$$
DevRTT=(1-\beta)\times DevRTT+\beta \times |SampleRTT-EstimatedRTT|(\beta=\frac{1}{4})
$$
​		EstimatedRTT和DevRTT的计算方式是**指数加权移动平均**。

​		TimeoutInterval表示超时时间，公式为$TimeoutInterval=EstimatedRTT+4\times DevRTT$。

##### 3.4.4 流量控制

​		TCP用**流量控制服务**来使发送端的发送速度和接收端的读取速度相匹配，这一服务通过让==发送端==维护**接收窗口**^【表示接收端可用缓存空间】^的变量来实现。

​		$RevBuffer$表示接收端的接收缓存的大小。$LastByteRead$表示接收端在接收缓存中读取的数据流的最后一个字节的编号。$LastByteRcvd$表示接收端缓存至接收缓存的数据流的最后一个字节的编号。$rwnd$表示接收窗口。
$$
rwnd=RcvBuffer-[LastByteRcvd-LastByteRead]
$$
​		接收端将报文段中窗口长度字段设置为$rwnd$来告知发送端可用缓存空间。

​		$LastByteSent-LastByteAcked$表示发送端待确认的数据量。
$$
LastByteSent-LastByteAcked\leqslant rwnd
$$
![deadlock_of_tcp](img/deadlock_of_tcp.png)

​		当发送端收到的窗口长度为零的ACK时，会设置坚持计时器并发送一个有效载荷为一字节的探测报文段，若计时器超时或收到ACK的窗口长度为零时会再次发送同样的报文段并重置计时器，反之则继续发送有效载荷为有效数据的报文段。

##### 3.4.5 TCP拥塞控制

​		网络层不向端系统提供显示的网络拥塞反馈，故TCP只能使用端到端拥塞控制。

​		TCP发送端相比接收端多个变量，即**拥塞窗口**，用来限制发送速度。发送端未被确认的数据不能超过接收窗口与拥塞窗口的最小值。
$$
LastByteSent-LastByteAcked \leqslant min\{cwnd,rwnd\}
$$
​		假设接收窗口足够大、忽略丢包与时延以及发送端总有数据需要发送，==发送速度大致等于$\frac{cwnd}{RTT}(B/s)$==。

​		当某路径出现拥塞时，该路径上的一/多个路由器的缓存会溢出并导致某个数据包丢失，进而引发发送端的丢包，此时发送端可以确定该路径出现了拥塞。

​		==带宽探测：丢包表示出现了拥塞，应该降低发送速度。当未确认报文段的ACK到达时应该提高发送速度。==

###### 3.4.5.1 TCP拥塞控制算法

​		TCP拥塞控制算法包括**慢启动**、**拥塞避免**和**快速恢复**。

​		TCP拥塞控制被称为**加性增、乘性减**拥塞控制方式。加性增指在拥塞避免阶段$cwnd$的线性增加，乘性减指进入快速恢复阶段时$cwnd$的减半。

![fsm_of_tcp_congestion_control](img/fsm_of_tcp_congestion_control.png)

​		在慢启动阶段，$cwnd$的初始值是$MSS$，$ssthresh$的初始值是$64KB$。每当报文段首次确认$cwnd$就增加$MSS$，即指数级增长。若出现超时导致的丢包，发送端令$ssthresh=\frac{cwnd}{2}$，$cwnd=MSS$并重新开始慢启动。当$cwnd \geqslant ssthresh$时发送端结束慢启动并进入拥塞避免阶段。当收到3个冗余ACK时，发送端结束慢启动并令$ssthresh=\frac{cwnd}{2}$，$cwnd=\frac{ssthresh}{2}+3MSS$，然后执行快速重传，最后进入快速恢复阶段。

​		在不考虑处理时间的情况下，客户端发送请求到远程数据中心并收到响应大致需要$4RTT$，其中建立TCP需要$RTT$，慢启动阶段需要$3RTT$。显然，当$RTT$较大时，时延也较大。可以使用**TCP分岔**解决这一问题，即通过CDN将请求转发至邻近客户端且与远程数据中心有很大窗口的连接的前端服务端，在这种情况下响应所需时间大致是$4RTT_{FE}+RTT_{BE}+处理时间$，其中$RTT_{FE}$表示客户端与前端服务端的往返时间，$RTT_{BE}$表示前端服务端与远程数据中心的往返时间。当前端服务端与客户端足够近，就可以忽略$RTT_{FE}$，此时响应所需时间大致等于$RTT$。

​		在拥塞避免阶段，每个$RTT$内$cwnd$仅增加$MSS$。通用实现方法是若$RTT$内发送了$n$个报文段，在此期间每个报文段首次确认时$cwnd$增加$\frac{cwnd}{n}$。超时和3个冗余ACK的情况同慢启动。

​		在快速恢复阶段，每收到一个冗余ACK，$cwnd$增加$MSS$。当收到了新报文段的首次ACK，发送端会结束快速恢复阶段并进入拥塞避免阶段。当出现超时导致的丢包时发送端会结束快速恢复并令$ssthresh=\frac{cwnd}{2}$，$cwnd=MSS$，然后进入慢启动阶段。

![evolution_of_tcp_congestion_window](img/evolution_of_tcp_congestion_window.png)

​		TCP的较新版本**TCP Reno**的快速恢复阶段符合上述情况。但TCP的早期版本**TCP Tahoe**在快速恢复阶段只要出现丢包都会结束快速恢复并令$ssthresh=\frac{cwnd}{2}$，$cwnd=MSS$，然后进入慢启动阶段。

​		TCP Vegas试图在维持较好吞吐量同时避免拥塞，通过测量RTT来衡量拥塞程度，根据拥塞程度线性地降低发送速度。TCP Vegas提供了慢启动、拥塞避免、快速恢复、快速重传和SACK。

###### 3.4.5.2 平均吞吐量

​	当计算一个长存活期连接的平均吞吐量时，因为慢启动阶段通常很短，可以忽略。在一个RTT内，窗口长度是$w(B)$，发送速度大约是$\frac{w}{RTT}$。在出现丢包之前，每个RTT内$w=w+MSS$。用$W$表示出现丢包时$w$的值。
$$
一条连接的平均吞吐量=\frac{4W}{3RTT}
$$
​		当计算一个高速连接的平均吞吐量时，在一个RTT内，窗口长度是$w(B)$，发送速度大约是$\frac{w}{RTT}$。丢包率为$L$。
$$
一条连接的平均吞吐量=\frac{1.22\times MSS}{RTT\sqrt{L}}
$$

###### 3.4.5.3 公平性

​		**瓶颈链路**指沿着某连接路径上的每条连接都不拥塞且相比该链路的传输容量都具有足够的传输容量。

​		假设$K$条TCP连接每条的端到端路径不同，但是都经过一段传输速率为$R(b/s)$的瓶颈链路。若每条连接都在传输一个大文件且无UDP流量通过该链路，而且每条连接的平均传输速度接近$\frac{R}{K}$，则认为该拥塞控制机制是**公平**的。在这种理想情况下，当所有连接的RTT相同时才能平等共享带宽。实际上这些条件不能满足，具有较小RTT的连接可以更快地扩大拥塞窗口。

​		UDP并没有内置的拥塞控制机制，UDP是不公平的，UDP可能抑制TCP。

​		当一个应用使用多条并行TCP连接时，对单条TCP连接可能是公平的，但对应用并不公平。

### 第四章 网络层

​		

​		

### 附录1 专业术语

> **access point(AP)** 访问接入点
>
> **acknowledgment(ACK)** 确认
>
> **active optical network terminator(AON)** 主动光纤网络
>
> **additive increase,multiplicative decrease(AIMD)** 加性增、乘性减
>
> **alternating bit protocol** 比特交替协议
>
> **application programming interface(API)** 应用程序编程接口
>
> **automatic repeat request(ARQ)** 自动重传请求
>
> **available bite rate(ABR)** 可用比特率
>
> **average throughput** 平均吞吐量
>
> **band width** 带宽
>
> **bandwidth sensitive application** 带宽敏感的应用
>
> **Berkeley Internet Name Domain(BIND/NAMED)** DNS服务器软件
>
> **best effort delivery service** 尽力而为交付服务
>
> **bidirectional data transfer** 双向/全双工数据传输
>
> **botnet** 僵尸网络
>
> **bottleneck link** 瓶颈链路
>
> **bring home** 客邀
>
> **cable internet access** 电缆因特网接入
>
> **cable modern** 电缆调制解调器
>
> **cable modern termination system(CMTS)** 电缆调制解调器端接系统
>
> **canonical hostname** 规范主机名
>
> **choke packet** 抑制分组
>
> **circuit** 电路
>
> **circuit switching** 电路交换
>
> **client** 客户端
>
> **cluster selection strategy** 集群选择策略
>
> **communication link** 通信链路
>
> **congestion avoidance** 拥塞避免
>
> **congestion control** 拥塞控制
>
> **congestion window reduced(CWR)**  拥塞窗口减少
>
> **congestion window(cwnd)** 拥塞窗口
>
> **connection oriented** 面向连接的
>
> **content distribution network(CDN)** 内容分发网络
>
> **content provider network** 内容提供商网络
>
> **countdown timer** 倒数计时器
>
> **cumulative acknowledgement** 累积确认
>
> **customer** 客户
>
> **data center** 数据中心
>
> **data center TCP(DCTCP)** 数据中心TCP
>
> **datagram** 数据报
>
> **datagram congestion control protocol(DCCP)** 数据报拥塞控制协议
>
> **denial of service(DOS)** 拒绝服务
>
> **demultiplexing** 多路分解
>
> **destination port number field** 目的端口号字段
>
> **digital subscriber line(DSL)** 数字用户线
>
> **distributed application** 分布式应用程序
>
> **distribution time** 分发时间
>
> **DNS caching** DNS缓存
>
> **domain name system(DNS)** 域名系统
>
> **duplicate data packet** 冗余数据分组
>
> **dynamic adaptive streaming over HTTP(DASH)** 经HTTP的动态适应流
>
> **ECN Echo(ECE)** 显式拥塞提醒回应
>
> **edge router** 边缘路由器
>
> **encapsulation** 封装
>
> **end system** 端系统
>
> **end-end principle** 端到端原则
>
> **end-to-end connection** 端到端连接
>
> **enter deep** 深入
>
> **elastic application** 弹性应用
>
> **event based programming** 基于事件的编程
>
> **explicit congestion notification(ECN)** 显式拥塞通知
>
> **extend simple mail transfer protocol(ESMTP)** 扩展简单邮件传输协议
>
> **exponential weighted moving average(EWMA)** 指数加权移动平均
>
> **fast recovery** 快速恢复
>
> **fast retransmit** 快速重传
>
> **flow control service** 流量控制协议
>
> **fiber to the home(FTTH)** 光纤到户
>
> **file transfer protocol(FTP)** 文件传输协议
>
> **finite-state machine(FSM)** 有限状态机
>
> **forwarding table** 转发表
>
> **frame** 帧
>
> **frequency-division multiplexing(FDM)** 频分复用
>
> **full duplex service** 全双工服务
>
> **geographically closest** 地理上最近
>
> **geostationary satellite** 同步卫星
>
> **go-back-n(GBN)** 回退N步
>
> **guided media** 导引型媒体
>
> **header line** 首部行
>
> **host** 主机
>
> **host aliasing** 主机别名
>
> **hostname** 主机名
>
> **hybrid fiber coax(HFC)** 混合光纤同轴
>
> **hyper text transfer protocol(HTTP)** 超文本传输协议
>
> **initial sequence number(ISN)** 初始序号
>
> **instantaneous throughput** 瞬时吞吐量
>
> **internet exchange point(IXP)** 因特网交换点
>
> **internet engineering task force(IETF)** 因特网工程任务组
>
> **internet mail access protocol(IMAP)** 因特网邮件访问协议
>
> **internet protocol(IP) **网络协议
>
> **internet service provider(ISP)**  因特网服务提供商
>
> **internet standard** 因特网标准
>
> **IP spoofing** IP哄骗
>
> **link-layer switch** 链路层交换机
>
> **layer** 分层
>
> **load distribution** 负载分配
>
> **logic communication** 逻辑通信
>
> **long-term evolution(LTE)** 长期演进
>
> **loss-tolerant application** 容忍丢失的应用
>
> **low-earth orbiting(LEO)** 近地轨道
>
> **mail server aliasing** 邮件服务别名
>
> **manifest file** 告示文件
>
> **malware** 恶意软件
>
> **maximum segment size(MSS)** 最大报文段长度
>
> **maximum transmission unit(MTU)** 最大传输单元
>
> **message** 报文
>
> **multi-home** 多宿
>
> **negative acknowledgment(NAK)** 否定确认
>
> **net file system(NFS)** 网络文件系统
>
> **network architecture** 网络体系结构
>
> **nodal processing delay** 节点处理时延
>
> **nonce sum(NS)** 随机数和
>
> **non-persistent connection** 非持续连接
>
> **offered load** 供给载荷
>
> **open system interconnection reference model(OSI model)** 开放式系统互联网通信参考模型
>
> **optical Carrier(OC)** 光载波
>
> **optical line terminator(OLT)** 光纤线路端连接器
>
> **optical network terminator(ONT)** 光纤网络端接器
>
> **output buffer** 输出缓存
>
> **output queue** 输出队列
>
> **packet** 分组
>
> **packet loss** 分组丢包
>
> **packet sniffer** 分组嗅探器
>
> **packet switch** 分组交换机
>
> **packet switching** 分组交换
>
> **passive optical network(PON)** 被动光纤网络
>
> **path** 路径
>
> **payload field** 有效载荷字段
>
> **peer** 对等
>
> **peer to peer(P2P)** 点对点
>
> **persistent connection** 持续连接
>
> **piggyback** 捎带  
>
> **pipelining** 流水线
>
> **point of presence(POP)** 存在点
>
> **point to point** 点对点
>
> **post office protocol-version 3(POP3)** 第三版邮局
>
> **propagation delay** 传播时延
>
> **protocol** 协议
>
> **provider** 提供商
>
> **physical medium** 物理媒体
>
> **pull protocol** 拉协议
>
> **push protocol** 推协议
>
> **queuing delay** 排队时延
>
> **quick UDP internet connection(QUIC)** 快速UDP互联网连接
>
> **rarest first** 最稀缺优先
>
> **real time measurement** 实时测量
>
> **reliable data transfer(RDT)** 可靠数据传输
>
> **request for comment(RFC)** 请求评论
>
> **request line** 请求行
>
> **resource record(RR)** 资源记录
>
> **round trip time(RTT)** 往返时间
>
> **route** 路径
>
> **router** 路由器
>
> **secure shell(SSH)** 安全外壳
>
> **segment** 报文段
>
> **selective acknowledgement(SACK)** 选择性确认
>
> **selective repeat(SR)** 选择重传
>
> **self clocking** 自计时的
>
> **sequence number** 序号
>
> **server** 服务器
>
> **shared medium** 共享媒体
>
> **silent period** 静默期
>
> **simple mail transfer protocol(SMTP)** 简单邮件传输协议
>
> **simple network management protocol(SNMP)** 简单网络管理协议
>
> **sliding-window protocol** 滑动窗口协议
>
> **slow start** 慢启动
>
> **slow start threshold(ssthresh)** 慢启动阈值
>
> **socket** 套接字
>
> **source port number field** 源端口号字段
>
> **source sockets layer** 安全套接字层
>
> **splitter** 分配器
>
> **stateless protocol** 无状态协议
>
> **stop and wait** 停等
>
> **store and forward transmission** 存储转发传输
>
> **stream control transmission protocol(SCTP)** 流控制传输协议
>
> **TCP friendly rate control(TFRC)** TCP友好速度控制
>
> **TCP splitting** TCP分岔
>
> **three way handshake** 三次握手
>
> **time division multiplexing(TDM)**  时分复用
>
> **time to live(TTL)** 生存时间
>
> **tit for tat** 一报还一报
>
> **top down approach** 自顶向下方方法
>
> **top level domain(TLD)** 顶级域
>
> **torrent** 洪流
>
> **total nodal delay** 节点总时延
>
> **tracker** 追踪器
>
> **traffic intensity** 流量强度
>
> **traffic volume** 通信容量
>
> **transmission control protocol(TCP)** 传输控制协议
>
> **transmission delay** 传输时延
>
> **transmission rate** 传输速度
>
> **unchoked** 疏通
>
> **unguided media** 非导引型媒体
>
> **unidirectional data transfer** 单向/半双工数据传输
>
> **unreliable service** 不可靠服务
>
> **unshielded twisted pair(UTP)** 无屏蔽双绞线
>
> **user agent** 用户代理
>
> **user datagram protocol(UDP)** 用户数据报协议
>
> **utilization** 利用率
>
> **well-known port number** 周知端口号

### 附录2 相关文档

> cookie相关：RFC 6265
>
> DCCP相关：RFC 4340
>
> DNS相关：RFC 1034、RFC 1035、RFC 2136、RFC 3007
>
> email相关：RFC 5322
>
> HTTP相关：RFC 1945、RFC 2616、RFC 7540
>
> IMAP相关：RFC 3501
>
> POP3相关：RFC 1939
>
> port相关：RFC 1700、RFC 3232
>
> SCTP相关：TFC 3286、RFC 4960
>
> SMTP相关：RFC 821、RFC 1425、RFC 1511、RFC 1521、RFC 1522、RFC 5321 
>
> telnet相关：RFC 854
>
> TFRC相关：RFC 5348
>
> TCP相关：RFC 793、RFC 1122、RFC 1323、RFC 2018、RFC 2581、RFC 3168、RFC 3390、RFC 3649、RFC 3782、RFC 5681
>
> UDP相关：RFC 768