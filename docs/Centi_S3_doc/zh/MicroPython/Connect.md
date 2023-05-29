## 连接开发板

### 在终端中使用mpremote

点击VScode的Terminal/终端，新建一个终端窗口后即可在终端输入命令。

如果使用的是其他编辑器，或是仅使用系统本地终端，亦可直接使用。

![](../assets/images/vscode_terminal.png)

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

![](../assets/images/vscode_open_folder.png)

![](../assets/images/vscode_open_folder2.png)

然后在VScode打开一个终端，即可在终端中进入此文件夹所在的路径。

![](../assets/images/vscode_terminal2.png)

如果你使用的是默认终端，则可使用 `cd [目标文件夹绝对路径]`进入此路径。

```sh
PS C:\Users\Wind> cd D:\temp\temp
PS D:\temp\temp>
```

使用mpbridge工具列出所有串行接口的命令：
```
mpbridge list
```

使用mpbridge工具同步文件，命令如下，注意将`COM1`修改为开发板实际对应的串口：
```
mpbridge dev --auto-reset hard COM1
```

![](../assets/images/vscode_terminal_mpbridge.png)

当同步完成后，会提示按`Enter`键，按下后开发板硬件复位，终端将进入MicroPython REPL。

按键盘快捷键`ctrl + ]`退出REPL后，会立即再次同步一次文件，此时可选择按`ctrl + C`退出。

每次同步，mpbridge都将自动进行这些文件操作：

1. 将存在于本地但不存在于设备中的文件推送到设备中。
2. 将不存在于本地但存在于设备中的文件拉取到本地。
3. 对同时存在于本地和设备中的文件进行哈希检查，将不同的文件从本地推送到设备中。
