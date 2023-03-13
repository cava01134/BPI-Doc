## 什么是 MicroPython？

![](assets\images\Mircopython.png)

[MicroPython](https://micropython.org/)是Python 3编程语言的精益高效实现，包括 Python 标准库的一小部分，并且经过优化，可在微控制器和有限的硬件资源中运行。

由 [Damien P. George](https://dpgeorge.net/) 于 2013年 众筹开源。

它与使用C程序开发微控制器最明显的差异性，就是验证代码时无需漫长的编译。

使用串口通信软件，通过REPL(read-eval-print-loop)输入命令来控制微控制器，和Python的REPL一样。

也可使用一些工具将 python 脚本文件上传到微控制器内运行。

它对Python 3 的实现，包括了支持多线程的 _thread 库，编写并发代码的 asyncio 库。

它尽可能与普通Python兼容，允许您轻松地将代码从桌面端移植到微控制器。

同时它还具备一些特定用于微控制器的库，以便充分利用微控制器芯片内的硬件功能，例如定时器，硬件中断，WiFi等，这取决于具体的硬件。

在具备上述特性的同时，它的硬件开销很少，最低只需 256k 的代码空间和 16k 的 RAM 即可运行。

如果你了解Python，很大程度上你就已经了解MicroPython了。

在另一方面，你深入地学习MicroPython，也能提升你对Python的理解。

## 配置开发环境

### 系统环境需求

支持在 Windows 10、Windows 11，MacOS，Ubuntu 或其他 Linux 桌面操作系统中开发。

本文所有的应用示例基于 Windows 10 操作系统，其他操作系统也可参考使用。


### 安装Python环境

打开[Python官网](https://www.python.org/) 。

对于Windows 系统来说，最便捷的安装包下载方法就是在官网首页点击如下图所示的图标进行下载。

![](assets/images/Micropython_operating_env_1.png)

其他操作系统或是其他发行版本则可以在 Downloads 选项栏中进行选择。

建议使用python 3.7以上的版本。

开始安装时一定要记得勾选Add Python 3.x to PATH，这样可以免除再手动添加进PATH。

![](assets/images/Micropython_operating_env_2.png)

按照安装提示逐步操作即可顺利完成安装 。

### 安装mpremote工具

MicroPython 开源社区现已推出一款开发辅助工具：mpremote ，我们可以通过它与开发板建立串口通信，使用REPL，管理开发板上的文件系统，它还具有 mount 和 mip 功能，将在后续章节详述（准备中）。

安装完Python环境后，即可在终端使用pip安装mpremote了。

在Windows系统中打开PowerShell，其他操作系统则打开对应的终端，输入以下命令安装 mpremote。

```
pip install mpremote
```

### 安装mpbridge工具

mpbridge 是基于 mpremote 开发的CLI工具，主要提供自动化同步文件的功能，提高开发效率。

在终端中输入以下命令安装 mpbridge。

```
pip install mpbridge
```

### 任选一个编辑器

MicroPython的使用并不依赖于特定的开发工具，只要能与开发板建立串口通信，即可获得 MicroPython的交互式解释器（REPL）。

很纯粹的说，我们基本只需要一个文本编辑器来编辑代码，然后通过mpremote工具或mpbridge工具上传我们的 .py 脚本文件或其他文件到开发板中即可。

对于具体的编辑器，综合基本的代码补全、语法高亮、集成终端以及轻量化、多平台适配的需求，我推荐使用 Visual Studio Code (VScode) ，它也可能已经是你最熟悉的工具之一了。

[Visual Studio Code 官网地址](https://code.visualstudio.com/)

可能需要参考的VScode文档：
* [VScode 中文简易教程](https://www.runoob.com/w3cnote/vscode-tutorial.html?ivk_sa=1025883i)
* [Visual Studio Code 官方文档](https://code.visualstudio.com/docs)
    * [基本安装，设置](https://code.visualstudio.com/docs/setup/setup-overview)
    * [设置显示语言](https://code.visualstudio.com/docs/getstarted/locales)
    * [使用集成终端](https://code.visualstudio.com/docs/terminal/basics)
> 官方文档为英文，中文用户可以使用网页在线翻译功能辅助阅览，[Edge浏览器的在线翻译使用方法](https://support.microsoft.com/zh-cn/topic/%E5%9C%A8microsoft-edge%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E4%BD%BF%E7%94%A8microsoft-%E7%BF%BB%E8%AF%91%E5%B7%A5%E5%85%B7-4ad1c6cb-01a4-4227-be9d-a81e127fcb0b)。

All in Web 人士，极简主义人士，试试[Web端的VScode](https://vscode.dev/)，本地操作系统开个终端使用mpremote或mpbridge同步文件，MicroPython REPL。

其他推荐的编辑器：
* [PyCharm 社区版](https://www.jetbrains.com/pycharm/download/#section=windows) ，免费的社区版，够用，还带有micropython特殊库的代码补全，虽然不完整且很久没更新了。
* [Jupyter](https://jupyter.org/) 同样是Web端的编辑器，专业性更强，更python，还支持网页终端。
* [Thonny](https://github.com/thonny/thonny/releases) ，树莓派基金会赞助过的开源图形化python编辑器，Raspberry Pi OS（Raspbian）出厂集成，树莓派用户入门首选。

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

## 连接开发板

### 在终端中使用mpremote

点击VScode的Terminal/终端，新建一个终端窗口后即可在终端输入命令。

如果使用的是其他编辑器，或是仅使用系统本地终端，亦可直接使用。

![](assets\images\vscode_terminal.png)

`--help`可查看所有mpremote的可用命令:
```
mpremote --help
```

列出所有串行接口的命令：
```
mpremote connect list
```

连接开发板所在的串行接口并进入MicroPython REPL：
```
mpremote connect COM1 repl
```

`COM1`是Windows系统中的串行接口的格式，在Linux中可能是`/dev/ttyACM0`，在MacOS中可能是`/dev/cu.usbmodem01`。

进入REPL后，可以输入MicroPython代码使其在开发板中运行。
```python
>>>print("Hello")
```

退出REPL的方法是键盘快捷键`ctrl + ]`。

### 在终端中使用mpbridge

mpbridge工具最主要的功能就是将本地一个文件夹与开发板中的文件系统同步，所以我们首先就是确定要同步的文件夹。

在PC本地某个你认为合适的位置新建一个文件夹，或是选择一个文件夹，然后在VScode中打开此文件夹。

![](assets\images\vscode_open_folder.png)

![](assets\images\vscode_open_folder2.png)

然后在VScode打开一个终端，即可在终端中进入此文件夹所在的路径。

![](assets\images\vscode_terminal2.png)

如果你使用的是默认终端，则可使用 `cd [目标文件夹绝对路径]`进入此路径。

```sh
PS C:\Users\Wind> cd D:\temp\temp
PS D:\temp\temp>
```

现在即可使用mpbridge工具同步文件了，命令如下：
```
mpbridge dev --auto-reset hard COM1
```

![](assets\images\vscode_terminal_mpbridge.png)

当同步完成后，会提示按`Enter`键进入REPL。

按键盘快捷键`ctrl + ]`退出REPL后，会立即再次同步一次文件，此时可选择按`ctrl + C`退出。

