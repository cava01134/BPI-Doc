# 开发板介绍
香蕉派Pico系列是專為物聯網設計的低功耗微控制器開發板。

BPI-Pico-RP2040是 Banana Pi 推出的一款搭载RP2040芯片的微控制器开发板，其最显著的特性是，在尽量保留Raspberry Pi Pico的功能，外形尺寸，引脚布局的前提下，增加一颗板载 WS2812 彩色LED；将 3-Pin DEBUG 接口替换为一个JST SH 1mm 4-Pin 插座，可与 Qwiic & STEMMA QT 或任何可能的外设连接；将micro-USB插座替换为USB Type-C插座，支持正反插，与绝大多数现代智能手机的USB C线通用，无需额外购买。

## 关键特性

- 双核 ARM Cortex M0+ CPU 内核（高达 133 MHz）
- 264K SRAM
- 2MB Flash
- 26个可用GPIO引脚，其中4个支持ADC模拟输入
- 外设:
  - 2 × UART
  - 2 × SPI 控制器
  - 2 × I2C 控制器
  - 16 × PWM 通道
  - 1 × USB 1.1 控制器和PHY，支持主机和设备
  - 8 × PIO 状态机
- 1 × LED
- 1 × WS2812 LED
- 1 × JST SH 1mm 4-Pin 插座
- 1 × USB Type-C插座

## 硬件

### 接口示意图

![](../assets/images/BPI-Pico-RP2040-V0.2-IO.jpg)

### 硬件尺寸

![](../assets/images/BPI-Pico-RP2040-V0.2-dimension.jpg)

<table>
   <tr>
      <td>BPI-Pico-RP2040 尺寸表</td>
   </tr>
   <tr>
      <td>管脚间距</td>
      <td>2.54mm</td>
   </tr>
   <tr>
      <td>安装孔间距</td>
      <td>17.6mm/ 11.4mm</td>
   </tr>
   <tr>
      <td>安装孔尺寸</td>
      <td>内径2.1mm/外径3.4mm</td>
   </tr>
   <tr>
      <td>主板尺寸</td>
      <td>11.4 × 55.8(mm)</td>
   </tr>
   <tr>
      <td>板厚</td>
      <td>1.2mm</td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>

管脚间距兼容万能板（洞洞板、点阵板），面包板，并且能直接贴在其他PCB上，便于调试应用。


## 参考资料与资源

- [GitHub: BPI-Pico-RP2040 开发板原理图PDF]() 

- [RP2040 技术规格书](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf)

- [RP2040 技术参考手册](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf)

- [rp2040-product-brief.pdf](https://datasheets.raspberrypi.com/rp2040/rp2040-product-brief.pdf)

- [raspberry-pi-pico-python-sdk.pdf](https://datasheets.raspberrypi.com/pico/raspberry-pi-pico-python-sdk.pdf)