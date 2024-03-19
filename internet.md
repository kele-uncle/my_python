
# Internet

## 协议
**HTTP 是一个在计算机世界里专门在两点之间传输文字、图片、音频、视频等超文本数据的约定和规范**

## 网络模型
为了给网络协议的设计提供一个结构，网络设计者以`分层(layer)`的方式组织协议，每个协议属于层次模型之一。每一层都是向它的上一层提供`服务(service)`，即所谓的`服务模型(service model)`。每个分层中所有的协议称为 `协议栈(protocol stack)`。因特网的协议栈由五个部分组成：物理层、链路层、网络层、运输层和应用层。我们采用自上而下的方法研究其原理，也就是应用层 -> 物理层的方式

### 应用层
应用层是网络应用程序和网络协议存放的分层，因特网的应用层包括许多协议，例如我们学 web 离不开的 `HTTP`，电子邮件传送协议 `SMTP`、端系统文件上传协议 `FTP`、还有为我们进行域名解析的 `DNS` 协议。应用层协议分布在多个端系统上，一个端系统应用程序与另外一个端系统应用程序交换信息分组，我们把位于应用层的信息分组称为 `报文(message)`。
### 运输层
因特网的运输层在应用程序断点之间传送应用程序报文，在这一层主要有两种传输协议 `TCP`和 `UDP`，利用这两者中的任何一个都能够传输报文，不过这两种协议有巨大的不同。<br />TCP 向它的应用程序提供了面向连接的服务，它能够控制并确认报文是否到达，并提供了拥塞机制来控制网络传输，因此当网络拥塞时，会抑制其传输速率。<br />UDP 协议向它的应用程序提供了无连接服务。它不具备可靠性的特征，没有流量控制，也没有拥塞控制。<br />我们把运输层的分组称为 `报文段(segment)`<br />	ＴＣＰ和ＵＤＰ的比较

|  | ＵＤＰ | ＴＣＰ |
| --- | --- | --- |
| 是否连接 | 无连接 | 面向连接 |
| 是否可靠 | 不可靠传输 | 可靠传输 |
| 连接对象个数 | 支持一对一，一对多，多对一，多对多交互通信 | 只能是一对一传输 |
| 传输方式 | 面向报文 | 面向字节流 |
| 首部开销 | 首部开销小，只有８字节 | 首部最小２０字节，最大６０字节 |
| 适用场景 | 适用于实时应用（ＩＰ电话，视频会议，直播等） | 适用于要求可靠传输的应用，例如文件传输 |

### 网络层
因特网的网络层负责将称为 `数据报(datagram)` 的网络分层从一台主机移动到另一台主机。网络层一个非常重要的协议是 `IP` 协议，所有具有网络层的因特网组件都必须运行 IP 协议，IP 协议是一种网际协议，除了 IP 协议外，网络层还包括一些其他网际协议和路由选择协议，一般把网络层就称为 IP 层，由此可知 IP 协议的重要性
### 链路层
为了将分组从一个节点（主机或路由器）运输到另一个节点，网络层必须依靠链路层提供服务。链路层的例子包括以太网、WiFi 和电缆接入的 `DOCSIS` 协议，因为数据从源目的地传送通常需要经过几条链路，一个数据包可能被沿途不同的链路层协议处理，数据链路层会将网络层传递下来的数据拆分为多段，并在每段数据前后分别添加首部和尾部，以构成一个完成的帧，帧是链路层传输的基本数据单元。帧首部用控制字符 `SOH` 表示，帧尾部用控制字符 `EOT` 表示 `帧(frame)`，
### 物理层
物理层的作用是将帧中的一个个 `比特` 从一个节点运输到另一个节点，物理层的协议仍然使用链路层协议
## Web 服务
输入地址后的操作，以 [http://www.someSchool.edu/someDepartment/home.index](http://www.someSchool.edu/someDepartment/home.index) 为例子<br />DNS服务器会首先进行域名的映射，在本地寻找网址对应的IP地址，如果没有，就到DNS服务器上寻找，找到访问`www.someSchool.edu`所在的地址，然后HTTP 客户端进程在 80 端口发起一个到服务器 `www.someSchool.edu` 的 TCP 连接（80 端口是 HTTP 的默认端口）。在客户和服务器进程中都会有一个`套接字`与其相连。<br />HTTP 客户端通过它的套接字向服务器发送一个 HTTP 请求报文。该报文中包含了路径`someDepartment/home.index` 的资源<br />HTTP 协议主要由三大部分组成：

- `起始行（start line）`：描述请求或响应的基本信息；
- `头部字段（header）`：使用 key-value 形式更详细地说明报文；
- `消息正文（entity）`：实际传输的数据，它不一定是纯文本，可以是图片、视频等二进制数据。

其中起始行和头部字段并成为 `请求头` 或者 `响应头`，统称为 `Header`；消息正文也叫做实体，称为 `body`。HTTP 协议规定每次发送的报文必须要有 Header，但是可以没有 body，也就是说头信息是必须的，实体信息可以没有。而且在 header 和 body 之间必须要有一个空行（CRLF）<br />HTTP 服务器通过它的套接字接受该报文，进行请求的解析工作，并从其`存储器(RAM 或磁盘)`中检索出对象 www.someSchool.edu/someDepartment/home.index，然后把检索出来的对象进行封装，封装到 HTTP 响应报文中，并通过套接字向客户进行发送。<br />HTTP 服务器随即通知 TCP 断开 TCP 连接，实际上是需要等到客户接受完响应报文后才会断开 TCP 连接。<br />HTTP 客户端接受完响应报文后，TCP 连接会关闭。HTTP 客户端从响应中提取出报文中是一个 HTML 响应文件，并检查该 HTML 文件，然后循环检查报文中其他内部对象。<br />检查完成后，HTTP 客户端会把对应的资源通过显示器呈现给用户。
### CDN
CDN的全称是`Content Delivery Network`，即`内容分发网络`，它应用了 HTTP 协议里的缓存和代理技术，代替源站响应客户端的请求。CDN 是构建在现有网络基础之上的网络，它依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户`就近`获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有`内容存储`和`分发技术`
### 响应状态码
**2开头的表示请求成功**

| 200 | 成功响应 |
| --- | --- |
| 204 | 请求成功，但是没有资源返回 |
| 206 | 对资源某一部分进行响应，由Content-Range 指定范围的实体内容 |

**以 **`**3xx**`** 为开头的都表示需要进行附加操作以完成请求**

| 状态码 | **含义** |
| --- | --- |
| 301 | 永久性重定向，该状态码表示请求的资源已经重新分配 URI，以后应该使用资源现有的 URI |
| 302 | 临时性重定向。该状态码表示请求的资源已被分配了新的 URI，希望用户（本次）能使用新的 URI 访问。 |
| 303 | 该状态码表示由于请求对应的资源存在着另一个 URI，应使用 GET 方法定向获取请求的资源。 |
| 304 | 该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况 |
| 307 | 临时重定向。该状态码与 302 Found 有着相同的含义。 |

**以 **`**4xx**`** 的响应结果表明客户端是发生错误的原因所在**

| **状态码** | **含义** |
| --- | --- |
| 400 | 该状态码表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求 |
| 401 | 该状态码表示发送的请求需要有通过 HTTP 认证（BASIC 认证、DIGEST 认证）的认证信息 |
| 403 | 该状态码表明对请求资源的访问被服务器拒绝了 |
| 404 | 该状态码表明服务器上无法找到请求的资源 |

**以 **`**5xx**`** 为开头的响应标头都表示服务器本身发生错误**

| **状态码** | **含义** |
| --- | --- |
| 500 | 该状态码表明服务器端在执行请求时发生了错误 |
| 503 | 该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求 |

## TCP的三次握手和四次挥手
###　建立连接的三次握手<br />![三次握手.webp.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/21956743/1640153581357-87f3b19d-cd8d-409b-ae79-b070590433e1.jpeg#clientId=ude358951-2fce-4&from=ui&id=u66603156&originHeight=871&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=195429&status=done&style=none&taskId=u216ef2e5-94ff-4062-8d11-243d2e9eb96&title=)

1. 服务端进程准备好接收来自外部的 TCP 连接，一般情况下是调用 bind、listen、socket 三个函数完成。这种打开方式被认为是 `被动打开(passive open)`。然后服务端进程处于 `LISTEN` 状态，等待客户端连接请求。
2. 客户端通过 `connect` 发起`主动打开(active open)`，向服务器发出连接请求，请求中首部同步位 SYN = 1，同时选择一个初始序号 sequence ，简写 seq = x。SYN 报文段不允许携带数据，只消耗一个序号。此时，客户端进入 `SYN-SEND` 状态。
3. 服务器收到客户端连接后，，需要确认客户端的报文段。在确认报文段中，把 SYN 和 ACK 位都置为 1 。确认号是 ack = x + 1，同时也为自己选择一个初始序号 seq = y。这个报文段也不能携带数据，但同样要消耗掉一个序号。此时，TCP 服务器进入 `SYN-RECEIVED(同步收到)` 状态。
4. 客户端在收到服务器发出的响应后，还需要给出确认连接。确认连接中的 ACK 置为 1 ，序号为 seq = x + 1，确认号为 ack = y + 1。TCP 规定，这个报文段可以携带数据也可以不携带数据，如果不携带数据，那么下一个数据报文段的序号仍是 seq = x + 1。这时，客户端进入 `ESTABLISHED (已连接)` 状态
5. 服务器收到客户的确认后，也进入 `ESTABLISHED` 状态
### 断开连接的四次挥手
![四次挥手.webp.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/21956743/1640153566420-970434b5-5e03-4848-bbca-cd3592b7a960.jpeg#clientId=ude358951-2fce-4&from=ui&id=u28d42d44&originHeight=1031&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=268781&status=done&style=none&taskId=u0cadfd9f-0971-4358-9e06-590dc475bb5&title=)<br />TCP 断开连接需要历经的过程如下

1. 客户端应用程序发出释放连接的报文段，并停止发送数据，主动关闭 TCP 连接。客户端主机发送释放连接的报文段，报文段中首部 FIN 位置为 1 ，不包含数据，序列号位 seq = u，此时客户端主机进入 `FIN-WAIT-1(终止等待 1)` 阶段。
2. 服务器主机接受到客户端发出的报文段后，即发出确认应答报文，确认应答报文中 ACK = 1，生成自己的序号位 seq = v，ack = u + 1，然后服务器主机就进入 `CLOSE-WAIT(关闭等待)` 状态。
3. 客户端主机收到服务端主机的确认应答后，即进入 `FIN-WAIT-2(终止等待2)` 的状态。等待客户端发出连接释放的报文段。
4. 这时服务端主机会发出断开连接的报文段，报文段中 ACK = 1，序列号 seq = v，ack = u + 1，在发送完断开请求的报文后，服务端主机就进入了 `LAST-ACK(最后确认)`的阶段。
5. 客户端收到服务端的断开连接请求后，客户端需要作出响应，客户端发出断开连接的报文段，在报文段中，ACK = 1, 序列号 seq = u + 1，因为客户端从连接开始断开后就没有再发送数据，ack = v + 1，然后进入到 `TIME-WAIT(时间等待)` 状态，请注意，这个时候 TCP 连接还没有释放。必须经过时间等待的设置，也就是 `2MSL` 后，客户端才会进入 `CLOSED` 状态，时间 MSL 叫做`最长报文段寿命（Maximum Segment Lifetime）`。
6. 服务端主要收到了客户端的断开连接确认后，就会进入 CLOSED 状态。因为服务端结束 TCP 连接时间要比客户端早，而整个连接断开过程需要发送四个报文段，因此释放连接的过程也被称为四次挥手。
## Http 中post 和 get
### **常用的HTTP请求方法都有哪些**
> `HTTP/1.1协议`中共定义了`八种方法`，有时也叫`动作`，来表明`Request-URL`指定的资源不同的操作方式
> 1. 在HTTP1.0中，定义了三种请求方法：`GET, POST 和 HEAD`方法。
> 2. 在HTTP1.1中，新增了五种请求方法：`OPTIONS, PUT, DELETE, TRACE 和 CONNECT`方法 但我们常用的一般就是`GET和POST`请求。

### GET和POST请求都有哪些区别
> GET请求在URL中传送的参数是有长度限制的，而POST没有。
> GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。而POST数据不会显示在URL中。是放在Request body中。
> 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
> GET请求参数会被完整保留在浏览器历史记录里；相反，POST请求参数也不会被浏览器保留。
> GET请求只能进行url编码（ application/x-www-form-urlencoded），而POST支持多种编码方式。
> GET请求会被浏览器主动缓存，而POST不会，除非手动设置。
> GET在浏览器回退时是无害的，而POST会再次提交请求

### Get请求有Request body么？如果有的话参数可以像Post请求一样放在里面么
> GET和POST在本质上没有区别，都是HTTP协议中的两种发送请求的方法。而HTTP呢，是基于TCP/IP的关于数据如何在万维网中如何通信的协议
> HTTP的底层是TCP/IP。所以GET和POST的底层也是TCP/IP，也就是说，GET/POST都是TCP链接。
> GET和POST能做的事情是一样一样的。你要给GET加上request body，给POST带上url参数，技术上是完全行的通的
> TCP就像汽车，我们用TCP来运输数据，它很`可靠`，从来`不会发生丢件少件`的现象。
> 但是如果路上跑的全是看起来一模一样的汽车，那这个世界看起来是一团混乱，送急件的汽车可能被前面满载货物的汽车拦堵在路上，整个交通系统一定会`瘫痪`。
> 为了避免这种情况发生，`交通规则HTTP`诞生了。HTTP给汽车运输设定了好几个服务类别，包括`GET, POST, PUT`等等，
> HTTP规定，当执行GET请求的时候，要给汽车贴上GET的标签（设置method为GET），而且要求把传送的数据放在车顶上（url中）以方便记录。
> 如果是POST请求，就要在车上贴上POST的标签，并把货物放在`车厢`里（request body中）。
> 当然，你也可以在用GET的时往车厢内偷偷藏点货物，但这并不不光彩；也可以在POST的时候在`车顶`上也放一些数据，也会让人觉得傻乎乎的
> HTTP只是个`行为准则`，而GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。

### URL中传送参数的长度限制在Get和Post中都是怎么样的
> 在Web中啊，还有另一个重要的角色：`运输公司`
> 不同的浏览器Client端（发起http请求）和服务器server端（接受http请求）就是不同的运输公司。
> 虽然理论上，你可以在车顶上无限的堆货物（url中无限加参数）。但是运输公司可不傻，装货和卸货也是有很大成本的，他们会`限制单次运输量`来控制风险，数据量太大对浏览器和服务器都是很大`负担`。
> 业界不成文的规定是，（大多数）浏览器通常都会限制url长度在`2K`个字节，而（大多数）服务器最多处理`64K`大小的url。
> 超过的部分，恕不处理。如果你用GET服务，在request body偷偷藏了数据，不同服务器的处理方式也是不同的，有些服务器会帮你卸货，读出数据，有些服务器直接忽略。
> 所以，虽然GET可以带request body，却`不能保证一定能被接收到`。

### GET 方法参数写法是固定的吗
> 参数是写在`?`后面，用`&`分割。就像下面这样
> 解析报文的过程是通过获取 TCP 数据，用正则等工具从数据中获取 `Header 和 Body`，从而提取参数。
> 比如header请求头中添加`token`，来验证用户是否登录等`权限`问题。
> 也就是说，我们可以自己约定参数的写法，只要服务端能够解释出来就行，万变不离其宗

### 是不是POST 方法比 GET 方法更安全呢
> 有人说POST 比 GET 安全，因为数据在地址栏上`不可见`。
> 然而，从传输的角度来说，他们都是不安全的，因为 HTTP 在网络上是`明文传输`的，只要在网络节点上捉包，就能完整地获取数据报文。
> 其实，要想安全传输，就只有加密，也就是 `HTTPS`

### Get、Post请求发送的数据包有什么不同
GET请求时产生`一个`TCP数据包；POST请求时产生`两个`TCP数据包
```bash
GET：浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
POST：浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 OK（返回数据）。
```
> 就像是GET只需要汽车跑一趟就把货送到了，而POST得跑两趟，第一趟，先去和服务器打个招呼`老铁，我等下要送一批货来，你们准备接收一下哈`，然后再回头把货送过去。
> 因为POST需要两步，理论上时间上消耗的要多一点，看起来GET比POST更有效。但并不是，后来发现原来是个坑。在我看来：
> 1. GET与POST都有自己的语义，不能随便混用。
> 2. 据研究，在网络环境好的情况下，发一次包的时间和发两次包的时间差别基本可以无视。而在网络环境差的情况下，两次包的TCP在验证数据包完整性上，有非常大的优点。
> 3. 并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。我去年用Chrome浏览器测试发现也是只发送一次，所以我认为Get、POST性能差可以人为忽略。
