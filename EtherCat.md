# EtherCAT
## 1. EtherCAT原理介绍

EtherCAT从站设备在报文经过其节点时读取相应的数据报文，同样输入数据也是在报文经过时插入到报文中。整个过程报文只有几纳秒的时间延迟，实时性获得极大提高

EtherCAT作为一种工业以太网总线，充分利用了以太网的全双工特性。使用主从通信模式，主站发送报文给从站，从站从中读取数据或将数据插入至从站。

 · 主站可使用标准网卡实现，

 · 从站选用特定的EtherCAT从站控制器ESC(EtherCAT Slave Controller)或者FPGA实现，

 主要完成通信和控制应用两部分功能，EtherCAT物理层选用标准以太网物理层器件。

从站能将收到的报文直接处理，并读取或插入有关的数据，再将报文发送给下一个EtherCAT从站。最末尾的EtherCAT从站返回处理完全的报文，然后由第一个从站发送给主站。整个通信过程充运行于全双工模式下，TX线发出的报文又通过RX线返回给主站:

![image.png](https://img-blog.csdnimg.cn/20201031083931190.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B3bDk5OQ==,size_16,color_FFFFFF,t_70#pic_center)


## 1.1 实时性  

· **数据包刷新时间的计算**

数据包中所有从站的 Process Datarocess Datarocess Data rocess Data rocess Data rocess Datarocess Data数据 决定了数据包的长度。

一个Ethernet thernet数据包最小84 字节，不足 84 字节会补齐84 字节。由于EtherCAT Frame中有一些公共开销， 84 字节的数据包最多含18字节的过程数据。*考虑到数据包必须经过每个从站两次才能回到主站*，所数据包以固定的波特率100 Mbps在网络上传输两次的时间 这就是它的总线刷新时间 。

1. 基于这个原则，以包含 1000路开关量信号的数据包为例，计算过程如下：

    过程数据长度：1000/8=125Bytes
    数据包长度：84-18+125=191Bytes=191*8 Bit= 1528 Bit
    
    总线刷新时间：(1528Bit/100,000,000 Bps)*2=15.28us * 2 = 30.56us

注意，通常的数字量模块， 都是单纯的输出或者输入模块，而不是混合模块。所以 1000 个数字 量信号， Frame 中就会分配 125 字节。

2. 再以包含100个EtherCAT伺服驱动器过程数据的EtherCAT数据包为例，假如每个伺服的过程数据只包括控制字（2字节）、状态字（2字节）、目标位置（4字节）、实际位置（4字节），其总线刷新时间的计算过程如下：

    过程数据长度：100*（2+4）=600 Byte。 

    数据包长度：84-18+600=1266 Byte =671*8 Bit =5328 Bit 

    总线刷新时间：(5328 Bit/100,000,000 Bps) *2=100.656µs 

注意，Frame中只为一个伺服分配了6个字节，这是因为根据Beckhoff公司的控制软件TwinCAT中关于EtherCAT的默认设置是从站的Input和Output使用同一数据段，所以数据包进入伺服驱动器时该数据段存放的是控制字和目标位置，而出来时则存放伺服的状态字和实际位置。

以上两个数据30.56µs和101.28 µs就是EtherCAT官方宣传资料中，刷新1000个数字量需要30µs，刷新100个伺服轴只需要100µs的数据由来。实际上，根据从站的类型、是否包含分布时钟、是否启用时钟同步、时钟同步的参数设置不同，在数据包中有可能还会增加8-12字节用于传输同步时钟值，以及相应的为每个从站增加一个Bit的标记等等，会增加几个微秒的刷新时间，暂且忽略不计。

以上计算只是数据包传输需要的理论时间，实际上，数据包经过每个从站会产生短暂的硬件延时。100M超五类网线接口的从站延时约1µs，而EBus的IO模块类从站延时约0.3µs，在毫秒级以下的控制任务中如果从站数量较多，这个时间也相当可观，计算刷新周期时应该考虑进去。


## 1.2  端口管理

一个从站控制器最多可以有4个端口，如果一个端口关闭了，控制器主动连接下一个端口。端口可以随着EtherCAT命令主动的打开或者关闭。逻辑端口设置决定了EtherCAT帧的处理和发送顺序。

![image.png](https://img-blog.csdnimg.cn/20201031083955465.PNG#pic_center)

![image.png](https://img-blog.csdnimg.cn/20201031084015603.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B3bDk5OQ==,size_16,color_FFFFFF,t_70#pic_center)

![image.png](https://img-blog.csdnimg.cn/20201031084027801.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B3bDk5OQ==,size_16,color_FFFFFF,t_70#pic_center)

![image.png](https://img-blog.csdnimg.cn/202010310840393.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B3bDk5OQ==,size_16,color_FFFFFF,t_70#pic_center)







