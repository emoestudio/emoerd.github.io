---
title: EmoeNAP 用户手册
keywords: analog
desc: EmoeNAP 用户手册
author: Floydfish
date: false
tags: analog, emoenap
---

# EmoeNAP 用户手册

![EmoeNAP](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/P2.jpg)

> 硬件版本： 1.1  
> 手册版本： 1.0  

## 货品清单

- EmoeNAP主机一台  
- EmoeNAP CalKit PCBA一套  
- SMA短路帽一个（安装在输入端口上）  
- SMA防尘帽3个（安装在输出端口上）

## 购买  

如果需要购买，请扫码加入Emoe R&D交流群获取货品备货信息：

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/Common%20Resource/erd_qrcode.jpg" alt="baidu" width=50% border="0"/>

</center>


## 简介

**EmoeNAP** 是一款超低噪声、高增益、高带宽的低噪声交流信号放大器，非常适合用于LDO、基准电压源芯片、运算放大器等模拟器件/电路的输出噪声测试。EmoeNAP的设计改进自 Linear Technology的 **[AN-159:Measuring 2nV/√Hz Noise and 120dB Supply Rejection on Linear Regulators](https://www.analog.com/en/app-notes/an-159.html)**。   

AN-159中使用了4个 **THAT300** 四单元匹配晶体管对。一对晶体管作为运放的差分输入级，同一封装内的4个晶体管两两并联，然后再将4个单独的放大器单元用反向加法器等比例相加，达到平均降噪的效果。  
相较于原设计，EmoeNAP使用了超低电压噪声的运算放大器，同样通过多通道并联来实现超低噪声性能。在兼顾性能的同时还降低了电路面积和成本。同时对于后面的第二、第三放大级，以及有源滤波部分做了部分改进：

- 将第二个高通RC的电容减小10倍，R增大10倍，以减小电容空间和成本  
- 将输出级的高通RC电容减小10倍，R增大10倍，以减小输出级运放的功耗（原设计带50R负载，改进后带510R负载），同时减小电容体积和成本  
- 改进电源部分，使用2节锂电池+虚拟地电路产生平衡正负电源（如果有成熟的BMS系统，可使用更多电池串联，以拓展系统动态范围），配有带电压均衡的2S升压充电器，通过Type-C接口输入5V1A即可充电。原设计使用不可充电电池，在2023年有点不环保了（bushi  

EmoeNAP的主要指标与特点如下：

- 10Hz-100kHz带宽输入底噪：176nVrms，源阻抗为0时，折合输入电压噪声谱密度为0.56nV/√Hz（未经过带宽修正，带宽修正后将更小）  
- 增益固定为80dB，可定制60dB增益（也可自行修改）  
- 3个输出带宽，分别为10Hz-100kHz、10Hz-1MHz、10Hz-2.5MHz  
- 典型输出动态范围8Vpp（满电状态下）  
- 输出阻抗：47Ω(典型值)  
- 电池供电，适合悬浮测试，消除接地环路噪声耦合  

![EmoeNAP](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/P1.jpg)

## 外观

输入侧，从左到右依次是：电子文档二维码、电源开关、输入SMA接口、USB-C充电口、电源状态指示灯

![EmoeNAP](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/P3.jpg)

输出侧，从左到右依次为宽带（10Hz-2MHz）、10Hz-1MHz、10Hz-100kHz带宽的输出SMA接口

![EmoeNAP](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/P4.jpg)

## 使用方法及注意事项

### 充电

**此设备只能在非工作状态下进行充电。**

首先将EmoeNAP的开关拨至OFF（右侧），找一个能提供5V1A输出能力的USB口，以及一根USB-TypeC数据线，接上EmoeNAP输入侧的C口，为EmoeNAP充电。  
充电过程中，红色指示灯常亮（CHRG），充满电后红色指示灯熄灭。如果电池异常，红色指示灯将闪烁，**此时建议将设备拆开检查**。  

### 正常工作状态

**此设备在工作状态下，不可接入USB电源，否则充电电路将带来严重的噪声干扰**

将EmoeNAP的开关拨至ON（左侧），绿色指示灯亮起（WORK），此时设备处于正常工作状态。如果电池电量过低，会自动触发电池保护功能，此时工作指示灯也将熄灭。

### 接入输入信号

请使用SMA同轴线连接到DUT进行测试。（🐟建议在DUT使用IPEX座子，配合IPEX转SMA测试线缆，非常方便）。  
同时应注意，应尽量使用同轴线缆/屏蔽线缆作为输入线，**如果输入线缆有任何未屏蔽的部分，都可能引入额外噪声和干扰**。 

### 输入信号范围

EmoeNAP的增益极高，输入1mVpp的交流信号就将导致输出饱和，所以 **请谨慎决定使用EmoeNAP的场合！！**  
输入级耦合电容的耐压为16V，**建议输入信号的直流成分不超过12V**，否则有损坏的风险。

短时间内为EmoeNAP输入大电压不会损坏，但会增加功耗。不建议对EmoeNAP输入施加 **800uVpp** 以上的大信号。

### 输出到示波器

输出端的输出阻抗为47R，此电阻串联在opamp输出，是用于防止opamp输出带大容性负载振荡的。如果将输出连接至示波器，请设置示波器为1MΩ输入阻抗，并设置DC耦合。（NAP输出本身就是交流耦合，不存在DC成分）
测量噪声时，应选择测量项为 **交流有效值（AC.RMS）**。

### 输出到DMM

常见DMM的交流档带宽都在300kHz左右，所以只有10Hz-100kHz带宽输出接到DMM，能获得可信的测量结果。  
DMM的交流档输入是高阻输入，无需设置输入阻抗。  

### 输出到自定义设备

只需要保持设备的输入阻抗为100kΩ以上，输入电容不要太大即可。  
如果必须使用50Ω输入阻抗，请注意电压分压比换算（在50Ω终端电阻上得到约0.5倍输出电压）  

### 应用EmoeNAP-CalKit


由于EmoeNAP的增益极高，输入1mVpp的正弦信号，输出就会直接饱和（80dB增益放大后理论上将输出10Vpp，但由于NAP的供电最高只有8.4V，且运放输出摆幅离电源轨约有百mV压差，无法无失真输出10Vpp信号）  

为了方便增益校准、测试与验证，🐟设计了配套的EmoeNAP-CalKit。  
其实非常简单，就是拼凑合适阻值的电阻，然后用拨码开关去选择切换衰减档位。

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/7.jpg" width="80%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/8.jpg" width="80%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/9.jpg" width="80%" border="0"/>

</center>

输入信号是交流信号，那么在通带内我们可以忽略输入耦合电容，将其视作短路，EmoeNAP的有效输入电阻是RL=499R。通过RATT与RL（RIN）分压，可以在放大器输入端得到一个较小的信号。

那么只需要选择RATT为499R、4.5k、49.5k、499.5k，就可以实现6dB、20dB、40dB、60dB的衰减值。

### 增益验证与校准

准备一台幅度准确的信号源，一台示波器/高精度DMM，以及EmoeNAP-CalKit。  
设置信号源输出20mVrms、频率10kHz的正弦信号，连接到EmoeNAP-CalKit上，设置CalKit衰减为40dB，在EmoeNAP输入端可得到200uVrms的交流信号。  

然后将EmoeNAP的10Hz-100kHz输出端接到示波器输入端，设置示波器DC耦合，1MΩ输入阻抗，开启AC.RMS测量项，调整波形合适，此时输出真有效值应该为2Vrms。

如果输出离2Vrms偏离较大，可以调整电路板上的Gain TRIM电位器，调整幅度至2Vrms，即可完成增益校准。

如果示波器精度不高，可以用DMM的交流档交叉验证测试结果。

### 测试注意事项

- 尽量不要让输入端接有任何陶瓷电容，陶瓷电容受到任何震动都会产生噪声电压，破坏测量结果。如果不能没有，请做好隔震措施再测试    
- 尽量不要在DUT和EmoeNAP输入端之间串联电阻，任何电阻值都会算入DUT的输出阻抗（源阻抗），使得EmoeNAP的本底噪声抬高，微弱信号测量时可能导致较大误差。  
- EmoeNAP适合低输出阻抗的DUT测试，不适用于高源阻抗型DUT测试。**源阻抗较高时，电流噪声将主导设备的底噪，使得测试结果不可用**。  


## 性能测试与表征

### 滤波器带宽测试

100kHz带宽的典型bode响应如下(由于增加了40dB衰减，所以测试出的增益为40dB)：

![100kHz_Variation](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/100k.jpg)

1MHz带宽的典型bode响应如下(由于增加了40dB衰减，所以测试出的增益为40dB)：

![1MHz_Variation](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/1M.jpg)

抽取6个样本，测试100kHz带宽的低通滤波器响应：

![100kHz](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/1.jpg)

抽取6个样本，测试1MHz带宽的低通滤波器响应(由于高频段输出幅度太小，示波器无法准确测量rms值，故有较大起伏)：

![1MHz](https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/emoerd/emoenap/2.jpg)

### 底噪测试

使用SMA-Short将输入短接，测量10Hz-100kHz、10Hz-1MHz、10Hz-Wideband输出端的噪声，分别为1.716mVrms、5.47mVrms、12.853mVrms。

由于器件差异和增益校准误差，底噪可能略有波动。

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/nf_100k.PNG" width="80%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/nf_1m.PNG" width="80%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/nf_wide.PNG" width="80%" border="0"/>

</center>

## 测试案例

### LT3042超低噪声LDO输出噪声测试

测量LT3042空载输出噪声（3.3V DC），10Hz-100kHz、10Hz-1MHz分别为0.673uVrms、1.057uVrms：

<div style="display: flex; justify-content: center; align-items: center;">

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/3042_100k.PNG" width="50%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/3042_1m.PNG" width="50%" border="0"/>

</div>

指标来看是满足其数据手册规格的。因为LDO输出侧接有MLCC电容，使得 **DUT对震动异常敏感**。手接近PCB时都会引起输出大幅度跳动。**测试时需要注意屏蔽和隔震措施**。

### TPS7A4901低噪声LDO输出噪声测试

测量TPS7A4901空载输出噪声（3.3V DC），1ms/div时，10Hz-100kHz为10.6uVrms；200ms/div时，10Hz-100kHz为12.4uVrms。这是因为1ms/div时采不到极低频噪声，所以读数偏小。

<div style="display: flex; justify-content: center; align-items: center;">

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/4901_100k.PNG" width="50%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/4901_1m.PNG" width="50%" border="0"/>

</div>

4901的噪声水平符合手册指标。从4901的手册中截取输出噪声谱密度曲线如下，在10Hz-1k段、10k-100k段有明显的陡降，FFT观测与此相符。

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/4901_ds.PNG" width="80%" border="0"/>

</center>

### LM399基准电压源输出噪声测试

时基1ms/div测试10Hz-100kHz噪声为48.6uVrms

<div style="display: flex; justify-content: center; align-items: center;">

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/399_100k.png" width="50%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/399_ds.png" width="50%" border="0"/>

</div>

LM399手册给出的10Hz-10kHz带宽噪声为7uVrms（典型值），最大50uVrms。Zener加热稳定后的噪声水平比常温下高，可以从噪声密度谱曲线看出，10Hz-100k段噪声除开1/f成分，基本是平坦的。可以根据10Hz-10kHz带宽的噪声水平估计100k带宽，乘以√10即可，测量结果在手册指标范围内。

### ADR1399基准电压源输出噪声测试

时基100ms/div测试10Hz-100kHz噪声、测试10Hz-1MHz噪声分别为23uVrms、46.7uVrms。

<div style="display: flex; justify-content: center; align-items: center;">

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/1399_100k.PNG" width="50%" border="0"/>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/1399_1m.PNG" width="50%" border="0"/>

</div>

100k带宽的结果是可信的，**但1MHz带宽的测量结果不是那么可信**，因为这个EVM上用了一个隔离的DC-DC LTM模组，开关频率约为500kHz，在隔离输出侧的滤波措施并不好，因此污染了基准的输出。在100kHz档位测试时，此ripple成分对总噪声值贡献较少，不影响测试结果（因为有100k的低通滤波器），但在1MHz档位时，此纹波位于通带内，占据了噪声的主要成分，所以读数不可信。

我们再看看100k带宽测试时的输出信号FFT频谱成分，可以看到500k左右的开关纹波被低通滤波器衰减了很多，低于噪声电压水平，对测试结果影响不是那么大。（这里测量项的平均值忘了清空了，当前值215.68是准确的）

<center>

<img src="https://emoe-blog.oss-cn-hangzhou.aliyuncs.com/article_img/%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8-%E9%87%8F%E5%8C%96%E5%AF%82%E9%9D%99/1399_100k_r.PNG" width="80%" border="0"/>

</center>


## 参考链接

- [噪声放大器构建指南](https://www.emoe.xyz/noise-amplifier-building-instruction/)   
- [噪声放大器进阶指南](https://www.emoe.xyz/noise-amplifier-step-further/)    
- [噪声放大器-量化寂静](https://www.emoe.xyz/emoenap-quantifying-silence/)
- [运算放大器噪声分析](https://www.emoe.xyz/opamp-noise-analyze/)  

此产品原理图开源，链接如下：

- [Github-Emoe R&D](https://github.com/emoestudio/EmoeNAP)  


## 鸣谢

- Linear Technology
- Analog Devices Inc
- Emoe R&D

## 修订历史

- 2024.1.31 初版发布（v1.0）  
- 2024.2.27 增加图片，部分内容修订


