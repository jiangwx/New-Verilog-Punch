## 题目

>1. 画一下电路图：CMOS反相器、与非门、或非门、三态输出门、漏极开路门。
>2. 解释一下Vih，Vil，Vol，Voh，Vt，Iddq
>3. CMOS反相器的速度与哪些因素有关？什么是转换时间（transition time）和传播延迟（propagation delay）？
>4. CMOS反相器的功耗主要包括哪几部分？分别与哪些因素相关？
>5. 什么是latch-up(闩锁效应)？
>6. 相同面积的cmos与非门和或非门哪个更快？

<!-- more -->

### 第一题

#### cmos集成电路的基本单元（反相器、与非门、或非门）

NMOS——电子导电，高电平通，低电平断，传递低电平$\to$连接地。

PMOS——空穴导电，低电平通，高电平断，传递高电平$\to$连接电源。

每个输入连接1P、1N；NMOS串联表示“与”；NMOS并联表示“或”。

<center>
    <img src="https://wx2.sinaimg.cn/large/7f79daaely1g2dhckwys2j219r0n90wk.jpg" alt="cmos集成电路的基本单元（反相器、与非门、或非门）" title="cmos集成电路的基本单元（反相器、与非门、或非门）" width="600">
</center>


#### 三态输出门

计算机系统的各部件模块（Module）及芯片（Chip）通常挂接在系统总线（System bus）上，在某一时刻只能有一个发送端。为了使各模块芯片能够分时传送信号，需要具有三态输出的门电路，简称三态门（Three-state gate），又称为三态缓冲器（Three-state buffer）三态门的输出端状态不仅有高电平和低电平，而且具有第三种状态--高阻状态（High-Impedance，Hi-Z）.

CMOS三态门是在普通门的基础上增加了控制端和控制电路。CMOS三态门的电路结构有很多种，大体可以分为以下3类：

1. 控制上下P/N沟道的截止或导通，实现三态输出功能。如下图（a）、(b)。

2. 在输出端串联传输门开关（TG）的三态线路。如下图（c）。

3. 利用或非门或者与非门实现三态功能。如下图（d）（e）。

<center>
    <img src="https://wx3.sinaimg.cn/large/7f79daaely1g2cwtjjdptj213w0tztaf.jpg" alt="三态门" title="三态门" width="600">
</center>


#### 漏极开路门

CMOS漏极开路门（Open-Drain Gate），又称漏极开路输出（open-drain output），简称OD门。

各种CMOS门电路都可构成漏极开路门，下图为CMOS与非OD门的电路结构。F=(AB)'

<center>
    <img src="https://wx2.sinaimg.cn/large/7f79daaely1g2cwtnubonj20a304cmxh.jpg" alt="OD门" title="OD门" >
</center>

漏极开路OD门电路内的输出MOS管漏极是开路的，应用时，需要通过外接电阻（Extension resistance）接电源。与TTL集成的OC门类似，CMOS集成的OD门也可实现线与（Wired and）连接。

<center>
    <img src="https://wx1.sinaimg.cn/large/7f79daaely1g2cwtpi31pj20kl0es117.jpg" alt="OD门实现线与逻辑" title="OD门实现线与逻辑" width="600">
</center>

可以看出，OD门就是将反相器的上面的PMOS管拿掉了而已。

当两个与非门的输出全为1时，输出为1；只要其中一个输出为0，则输出为0，所以该电路符合与逻辑功能，即L=(AB)'(CD)'。

### 第二题

部分复习

ViH:输入高电平，保证逻辑门的输入为高电平时所允许的最小输入电平，当输入电平高于Vih时，则认为输入电平为高电平。

Vil:输入低电平，保证逻辑门的输入为低电平时所允许的最大输入电平，当输入电平低于Vil时，则认为输入电平为低电平。

Voh:输出高电平，保证逻辑门的输出为高电平时的输出电平的最小值，逻辑门的输出为高电平时的电平值都必须大于Voh。

Vol:输出低电平，保证逻辑门的输出为低电平时的输出电平的最大值，逻辑门的输出为高电平时的电平值都必须小于Vol。

Vt:阀值电平，数字电路芯片都存在一个阈值电平，就是电路能勉强翻转动作时的电平。它是一个界于ViL、ViH之间的电压值，CMOS电路的阈值电平，是二分之一的电源电压值。如果要保证输出稳定，则必须要求输入的高电平>ViH，输入的低电平<ViL；如果输入电平在阈值上下，也就是ViL-ViH这个区域，电路的输出会处于不稳定状态（即亚稳态）。

对于一般的逻辑电平，以上参数的关系：VoH>ViH>Vt>ViL>VoL

IDDQ:IDDQ是指当CMOS集成电路中的所有管子都处于静止状态时的电源总电流。[Iddq testing](https://en.wikipedia.org/wiki/Iddq_testing)

### 第三题

复习

影响因素：（这个小问只是简单看了一下把答案总结了一下，部分原理没搞懂）
1. 电容——门本身的扩散电容，互连线电容（通过版图优化）和扇出电容（尽量减少漏区面积）。
2. 增加晶体管的W/L：增加晶体管的尺寸也增加扩散电容。这个有限值，到一定程度，再增加门尺寸就不能对减少延时有帮助了。
3. 提高$V_{DD}$:以能量损耗来换取性能，但电压超过一定程度后改善就会非常有限。

传播延迟（propagation delay）：由于PN结上储存电荷的积累和消散都需要时间，因此MOS管由导通到截至或由截止到导通也需要时间。电路中寄生电容和负载电容的影响，也使得输出波形总是滞后于输入波形，这个延迟时间成为传播延迟（propagation delay），或者传输延迟时间。它反映了门电路的开关速度和信号传递时间。
通常，输出信号由低电平变为高电平时，输出相对输入的延迟时间记为$t_{\mathrm { PLH }}$：而输出信号由高电平变为低电平时，输出相对输入的延迟时间记为$t _ { \mathrm { PHL } }$。

<center>
    <img src="https://wx2.sinaimg.cn/large/7f79daaely1g2cwtvdiefj20bv04w3z2.jpg" alt="传播延迟" title="传播延迟">
</center>

转换时间（transition time）：用来描述逻辑电路的输出从一种状态变为另一种状态所需的时间。其中输出从低态到高态的转换时间称为上升时间（$\mathrm{t}_{\mathrm{r}}$,rise time）；从高态到低态的转换时间称为下降时间（$\mathrm{t}_{\mathrm{f}}$,fall time）。

<center>
    <img src="https://wx2.sinaimg.cn/large/7f79daaely1g2cwtwqawwj20bt04it97.jpg" alt="上升时间和下降时间" title="上升时间和下降时间">
</center>

### 第四题

将相关答案总结罗列至此，[参考链接](https://www.cnblogs.com/IClearner/p/6893645.html)作备忘和进一步学习。

CMOS反相器的功耗主要由三部分组成：
1. 开关功耗（Switch power）:对输出电容负载供电$\to$与供电电压、负载电容大小、时钟频率有关。
2. 内部功耗（Internal power）：短路功率$\to$与供电电压、负载电容大小、时钟频率有关。
3. 漏电功耗（Leakage power）：稳定态（静态）功率$\to$往往和工艺有关。

其中开关功耗和内部功耗为动态功耗，漏电功耗为静态功耗。

### 第五题

闩锁效应是CMOS工艺所特有的寄生效应，严重会导致电路的失效，甚至烧毁芯片。闩锁效应是由NMOS的有源区、P衬底、N阱、PMOS的有源区构成的n-p-n-p结构产生的，当其中一个三极管正偏时，就会构成正反馈形成闩锁。来源：[百度百科](https://baike.baidu.com/item/%E9%97%A9%E9%94%81%E6%95%88%E5%BA%94/9391913)

另提供一份文档：[latch-up.pdf](https://e2echina.ti.com/cfs-file/__key/telligent-evolution-components-attachments/00-60-01-00-00-12-36-05/latch-up.pdf)

### 第六题

PMOS采用空穴导电，NMOS采用电子导电，由于PMOS的载流子的迁移率比NMOS的迁移率小，所以，同样尺寸条件下，PMOS的充电时间要大于NMOS的充电时间长，在互补CMOS电路中，与非门是PMOS管并联，NMOS管串联，而或非门正好相反，所以，同样尺寸条件下，与非门的速度快，所以，在互补CMOS电路中，优先选择与非门。