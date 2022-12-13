# 基础功能使用

## REPL简单使用

###  Hello World!

1. 确保已在Mu编辑器中正确连接开发板，参考[配置使用环境(Mu编辑器)](config_mu-editor.md)。
2. 在CircuitPython REPL窗口中通常会出现如下信息，`>>>`符号的出现即代表我们可以开始在其后输入命令与其交互了。
```
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\Auto-reload is on. Simply save files over USB to run them or enter REPL to disable.

Press any key to enter the REPL. Use CTRL-D to reload.
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\]0;�Wi-Fi: off | REPL | 8.0.0-beta.0-49-g14fc4a079\
Adafruit CircuitPython 8.0.0-beta.0-49-g14fc4a079 on 2022-09-20; BPI-Bit-S2 with ESP32S3
>>> 
```
3. 在`>>>`符号右侧开始输入命令，例如：`print("Hello World！")`。
> 注意使用英文输入法，中文字符无法被REPL识别。
```py
>>> print("Hello World!")
Hello World!
>>> 
```

### REPL快捷键

1. 复制 `ctrl + shift + c`。
2. 粘贴 `ctrl + shift + v`。
   使用鼠标左键在REPL中拖选需要复制的命令，键盘按下复制快捷键，再按下粘贴快捷键即可复制粘贴命令。
3. 软复位 `ctrl + d`。
4. 中断 `ctrl + c`, 中断当前正在执行的程序，但不会重启复位。

### 查看内置模块

1. 在REPL中输入 `help("modules")` 将列出当前CircuitPython开发板内所有模块。
2. 导入模块后可再使用`help()`函数查看该模块内部可用的函数名或变量名，例如查看`board`模块，即可看到开发板所有可用的引脚与外设功能。
```py
>>> import board
>>> help(board)
object <module 'board'> is of type module
  __name__ -- board
  board_id -- bpi_bit_s2
  IO0 -- board.IO0
  A0 -- board.IO0
  D0 -- board.IO0
  DAC1 -- board.IO0
  BUZZER -- board.IO0
  IO1 -- board.IO1
  A1 -- board.IO1
  D1 -- board.IO1
  IO2 -- board.IO2
  A2 -- board.IO2
  D2 -- board.IO2
  IO3 -- board.IO3
  A3 -- board.IO3
  D3 -- board.IO3
  IO4 -- board.IO4
  A4 -- board.IO4
  D4 -- board.IO4
  IO5 -- board.IO5
  A5 -- board.IO5
  D5 -- board.IO5
  IO6 -- board.IO6
  A6 -- board.IO6
  D6 -- board.IO6
  IO7 -- board.IO7
  A7 -- board.IO7
  D7 -- board.IO7
  IO8 -- board.IO8
  A8 -- board.IO8
  D8 -- board.IO8
  IO9 -- board.IO9
  A9 -- board.IO9
  D9 -- board.IO9
  IO10 -- board.IO10
  A10 -- board.IO10
  D10 -- board.IO10
  IO11 -- board.IO11
  A11 -- board.IO11
  D11 -- board.IO11
  IO12 -- board.IO12
  D12 -- board.IO12
  IO13 -- board.IO13
  SCK -- board.IO13
  D13 -- board.IO13
  IO14 -- board.IO14
  MISO -- board.IO14
  D14 -- board.IO14
  IO15 -- board.IO15
  MOSI -- board.IO15
  D15 -- board.IO15
  IO16 -- board.IO16
  CS -- board.IO16
  D16 -- board.IO16
  SCL -- board.SCL
  IO19 -- board.SCL
  SDA -- board.SDA
  IO20 -- board.SDA
  BOOT0 -- board.BOOT0
  LED -- board.BOOT0
  BUTTON_A -- board.BUTTON_A
  BUTTON_B -- board.BUTTON_B
  LUM1 -- board.LUM1
  LUM2 -- board.LUM2
  TEMPERATURE -- board.TEMPERATURE
  NEOPIXEL -- board.NEOPIXEL
  TX -- board.TX
  RX -- board.RX
  I2C -- <function>
  SPI -- <function>
  UART -- <function>
>>> 
```

## 使一颗WS2812彩灯闪烁

1. 在Mu编辑器中点击**Load**按钮，选择CircuitPython开发板上的 code.py 文件，点击 **打开**，即可开始编辑 code.py 。

2. 在编辑器中输入如下代码：

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
    pixels[0] = (255,0,0)
    pixels.show()
    time.sleep(0.5)
    pixels[0] = (0,255,0)
    pixels.show()
    time.sleep(0.5)
    pixels[0] = (0,0,255)
    pixels.show()
    time.sleep(0.5)
    pixels[0] = (255,255,255)
    pixels.show()
    time.sleep(0.5)
```

3. 点击**Save**按钮，编辑的内容将保存到CircuitPython开发板，代码无误的情况下，开发板上的第一颗彩色LED将循环闪烁 红绿蓝白。将开发板复位或重新上电，程序将重新开始运行。
4. 在REPL中使用中断快捷键即可停止程序的运行。
5. 代码也可直接复制粘贴到REPL中运行。

> 后续所有示例都可如此编辑code.py文件或复制粘贴到REPL中运行。但在code.py文件中的程序代码执行完毕后，开发板会恢复未运行时的状态，不会保留状态，但在REPL中执行则会保留状态。

## 使用25颗WS2812彩灯

1. 在上一节**使一颗WS2812彩灯闪烁**的代码基础上，使用for循环即可依次点亮25颗WS2812彩灯。

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
    for i in range(25):
        pixels[i] = (255,0,0)
        pixels.show()
        time.sleep(0.1)
    for i in range(25):
        pixels[i] = (0,255,0)
        pixels.show()
        time.sleep(0.1)
    for i in range(25):
        pixels[i] = (0,0,255)
        pixels.show()
        time.sleep(0.1)
    for i in range(25):
        pixels[i] = (255,255,255)
        pixels.show()
        time.sleep(0.1)
```

2. 若想同时控制所有彩灯的颜色，则在for循环结束后再使用`pixels.show()`将数据发送给WS2812彩灯。

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
    for i in range(25):
        pixels[i] = (255,0,0)
    pixels.show()
    time.sleep(0.5)
    for i in range(25):
        pixels[i] = (0,255,0)
    pixels.show()
    time.sleep(0.5)
    for i in range(25):
        pixels[i] = (0,0,255)
    pixels.show()
    time.sleep(0.5)
    for i in range(25):
        pixels[i] = (255,255,255)
    pixels.show()
    time.sleep(0.5)
```

1. WS2812彩灯的通信协议采用单线归零码的通讯方式，即一条信号线即可控制串联在一起的所有灯珠。可以将每一颗灯珠看作一个 8bit RGB 像素点，像素点在上电复位以后，DIN端（数据接收端）接受从控制器传输过来的数据，**首先送过来的24bit数据被第一个像素点提取后，送到像素点内部的数据锁存器，剩余的数据经过内部整形处理电路整形放大后通过DO端（数据发送端）开始转发输出给下一个级联的像素点，每经过一个像素点的传输，信号减少24bit。** WS2812彩灯采用自动整形转发技术，使得像素点的级联个数不受限制，仅受限于信号传输速度的要求。

## 使引脚输出高低电平，控制LED

1. `board.LED`控制着Bit-S2上的一颗单色LED发光二极管，高电平点亮，低电平熄灭，在REPL中输入以下代码：
```py
import board
import digitalio
ledpin = digitalio.DigitalInOut(board.LED)
ledpin.direction = digitalio.Direction.OUTPUT
ledpin.value = True
```

2. 或者这么做：
```py
import board
import digitalio
ledpin = digitalio.DigitalInOut(board.LED)
ledpin.switch_to_output(value=True) # value=1
```

3. 让LED间隔0.5秒闪烁：
```py
import board
import digitalio
import time
ledpin = digitalio.DigitalInOut(board.LED)
while True:
    ledpin.switch_to_output(value=1)
    time.sleep(0.5)
    ledpin.switch_to_output(value=0)
    time.sleep(0.5)

```
4. 在REPL中使用中断快捷键即可停止程序的运行。

5. 在REPL中输入`import board;help(board)`即可列出所有可控制的引脚。`board.GP25` 与 `board.LED`完全相同。

