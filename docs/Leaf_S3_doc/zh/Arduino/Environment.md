# 环境搭建

这篇文章将会指引您安装Leaf-S3的Arduino支持。

## 使用Arduino IDE安装支持

![](../assets/images/logo_arduino.png)

这是直接从 Arduino IDE 安装 Arduino-ESP32 的方法。

> 注意：有关 SoC 支持的概述，请查看[Supported Soc](https://docs.espressif.com/projects/arduino-esp32/en/latest/getting_started.html#supported-soc-s) 的表格，您可以在其中找到特定芯片是否处于稳定或开发版本。

- 稳定版链接：https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

- 开发版链接：https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json

> 从 Arduino IDE 版本 1.6.4 开始，Arduino 允许使用 Boards Manager 安装第三方平台包。我们有适用于 Windows、macOS 和 Linux 的软件包。

要使用 Boards Managaer 开始安装过程，请执行以下步骤：

- 安装 1.8 或更高版本的当前上游 Arduino IDE。当前版本位于arduino.cc网站。

- 启动 Arduino 并打开 文件>首选项 窗口，并点击图示中的位置。

![](../assets/images/install_guide_preferences.png)

- 在Additional Board Manager URLs后面输入上述发布链接之一。您可以添加多个 URL，一行一个。

![](../assets/images/install_guide_boards_manager_url.png)

从 工具 > 开发板 菜单打开 开发板管理器 并安装esp32平台。

![](../assets/images/install_guide_boards_manager_esp32.png)

重启arduino IDE之后可以看到多了ESP32选项，按照图示配置即可

![](../assets/images/Board_chose.jpg)
