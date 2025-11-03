---
title: Emoe Pulse Lite V2 用户手册
keywords: analog
desc: Emoe Pulse Lite V2 用户手册
author: CNPP
date: false
tags: analog, emoepulse
---
# Emoe Pulse Lite V2 用户手册

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_1.jpg" alt="EmoePulse_1" width=80%/>

</center>

> 硬件版本： 2.4
> 手册版本： 1.2


## 购买

如果需要购买，请扫码加入Emoe R&D交流群获取货品备货信息：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/Common%20Resource/erd_qrcode.jpg" alt="baidu" width=50% border="0"/>

</center>

也可直接访问我们的淘宝店铺，有库存即可下单：https://shop404388371.taobao.com/?spm=pc_detail.29232929/evo365560b447259.shop_block.dshopinfo.103e7dd6y8VG55

## 简介

Emoe Pulse（下简称Pulse）是一款超低功耗的便携高速窄脉冲发生器，用于一定范围内的带宽测量及线缆质量检测。Pulse 的电路衍生自Jim Williams于[LT AN47](https://www.analog.com/media/en/technical-documentation/application-notes/an47fa.pdf)等参考手册中发布的雪崩脉冲发生电路，如下图：

![1-1](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-1.png)

Pulse对Jim的设计进行了低功耗方向的优化，采用了雪崩击穿电压更低的射频三极管，并设计了超低功耗的DC-DC系统，使得整机可以在一片CR2032的供电下稳定工作较长时间。同时，Pulse亦可选择使用Type-C接口供电，两种供电方式通过开关选择，在通过Type-C接口供电时不需取下电池。

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_2.jpg" alt="EmoePulse_2" width=70%/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_3.jpg" alt="EmoePulse_3" width=70%/>

</center>

## 货品清单

- EmoePulse主板
- CR2032电池(安装在主板上)

## 主要指标

项目|参数
---|---
Type-C接口供电电压范围|4.5 - 5.5V
CR2032电池座供电电压范围|2.5 - 3.1V
上升时间|150ps(典型值)
脉冲峰值@50Ohm|2Vpk
脉冲峰值@1MOhm|1.2Vpk
脉冲重复频率|约200kHz
工作电流@3.0V|750uA

经Keysight DSAV334A 33GHz 80Gsa/s 测试的脉冲参数：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_Keysight_1.png" alt="EmoePulse_k1" width=70%/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_Keysight_2.png" alt="EmoePulse_k2" width=70%/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_Keysight.jpg" alt="EmoePulse_keysight" width=70%/>

</center>

## 使用方法

首先应该将包装中的CR2032电池装入Pulse背面的电池座（出厂已安装）

或接入5V供电的Type-C线缆。开关拨至`BAT`处，Pulse由电池供电；拨至对侧则为Type-C接口供电。

Pulse的主要功能为单一窄脉冲源或线缆测试仪。脉冲输出端口与待测端口连接时，应根据需要加入50Ohm匹配器，下图展示了使用Pulse测试示波器的模拟输入带宽（端接50欧匹配）：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_match.jpg" alt="EmoePulse_test_match" width=80%/>

</center>

<center>

在以较大水平时间观察时，示波器应显示如下的窄脉冲串：

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_coarse.jpg" alt="EmoePulse_Test_coarse" width=80%/>

</center>

<center>

在设置好触发电平后将水平时间减小至5ns/div左右，可观察窄脉冲及上升时间（未进行端接匹配）：

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_fine.jpg" alt="EmoePulse_Test_fine" width=80%/>

</center>


当将Pulse作为线缆测试仪使用时，应使用BNC三通连接器紧贴示波器安装，另一端口接待测线缆。下图展示了测试开路传输线的效果：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_coax_open.jpg" alt="EmoePulse_Test_open" width=80%/>

</center>

在线缆终端端接50欧匹配负载后，线缆反射的波包大部分消失（完美匹配时，不应存在反射，此处转接器中存在轻微阻抗不匹配）：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_coax_match.jpg" alt="EmoePulse_Test_match" width=80%/>

</center>

在线缆终端短路后，线缆反射的波包呈现另一种形态：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoePulse/EmoePulse_Test_coax_short.jpg" alt="EmoePulse_Test_short" width=80%/>

</center>


## 参考链接

+ Application Note 47, High Speed Amplifier Techniques  
+ Application Note 94, Slew Rate Verification for Wideband Amplifiers  

## 修订历史

日期|版本|说明
---|---|---
2024-01-28|1.0|首次发布
2024-03-02|1.1|增加了电池安装方法图以及上升沿测试图
2025-11-03|1.2|EmoePulse V2.4发布

