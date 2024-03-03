---
title: Emoe Pulse Lite V2 用户手册
keywords: analog
desc: Emoe Pulse Lite V2 用户手册
author: CNPP
date: false
tags: analog, emoepulse
---
# Emoe Pulse Lite V2 用户手册

![0-1](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/0-1.JPG)

> 硬件版本： 2.1  
> 手册版本： 1.0

## 1. 简介

Emoe Pulse Lite（下简称Pulse）是一款超低功耗的便携高速窄脉冲发生器，用于一定范围内的带宽测量及线缆检测。Pulse 的电路衍生自Jim Williams于[LT AN47](https://www.analog.com/media/en/technical-documentation/application-notes/an47fa.pdf)等参考手册中发布的雪崩脉冲发生电路，如下图：

![1-1](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-1.png)

Pulse对Jim的设计进行了低功耗方向的优化，采用了雪崩击穿电压更低的射频三极管，并设计了超低功耗的DC-DC系统，使得整机可以在一片CR2032的供电下稳定工作较长时间。同时，Pulse亦可选择使用Type-C接口供电，两种供电方式通过开关选择，在通过Type-C接口供电时不需取下电池。

![1-2](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-2.JPG)

## 2. 主要指标

注意：由于Pulse电路的脉冲振荡受可调电容C影响，以下参数仅供参考。

项目|参数
---|---
Type-C接口供电电压范围|4.5 - 5.5V
CR2032电池座供电电压范围|2.5 - 3.1V
上升时间|< 350ps
脉冲峰值@50Ohm|0.8V
脉冲峰值@1MOhm|1.2V
脉冲重复频率|200kHz
工作电流@3.0V|750uA

经Keysight DSAV334A 33GHz 80Gsa/s 测试的脉冲参数：

## 3. 使用方法

首先应该将包装中的CR2032电池装入Pulse背面的电池座：

![1-3-0](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-3-0.jpg)

或接入5V供电的Type-C线缆。开关拨至`BAT`处，Pulse由电池供电；拨至对侧则为Type-C接口供电。

Pulse的主要功能为单一窄脉冲源或线缆测试仪。Pulse拥有两个对称的**输出**端口，在作为单一窄脉冲源时，应在不用的端口装上附带的50Ohm负载帽。脉冲输出端口与待测端口连接时，应根据需要加入50Ohm匹配器，下图展示了使用Pulse测试示波器的模拟输入带宽：

![1-3](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-3.JPG)

在以较大水平时间观察时，示波器应显示如下的窄脉冲串：

![1-4](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-4.JPG)

在设置好触发电平后将水平时间减小至5ns/div左右，可观察窄脉冲及上升时间：

![1-5](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-5.JPG)

当将Pulse作为线缆测试仪使用时，应将一端口紧贴示波器安装，另一端口接待测线缆。下图展示了测试开路传输线的效果：

![1-6](https://cnpp-image.oss-cn-beijing.aliyuncs.com/emoe_rd_project/cnpp_01_emoe_pulse/emoe_pulse_lite_v2_1/img/1-6.JPG)

## 参考链接

+ Application Note 47, High Speed Amplifier Techniques: <https://www.analog.com/media/en/technical-documentation/application-notes/an47fa.pdf>
+ Application Note 94, Slew Rate Verification for Wideband Amplifiers: <https://www.analog.com/media/en/technical-documentation/application-notes/an94f.pdf>

## 鸣谢

+ @FloydFish的测试与线上手册支持
+ @HAIDERdunk的33GHz示波器测试支持

## 修订历史

日期|版本|说明
---|---|---
2024-01-28|1.0|首次发布
2024-03-02|1.1|增加了电池安装方法图以及上升沿测试图
