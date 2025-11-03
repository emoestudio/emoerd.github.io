---
title: EmoeDAQ Datasheet
keywords: EmoeDAQ
desc: EmoeDAQ Datasheet
author: Floydfish
date: false
tags: analog, emoedaq
---

# EmoeDAQ 数据手册

EmoeDAQ是一款双通道、全差分输入的精密数据采集卡，具有±4.95V输入范围和24bit/32bit分辨率（24bit下，1 LSB等于0.6uV），同时具有极低的Noise Floor、极高的输入阻抗、低输入偏置电流，非常适合用于精密直流数据采集应用。

> 硬件版本： 1.4.0 
> 手册版本： 1.0.5
> 最后更新时间： 2025-03-28

## EmoeDAQ特性概览

- 测量端与数据接口全电气隔离，耐压1.5kV，浮地测量  
- 2个全差分输入通道，双极性电压测量，差分输入范围±4.95V  
- 采样积分周期从NPLC=100至NPLC=0.1可调(非连续可调)  
- 高达22bit无噪声分辨率（同量程范围下，6½数字万用表同等级指标）  
- 校准后最大积分非线性误差（INL）为 **±3.5ppm**（7ppm-Peak to Peak，以34465A的10V档为参考）  
- 基准老化性能为25ppm/khr，出厂的DAQ均经过3周早期老化  
- 内部主动加热恒温设计，恒温温度一定范围内可调  
- 采样率500Hz时，THD<-100dB  
- 支持内部失调实时补偿  
- 短期稳定性（24小时）等同于6½数字万用表水平  
- 支持用户自定义校准  
- 兼容标准SCPI指令集  

## EmoeDAQ典型应用  

- 电压输出型传感器数据采集  
- 精密电压测量与长时间观测  
- 1/f噪声测量  
- 自定义设备的模拟采集核心  
- 极低频交流信号精密分析  
- 自定义测试系统组件  
  
搭配Emoe R&D提供的开源选件，可实现如下应用：  

- 精密RTD/热电偶温度测量（RTD/热电偶选件）  
- 双向高/低边电流测量（检流电阻、INA选件）  
- 超宽动态范围I-V变换电流测量（与EmoeFemto搭配使用）  
- 超低噪声小信号测量，nV-Meter（nV-Meter前端选件）  
- 多通道电信号扫描测量（扫描卡选件）  

更多选件仍在开发中，也欢迎在我们的github organization仓库发起讨论，一起开发新玩法。  

## 物理接口与标识

- 外壳尺寸100mm / 66mm / 27mm（长宽高），与EmoeNAP相同大小
- 后面板：
  - Type-C，用于供电+USB VCP串口通信，以及DFU固件升级    
  - SCPI通信错误状态指示LED、内部温度状态指示LED  
- 前面板：
  - 8PIN 2.54mm间距插拔接线端子（KF-2.54），用于输入全差分模拟信号  
  - 2*3PIN 2.54mm牛角座，用于内部低噪声稳压电源输出，对外I2C接口（连接选件）  
  - 输入过载指示灯（差分电压大于5V时，亮红灯）  
- 电源  
  - USB端口供电，电压为+5V±5%

## 通信与协议

- 通过USB-VCP（Virtual COM Port）与PC通信    
- 兼容标准SCPI指令集，同时DAQ部分兼容ADI的ADMX3652模块指令集  

## 测量端参数

- 测量端与供电端电气隔离（绝缘等级1.5kV），可实现真正的浮地测量
- 每个输入端输入阻抗>10GΩ，输入偏置电流＜300pA（+2.5V/-2.5V输入下）
- 全差分输入测量范围为-4.95V至+4.95V（常用的单端测量接法是将IN-接GND，IN+接输入电压）  
- 输入端极限过载电压为±9V，内部设有气体放电管及PTC熔丝用于过载保护（长时间过载将损坏前端，不建议在信号幅度超出输入范围的场合使用）  

## 规格表

|  指标 |  |  备注(测试条件) |  
| -- | -- | -- |  
| 输入阻抗 |  大于10GΩ | 任意一端,输入信号在输入范围内 |  
| 输入漏电流 | <300pA | 输入信号在输入范围内 |  
| 可用输入测量范围 | -4.95V ~ +4.95V | 可精确测量范围(受基准精度限制) |  
| 系统无噪声分辨率 | 22.5bit | EmoeDAQ-24,NPLC=10,典型值 |  
| | 25.5bit | EmoeDAQ-32,NPLC=10,典型值 |
| 系统非线性度(INL) | <±3.5ppm | 典型值,以34465A的10V档(标称1ppm)为参考 |   
| 电压回读分辨率 | 提供小数点后8位数字，稳定至后6位(1uV) | NPLC=10，DAQ通电稳定后 |  
| 基准老化性能 | 25ppm/1000 hours, 51ppm/4500hours | 典型值，制造商保证 |  
| 系统温度系数 | TBD | TBD |  
| 24小时稳定性 | TBD | TBD |
| CMRR | DC典型值: -106dB | NPLC=10 |
| 扫描转换通道间隔离度 | DC典型值: -114dB | NPLC=10 |
|  | DC典型值: -95.4dB | NPLC=1 |
|  | DC典型值: -78.4dB | NPLC=0.1 |
| 交流动态范围 | > 130dB | NPLC=0.1 |
| 推荐校准周期 | 6 month | 不掉电使用情况下 |  
| 推荐恒温温度 | 35°C | 室温25°C下 |  

## 部分详细测试数据

测试仪器：
- Keysight 34465A  
- Keysight 34420A  
- Yokogawa 7651  
- ADR1000 PWM Reference  
- EmoeCalibrator  

### 输入对地短接失调与噪声

![zero_noise](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoeDAQ/zero_offset.png)

测试对象为EmoeDAQ-24，温度稳定后，读数仅有2个LSB的跳动。

### 上电后内部温升曲线  

![temp_rise](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoeDAQ/temp_rise.png)

测试时室温15度，上电后约15分钟达到设定的加热温度

### 交流噪声

![ac_noise](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoeDAQ/AC_NoiseFloor.png)

测试对象为EmoeDAQ-24，NPLC=0.1,采集50000个数据点后导出csv文件，使用numpy分析FFT。

### 校准后非线性度测试

![INL_Test](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoeDAQ/inl_vs34420.png)

使用7651作为DC源，34465A作为参考表对2个DAQ进行INL校准，校准后测试DAQ的INL曲线如上图(2个DAQ均为EmoeDAQ-24)

### 24小时短期稳定性与多单元一致性测试

![24h_test](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/EmoeDAQ/daq_vs_34420_24h.png)

使用以ADR1000为基准的PWM基准源，输出1V为4个设备的公共输入，以34420的1V档位测量值为参考，测试3个DAQ的数据，3个DAQ中DAQ1内部恒温开启，DAQ2和3未开启。测试时间24小时。

可以看出开启了内部恒温的DAQ1读数与34420的读数趋势几乎相同，只存在失调误差；DAQ2和DAQ3的一致性非常好，但是相对于34420的读数而言，稳定性不佳。可见使用温度控制的重要性。

# Change Log

- 2025.04.12 修复测试数据部分描述bug    










