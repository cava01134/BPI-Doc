# 基础功能使用

## REPL简单使用

###  Hello World!

1. 确保已在Mu编辑器中正确连接开发板，参考[配置使用环境(Mu编辑器)](config_mu-editor.md)。
2. 在CircuitPython REPL窗口中通常会出现如下信息，`>>>`符号的出现即代表我们可以开始在其后输入命令与其交互了。
```
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\Auto-reload is on. Simply save files over USB to run them or enter REPL to disable.

Press any key to enter the REPL. Use CTRL-D to reload.
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\]0;�Wi-Fi: off | REPL | 8.0.0-beta.0-49-g14fc4a079\
Adafruit CircuitPython 8.0.0-beta.0-49-g14fc4a079 on 2022-09-20; BPI-PicoW-S3 with ESP32S3
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
  board_id -- bpi_picow_s3
  GP0 -- board.GP0
  GP1 -- board.GP1
  GP2 -- board.GP2
  GP3 -- board.GP3
  GP4 -- board.GP4
  GP5 -- board.GP5
  GP6 -- board.GP6
  GP7 -- board.GP7
  GP8 -- board.GP8
  GP9 -- board.GP9
  GP10 -- board.GP10
  GP11 -- board.GP11
  GP12 -- board.GP12
  GP13 -- board.GP13
  GP14 -- board.GP14
  GP15 -- board.GP15
  GP16 -- board.GP16
  GP17 -- board.GP17
  GP18 -- board.GP18
  GP19 -- board.GP19
  GP20 -- board.GP20
  GP21 -- board.GP21
  GP22 -- board.GP22
  GP25 -- board.GP25
  LED -- board.GP25
  GP26 -- board.GP26
  GP26_A0 -- board.GP26
  A0 -- board.GP26
  GP27 -- board.GP27
  GP27_A1 -- board.GP27
  A1 -- board.GP27
  GP28 -- board.GP28
  GP28_A2 -- board.GP28
  A2 -- board.GP28
  GP29 -- board.GP29
  GP29_A3 -- board.GP29
  A3 -- board.GP29
  NEOPIXEL -- board.NEOPIXEL
  TX -- board.GP0
  RX -- board.GP1
  BOOT0 -- board.BOOT0
  UART -- <function>
>>> 
```

## 输出

### 使WS2812彩灯闪烁

1. 在Mu编辑器中点击**Load**按钮，选择CircuitPython开发板上的 code.py 文件，点击 **打开**，即可开始编辑 code.py 。

2. 在编辑器中输入如下代码：

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 1, brightness=0.1)

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

3. 点击**Save**按钮，编辑的内容将保存到CircuitPython开发板，代码无误的情况下，开发板上的彩色LED将循环闪烁 红绿蓝白。
4. 在REPL中使用中断快捷键即可停止程序的运行。
5. 代码也可直接复制粘贴到REPL中运行。

> 后续所有示例都可如此编辑code.py文件或复制粘贴到REPL中运行。但在code.py文件中的程序代码执行完毕后，开发板会恢复未运行时的状态，不会保留状态，但在REPL中执行则会保留状态。

### 使引脚输出高低电平，控制LED

1. `board.LED`控制着PicoW-S3上的一颗单色LED发光二极管，高电平点亮，低电平熄灭，在REPL中输入以下代码：
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

### PWM输出，控制LED亮度

1. 可通过控制PWM占空比来控制LED灯亮度，控制占空比从0%~100%，采用16位精度，十进制为 0~65535 ，16进制为 0~FFFF 。在REPL中输入以下代码：
```py
import board
import pwmio
ledpin = pwmio.PWMOut(board.LED, frequency=25000, duty_cycle=0)
ledpin.duty_cycle = 32768  # mid-point 0-65535 = 50 % duty-cycle
```
2. 仅需在REPL中再次输入最后一行代码即可改变PWM占空比，使LED到最大亮度：
```py
ledpin.duty_cycle = 65535
```
3. 呼吸灯：
```py
import board
import pwmio
import time

ledpin = pwmio.PWMOut(board.LED, frequency=25000, duty_cycle=0)

while True:
    for i in range(0, 65535, 1):
        ledpin.duty_cycle = i
    for i in range(65535, 0, -1):
        ledpin.duty_cycle = i
```
