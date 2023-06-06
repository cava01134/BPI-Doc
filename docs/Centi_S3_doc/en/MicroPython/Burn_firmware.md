## Burn firmware

> BPI-Centi-S3 has been programmed with MicroPython firmware with st7789 parallel port driver, usually this step can be skipped.

If you have an unexpected vicious BUG during development that prevents the development board from starting normally, or other reasons cause the firmware to be erased or damaged, you can find the compiled firmware from the github link below and burn it yourself.

[BPI-Centi-S3 micropython firmware, github archive](https://github.com/BPI-STEAM/BPI-Centi-S3-Doc/tree/main/micropython_st7789s3_firmware)

### Set firmware download mode

There are two methods of operation:

1. Connect to the computer via USB, press and hold the BOOT button, then press the RESET button and release it, and finally release the BOOT button.

2. Press and hold the BOOT button when the power supply is disconnected, then connect to the computer via USB, and finally release the BOOT button.

It can be seen from this that the chip selects the startup mode when reset or re-powered through the GPIO0 controlled by the BOOT key.

Confirm the COM interface in the device manager. The serial number of the COM interface in the firmware download mode and the normal mode is usually different.

### Install the esptool tool

Enter the following command in the terminal to install esptool.

```
pip install esptool
```

### esptool command

> All operations on flash are irreversible, pay attention to backup valuable data.

* Erase flash

```
python -m esptool --chip esp32s3 --port COM1 --baud 460800 erase_flash
```

* write to flash

```
python -m esptool --chip esp32s3 --port COM1 --baud 460800 --before=usb_reset --after=no_reset write_flash 0x0 micropython1.19.1_esp32s3_qspram_st7789s3.bin
```
