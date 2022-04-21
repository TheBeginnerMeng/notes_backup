# 计算机是怎样跑起来的

# 第9章 通过七个简单实验理解TCP/IP网络

## 本章大纲

![](D:\software\Typora\images\IP网络.png)

## 热身问题

- LAN：Local Area Network-局域网（小规模网络），部署在一栋建筑或者一间办公室内的小规模网络
WAN：Wide Area Network-广域网（大规模网络），像互联网式把企业和企业联结起来的大规模网络
- TCP/IP（Transmission Control Protocol / Internet Protocol）：传输控制协议和网际协议，协议：对信息发送方式的规定或约束
- MAC（Media Access Control）：能够标识网卡的编号

## 实验环境

集线器（Hub）：负责把各台计算机的网线连接在一起的集线设备

路由器（Router）：负责把公司内的网络和互联网连接起来的设备，LAN –> WAN

![LAN示意图](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412110439709.png)

## 实验

### 9.1 查看网卡的MAC地址

需要的设备：网卡NIC（Network Interface Card）目前一般用以太网（Ethernet）网卡，网线，集线器，路由器

以太网中的计算机都需要确认一件事：在网线上有没有其他的计算机正在传输电信号，也就是说要先确保没有人在占用网络， 然后才能发送自己想传输的电信号。谁先抢到了网线的使用权，谁就先发送。万一都想发送，只需让这些计算机等待一段长度随机的时间后再重新发送相同的电信号即可。这套机制就是 CSMA/CD-Career Sense Multiple Access with Collision Detection，带冲突检测的载波监听多路访问。

**载波监听**（Career Sense）：指的是这套机制会去监听表示网络是否正在使用的电信号（Career）
**多路复用**（Multiple Access）：多个（Multiple）设备可以同时访问（Access）传输介质
**带冲突检测**（with Collision Detection）：表示这套机制会去检测（Detection）因同一时刻的传输而导致的电信号冲突（Collision）

  ![image-20220412111755709](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412111755709.png)

  使用MAC地址的编号来指定电信号的接收者。在每一块网卡所带有的 ROM（Read Only Memory，只读存储器）中，都预先烧录了一 

  个唯一的 MAC 地址。MAC 地址是由制造厂商的编号和产品编号两部分组成的，所以世界上的每一个 MAC 地址都是独一无二的

  实验结果：

  ![image-20220412112352485](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412112352485.png)

### 9.2 查看计算机的IP地址

在TCP/IP网络中，除了硬件上的MAC地址，还为每台计算机指定了一个软件意义上的编号，即IP地址。

设定了IP地址的计算机称为主机（Host），在TCP/IP中，传输的数据携带有MAC地址和IP地址。

IP地址通常是一个32位（bit）的整数，每8个比特位为一组。 8比特位所表示的整数换算成十进制后范围是 0～255，因此可用作 IP 地址的整数是 0.0.0.0～255.255.255.255。通常把IP地址中表示分组（LAN）的部分称作”网络地址”，表示各台计算机（即主机）的部分称作“主机地址”。

比如：比如用IP地址中第1段到第3段的数值代表公司，用第4段的数值代表公司内部的计算机。

子网掩码的作用是标识出在IP地址中，从哪一位到哪一位是网络地址，哪些是主机地址。如255.255.255.0用二进制表示为`11111111.11111111.11111111.00000000`，子网掩码中，值为 1 的那些位对应着 IP 地址中的网络地址，后面值为 0 的那些位则对应着主机地址。因此子网掩码`255.255.255.0`表示，其所对应的IP地址的前24位为网络地址，后8位为主机地址。

实验结果：

  ![image-20220412144942929](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412144942929.png)

### 9.3 了解DHCP服务器的作用

在计算机中可以手动设置 IP 地址和子网掩码，但是大多数情况下选择的还是“自动获得 IP 地址”这个选项。这个选项使得计算机在启动时会去从 DHCP 服务器获取 IP 地址和子网掩码，并自动地配置它们。

DHCP（Dynamic Host Configuration Protocol）动态主机设置协议，DHCP服务器上记录着可以被分配到LAN内计算机的IP地址范围和子网掩码的值。作为 DHCP 客户端的计算机在启动时，就可以从中知道哪些 IP 地址还没有分配给其他计算机。

![image-20220412150057046](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412150057046.png)

如上图所示，默认网关地址通常填入路由器的IP地址，也就是说路由器是从LAN中通往互联网世界的入口（Gateway），路由器的IP地址也可以用DHCP服务器中获取。如果在上图中选择自动获取DNS服务器地址，也就是继续从DHCP中获取DNS服务器的地址。

### 9.4 路由器是数据传输过程中的指路人

在分组管理下，IP地址中的网络地址部分可以代表一个组中的全部计算机，即一个LAN中的计算机全体。互联网就是用路由器把多个LAN连接起来所形成的一张大网。

**路由器**：决定数据传输路径的设备。与 LAN 内的其他计算机一样，路由器也是连接在集线器上的。因为 LAN 内采用了 CSMA/CD 机制，所以所有发送出去的数据也都会发到路由器上。

当从公司内的计算机向另一家公司的计算机发送数据时会发生什么呢？路由器的工作原理：查看附加到数据上的IP地址的网络地址部分，如果不是发送给LAN内的计算机，就把它发送到LAN外，即互联网世界中。

分布在世界各地的 LAN 中的路由器相互交换着信息，互联网正是由于这种信息的交换才得以联通。这种信息被称作“路由表”，用来记录应该把数据转发到哪里。在一台路由器的路由表中，只会记录通往与之相邻的路由器的路径，而并不会记录世界范围内的所有传输路径。

实验结果：

![image-20220412151318173](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412151318173.png)

### 9.5 查看路由器的路由过程

路由：比如访问google.com，Google服务器上的数据，需要经过若干个路由器的层层转发才能到我们的计算机上。这个数据经过路由器转发的过程，就叫路由（Routing）。

  - windos: cmd中执行tracert www.xxx.com

实验结果：可以看到，从本机所处的LAN网络出发，经过了7次转发，才到达百度（baidu.com）

![image-20220412152408486](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412152408486.png)

### 9.6 DNS服务器可以把主机名解析为IP地址

互联网中还存在DNS（Domain Name System）：域名系统服务器，正是该服务器为我们把www.baidu.com这样的域名解析为了14.215.177.38这样的 IP 地址。

每个计算机都有一个主机名，每个LAN都有一个域名，如本机的主机名是DESKTOP-J2I5BBM，所在的LAN的域名是xxx.xx。把主机名和域名组合起来所形成的 DESKTOP-J2I5BBM.xxx.xx，就是能够标识笔者这台计算机的一个世界范围内独一无二的名字，这个名字与 IP 地址的作用是等价的。通常把这种由主机名和域名组合起来形成的名字称作 FQDN（Fully Qualified Domain Name，完整限定域名）。 

由于IP地址难以记住，人们发明了DNS服务器，这样只需要提供FQDN，就能通过DNS服务器，把FQDN解析为IP地址（域名解析）。

DNS服务器通常被部署在各个LAN中，里面记录着各个FQDN和IP地址的对应关系，世界范围内的 DNS 服务器是通过相互合作运转起来的。如果一台 DNS 服务器无法解析域名，它就会去询问其他的 DNS 服务器。

实验过程，输入`nslookup`，即可操作DNS服务器，屏幕上就会显示出一个提示符“>”，表示现在可以询问 DNS 服务器了。而提示符上面的 public-dns-a.dnspai.com 和 101.198.198.198，则是笔者主机所处 LAN 内的 DNS 服务器的 FQDN 和 IP 地址。

![image-20220412153707499](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412153707499.png)

试着输入www.baidu.com，然后按下 Enter 键。结果输出了14.215.177.38和14.215.177.39，这正是百度公司 的 Web 服务器的 IP 地址。www.baidu.com 和14.215.177.38/14.215.177.39的对应关系，是通过询问其他互联网上的 DNS 服务器才得知的，并没有被事先录入到笔者主机所处的 LAN 中的 DNS 服务器上。

![image-20220412153928492](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412153928492.png)

### 9.7 查看 IP 地址和 MAC 地址的对应关系

在互联网的世界中，到处传输的都是附带了 IP 地址的数据。但是能够标识作为数据最终接收者的网卡的，还是 MAC 地址。于是在计算机中就加入了一种程序，用于实现由 IP 地址到 MAC 地址的转换，这种功能被称作 ARP（Address Resolution Protocol，地址解析协议）。

ARP协议：Address Resolution Protocol，地址解析协议，实现IP地址到MAC地址的转换。ARP 的工作方式很有意思。它会对 LAN 中的所有计算机提问： “有谁的 IP 地址是 210.160.205.80 吗？有的话请把你的 MAC 地址告诉我。”通常把这种同时向所有 LAN 内的计算机发送数据的过程称作“广播“（Broadcast）。

ARP缓存：如果为了查询 MAC 地址，每回都要进行广播询问，那么查询的效率就会降低。于是 ARP 还提供了缓存的功能，当向各个计算机都询问 完一轮之后，就会把得到的 MAC 地址和 IP 地址的对应关系缓存起来 （临时保存在内存中）。存起来的这些对应关系信息称作“ARP 缓存表”。

实验：查看ARP缓存，arp -a 查看当前ARP缓存表的内容

![image-20220412161243082](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412161243082.png)



## TCP的作用及TCP/IP网络的层级模型

IP 协议用于指定数据发送目的地的 IP 地址以及通过路由器转发数据。

TCP 协议则用于通过数据发送者和接收者相互回应对方发来的确认信号，可靠地传输数据。通常把像这样的数据传送方式称作“握手”（Handshake）。TCP 协议中还规定，发送者要先把原始的大数据分割成以“包”（Packet）为单位的数据单元，然后再发送， 而接收者要把收到的包拼装在一起还原出原始数据。

硬件上发送数据的是网卡。在网卡之上是设备驱动程序（用于控制网卡这类硬件的程序），设备驱动程序之上是实现了 IP 协议的程序，IP 程序之上则是实现了 TCP 协议的程序，而再往上才是应用程序，比如 Web 或电子邮件。这样就构成了一幅在硬件之上堆叠了若干个软件层 的示意图。TCP 协议使用被称作“TCP 端口号”的数 字识别上层的应用程序。TCP 端口号中有一些是预先定义好的，比如 Web 使用 80 端口，电子邮件使用 25 端口（用于发送）和 110 端口（用于接收）

![image-20220412162539242](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220412162539242.png)

