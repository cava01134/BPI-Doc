## 烧录固件

> BPI-Centi-S3 出厂已烧录了具备st7789并口驱动的MicroPython固件，通常可跳过此步。

如果你在开发产生意外的恶性BUG使开发板无法正常启动，或其他原因导致固件被擦除或损坏，你可以从下面的 github 链接中找到已编译好的固件自行烧录。

[BPI-Centi-S3 micropython固件，github存档](https://github.com/BPI-STEAM/BPI-Centi-S3-Doc/tree/main/micropython_st7789s3_firmware)

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
