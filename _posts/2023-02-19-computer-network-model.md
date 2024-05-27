---
title: Computer Network Model
date: 2023-07-26
---



# 网络通信模型

## OSI模型

OSI模型（Open System Interconnection Model，开放式系统互联模型）是网络通信的概念模型，由ISO（International Organization for Standardization，国际标准化组织）提出。

### 层次划分

根据OSI/RM（Open Systems Interconnection Reference Model），将模型分为七层。

从上到下依次为：

- **应用层（Application Layer）**提供为应用软件而设计的接口，以设置与另一应用软件之间的通信。
- **表示层（Presentation Layer）**把数据转换为能与接收者的系统格式兼容并适合传输的格式。 
- **会话层（Session Layer）**负责在数据传输中设置和维护计算机网络中两台计算机之间的通信连接。
- **传输层（Transport Layer）**把传输表头（TH）加至资料以形成分组。传输表头包含了所使用的协议等发送信息。
- **网络层（Network Layer）**决定数据的路径选择和转寄，将网络表头（NH）加至数据包，以形成分组。网络表头包含了网络资料。
- **数据链路层（Data Link Layer）**负责网络寻址、错误侦测和改错。
- **物理层（Physical Layer）**在局域网上传送数据帧（Data Frame），它负责管理电脑通信设备和网络媒体之间的互通。

## 互联网协议套件

OSI模型只是概念模型，**互联网协议套件**才是真正被用于网络通信的模型。

互联网协议套件（Internet Protocol Suite）又称TCP/IP协议簇（TCP/IP Protocol Suite）。

### 层次划分

OSI模型的最上面三层（应用层、表示层和会话层）在TCP/IP组中合并成应用层；最下面两层（数据链路层、物理层）合并成网络访问（链接）层。

从上到下依次为：

| Layer                       | Descript | Protocal |
| --------------------------- | -------- | -------- |
| 应用层（application layer） |          |          |
|                             |          |          |
|                             |          |          |

- 应用层：HTTP
- 网络层：网际层是在TCP/IP标准中正式定义的第一层。所执行的主要功能是处理来自传输层的分组，将分组形成的数据包（IP数据包），并为该数据包（IP数据包），并为该数据包进行路径选择，最终将数据包从源主机发送到目的主机，最常用的协议是网际协议IP。  
- 传输层：TCP/IP的传输层也被称为主机至主机层。
- 应用层：在TCP/IP模型中，应用程序接口是最高层，用于提供网络服务，比如文件传输、远程登录、域名服务和简单网络管理等。  

## 总结

TCP/IP模型被广泛使用，而OSI模型主要用作学术和标准化参考。