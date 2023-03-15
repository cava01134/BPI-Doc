## What is MicroPython?

![](assets\images\Mircopython.png)

[MicroPython](https://micropython.org/) is a lean and efficient implementation of the Python 3 programming language that includes a small subset of the Python standard library and is optimised to run on microcontrollers and in constrained environments.

Crowdfunded and open sourced in 2013 by [Damien P. George](https://dpgeorge.net/).

The most obvious difference between it and the use of C programs to develop microcontrollers is that there is no need for lengthy compilation when verifying code.

Using serial communication software, enter commands through the REPL(read-eval-print-loop) to control the microcontroller, just like Python's REPL.

It is also possible to use some tools to upload a python script file to run inside the microcontroller.

Its implementation of Python3 includes the _thread library that supports multithreading and the asyncio library for writing concurrent code.

MicroPython aims to be as compatible with normal Python as possible to allow you to transfer code with ease from the desktop to a microcontroller or embedded system.

At the same time it also has some libraries specific for microcontrollers in order to take full advantage of the hardware features inside the microcontroller chip, such as timers, hardware interrupts, WiFi, etc., depending on the specific hardware.

While having the above features, it is compact enough to fit and run within just 256k of code space and 16k of RAM.

If you know Python you already know MicroPython.

On the other hand, the more you learn about MicroPython the better you become at Python.

## Configure the development environment

### System Environment Requirements

Supports development in Windows 10, Windows 11, MacOS, Ubuntu or other Linux desktop operating systems.

All the application examples in this article are based on the Windows 10 operating system, and other operating systems can also be used for reference.


### Install the Python environment

Open [Python official website](https://www.python.org/).

For the Windows system, the most convenient way to download the installation package is to click the icon shown in the figure below on the homepage of the official website to download.

![](assets/images/Micropython_operating_env_1.png)

Other operating systems or other distributions can be selected in the Downloads option bar.

It is recommended to use a version above python 3.7.

Be sure to check Add Python 3.x to PATH when you start the installation, so that you can avoid adding it to PATH manually.

![](assets/images/Micropython_operating_env_2.png)

Follow the installation prompts step by step to complete the installation smoothly.

### Install the mpremote tool

The MicroPython open source community has launched a development aid tool: mpremote, through which we can establish serial communication with the development board, use REPL, and manage the file system on the development board. It also has mount and mip functions, which will be described in detail in subsequent chapters (preparing).

After installing the Python environment, you can use pip to install mpremote in the terminal.

Open PowerShell in the Windows system, and open the corresponding terminal in other operating systems, and enter the following command to install mpremote.

```
pip install mpremote
```

### Install the mpbridge tool

mpbridge is a CLI tool developed based on mpremote. It mainly provides the function of automatically synchronizing files to improve development efficiency.

Enter the following command in Terminal to install mpbridge.

```
pip install mpbridge
```

### choose an editor

The use of MicroPython does not depend on specific development tools, as long as the serial port communication with the development board can be established, the MicroPython interactive interpreter (REPL) can be obtained.

Purely speaking, we basically only need a text editor to edit the code, and then upload our .py script files or other files to the development board through the mpremote tool or mpbridge tool.

For a specific editor that combines basic code completion, syntax highlighting, integrated terminal, and lightweight, multi-platform adaptation requirements, I recommend using Visual Studio Code (VScode), which may already be the tool you are most familiar with one of them.

[Visual Studio Code official website address](https://code.visualstudio.com/)

VScode documentation that may need to be referred to:
* [Visual Studio Code Official Documentation](https://code.visualstudio.com/docs)
     * [Basic installation, setup](https://code.visualstudio.com/docs/setup/setup-overview)
     * [Set Display Language](https://code.visualstudio.com/docs/getstarted/locales)
     * [Use the integrated terminal](https://code.visualstudio.com/docs/terminal/basics)


All in Web people, minimalists, try [VScode on the Web](https://vscode.dev/), open a terminal on the local operating system and use mpremote or mpbridge to synchronize files, MicroPython REPL.

Other recommended editors:
* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/#section=windows), the free community edition is sufficient, and it also has code completion for micropython special libraries, although incomplete and It hasn't been updated for a long time.
* [Jupyter](https://jupyter.org/) is also an editor on the web side, more professional, more pythonic, and supports web terminals.
* [Thonny](https://github.com/thonny/thonny/releases), an open source graphical python editor sponsored by the Raspberry Pi Foundation, factory integrated with Raspberry Pi OS (Raspbian), the first choice for Raspberry Pi users to get started .

## Burn firmware

> BPI-Centi-S3 has been programmed with MicroPython firmware with st7789 parallel port driver, usually this step can be skipped.

If you have an unexpected vicious BUG during development that prevents the development board from starting normally, or other reasons cause the firmware to be erased or damaged, you can find the compiled firmware from the github link below and burn it yourself.

[BPI-Centi-S3 micropython firmware, github archive](https://github.com/BPI-STEAM/BPI-Centi-S3-Doc/tree/main/micropython_st7789s3_firmware)

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
python -m esptool --chip esp32s3 --port COM1 --baud 460800 --before=usb_reset --after=no_reset write_flash 0x0 esp32s3_micropython_qspram_st7789s3_idf4.4.3.bin
```

## Connect to the development board

### Use mpremote in terminal

Click the Terminal/terminal of VScode to create a new terminal window and enter commands in the terminal.

If you are using other editors, or only use the local terminal of the system, you can also use it directly.

![](assets\images\vscode_terminal.png)

`--help` can view all available commands of mpremote:
```
mpremote --help
```

Command to list all serial interfaces:
```
mpremote connect list
```

Connect the serial port where the development board is located and enter the MicroPython REPL:
```
mpremote connect COM1 repl
```

`COM1` is the format of the serial interface in Windows systems, it may be `/dev/ttyACM0` in Linux, and it may be `/dev/cu.usbmodem01` in MacOS.

After entering the REPL, you can enter MicroPython code to make it run on the development board.
```python
>>>print("Hello")
```

The way to exit the REPL is the keyboard shortcut `ctrl + ]`.

### Use mpbridge in terminal

The main function of the mpbridge tool is to synchronize a local folder with the file system in the development board, so we first determine the folder to be synchronized.

Create a new folder in a location you think is suitable locally on the PC, or select a folder, and then open the folder in VScode.

![](assets\images\vscode_open_folder.png)

![](assets\images\vscode_open_folder2.png)

Then open a terminal in VScode, and you can enter the path where this folder is located in the terminal.

![](assets\images\vscode_terminal2.png)

If you are using the default terminal, you can enter this path with `cd [absolute path to target folder]`.

```sh
PS C:\Users\Wind> cd D:\temp\temp
PS D:\temp\temp>
```

Command to list all serial interfaces using mpbridge tool:
```
mpbridge list
```

Use the mpbridge tool to synchronize files, the command is as follows, pay attention to modify `COM1` to the actual corresponding serial port of the development board:
```
mpbridge dev --auto-reset hard COM1
```

![](assets\images\vscode_terminal_mpbridge.png)

When the synchronization is completed, you will be prompted to press the `Enter` key. 

After pressing the key, the development board will be hard reset, and the terminal will enter the MicroPython REPL.

After pressing the keyboard shortcut `ctrl + ]` to exit the REPL, the file will be synchronized again immediately. At this time, you can choose to press `ctrl + C` to exit.

Every sync, mpbridge will automatically perform these file operations:

1. Pull the files that are not in the local but exist in the device to the local.
2. Push files that are not in the device but exist locally to the device.
3. Check the hash of the files both in the local and the device,and then push the different files from the local to the device.

