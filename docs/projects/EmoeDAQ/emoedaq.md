---
title: EmoeDAQ 用户手册
keywords: EmoeDAQ
desc: EmoeDAQ 用户手册
author: Floydfish
date: false
tags: analog, emoedaq
---

# EmoeDAQ 用户手册

EmoeDAQ是一款双通道、全差分输入的精密数据采集卡，具有±5V输入范围和24bit/32bit分辨率（24bit下，1 LSB等于0.6uV），同时具有极低的Noise Floor、极高的输入阻抗、低输入偏置电流，非常适合用于精密直流数据采集应用。

> 硬件版本： 1.3.0  
> 手册版本： 0.1.3  
> 最后更新时间： 2024-12-16

## 物理接口与标识

- 外壳尺寸100mm / 66mm / 27mm（长宽高），与EmoeNAP相同大小
- 后面板：
  - Type-C，用于供电+VCP串口通信  
  - SCPI通信错误状态指示LED、内部温度状态指示LED  
- 前面板：
  - 8PIN 2.54mm间距插拔接线端子（KF-2.54），用于输入全差分模拟信号  
  - 2*3PIN 2.54mm牛角座，用于内部低噪声稳压电源输出，对外I2C接口（连接选件）  
  - 输入过载指示灯（差分电压大于5V时，亮红灯）  
- （板内留有SWD接口，方便用户更新固件（一点也不方便））  
- 电源  
  - USB端口供电，电压为+5V±5%



## 通信与协议

- 通过USB-VCP（Virtual COM Port）与PC通信  
- 兼容标准SCPI指令集，同时DAQ部分兼容ADI的ADMX3652模块指令集

## 测量端参数

- 测量端与供电端电气隔离（绝缘等级1.5kV），可实现真正的浮地测量
- 每个输入端输入阻抗>10GΩ，输入偏置电流＜300pA（+2.5V/-2.5V输入下）
- 全差分输入测量范围为-5V至+5V（常用的单端测量接法是将IN-接GND，IN+接输入电压）
- 输入端极限过载电压为9V，内部设有气体放电管及PTC熔丝用于过载保护（长时间过载将损坏前端，不建议在信号幅度超出输入范围的场合使用）


## DAQ控制指令集

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
- 详细解释：接收到该命令后，DAQ会进行上电初始化操作。重新初始化后，DAQ进入出厂配置的默认工作状态  
- 示例：*RST
- 返回值："system boot complete"

#### *IDN?

- 格式：***IDN?**
- 功能：查询仪器ID信息
- 详细解释：接收到该命令后，设备返回仪器型号，以及软件版本号等信息
- 示例：*IDN?
- 返回值：仪器型号，以及软硬件版本号等信息

#### *CLS

- 格式：***CLS**
- 功能：清除错误信息
- 详细解释：接收到该命令后，设备将清除累积的SCPI错误信息
- 示例：*CLS
- 返回值：无

### DAQ采样控制命令

#### 单次测量电压

- 格式：**MEASure:VOLTage:DC? {1|2}**
- 功能：测量指定通道的电压
- 详细解释：用该命令触发DAQ对某一通道进行采样，采样的积分时间取决于目前的系统设置。支持AutoZero功能，**需要注意，在开启AutoZero功能时，实际转换时间将加倍。**
- 示例：MEASure:VOLTage:DC? 1 或 MEAS:VOLT:DC? 1
- 返回值：对应通道的电压值


#### 单次测量电压，附带温度

- 格式：**MEASure:VOLTage:DC:TEMPerature? {1|2}**
- 功能：测量指定通道的电压，以及当前DAQ内部温度值
- 详细解释：用该命令触发DAQ对某一通道进行采样，采样的积分时间取决于目前的系统设置。支持AutoZero功能，**需要注意，在开启AutoZero功能时，实际转换时间将加倍。**。输出电压的同时，还会输出DAQ内部的温度。该指令适合长期监测时使用，测量电压的同时测量DAQ内部温度
- 示例：MEASure:VOLTage:DC:TEMPerature? 1 或 MEAS:VOLT:DC:TEMP? 1
- 返回值：对应通道的电压值，以及当前DAQ内部温度，两个数据以','分隔
  
#### 单次测量通道间电压比例

- 格式：**MEASure:VOLTage:RATio? {1|2}**
- 功能：测量2个通道间的电压值比例
- 详细解释：用该命令触发DAQ进行单次采样，分别测量通道1和通道2的电压值（支持AutoZero），然后计算出2个通道间的电压比例。**命令的参数为比例计算中的分子** ——即当命令的参数为1时，返回比例值为 (Volt1 / Volt2)；当命令参数为2时，返回比例值为 (Volt2 / Volt1)
- 示例：MEAS:VOLT:RATio? 1
- 返回值：2个通道的电压比例值，保留8位小数

#### 设置采样积分周期

- 格式：**CONFigure:VOLTage:DC:NPLCycles {0.1|0.25|0.5|1|10|100}**
- 功能：配置DAQ采样的积分时间 **(Number of PowerLine Cycles)**
- 详细解释：用该命令设置DAQ采样的积分时间，积分时间越长，输出数据的噪声越低，电压有效位数越高。积分时间计算是依据工频电源周期数决定的，比如NPLC=10时，积分10个工频周期。假设市电频率是50Hz，周期20ms，那么10NPLC=200ms，对应5SPS/s的数据更新速率。**需要注意，在开启AutoZero功能时，实际采样率将减半。**
- 示例：CONFigure:VOLTage:DC:NPLCycles 100
- 返回值：无


#### 查询采样积分周期

- 格式：**CONFigure:VOLTage:DC:NPLCycles?**
- 功能：查询DAQ采样的积分周期 **(Number of PowerLine Cycles)**
- 详细解释：用该命令查询DAQ采样的积分周期，积分周期越多，输出数据的噪声越低，电压有效位数越高。**需要注意，在开启AutoZero功能时，实际采样率将减半。**
- 示例：CONFigure:VOLTage:DC:NPLCycles?
- 返回值：返回当前DAQ的采样积分周期


#### 设置DAQ连续采样

- 格式：**CONFigure:CONTinuous:READ {1|2},{ON|OFF}**
- 功能：设置DAQ的采样模式为单次（关闭CONT）或连续（开启CONT）
- 详细解释：用该命令设置DAQ的采样模式，某通道在单次采样模式下，DAQ只有接收到触发信号时才会采样并输出数据（触发信号来自SCPI指令）；某通道在连续转换模式下，DAQ将自动按照设置的NPLC进行连续转换并输出数据。请注意，同时只有一个通道能被激活连续转换模式。如果需要2个通道同时连续转换，请使用扫描模式。
- 示例：CONFigure:CONTinuous:READ 1,ON
- 返回值：无，但如果激活连续转换，随后紧跟对应的DAQ采样的数据


#### 设置DAQ扫描采样

- 格式：**CONFigure:CONTinuous:SCAN {ON|OFF}**
- 功能：设置DAQ的采样模式为扫描转换
- 详细解释：用该命令设置DAQ为连续扫描转换模式，该模式下DAQ先对CH1进行转换，然后对CH2进行转换，最后依次、同时输出2个通道的电压值。
- 示例：CONFigure:CONTinuous:SCAN ON
- 返回值：无，但如果激活连续扫描转换，随后紧跟对应的DAQ采样的数据


#### 设置AutoZero

- 格式：**CONFigure:AutoZero:DC {ON|OFF}**
- 功能：设置DAQ采样的自动校零功能
- 详细解释：用该命令设置DAQ的自动校零功能，AutoZero开启时，DAQ先测量信号链自身的失调电压，然后再测量输入信号电压，最后输出失调矫正过的数据。该模式仅对单次转换、单通道连续转换适用，暂不支持扫描转换。开启AutoZero后，对应的转换速率减半。（**该功能不能消除外部热电势影响**）
- 示例：CONFigure:AutoZero:DC ON
- 返回值：无


### DAQ硬件配置命令

#### 设置通信波特率（仅使用USART接口的固件支持）

- 格式：**SYSTem:BAUDRATE:SET {9600|14400|19200|38400|57600|115200|230400|460800|921600|1500000}**
- 功能：设置DAQ的串口波特率
- 详细解释：用该命令设置DAQ的串口通信波特率，立刻生效，**设置完成后需要切换到新波特率与DAQ通信**。
- 示例：SYSTem:BAUDRATE:SET 115200
- 返回值：无

#### 查询通信波特率（仅使用USART接口的固件支持）

- 格式：**SYSTem:BAUDRATE:SET?**
- 功能：查询DAQ当前的串口波特率
- 详细解释：用该命令查询DAQ的串口通信波特率。
- 示例：SYSTem:BAUDRATE:SET?
- 返回值：返回当前的串口波特率

#### 查询DAQ系统设置

- 格式：**CONFigure:INFormation?**
- 功能：查询DAQ当前的系统设置
- 详细解释：用该命令查询DAQ的系统设置，包括 **波特率（仅使用USART接口的固件支持）**、NPLC频率、NPLC周期数、AutoZero是否开启
- 示例：CONFIGURE:INFormation?
- 返回值：波特率、NPLC频率、NPLC周期数、AutoZero是否开启


### DAQ系统命令

#### 测量板上温度

- 格式：**MEASure:TEMPerature?**
- 功能：查询DAQ当前的系统设置
- 详细解释：用该命令激活板上温度传感器，测量基准和ADC附近的温度值，返回给用户。
- 示例：MEASure:TEMPerature?
- 返回值：板上温度，保留3位小数

#### 灯光指示

- 格式：**SYSTem:IDENtify**
- 功能：闪烁后面板的ERR指示灯
- 详细解释：闪烁后面板的ERR指示灯，用于指示当前收到指令的DAQ。在连接多个DAQ时用于区分DAQ。DAQ收到该命令后，背板上的蓝色ERR指示灯将闪烁3次。    
- 示例：SYSTem:IDEN
- 返回值：无













