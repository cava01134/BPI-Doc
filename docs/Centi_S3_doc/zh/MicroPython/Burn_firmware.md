## 烧录固件

> BPI-Centi-S3 出厂已烧录了具备st7789并口驱动的MicroPython固件，通常可跳过此步。

如果你在开发产生意外的恶性BUG使开发板无法正常启动，或其他原因导致固件被擦除或损坏，你可以从下面的 github 链接中找到已编译好的固件自行烧录。

[BPI-Centi-S3 micropython固件，github存档](https://github.com/BPI-STEAM/BPI-Centi-S3-Doc/tree/main/micropython_st7789s3_firmware)

## 固件信息

1. micropython1.19.1_esp32s3_qspram_st7789s3.bin
    * micropython 1.19.1 release tag
    * esp32s3, Quad SPIRAM, 8M flash
    * russhughes/st7789s3_esp_lcd
2. micropython1.20.0dev_esp32s3_qspram_st7789s3.bin
    * micropython 1.20.0 master dev, 直到2023/06/06(mip, espnow)
    * esp32s3, Quad SPIRAM, 8M flash
    * russhughes/st7789s3_esp_lcd

### 设置固件下载模式

有两种操作方法：

1.通过USB连接到电脑，按住BOOT键，再按一下RESET键并松开，最后松开BOOT键。

2.在断开供电的条件下按住BOOT键，再通过USB连接到电脑，最后松开BOOT键。

由此可知，芯片是通过BOOT键所控制的GPIO0来选择复位或重新上电时的启动模式。

在设备管理器中确认COM接口，固件下载模式与普通模式下的com接口序号通常是不一样的。

### 安装esptool工具

在终端中输入以下命令安装 esptool。

```
pip install esptool
```

### esptool命令

> 所有对flash的操作都是不可逆的，注意备份有价值的数据。

* 擦除flash

```
python -m esptool --chip esp32s3 --port COM1 --baud 460800 erase_flash
```

* 写入flash

```
python -m esptool --chip esp32s3 --port COM1 --baud 460800 --before=usb_reset --after=no_reset write_flash 0x0 esp32s3_micropython_qspram_st7789s3_idf4.4.3.bin
```
