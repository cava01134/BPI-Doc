# Micropython固件下載與燒錄

支持ESP32S3芯片的固件可以在 [MicroPython官網 ESP32S3固件下載區](https://micropython.org/download/GENERIC_S3/) 找到。

點擊鏈接進入頁面後，可以看到下方幾個固件下載地址，選擇一個.bin後綴的文件下載到本地即可。

![](../assets/images/Micropython_operating_env_6.png)

注意固件名稱中標註的日期，越接近當前時間，功能越新。

燒錄固件可以使用兩種工具，樂鑫官方FLASH下載工具或者esptool，兩者任選其一。

## 設置固件下載模式

有兩種操作方法：

1、通過USB連接電腦，按住BOOT鍵，然後按下RESET鍵再鬆開，最後鬆開BOOT鍵。

2、在斷電狀態下按住BOOT鍵，然後通過USB連接電腦，最後鬆開BOOT鍵。

由此可見，芯片通過BOOT鍵控制的GPIO0選擇復位或重新上電時的啟動方式。

在設備管理器中確認 COM 接口。固件下載模式和普通模式下的COM接口序列號通常是不同的。

![](../assets/images/Micropython_operating_env_5.png)

## Windows FLASH download tool

下載解壓：[FLASH download tool download address](https://www.espressif.com/zh-hans/support/download/other-tools)

打開軟件選擇芯片型號為ESP32S3，設置下載方式為usb：
![](../assets/images/Micropython_operating_env_7.png)

此時需要將開發板設置為固件下載模式。

在芯片處於固件下載模式的情況下，將COM接口修改為FLASH下載工具窗口中對應的接口，此處為COM22。

添加MicroPython固件，將ESP32-S3芯片的flash起始地址設置為`0x0`。

![](../assets/images/Micropython_operating_env_8.png)

首先點擊ERASE按鈕清除flash上的數據，然後點擊START將固件燒錄到flash中。

燒錄完成後，按一次RESET鍵，使開發板進入正常使用模式。

## esptool

以Windows PowerShell的具體操作步驟為例。

使用以下命令安裝 esptool：

```shell
pip install esptool
```

如果將來需要，您可以使用以下命令升級 esptool：

```shell
pip install -U esptool
```

通過命令或其他方法進入 PowerShell 中固件所在的目錄。

通過按住 shift 鍵並在 Windows 文件夾窗口中單擊鼠標右鍵，可以在此文件夾中打開 PowerShell 窗口。

此時需要將開發板設置為固件下載模式，詳見上文。

通過以下命令清除flash，需要將COM接口修改為對應的接口，這裡是COM1。

```shell
python -m esptool --chip esp32s3 --port COM1 erase_flash
```

通過以下命令燒錄固件，需要修改當前待燒錄文件名對應的固件文件名。

```shell
python -m esptool --chip esp32s3 --port COM1 --baud 460800 --before=usb_reset --after=no_reset write_flash 0x0 GENERIC_S3_SPIRAM-20220618-v1.19.1.bin
```

如果是通過USB燒錄，完成後按一次RESET鍵復位，使開發板進入正常使用模式。

如果通過UART編程，完成後會自動復位。