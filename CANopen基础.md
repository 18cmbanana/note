# CANopen
## 基本原理

![image.png](https://s2.loli.net/2022/08/06/zF7ZPYlobfDwWh3.png)

**数据链路层** ：包括介质访问控制（MAC）层和逻辑链路控制（LLC）层，关键是形成数据帧。

**物理层**：有时也称物理接口，是实现物理连接的功能描述和执行链接的规程，提供用于建立、保持和断开物理连接的机械、电气、功能和规程的条件，物理层有四个重要特征：

·机械特征----连接器、引脚数、引脚排列

·电气特征----信号电平高低、阻抗和阻抗匹配、传输速度和距离

·功能特征----信号线（数据、控制、地线）的功能分配和确切定义

·规程特征----各信号的工作规则和时序


![image.png](https://s2.loli.net/2022/08/06/A835p9nHrTqMXKd.png)

## 什么是CANopen协议

![image.png](https://s2.loli.net/2022/08/06/HGJrhv7xutYewsR.png)

![image.png](https://s2.loli.net/2022/08/06/aGlKSVwpLcF1Hxi.png)


**对象字典（object Dcitionary）**

![image.png](https://s2.loli.net/2022/08/06/1pzDNL9seJdgmxU.png)

**通用标识符（COB_ID）**

![image.png](https://s2.loli.net/2022/08/06/p3dPJhIMlCWOkvE.png)

**常用术语**

![image.png](https://s2.loli.net/2022/08/06/JPipKe1Ttg5BE7r.png)

## 管理报文


**状态机**

![image.png](https://s2.loli.net/2022/08/06/6Hycar21ipOPETL.png)

![image.png](https://s2.loli.net/2022/08/06/gBQxL31KTAW69z7.png)

·如果COB_ID大于零，则表示是发给固定从站的

![image.png](https://s2.loli.net/2022/08/06/EOhbipnFvR2laMT.png)

![image.png](https://s2.loli.net/2022/08/06/opjUlbGHeW8Pcq7.png)

![image.png](https://s2.loli.net/2022/08/06/wXsCRbo3jOc9QGn.png)

![image.png](https://s2.loli.net/2022/08/06/Yxk13DQZEsMGtrH.png)

![image.png](https://s2.loli.net/2022/08/06/23pwQuaA4N1Dich.png)

