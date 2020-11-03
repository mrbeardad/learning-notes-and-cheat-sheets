# 目录
<!-- vim-markdown-toc GFM -->

- [互联网结构](#互联网结构)
  - [ISP](#isp)
  - [接入网](#接入网)
  - [物理媒体](#物理媒体)
- [协议分层](#协议分层)
  - [应用层](#应用层)
    - [DNS](#dns)
    - [DHCP](#dhcp)
    - [HTTP](#http)
    - [SMTP](#smtp)
    - [BitTorrent](#bittorrent)
    - [Socket接口](#socket接口)
      - [TCP](#tcp)
      - [UDP](#udp)
  - [传输层](#传输层)
    - [TCP](#tcp-1)
      - [全双工连接](#全双工连接)
      - [可靠数据传输](#可靠数据传输)
      - [流量控制](#流量控制)
      - [拥塞控制](#拥塞控制)
    - [UDP](#udp-1)
  - [网络层](#网络层)
    - [数据平面](#数据平面)
    - [控制平面](#控制平面)
  - [链路层](#链路层)
    - [WiFi](#wifi)
- [分组交换技术](#分组交换技术)
- [差错校验技术](#差错校验技术)
- [安全通讯](#安全通讯)
- [总览](#总览)

<!-- vim-markdown-toc -->
a
# 互联网结构
## ISP
<!-- 层次结构 -->
![Internet1](../images/internet1.png)
![Internet2](../images/internet2.jpg)

## 接入网
* 广域网
    * DSL （基于电话线）
    * 电缆（基于电视线）
    * FTTH（光纤）
    * 拨号（传统电话线）
    * 卫星（卫星无线电）
    * 蜂窝网络
* 局域网
    * 以太网
    * WiFi

## 物理媒体
![wlmt](../images/OIP.jpg)
* 双绞铜线      ：最便宜且最常用、电话机、高速LAN主导性方案
* 同轴电缆      ：电缆电视系统、共享媒体
* 光纤          ：稳定安全、高成本光设备、海底光缆
* 陆地无线电信道：穿墙、移动用户、长距离
* 卫星无线电信道：同步卫星、近地轨道卫星

# 协议分层
## 应用层
### DNS
**UDP 53**
<!-- 层次结构 -->
* 分布式层次数据库
    * Public
        * 根DNS
        * 顶级DNS
    * Private
        * 权威DNS
        * 本地DNS

* DNS记录
    > `(Name, Value, Type ,TTL)`
    * `(DomainName, DNSHostName, NS)`
        > 获取一个域对应的DNS域名
    * `(HostName, IP, A)`
        > 将规范主机名映射到一个IP
    * `(HostAlias, HostName, CNAME)`
        > 获取主机别名对应的规范主机名
    * `(MailAlias, MailHostName, MX)`

* 查询DNS记录
    * 递归查询与迭代查询
    * DNS缓存
    * 负载分配

* 插入DNS记录
    * 向[注册登记机构](http://www.internic.net)申请
    * 向所有适当的TLD插入DNS记录
    * 若有多个IP主机则应该提供自己的DNS服务器，并设置别名

### DHCP
**UDP 67**
* DHCP发现：利用广播目的地址发现并请求DHCP主机

* DHCP响应：DHCP主机响应请求
    * 上述DHCP发现报文的事务ID
    * 向客户推荐的IP/Mask、DNS、GW
    * IP租用期

* DHCP请求：客户端选择接收一个DHCP响应并确认

* DHCP接收：服务端接收并确认客户端的确认

### HTTP
**TCP 80**
* 请求报文格式  
![http请求报文格式](../images/http.jpg)
    * 方法：
        * `GET`
            > 请求URL定位的资源，URL后`?`结尾表示URL结尾与请求参数开始
        * `POST`
        * `HEAD`
        * `PUT`
        * `DELETE`
        * `OPTIONS`
        * `TRACE`
        * `CONNECT`
    * 首部行：
        * `Host`                    ：目的主机名`google.com`
        * `User-Agent`              ：浏览器类型`Mozilla/5.0`
        * `Connection`              ：连接方式`close|Keep-Alive`
        * `Accept`                  ：内容类型`*/*`
        * `Accept-Language`         ：自然语言`zh-CN`
        * `Accept-Encoding`         ：压缩格式`gzip`
        * `Accept-Charset`          ：字符编码`ISO-8859-1`
        * `If-Modified-Since`       ：是否已变更`Wed, 9 Sep 2015 09:23:24`
        * `Cookie`                  ：客户端存储目的域名的Cookie
        * `Reference`               ：由`URL`跳转而来

* 响应报文格式  
![http响应报文格式](../images/httpd.png)
    * 方法：`GET` `POST` `HEAD` `PUT` `DELETE`
    * 状态码：
        * 200 OK                    ：表示客户端请求成功
        * 400 Bad Request           ：表示客户端请求有语法错误，不能被服务器所理解
        * 401 Unauthonzed           ：表示请求未经授权，该状态代码必须与 WWW-Authenticate 报头域一起使用
        * 403 Forbidden             ：表示服务器收到请求，但是拒绝提供服务，通常会在响应正文中给出不提供服务的原因
        * 404 Not Found             ：请求的资源不存在，例如，输入了错误的URL
        * 500 Internal Server Error ：表示服务器发生不可预期的错误，导致无法完成客户端的请求
        * 503 Service Unavailable   ：表示服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复
    * 首部行：
        * `Location`                ：重定向
        * `Server`                  ：服务器类型`Apache/2.2.3`
        * `Date`
        * `Last-Modified`
        * `Content-Length`
        * `Content-Type`
        * `Set-Cookie`

* 其它特性
    * Cookie：
        * 客户端：记录本机在目的域名上的Cookie号
        * 服务器：记录访问本网站的主机的信息

    * Web代理/缓存：进行代理访问，并缓存http报文

    * DASH：基于HTTP的动态适应性流
        * 视频编码为多个清晰度的版本
        * 下载时动态选择将要下载的块的版本

### SMTP
**TCP 25**
* 特点
    * SMTP握手
    * 持续连接
    * ASCII7

* 一般流程
    * 发送方发送报文给SMTP服务器
    * SMTP服务器利用SMTP协议转发报文给接收方SMTP服务器（循环尝试，失败则发邮件提醒发送方）
    * 接收方连接SMTP服务器接收报文，并存入接收方邮箱
    * 接收方通过客户端与其SMTP服务器通讯获取邮件

* 客户端与邮件服务器通讯
    * POP3
    * IMAP
    * HTTP

### BitTorrent
* 特点
    * 文件分块
    * 同时下载和上传文件块

* 一般流程
    * 向追踪器注册自己
    * 追踪器随机提供一个对等方子集
    * 并行连接所有对等方子集
    * 周期性向对等方询问其已有块列表
    * 应该选择下载哪个块：最稀缺优先原则
    * 响应哪些邻居请求：疏通
        > 疏通即4个最高带宽的对等方，每30秒随机选取新的试探对等方加入筛选

### Socket接口
#### TCP
* server
    * socket
    * bind
    * listen
    * accept
    * recv
    * send
    * close
* client
    * socket
    * connect
    * send
    * recv
    * close
#### UDP
* server
    * socket
    * bind
    * recv
    * send
    * close
* client
    * socket
    * send
    * recv
    * close

## 传输层
### TCP
#### 全双工连接
* 三次握手
    > 同时确定初始序号、超时间隔
    * `SYN->`
    * `<-SYN ACK`
    * `ACK->`

* 四次挥手
    * `FIN->`
    * `<-ACK`
    * `<-FIN`
    * `ACK->`

连接后双方会为对方保持接收/发送缓存

#### 可靠数据传输
* 可能出现的问题：
    * 数据损坏：校验、丢弃、重传
    * 数据缺失：单段重传、累积确认
    * 数据冗余：丢弃
    * 数据失序：缓存、排序

* 超时间隔计算
    > 指数加权移动平均（EWMA）
    * SampleRTT均值：$EstimatedRTT = (1 - \alpha) \times EstimatedRTT + \alpha \times SampleRTT$
        > 推荐$\alpha = 0.125$，仅在某时刻测量SampleRTT而非对每个报文都测量，且不会为重传的报文测量
    * SampleRTT偏离程度：$DevRTT = (1 - \beta) \times DevRTT + \beta \times |SampleRTT - EstimatedRTT|$
        > 推荐$\beta = 0.25$
    * 超时间隔：$TimeoutInterval = EstimatedRTT + 4 \times DevRTT$
        > 推荐$初始TimeoutInterval = 1s$，每次超时时会将TimeoutInterval加倍，
        > 下述定时器的第一、二个事件启动定时器时会重新用该公式计算

* 定时器：
    > 不是为每个数据包都维护一个定时器（开销大），而是只维护一个定时器，超时时重传未确认接收的最早的报文（单段重传）；  
    > 对于发送方，接收方回传的`ACK n`表示序号`n`之前的所有报文都已接收（累积确认）；  
    > 对于发送方，有如下三种事件：
    * 从应用层接收到待发送的数据
        * 若未启动定时器则计算超时间隔并启动之
    * 收到正常ACK
        * 若存在未确认的报文段，则重新计算超时间隔并重启定时器
    * 收到冗余ACK
        * 若连续收到3次冗余ACK则重传序号最小的未被确认的数据段
    * 定时器正常超时
        * 此时无所有发送的数据段均已被确认，重启计时器即可
    * 定时器异常超时
        * 重传序号最小的未被确认的数据段，超时间隔加倍并重启计时器

* 接收方回传ACK事件
    * 期望序号按序到达：延迟ACK，对下一个报文最多等待500ms（增加带宽利用率）
    * 期望序号按序到达，另一报文等待ACK：立即发送累积ACK（防止发送方误以为己方未收到数据）
    * 检测出间隔：立即发送冗余ACK
    * 能填充间隔的报文段到达：若其序号刚好为间隔开端，则立即发送ACK

#### 流量控制
计算已发送但未确认的数据段总大小，其长度不能超过接收窗口和发送窗口的大小：
* 未确认的数据在发送方看来是正在路上还未到达目的地的数据，故这些数据的长度不能超过接收窗口大小
* 且因为发送方需缓存这些数据直到接收方确认（可能会重传），故这些数据的长度也不能超过发送窗口大小

当接收窗口大新为0时，仍需持续发送1字节数据的数据段，直到接收窗口空闲

#### 拥塞控制
* 原因与代价：
    * 存储转发引起的排队延迟
    * 报文重传导致的带宽下降

* 状态模式：
    * 慢启动  ：拥塞窗口大小呈指数增加，即每接收到一个数据段的ACK便增大1×MSS
    * 拥塞避免：拥塞窗口大小呈线性增加，即每RTT值增大1×MSS
    * 快速恢复：每收到的一个冗余ACK便增大1×MSS

* 事件触发：
    * 初始：拥塞窗口置1×MSS，进入慢启动
    * 超时：记录拥塞窗口半值，拥塞窗口置1×MSS，进入慢启动
    * 窗口大小达到上次记录的拥塞窗口半值：进入拥塞避免
    * 冗余ACK：进入快速恢复，接收到正常ACK后减小窗口并进入拥塞避免

**拥塞控制与可靠传输中的事件结合：**

* 从应用层接收到待发送的数据<u>（->慢启动->拥塞避免）</u>
    * 若未启动定时器则计算超时间隔并启动之
* 收到正常ACK
    * 若存在未确认的报文段，则重新计算超时间隔并重启定时器
* 收到冗余ACK <u>（->快速重传 -> 拥塞避免）</u>
    * 若连续收到3次冗余ACK则重传序号最小的未被确认的数据段
* 定时器正常超时
    * 此时无所有发送的数据段均已被确认，重启计时器即可
* 定时器异常超时<u>（->慢启动->拥塞避免）</u>
    * 重传序号最小的未被确认的数据段，超时间隔加倍并重启计时器

**关于公平性：** TCP链接越多带宽占有率越大

### UDP
* 不保证数据不缺失
* 不保证数据不失序
* 不保证数据不冗余
* 校验但不恢复差错

## 网络层
### 数据平面
* 利用CIDR（无类别域间路由选择）将传统分类编址的IP地址的分类一般化：`IP/Mask`

* **主机** 根据路由表选择下一条地址

* **网关** 接收子网主机的数据包并进行转发，可能会利用到NAT与UPnP

* 网络核心中的 **路由器** 根据路由表选择下一条地址，直到交付成功、丢包、TTL为0

* 防火墙机制
![图片来自网络](../images/netfilter.jpg)

* Linux路由表：用户可自定义1-252号路由表，内核维护如下4个路由表

| 表号 | 名称    | 说明                 |
|------|---------|----------------------|
| 0    | system  | 系统保留             |
| 253  | default | 没特别指定的默认路由 |
| 254  | main    | 没指定表号的所有路由 |
| 255  | local   | 保存本地接口地址     |

### 控制平面
<!-- 层次结构 -->
* **AS（自治系统）内部的路由选择：OSPF（开放最短路优先）**  
利用OSPF报文，AS内部路由器进行周期性广播通报，从而使每台路由器维护整个AS拓扑图（邻接表），
从而通过Dijkstra算法求得最短路由

* **ISP之间的路由选择：BGP（边界网关协议）**  
AS之间使用BGP报文来互相通告每个AS中含有的子网信息，并最终扩散到整个互联网

## 链路层
* 成帧：MTU
* 链路接入：MAC
    > 为何不用IP代替MAC
    > * 保证独立性
    > * 避免移动时重新配置IP
    > * 避免主机频繁被无关帧中断
* 数据有效

**交换机：**
* 作用
    * 消除碰撞
    * 异质链路
    * 网络管理
    * 自学习
* 转发原理
    * 情况一：无对应MAC地址的表项
        > 泛洪
    * 情况二：存在对应表项，且来源端口与表项中绑定的端口相同
        > 过滤
    * 情况三：存在对应表项，且来源端口不与表项中绑定的端口相同
        > 转发

### WiFi
* 扫描：被动扫描与主动扫描信道以发现AP
* 鉴别：通过身份验证
* 关联：无线主机在AP注册成功，并获取临时密钥进行链路层加密

**注意：一般无线网卡无法抓取MAC地址非本机的包（即使开启混杂模式）** 

# 分组交换技术
**时延：**
> 分类
* 处理时延（验证、路由等计算）
* 排队时延（按队列接收并转发数据）
* 传输时延（存储转发）
* 传播时延（信号亚光速传播）

**带宽：**
> 影响因素
* 网络设备
* 端设备
* 拓扑结构
* 数据类型
* 用户数量

# 差错校验技术
* （二维）奇偶校验
    > 通过附加检验比特位来使行（列）中1的个数为奇数，
    > 一维奇偶校验无法检验偶数个比特差错，
    > 二维奇偶校验不但可以检验、定位并矫正单个比特差错，还可以检验任意两个组合的比特差错

* 检验和方法（TCP与UDP）
    > 将数据分为一个个int16_t并回卷求和，再求反码，即为检验和。
    > 检验时将所有数据（包括检验和）分为一个个int16_t并求和，所得应为数位全1

* CRC（循环冗余检测）
    > 链路层

# 安全通讯
* 机密性
* 完整性
* 端点鉴别
* 运行安全性

# 总览
* 网络协议栈
    * 发送：
        * 应用层调用socket接口函数，发送message
        * 传输层确认参数（协议、目的端口号、本地端口号），并封装为segment
        * 网络层利用路由表确认参数（协议、目的IP、本地IP等），分片并封装为datagram，若目的IP为本地则直接上传
            > 将目的IP与本地子网掩码进行位与运算，得到网络号，再与路由表对比，收包时同理
        * 链路层通过驱动程序，利用ARP表确认目的MAC地址，若不存在则发送ARP请求，最后封装成frame，然后发送到网卡进行信号转换并传输
    * 接收：
        * 链路层中网卡默认会将MAC地址非本机且非广播的frame丢弃，若为ARP包则本层(驱动)解决
        * 网络层确认目的IP是否为本机IP或本机网段广播，若为ICMP则本层解决
        * 传输层确认socket的协议与端口，上传给应用层
        * 应用层控制会话的挂断

* 虚拟网卡：在网络层根据路由表即返回
* 虚拟网桥：相当于转发路由器
* 虚拟网关：利用NAT进行转发