# 理论基础
1. 永磁同步电机MTPA的控制原理

    1.1 MTPA控制方式与id=0控制方式的区别

    当电机采用id=0的控制策略，但是这种控制方法忽略和磁阻转矩的作用

    ![image.png](https://s2.loli.net/2022/03/01/XBbRIusZzlcg48a.png)

    这个从转矩方程最容易看出来，转矩分为永磁转矩Tr和磁阻转矩Tm，而id=0只剩下Tr。这会导致电流的利用率不高，系统的效率降低。所以id=0的控制比较适用于隐极式电机（Ld=Lq），而对于凸极式电机并不最优，所以需要重新考虑控制策略。

    1.2 推导过程

    为了找到电流和转矩的最佳匹配，使电机能最小的电流产生最大的转矩，这就是数学上的事情了。

    列下以下公式，转矩电流之间的关系式

    ![image.png](https://s2.loli.net/2022/03/01/ulQTpRrqwUXfYv5.png)

    为了找到极值关系，利用数学中的拉格朗日定理，引入辅助函数。

    ![image.png](https://s2.loli.net/2022/03/01/EpJPFN4ro8hSvXL.png)

    接着开始拉格朗日求极值的过程，

    ![image.png](https://s2.loli.net/2022/03/01/LjmYekyQcvw6TuC.png)

    对上式进行求解，得到了直轴电流id和交轴电流iq的关系，

    ![image.png](https://s2.loli.net/2022/03/01/ecBMN7OrduLz6fP.png)

    其实到了这里我们还是一个蒙蔽状态，因为这个公式没法用啊，我们矢量控制的转速环输出的是转矩给定Te，而且这个公式里面用iq来求id，我本来就不知道iq我还咋求id啊，仿真没法搭，好多的论文里面就到了这里就没有了，最后在一篇弱磁的论文里面才找到了最后可用的计算公式。

    ![image.png](https://s2.loli.net/2022/03/01/zwuoLaBUTZpF2kO.png)

    以上公式就是用转速环的输出来计算，来算出给定转矩所对应最低的id和iq，反映到电机上就是定子电流。

  1.3 控制框图以及仿真搭建

    从转速环输出部分框图

![image.png](https://img-blog.csdn.net/20181002114810199?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

整体控制框图：

![image.png](https://img-blog.csdn.net/2018100211485650?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2. 仿真结果分析

 2.1 电机参数

![image.png](https://img-blog.csdn.net/20181002122300973?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2.2 id=0 的转矩和定子电流结果

   id=0 带140N负载，转矩与定子电流波形。

![image.png](https://img-blog.csdn.net/20181002122016395?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

放大结果

![image.png](https://img-blog.csdn.net/20181002122138837?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图可以看出，id=0控制方式下，在140N负载时，定子电流在26.8A。

2.3 MTPA 的转矩和定子电流结果

    MTPA 带140N负载，转矩与定子电流波形。

![image.png](https://img-blog.csdn.net/20181002132317158?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![image.png](https://img-blog.csdn.net/20181002132331494?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图可以看出，最大转矩电流比控制方式下，在 140N 负载时，定子电流在24.5A。交轴电流为 iq = 22.94 A，直轴电流为 id = -8.75 A。

3. 小结

从控制方式角度将，最大转矩电流比是凸极电机在矢量控制上的一种优化，提高逆变器电压的利用率，减少损耗，提高电机的效率。
从数学上来讲，最大转矩电流比根据电流和转矩方程求最值。

![image.png](https://img-blog.csdn.net/20181002134029661?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N5MjQzNzcyOTAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

反映在图上就是在最大转矩曲线上，我们的 id 和 iq 就是在这条曲线上取得。
从应用角度讲，最大转矩电流比充分利用了凸极电机的磁阻转矩，降低损耗提高效率，降低了成本，而且在更为永磁同步电机弱磁控制提供了更好的基础。
存在的部分问题，
1、电机的参数在电机运行时会随电机的温度以及转速产生波动，这将影响MTPA的精确程度。
2、在电机交直轴电感差值（Ld-Lq）较小时，磁阻转矩较小，MTPA的改善效果不明显。
3、因为计算公式复杂，会给控制模块带来很大的负担，所以一般在实际工程应用中，会用到查表法，即先将对应转矩的 iq 和 id 计算出来。另外拟合法也是非常不错的方法。
