# Micropython固件下载与烧录

支持ESP32S3芯片的固件可以在 [MicroPython官网 ESP32S3固件下载区](https://micropython.org/download/GENERIC_S3/) 找到。

点击链接进入页面后，可以看到下方几个固件下载地址，选择一个.bin后缀的文件下载到本地即可。

![](../assets/images/Micropython_operating_env_6.png)

注意固件名称中标注的日期，越接近当前时间，功能越新。

烧录固件可以使用两种工具，乐鑫官方FLASH下载工具或者esptool，两者任选其一。

## 设置固件下载模式

有两种操作方法：

1、通过USB连接电脑，按住BOOT键，然后按下RESET键再松开，最后松开BOOT键。

2、在断电状态下按住BOOT键，然后通过USB连接电脑，最后松开BOOT键。

由此可见，芯片通过BOOT键控制的GPIO0选择复位或重新上电时的启动方式。

在设备管理器中确认 COM 接口。 固件下载模式和普通模式下的COM接口序列号通常是不同的。

![](../assets/images/Micropython_operating_env_5.png)

## Windows FLASH download tool

下载解压：[FLASH download tool download address](https://www.espressif.com/zh-hans/support/download/other-tools)

打开软件选择芯片型号为ESP32S3，设置下载方式为usb：
![](../assets/images/Micropython_operating_env_7.png)

此时需要将开发板设置为固件下载模式。

在芯片处于固件下载模式的情况下，将COM接口修改为FLASH下载工具窗口中对应的接口，此处为COM22。

添加MicroPython固件，将ESP32-S3芯片的flash起始地址设置为`0x0`。

![](../assets/images/Micropython_operating_env_8.png)

首先点击ERASE按钮清除flash上的数据，然后点击START将固件烧录到flash中。

烧录完成后，按一次RESET键，使开发板进入正常使用模式。

## esptool

以Windows PowerShell的具体操作步骤为例。

使用以下命令安装 esptool：

```shell
pip install esptool
```

如果将来需要，您可以使用以下命令升级 esptool：

```shell
pip install -U esptool
```

通过命令或其他方法进入 PowerShell 中固件所在的目录。

通过按住 shift 键并在 Windows 文件夹窗口中单击鼠标右键，可以在此文件夹中打开 PowerShell 窗口。

此时需要将开发板设置为固件下载模式，详见上文。

通过以下命令清除flash，需要将COM接口修改为对应的接口，这里是COM1。

```shell
python -m esptool --chip esp32s3 --port COM1 erase_flash
```

通过以下命令烧录固件，需要修改当前待烧录文件名对应的固件文件名。

```shell
python -m esptool --chip esp32s3 --port COM1 --baud 460800 --before=usb_reset --after=no_reset write_flash 0x0 GENERIC_S3_SPIRAM-20220618-v1.19.1.bin
```

如果是通过USB烧录，完成后按一次RESET键复位，使开发板进入正常使用模式。

如果通过UART编程，完成后会自动复位。