基于别人的Demo修改，PCB地址：https://pro.lceda.cn/editor#id=f1b557426a1842b895f9043f481c53c4。


参考网上的例子，自己也做了个JLINK OB，为什么想做呢，手上有JLINK V8，个头太大，用到串口的时候还要另外再接一块USB-TTL转接板，甚是麻烦，偶然了解JLINK OB，外围电路简单，SWD接口，只需三根线就能实现调试，后来又接着了解到了F072主控的JLINK OB，步进省去了外部晶振，还支持虚拟串口和SWO，这不是神器是什么！！接着就开始做了，参考网上的案例，第一版画错了一根线，第二版修正了错误，添加了SWO。

#### 关于固件提取

在JLinkARM.dll里面寻找描述信息关键字，然后向前查找0020，××××0020就是固件开头，向下截取到下一个0020，然后在前面填充固定大小的00或者FF，补上开头的八个字节即可。STM32F072主控的要在前边填充开头8字节+0x4800-8个00或者FF。

#### 关于固件下载

三种下载方式

1.板子底部预留了SWD接口，可以直接通过JLINK下载固件。

2.通过USB DFU下载固件，首先将BOOT0拉高上电，然后通过DFU更新固件即可。

3.将BOOT0拉高上电,通过串口更新固件。

#### 关于JLINK OB的LICENSE和SN

DLL文件里面的固件是没有SN的，刷好固件之后可以通过jlink commander进行添加，exec setsn=########

设置好SN之后要在license manager里面添加激活码，这个激活码可以通过网上的一个小程序进行生成，

2.这个license也可以在固件提取出来之后直接添加到固件里面，这样即使没有SN，也没添加license到manager也不影响使用。

生成license的小程序里面带一个病毒，通过winhex分析发现最后一段55k大小的程序段会被释放出来，删除后成为干净的激活码生成器。

总之是两种方式

1.直接烧录提取出来的原固件，但是烧录好之后需要自己设置SN并添加激活码

2.修改提取出来的原固件，添加激活信息，直接烧录使用

后者的优点是更换电脑也可以使用

**在固件里添加内建license的方法**，在0x3f20-0x3fb0地址空间添加明文以0x00结束作为分割点

JFlash

JFlashBP

JFlashDL

RDDI

GDB

RDI

![image](https://github.com/geekchun/Jlink-OB/blob/master/README.assets/%E6%88%AA%E5%9B%BE00.png)

