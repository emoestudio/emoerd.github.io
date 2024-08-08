---
title: EmoeDAQ 用户手册
keywords: analog
desc: EmoeDAQ 用户手册
author: Floydfish
date: false
tags: analog, emoedaq
---

# EmoeDAQ 用户手册

EmoeDAQ是一款双通道、全差分输入的精密数据采集卡，具有±5V输入范围和24bit/32bit分辨率（24bit下，1 LSB等于0.6uV，32bit需要更换ADC），同时具有极低的Noise Floor、极高的输入阻抗、低输入偏置电流，非常适合用于精密直流数据采集应用。

> 硬件版本： 1.1  
> 手册版本： 0.1  

## 物理接口与标识

- 外壳尺寸100mm*66mm*27mm（长宽高），与EmoeNAP相同大小
- 后面板：
  - Type-C，用于供电+串口通信
  - 预留PMOD接口，用于后续功能开发
  - 电源状态指示LED、采样状态指示LED
- 前面板：
  - 8PIN 2.54mm间距插拔接线端子（KF-2.54），用于输入全差分模拟信号
  - 4PIN 2.54mm间距插拔接线端子（KF-2.54），用于内部电源输出（连接ULNA选件）
  - 输入过载指示灯（差分电压大于5V时，亮红灯）
- （板内留有SWD接口，方便用户更新固件（一点也不方便））
电源
- USB端口供电，电压为+5V±10%，单DAQ整机功耗约0.5W（5V 100mA）
- 可从PMOD端口供电+通信（与USB二选一，二者间没有隔离），详见PMOD PIN MAP


## 通信与协议

- 通过USART-USB与PC通信，用户也可自行从PMOD端与USART连接通信
- 兼容标准SCPI指令集，同时DAQ部分兼容ADI的ADMX3652模块指令集


## 测量端参数

- 测量端与供电端电气隔离（绝缘等级1.5kV），可实现真正的浮地测量
- 每个输入端输入阻抗>10GΩ，输入偏置电流＜300pA（+2.5V输入下）
- 全差分输入测量范围为-5V至+5V（更常用的接法是将IN-接GND，IN+接输入电压，此时输入电压范围为0V至5V）
- 输入端极限过载电压为10V（长时间过载将损坏前端）



## SCPI指令集

EmoeDAQ采用SCPI指令集与上位机通过串口进行通讯。
DAQ的SCPI指令集可以分为4类，分别是：

- IEEE 488.2标准命令  
- DAQ硬件配置命令  
- DAQ采样控制命令  
- DAQ系统命令

SCPI指令集的简写为指定的单词开头（大写），比如 `CONFigure:VOLTage:DC:NPLCycles 10` 这句可以简写为 `CONF:VOLT:DC:NPLC 10`。


### IEEE 488.2标准命令

#### *RST

- 格式：***RST**
- 功能：复位EmoeDAQ，使其硬件重新初始化
- 详细解释：接收到该命令后，DAQ会将隔离侧断电，并重新进行上电初始化操作。重新初始化后，DAQ进入出厂配置的默认工作状态  
- 示例：*RST
- 返回值："DAQ reset complete"

#### *IDN?

- 格式：***IDN?**
- 功能：查询仪器ID信息
- 详细解释：接收到该命令后，设备返回仪器型号，以及软件版本号等信息
- 示例：*IDN?
- 返回值：仪器型号，以及软件版本号等信息

### DAQ配置命令

#### CONFigure:VOLTage:DC:NPLCycles

- 格式：**CONFigure:VOLTage:DC:NPLCycles {0.1|0.25|0.5|1|10|100}**
- 功能：配置DAQ采样的积分时间 **(Number of PowerLine Cycles)**
- 详细解释：用该命令设置DAQ采样的积分时间，积分时间越长，输出数据的噪声越低，电压有效位数越高。积分时间计算是依据工频电源周期数决定的，比如NPLC=10时，积分10个工频周期。假设市电频率是50Hz，周期20ms，那么10NPLC=200ms，对应5SPS/s的数据更新速率。**需要注意，在开启AutoZero功能时，实际采样率将减半。**
- 示例：CONFigure:VOLTage:DC:NPLCycles 100
- 返回值：无

#### CONFigure:VOLTage:DC:NPLCycles?

- 格式：**CONFigure:VOLTage:DC:NPLCycles?**
- 功能：查询DAQ采样的积分周期 **(Number of PowerLine Cycles)**
- 详细解释：用该命令查询DAQ采样的积分周期，积分周期越多，输出数据的噪声越低，电压有效位数越高。**需要注意，在开启AutoZero功能时，实际采样率将减半。**
- 示例：CONFigure:VOLTage:DC:NPLCycles?
- 返回值：返回当前DAQ的采样积分周期

#### SYSTem:BAUDRATE:SET

- 格式：**SYSTem:BAUDRATE:SET {9600|14400|19200|38400|57600|115200|230400|460800|921600|1500000}**
- 功能：设置DAQ的串口波特率
- 详细解释：用该命令设置DAQ的串口通信波特率，设置完成后需要切换到新波特率与DAQ通信。
- 示例：SYSTem:BAUDRATE:SET 115200
- 返回值：无

#### SYSTem:BAUDRATE:SET?

- 格式：**SYSTem:BAUDRATE:SET?**
- 功能：查询DAQ当前的串口波特率
- 详细解释：用该命令查询DAQ的串口通信波特率。
- 示例：SYSTem:BAUDRATE:SET?
- 返回值：返回当前的串口波特率

#### CONFigure:INFormation?

- 格式：**CONFigure:INFormation?**
- 功能：查询DAQ当前的系统设置
- 详细解释：用该命令查询DAQ的系统设置，包括波特率、NPLC频率、NPLC周期数、AutoZero是否开启
- 示例：CONFIGURE:INFormation?
- 返回值：波特率、NPLC频率、NPLC周期数、AutoZero是否开启

#### CONFigure:CONTinuous:READ

- 格式：**CONFigure:CONTinuous:READ {1|2},{ON|OFF}**
- 功能：设置DAQ的采样模式为单次（关闭CONT）或连续（开启CONT）
- 详细解释：用该命令设置DAQ的采样模式，某通道在单次采样模式下，DAQ只有接收到触发信号时才会采样并输出数据（触发信号来自SCPI指令）；某通道在连续转换模式下，DAQ将自动按照设置的NPLC进行连续转换并输出数据。请注意，同时只有一个通道能被激活连续转换模式。如果需要2个通道同时连续转换，请使用扫描模式。
- 返回值：无，但如果激活连续转换，随后紧跟对应的DAQ采样的数据


未完待续......











