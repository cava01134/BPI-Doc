# 【 BPI-Leaf-S3 Development Board 】


## key features

- ESP32-S3, Xtensa® 32 bit LX7
- Off-chip PSRAM, FLASH
- Ultra-low power 10uA
- 2.4G WIFI, Bluetooth 5, Bluetooth mesh
- GPIO , ADC , TOUCH , PWM , I2C , SPI , RMT , I2S , UART , LCD, CAMERA , USB , JTAG
- 1* 4pin I2C connector
- 1*USB Type-C
- 1 * 2pin battery connector, support charging
- 1 * Full Color LED

## hardware

### Interface diagram

![](../assets/images/Leaf-S3_board.png)

### Hardware Specifications

<table>
   <tr>
      <td></td>
   </tr>
   <tr>
      <td>BPI-Leaf-S3 Spec Sheet</td>
   </tr>
   <tr>
      <td>SoC main control chip</td>
      <td>ESP32-S3, Xtensa® 32-bit LX7 Dual-Core Processor</td>
   </tr>
   <tr>
      <td>Frequency</td>
      <td>240MHz MAX</td>
   </tr>
   <tr>
      <td>Operating Temperature</td>
      <td>-40℃~+85℃</td>
   </tr>
   <tr>
      <td>On-Chip ROM</td>
      <td>384KB</td>
   </tr>
   <tr>
      <td>On-chip SRAM</td>
      <td>320KB</td>
   </tr>
   <tr>
      <td>Off-chip FLASH ROM</td>
      <td>8MB</td>
   </tr>
   <tr>
      <td>Off-chip PSRAM</td>
      <td>2MB</td>
   </tr>
   <tr>
      <td>WIFI</td>
      <td>IEEE 802.11 b/g/n, 2.4Ghz band, 150Mbps</td>
   </tr>
   <tr>
      <td>Bluetooth</td>
      <td>Bluetooth 5, Bluetooth mesh</td>
   </tr>
   <tr>
      <td>GPIO</td>
      <td>BPI-Leaf-S3 has brought out 36 available GPIOs</td>
   </tr>
   <tr>
      <td>ADC</td>
      <td>2 × 12-bit SAR ADC supporting 20 analog channel inputs</td>
   </tr>
   <tr>
      <td>TOUCH Capacitive Touch Sensor</td>
      <td>14</td>
   </tr>
   <tr>
      <td>SPI</td>
      <td>4</td>
   </tr>
   <tr>
      <td>I2C</td>
      <td>2, supports master or slave mode</td>
   </tr>
   <tr>
      <td>I2S</td>
      <td>2, serial stereo data input and output</td>
   </tr>
   <tr>
      <td>LCD</td>
      <td>1, supports 8-bit ~16-bit parallel RGB, I8080, MOTO6800 interface</td>
   </tr>
   <tr>
      <td>CAMERA</td>
      <td>1, supports 8-bit ~16-bit DVP image sensor interface</td>
   </tr>
   <tr>
      <td>UART</td>
      <td>3, supports asynchronous communication (RS232 and RS485) and IrDA</td>
   </tr>
   <tr>
      <td>PWM</td>
      <td>8 independent channels, 14-bit precision</td>
   </tr>
   <tr>
      <td>MCPWM</td>
      <td>2</td>
   </tr>
   <tr>
      <td>USB</td>
      <td>1 × Full Speed ​​USB 2.0 OTG, Type-C Female</td>
   </tr>
   <tr>
      <td>USB Serial/JTAG Controller</td>
      <td>1, USB Full Speed ​​Standard, CDC-ACM, JTAG</td>
   </tr>
   <tr>
      <td>Temperature Sensor</td>
      <td>1, measuring from –20 °C to 110 °C, for monitoring chip internal temperature</td>
   </tr>
   <tr>
      <td>SD/MMC</td>
      <td>1 × SDIO host interface with 2 card slots, supports SD card 3.0 and 3.01, SDIO 3.0, CE-ATA 1.1, MMC 4.41, eMMC 4.5 and 4.51</td>
   </tr>
   <tr>
      <td>TWAI® Controller</td>
      <td>1, compatible with ISO11898-1 (CAN Specification 2.0)</td>
   </tr>
   <tr>
      <td>Generic DMA Controller</td>
      <td>5 receive channels and 5 transmit channels</td>
   </tr>
   <tr>
      <td>RMT</td>
      <td>4 channels transmit, 4 channels receive, shared 384 x 32-bit RAM</td>
   </tr>
   <tr>
      <td>Pulse Counter</td>
      <td>4 pulse count controllers (units), each with 2 independent channels</td>
   </tr>
   <tr>
      <td>Timer</td>
      <td>4 × 54-bit general-purpose timers, 16-bit clock prescaler, 1 × 52-bit system timer, 3 × watchdog timers</td>
   </tr>
   <tr>
      <td>External Crystal</td>
      <td>40Mhz</td>
   </tr>
   <tr>
      <td>RTC and Low Power Management</td>
      <td>Power Management Unit (PMU) + Ultra Low Power Coprocessor (ULP)</td>
   </tr>
   <tr>
      <td>Low Power Current</td>
      <td>10uA</td>
   </tr>
   <tr>
      <td>Operating Voltage</td>
      <td>3.3V</td>
   </tr>
   <tr>
      <td>Input Voltage</td>
      <td>3.3V~5.5V</td>
   </tr>
   <tr>
      <td>Maximum Discharge Current</td>
      <td>2A@3.3V DC/DC</td>
   </tr>
   <tr>
      <td>USB charging</td>
      <td>Support</td>
   </tr>
   <tr>
      <td>Maximum Charge Current</td>
      <td>500mA</td>
   </tr>
   <tr>
      <td>Controllable full color LED</td>
      <td>1</td>
   </tr>
</table>


### Hardware Dimensions


![](../assets/images/Leaf-S3_board_dimension.png)

<table>
   <tr>
      <td>BPI-Leaf-S3 Size Chart</td>
   </tr>
   <tr>
      <td>Pin spacing</td>
      <td>2.54mm</td>
   </tr>
   <tr>
      <td>Mounting Hole Spacing</td>
      <td>23mm/ 62.25mm</td>
   </tr>
   <tr>
      <td>Mounting Hole Dimensions</td>
      <td>Inner Diameter 2mm/Outer Diameter 3mm</td>
   </tr>
   <tr>
      <td>Motherboard Dimensions</td>
      <td>26 × 65.25(mm)/1.02 x 2.57(inches)</td>
   </tr>
   <tr>
      <td>plate thickness</td>
      <td>1.2mm</td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>

The pin spacing is compatible with universal boards (hole boards, dot matrix boards) and breadboards, which is convenient for debugging applications.


## software

### ESP-IDF

![](../assets/images/Esp-idf-logo.png)

ESP-IDF is an IoT development framework officially launched by Espressif, supporting Windows, Linux and macOS operating systems.

It is recommended that developers install ESP-IDF via an integrated development environment (IDE):

- [GitHub: ESP-IDF Eclipse Plugin Installation and Usage Guide](https://github.com/espressif/idf-eclipse-plugin/blob/master/README_CN.md)

- [ESP-IDF VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=espressif.esp-idf-extension) | [GitHub: Installation and Usage Guide](https://github.com/espressif /vscode-esp-idf-extension/blob/master/docs/tutorial/toc.md) | [bilibili: ESP-IDF VSCode Plugin Quick Operation Guide](https://www.bilibili.com/video/BV17p4y167uN)

Or select the corresponding manual installation process according to the operating system:

- [Standard setup for Windows platform toolchain](https://docs.espressif.com/projects/esp-idf/en_US/latest/esp32s3/get-started/windows-setup.html)

- [Standard setup for Linux and macOS platform toolchains](https://docs.espressif.com/projects/esp-idf/en_US/latest/esp32s3/get-started/linux-macos-setup.html)

Or select the corresponding manual installation process according to the operating system:

- [API Reference](https://docs.espressif.com/projects/esp-idf/en_US/latest/esp32s3/api-reference/index.html#api)

- [API Guide](https://docs.espressif.com/projects/esp-idf/en_US/latest/esp32s3/api-guides/index.html#api)

In order to enable your BPI-Leaf-S3 development board to flash FLASH through USB-CDC, you need to set the development board to firmware download mode.

There are two methods of operation:

1. Connect to the computer via USB, press and hold the BOOT button, then press the RESET button and release it, and finally release the BOOT button.

2. Press and hold the BOOT button when the power supply is disconnected, then connect to the computer via USB, and finally release the BOOT button.

need to Confirm the interface in the device manager. The serial number of the interface in the firmware download mode and the normal mode may be different.

## MicroPython

![](../assets/images/Mircopython.png)

MicroPython implements most of the features and syntax of Python 3, which is easy to learn and use, and can be downloaded directly into the chip to run the verification program without compiling.

Regardless of whether you have a programming foundation or not, MicroPython is far less difficult to get started with than other programming languages. Its code is easy to read, and the open source community has accumulated rich resources over the years. Just like Python, it has strong vitality and application value.

- [GitHub: MicroPython Quick Start](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/tree/main/Example/MicroPython-zh#1%E5%BF%AB%E9%80 %9F%E4%B8%8A%E6%89%8B)

- [Building a MicroPython runtime environment (Thonny IDE)](https://wiki.banana-pi.org/Micropython_%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83% E6%90%AD%E5%BB%BA)

- [MicroPython firmware download and programming](https://wiki.banana-pi.org/Micropython_%E5%9B%BA%E4%BB%B6%E4%B8%8B%E8%BD%BD%E4% B8%8E%E7%83%A7%E5%BD%95)

## Arduino

![](../assets/images/Arduino_logo_1200x350.png)

Arduino is an open source embedded software and hardware development platform for users to create interactive embedded projects.

- [Arduino IDE download address](https://www.arduino.cc/en/software) | [Install and configure Arduino-ESP32 runtime environment](https://docs.espressif.com/projects/arduino-esp32/ en/latest/installing.html#installing)

- [GitHub: BPI-Leaf-S3 Arduino Quick Start](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/blob/main/Example/Arduino)

- [Arduino-ESP32 APIs](https://docs.espressif.com/projects/arduino-esp32/en/latest/libraries.html#apis)

## Information and resources

- [GitHub: BPI-Leaf-S3 Development Board Schematic PDF](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/blob/main/sch/BPI-Leaf-S3-Chip- V0.1A.pdf)

- [ESP32-S3 Specifications](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/blob/main/Example/Arduino)

- [ESP32-S3 Technical Reference Manual](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_cn.pdf)

## Sample purchase

- [Official AliExpress](https://www.aliexpress.com/item/1005004428945296.html?spm=5261.ProductManageOnline.0.0.48af4edfYbyEoI)

- [Official Taobao](https://item.taobao.com/item.htm?spm=a2126o.success.0.0.29034831FGnLQW&id=677287234553)

- OEM&OEM customized service: sales@banana-pi.com