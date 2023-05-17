# 板载资源的使用

本章主要是通过一些示例项目，阐述 Centi-S3 主控板的外设基本使用方法，通过下面的项目，您可以进行修改完成您的自己的项目。 
其中 Centi-S3 外设主要包括：UART、I2C、SPI、ADC、PWM、DAC等。 

## 开始之前的准备

BPI-Centi-S3 开发板上的typec使用的是ESP32-S3的原生USB接口，而不是传统的USB转TLL芯片。

为了让您的开发板能正确下载程序，您需要将BPI-Centi-S3设置为下载模式，有以下两种方法：

- 通过USB连接到电脑，按下BOOT键，再按一下Reset键并松开，最后松开BOOT键。

- 在断开所有供电的状态下，按住BOOT键，然后将开发板插上电脑，最后松开BOOT键。

这时候可以在设备管理器中看到一个多的COM口

![](../assets/images/Device_manager.jpg)

在IDE中选择这个端口

![](../assets/images/Device_manager_1.jpg)