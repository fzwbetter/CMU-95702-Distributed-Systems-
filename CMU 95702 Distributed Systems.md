# CMU 95702 Distributed Systems 期末复习

- Author@Zhenwei Feng
- modified from [http://bgmeow.xyz/2016/12/12/95702-Review/](http://bgmeow.xyz/2016/12/12/95702-Review/)，本版本相对更加接地气，final-exam oriented
- 红标为2019.12.10期末考试中考到的，期末考3小时，总共103题，两道编程题，其他为选择题
- 本page未包含blockchain（基本不考），security（<5题），android（有6-10题），web service（<3题，与其他概念混着考），raft（5-8题）

# ch1 Introduction

## Fundamental characterization of distributed systems

Concurrency:组件在位于不同位置的计算机上同时运行

Independent failures

No Global clock时间不同

## Communication Paradigms:

Interprocess Communications, 

Remote Invocation, 

Indirect Communication

## Challenges in Constructing Distributed Systems（期中考有）

### Heterogeneity

不同的网络/硬件/操作系统/语言

### Openness

是否可被延展，比如interface的documentation（publish）

### Security

confidentiality (protection against disclosure to unauthorized individuals)

integrity(protection against alteration or corruption)

availability (protection against interference with the means to access the resources)

### Scalability

如果在增加大量资源和用户的情况下，系统的效率仍能保持不变，则认为这个系统的是可延展的。

包括控制成本、性能控制、避免资源耗尽（比如IPV4）、避免瓶颈

### Failure handling

监测失败。

覆盖失败。将失败掩盖或者使它不那么剧烈。比如重新发送信息和备份资源。

忍受失败

从失败恢复

冗余

### Concurrency

everal clients will attempt to access a shared resource at the same time.

### Transparency

The aim is to make certain aspects of distribution invisible to the
application programmer so that they need only be concerned with the design of their
particular application.

### Quality of Service

reliability, security and performance

# ch2 System Models

An **architectural model** is concerned with the placement if its components and the relationships between them.
‣ client-server systems
‣ peer-to-peer systems

**Fundamental models** are concerned with a more formal description of the properties that are common in all the architectural models.

• **Interaction model:** computation occurs within processes that interact bypassing messages, resulting in communication (i.e., information flow) and coordination (synchronization and ordering of activities) between processes.
• **Failure model:** the correct operation of a distributed system is threatened whenever a fault occurs in any of the computers on which it runs or in the network that connects them.

**Security model:** the openness of distributed systems exposes them to attack by both external and internal agents.

### Factors Affecting Interacting Processes

### Communication performance

latency/bandwidth

### single global notion of time.

### Two Variants of the Interaction Model

‣ Synchronous distributed systems: strong assumption of time
‣ Asynchronous distributed systems: no assumptions about time

## Failure Model

‣ **Omission failures:** a process or communication channel fails to performactions that it is supposed to do(crash/channel omission/send-ommision/receive-omission)
‣ **Arbitrary failures:** any type of error may occur
‣ **Timing failures**: applicable in **synchronous** distributed systems

## Security Model

The security of a distributed system can be achieved by securing the processes and the channels used for their interactions and by protecting the objects that they encapsulate against unauthorized access.

- Protecting Objects:Access rights specify who is allowed to perform the operations on an object
- Security Threats:

The threats from potential enemy are discussed under the headings **threats to
processes, threats to communication channels and denial of service**.

**threats to processes**

This lack of reliable knowledge is a threat to the correct functioning of both
servers and clients:

**threats to communication channels**

An enemy can copy, alter or inject messages as they travel across the
network.

**denial of service**

delaying or preventing actions by other users.

### Agreement in Pepperland

将B相对A的 latency 控制在   0到（max - min + 1）内。

A 和 B 准备一起打 C ，但是 A B 隔太远，又没有电话，只能靠人马。就约定好，A 发出攻击指令后，等待 MIN ，然后就出发；而 B 收到攻击指令后，等待 1 秒后出发

### Failure Detection in Pepperland

双方无法同时知道交流失败了

## www(IETF W3C来maintain)

三个main standard technological components：HTTP, URL, HTML

Http protocol：一般是request/response，request/acknowledge也行，必须是coupled。

### Request Header：

<method> <resource identifier> <HTTP Version>

[<Header>: <value>]比如：

Host: www.andrew.cmu.edu
User-Agent: Joe typing
Accept: text/html

### Response Header

<HTTP Version> <Status>

日期，服务器系统一些乱七八糟的

### Safe（只读）

### Idempotent（不管取几次都是一样）

get和head是safe&idempotent

post是既不也不

其他都是idempotent但是不safe

HTTP/2与 **HTTP/1.x**的区别**?（期中考包括）**

At a high level, HTTP/2:

- is binary, instead of textual
- is fully multiplexed, instead of ordered and blocking
- can therefore use one connection for parallelism
- uses header compression to reduce overhead
- allows servers to “push” responses proactively into client caches

### JEE

Java Server Pages (JSP)

# Ch3 Networking

### Some common application protocal

DHCP:

Dynamic Host Configuration Protocol 作用：Server assigns IP address to a host使得新连接的电脑可以动态的在可用的子网区间内获取 IP 地址。assigns the IP addresses to the computers or other devices on each LAN network.

ARP：

Address Resolution Protocol ：有些计算机的网络地址和 IP 地址没有直接联系，而 ARP 可以将 IP 地址和主机在本地网络的地址进行转换.find out the MAC Address of a device in LAN.

如果有IP / MAC，就不用arp，不然就广播寻找matching host，收对应的MAC，也就是arp。

HTTP:

Hypertext Transfer Protocol,Browser to server, web app to web service,etc.

HTTPS:安全的HTTP

DNS:

Domain Name System 作用Map domain name to IP address

RTP/RTCP：

Real-time Transport Protocol /eal-time Transport ControlProtocol

Delivers audio and video streams / statisticsand control information about RTP stream

LDAP：

Querying and modifying directory services information

POP/IMAP:

收邮件

SMTP：

发邮件，邮件传输

SSH:

Terminal-type (shell) access to remote computers

### Transport Layer（TCP/UDP）

PORT是16位的

端口号的区间是 0 ~ 65535 （216−1 。但是 1023 以下为 著名端口，由IANA注册，不能使用。1024 ~ 49151 是 已注册端口，一般也能用，除非由IANA注册。其他的49152-65535随便用。

1024,49152,65535

### Layer

transport layer：port

Internet layer：IP

Data link layer：MAC（每个机子都有一个）

### Datagrams

- 包交换网络中传输的最小单位。只能传输字母或者电码。
- 最大特点为 **one-shot** 过程：不用任何设置，一旦发送了就不管了。所以是 **unreliable**

发出去就不管了

Every datagram packet contains the full network address of the source and
destination hosts

### Routing

由router maintain 的routing table，至少每30秒由临近的人更新

寻找去其他网络的最佳路径（非同一LANs）

### RIP algorithm（期中考涵盖）

忽略那些经过自己的link update

如果是新的destination，加进去

如果remote node有更好的路线（比自己的cost少1以上，不含），

If the table was received on link n and it gives a different cost than the local table for a route that begins with link n, replace the cost in the local table with the new cost

### Protocol

a well-known set of rules and formats to be used for communication between processes in order to perform a given task.

### TCP and UDP Compared

**TCP**：

面向连接的协议。打电话，一头连另一头。速度慢。有顺序。有很多头部信息。可靠。

Sequencing：知道顺序

Flow control：有acknowledgement，把控传输速度（仅对有线有用）

Retransmission：丢包重传

Buffering：receiver可调整负载

Checksum：保证传输指质量

**UDP**：

面向数据的协议，不管连接。寄信一样，写了地址仍邮筒我们就无法掌控它了。速度快。没顺序。没啥头要包起来的。不可靠，容易丢包。

![CMU%2095702%20Distributed%20Systems/Untitled.png](CMU%2095702%20Distributed%20Systems/Untitled.png)

# Chapter 4 Interprocess Communications

### Middleware

Middleware provides a higher level programming abstraction for the development of distributed systems.

![CMU%2095702%20Distributed%20Systems/Untitled%201.png](CMU%2095702%20Distributed%20Systems/Untitled%201.png)

### External Data Representations

an agreed standard for the representation of data structures and primitive values

包括以下三种

### CORBA's CDR

Binary，支持各种语言；两边需要用到IDL(interface definition language)

### Java serialization

Binary，一定要该种语言才行。用Java Serialization来marshal和unmarshal，不需要IDL（其他example还有.NET remoting）

### XML and JSON

interoperable,JSON比XML更compact

对XML来说，XSDL or WSDL充当与IDL

### UDP based request response protocol

**Client：**

doOperation (RemoteObjectRef o, int methodId, byte[] arguments)

**Server：**

public byte[] getRequest ();

public void sendReply (byte[] reply, InetAddress clientHost, int clientPort);

![CMU%2095702%20Distributed%20Systems/Untitled%202.png](CMU%2095702%20Distributed%20Systems/Untitled%202.png)

### Fault tolerance measures for the UDP request response

Timeout了怎么办？

1.返回error message

2.如果server挂掉了，client会不停的发call。假设此时server又好了，会有两个选择，如果是idempotent就直接返回可以了， 如果不是的话，就返回之前计算过的一个给client。

### RPC Exchange protocols (R, RR, RRA)

![CMU%2095702%20Distributed%20Systems/Untitled%203.png](CMU%2095702%20Distributed%20Systems/Untitled%203.png)

# Chapter 5 Remote Method Invocation

### Interface Definition Language (IDL)

为了让不同编程语言的进程可以相互调用的一种语言

### Goals of Java RMI

- Distributed Java
- Almost the same syntax and semantics used by non-distributed applications
- Promote separation of concerns – the implementation is separate from the interface
- The transport layer is TCP/IP
- 比web service 要快

### Remote Object References

与调用本地object非常类似

1. The invoker uses the remote object reference to identify the object
2. The remote object reference may be passed as an argument to or return value from a local or remote method.

### EJB remote and local interfaces

Enterprise Java Beans (EJB’s) is component based middleware,提供远程与本地interface

（下图中M1-M6即是EJB）

![CMU%2095702%20Distributed%20Systems/Untitled%204.png](CMU%2095702%20Distributed%20Systems/Untitled%204.png)

### Generic RMI component interactions

使用TCP

Remote reference module：记录local object references与remote object references

Proxy：一个object对应一个，作用是marshals and unmarshals parameters

dispatcher and skeleton：一个class一个，作用跟proxy差不多

过程：communication module根据remote object references选择dispatcher，dispatcher在skeleton中选择method，skeleton unmarshal然后调用B中的方法

![CMU%2095702%20Distributed%20Systems/Untitled%205.png](CMU%2095702%20Distributed%20Systems/Untitled%205.png)

### Registries

Java：rmiregistry

CORBA：CORBA naming service

RMIresigtry仅仅在一开始被使用，传递remote object reference。server用Naming.rebind（），client用Naming.lookup（）

### Java RMI code examples

Client：

默认是没有security的，可以使用setSecurityManager（）

server objects also use ports>= 1024

需要set a system property that describes where we have stored the policy

Server：

Interface必须同时存在于server和client，体现separation of concerns

需要handle RemoteException

# Chapter 6, 9 Indirect Communication and Naming

### Indirect Messaging

发送者与接收者通过某种介质（如第三方），使得时间与空间都解耦（time uncoupling and space uncoupling）的一种信息交流方式。

好处：abstract interface/allow for redundancy/allow offline providers//asychronous

包括两种传输模式

![CMU%2095702%20Distributed%20Systems/Untitled%206.png](CMU%2095702%20Distributed%20Systems/Untitled%206.png)

### Point-to-point

只能一对一

### Publish subscribe

一对多

### Java's JMS API

![CMU%2095702%20Distributed%20Systems/Untitled%207.png](CMU%2095702%20Distributed%20Systems/Untitled%207.png)

全称为 Java 消息服务（Java Message Service）。

- 是一个用于indirect messaging的 API
- 是个抽象 API，类似于 JNDI 和 JDBC
- 与一些 **MOM（Message Oriented Middleware）**（如Apache ActiveMQ, Oracle Weblogic）交互。JMS并不是MOM
- 面向客户的接口, abstract MOM
- javax.jms
- 可与non-JMS交互
- JMS connector用于匹配格式
- Two vendor options：
    - Translate JMS into native mode（可与non-JMS交互）
    - Encapsulate JMS within native mode（不可与non-JMS交互）

### JMS Queues and topics

都不是client's code，并且被MOM 销毁

Queues：point to point（也可以一对多），guarantee delivery

Topics：publish-subscribe

### JMS message types

Stream：primitive type

Map

Text

Object

Bytes

### Message driven beans (MDBs)

components that are executed asynchronously by messages coming available in a Queue or Topic

发message的时候：

synchronous：

• MessageProducer writer…
• writer.send(message);

收message的时候

synchronous：用MessageConsumer

**asynchronous：用MDB  registers a listener**

# Chapter 12 Distributed File Systems

### NFS

NFS:use RPC over TCP/UDP

VFS（virtual file system）虚拟文件系统整合了本地和远端的操作，实现了访问透明：用户可以对本地和远端的文件进行无差别的操作

NFS 目标：

- 可以进行 UNIX FS 的所有操作
- 实现所有 POSIX API。
- 文件在任何一台机器上都可以获取
- 分布式存放分拣，不再需要实现新的协议

![CMU%2095702%20Distributed%20Systems/Untitled%208.png](CMU%2095702%20Distributed%20Systems/Untitled%208.png)

### AFS

AFS 与 NFS 最大的不同便是可扩展性（scalability），是weak consistency.

所有的file都是存在client的，除了file close以后，才会同步到server。

open file的时候，优先使用本地的，如果本地没有，才会尝试去用server的

### GFS

解决问题：not a massive number of files but massively large files are common. Write once, append, read many times.Throughput more important than latency.

Not POSIX.

![CMU%2095702%20Distributed%20Systems/Untitled%209.png](CMU%2095702%20Distributed%20Systems/Untitled%209.png)

### MapReduce

Programmer implements two interfaces: one for mappers and one for reducers.

Map takes records from source in the form of key value pairs. The key might be a document name and the value a document. The key might be a file offset and the value a line of the file.

A mapper object is initialized for each map task.

A reducer object is initialized for each reduce task.

Final output is written to the distributed file system, one file per reducer.

**Maps run in parallel. Reducers run in parallel. Map phase must be completely finished before the reduce phase can begin.
The combiner phase is run on mapper nodes after map phase.
This is a mini-reduce on local map output.**

### HDFS

Hadoop is an open source implementation of GFS.

These cluster operating systems (like Hadoop and GFS) are all about:
(a) storing files
(b) running applications on top of files

### Spark

RDD's, Transformations, Actions（注意红色字部分）

**Two Typical Transformations performed on an RDD：maps and filters**

![CMU%2095702%20Distributed%20Systems/Untitled%2010.png](CMU%2095702%20Distributed%20Systems/Untitled%2010.png)

    

# Chapter 14 Time and Global State

### Internal and external synchronization

Internal：几个人自己同步，如果最大skew是D，那么所有人的bound就是D

external：与外边的表同步，比如UTC

### Global state terminology (histories, happens-before relation,  cuts, linearization)

**histories:**

一个进程的历史（history）代表了该进程内发生的，并按照−>i−>i关系排序的一系列事件：history(pi)=hi=<e0i,e1i,e2i,…>

**happens-before relation:**逻辑上先后发生的东西

**cuts：**对事件进行切割，若切割线前的事件均发生与切割线后的事件前，则为一致切割。否则则为非一致切割

**linearization：**a total ordering of all events in a global history that is consistent with each
local history.

### Lamport clocks

逻辑时钟，有两个方式显示逻辑

1.同一进程中时间发生的时间顺序

2.消息传递的先后顺序

### Chandy and Lamport Snapshot Algorithm

![CMU%2095702%20Distributed%20Systems/Untitled%2011.png](CMU%2095702%20Distributed%20Systems/Untitled%2011.png)

# Transactions

### ACID Transactions

**原子性（atomic）**

可以使用Checkpointing and Logging and recoverable objects来保证

**一致性（consistent）**

类似于看成能量守恒。无论怎么交易，系统总量不变。

**隔离性（isolated）**

两个交易至少看起来要是按照一定顺序的。

**持久性（durable）**

Commit以后该对象的所有属性都保持不变。为了防止操作发生而实际对象没有更新的意外，log-based recovery algorithms用于回滚

### Concurrency control

就是让两个即使interleaving的process看起来也要是前后发生的。

### Recoverable objects

黄金规则：“永远不要只修改一个副本。”

When the server is running it can keep all of its objects in its volatile memory and records its committed objects in a recovery file.

Upon recovery, the server can restore the object’s **latest committed** versions.

The transaction make changes to local copies， not non-volatile copy.

### Locks and deadlock

Locking is the most popular mechanism to achieve **transaction Isolation**

目的：serializability

操作：read locks rL1(x) and write locks wL2(x).（除了两个read以外，都冲突）

**Deadlock 要求（必须同时达到）**

(1) Resources need mutual exclusion. They are not thread safe.
(2) Resources may be reserved while a process is waiting for more.
(3) Preemption is not allowed. You can't force a process to give up a resource.
(4) Circular wait is possible. X wants what Y has and Y wants what Z
has but Z wants what X has

**解决deadlock**

Prevention (disallow one of the four)
Avoidance (study what is required by all before beginning)
Detection (using time-outs or wait-for graphs) and recovery

**Waits-for graph is related to deadlock detection**

### Conflicting operations

只有read-read不是conflicting operation，其他都是

### Eventual consistency

就是可以容忍一时的inconsistent，多一些availability，只要最后consistent即可

### Serial equivalence

For two transactions to be serially equivalent, it is necessary and sufficient that all pairs of conflicting operations of the two transactions be executed in the same order at all of the objects they both access.

也就是说，如果A→B，那么必须C→D

参考quiz11 q3

### Synchronized keyword

针对在同一JVM下的使用：Java 中为了保证每次只有一个线程能对某个对象进行访问的关键字。等于是锁起了这个对象

### wait and notify

wait的目的：waits for a reply until another client has produced whatever they need.

notify()目的： waking up threads that are waiting for an access to this object's monitor.

### 2PL

Two phase locking (2PL) demands that all locks are obtained for each transaction before releasing any of them!

解开了上一个锁才能去解开下一个锁，必须干完一件事（对X操作）再干另一件事（对Y操作）

本质上是concurrency control protocol

目的：serial execution

### 2PC

本质：atomic commitment protocol – a special type of consensus protocol.

### CAP Theorem

**一致性（consistency）**

所有节点持有的所有对象的值都是最新的（一致）

**可用性（availability）**

每个节点上的对象都可以实现更新

**分区容差（partition tolerance）**

即使部分网络崩溃了，系统仍然可以正常运转。以上三点不可同时兼得，只能实现其中的任意两个，这就是 CAP 定理

# Microservice Architecture

### Basic properties

- Small: as measured in complexity, not code
- Focused: high cohesion, low coupling: separation of concerns
- Autonomous: separate process or service
- Has a well-defined API: how other servicesconnect to it
- Replication is certainly easier
- Data consistency is probably a lot harder
- Using network communication (even internal networks) instead of memory-based communication
- More flexible than a monolithic service
- Scaling of individual services should beeasier, since they’re all independent
• 0-downtime services should be easier to deliver
- replicated services won’t (or shouldn’t!) all fail at once
- Management of microservices is different（DevOps more easy）

### Comparison to other styles

**Client——server**

相同点：都可以用transient同步通讯；可以堆叠（server可以再连环扣）

不同点：microservice是一个component，可自由组合

**Pipe and Filter**

相同点：可以propagate message，很难描绘边界

不同点：microservice通常有databse；可以自由组合；与商业更好结合

**Publish-Subscribe**

相同点：microservice可以当publisher也可以当subscriber

不同点：Publish-Subscribe通常多一layer（包含data的queue）；queue一般不作为一对多使用

**Service-Oriented Architecture**

相同点：可以用JSON或者SOAP/XML;组合自由

不同点：SOA通过middleware提供更多的integration services

Microservice是以上几种的融合

### API's

**Protocol：**

RPC/RMI
REST over HTTP
TCP or UDP based: delivery guarantees over lower latency

**Asychronous：**

用queueing

**Data Style**

Protocol buffers: binary format, fast
JSON: text format, support in many languages
XML: text format, slower to parse, but more
flexible

### Instrumentation and logging

Instrumentation：直接衡量performance（以一直衡量或者turn on/off的方式）

Instrumentation是很难的

衡量Latency与throughput

每个microservice都有自己的log，可以用Chandy & Lamport来看，或者可以使用GoogleDapper或者Splunk

### Deployment

Local installation

Cloud

Virtual Machines

Container

### Persistence and consistency

Persistence：管理自己的databse的意思，大一点的app用nosql

Consistency：保证结果的一致性，金融app需要high-latency algorithm, e.g.
read-write quorums or centralized writes。其他则eventual consistency即可（noSql这点也不行）

### Use with Dev/Ops

Ops *must* be involved early and often

• Devs *think* they know what productionenvironment looks like
• Ops *think* they know what build packageslook like
• Software architects *better* know both parts

**issues**

Continuous delivery and continuous deployment
• Infrastructure as Code
• Handling production incidents
• Scrum (or other agile method)