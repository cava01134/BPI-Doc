# 基础功能使用

>[GitHub BPI-Leaf-S3 例程](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/tree/main/Example/MicroPython-zh/02.Use_Peripherals)

## 点亮板载LED灯珠

在完成[MicroPython 运行环境设置](Environment.md)后，可以立即尝试编程。

新建一个 main.py 脚本文件，在其中输入以下代码：

```py
from machine import Pin
from neopixel import NeoPixel
import time 
pin_48 = Pin(48) 
np = NeoPixel(pin_48, 1,bpp=3, timing=1)
while True:
    np[0] = (25,25,25)
    np.write()
    time.sleep_ms(250)
    np[0] = (0,0,0)
    np.write()
    time.sleep_ms(250)
```

保存文件到MicroPython设备中，点击“Run”运行按钮，即可让板载彩色LED灯珠闪烁。

修改 `np[0] = (25,25,25)`等号右侧元组内的数据，可以改变颜色，分别对应R，G，B三色亮度等级，设定范围是0-255，建议使用范围0-25，亮度过高时请勿长时间直视！

[neopixel — control of WS2812 / NeoPixel LEDs — MicroPython 文档](https://docs.micropython.org/en/latest/library/neopixel.html)

## 使彩灯循环显示九种颜色

```py
from machine import Pin
from neopixel import NeoPixel
import time

pin_48 = Pin(48, Pin.OUT)
np = NeoPixel(pin_48, 1,bpp=3, timing=1)

RED = (255, 0, 0)
ORANGE = (255, 100, 0)
YELLOW = (255, 255, 0)
GREEN = (0, 255, 0)
CYAN = (0, 255, 255)
BLUE = (0, 0, 255)
PURPLE = (180, 0, 255)
WHITE = (255, 255, 255)
OFF = (0, 0, 0)

color_list = [RED,ORANGE,YELLOW,GREEN,CYAN,BLUE,PURPLE,WHITE,OFF]
brightness = 0.1

while True:
    for i in color_list:
        color = (round(i[0]*brightness),round(i[1]*brightness),round(i[2]*brightness))
        np[0] = color
        np.write()
        time.sleep(1)

```

## 全彩LED灯珠循环显示彩虹色

基于上一节，我们可以更进一步，编写循环自动改变灯珠颜色。

```py
from machine import Pin
from neopixel import NeoPixel
import time

def rainbow(num=1,level=25,delay=100):
    def write_all(num,delay,red,green,blue):
        for j in range (num):
            np[j] = (red,green,blue)
        np.write()
        time.sleep_ms(delay)
    
    red,green,blue = level,0,0
    
    rainbow_step_list2 = [(0,1,0),(-1,0,0),(0,0,1),(0,-1,0),(1,0,0),(0,0,-1)]
    
    for step in rainbow_step_list2:
        for i in range (level):
            red+=step[0]
            green+=step[1]
            blue+=step[2]
            write_all(num,delay,red,green,blue)
            

pin_48 = Pin(48, Pin.OUT)
np = NeoPixel(pin_48, 1,bpp=3, timing=1)

while True:
    rainbow(num=1,level=25,delay=10)

```
此例程可适用于任意长度的ws2812灯带。

修改 `NeoPixel(pin_48, 1,bpp=3, timing=1)` 中第一个参数至任意想要串接灯带的GPIO管脚，修改其第二个参数为灯带上对应灯珠的数量。

修改 `rainbow(num=1,level=25,delay=100)` 中的num参数为灯带上对应灯珠的数量。

当然我们也可以根据自己的想法使用for循环或while循环制作自己想要的颜色变化规律。

## 设计按键中断程序,控制彩灯

BPI-Leaf-S3 有两颗按键，BOOT 与 RST，RST控制芯片硬件复位，而BOOT则与GPIO0相连，其电路如下图所示。

![](../assets/images/bpi-leaf-s3_boot_sch.png)

可见当开发板正常通电工作时，GPIO0在BOOT按键未按下时，串联一颗电阻接到3.3v，此时为高电位；当BOOT按键按下时，GPIO0将直接接地，此时则为低电位。ESP32-S3芯片通过检测此GPIO管脚的电位即可判断按钮是否被按下。

[MicroPython GPIO中断程序 machine.Pin.irq 文档](https://docs.micropython.org/en/latest/library/machine.Pin.html#machine.Pin.irq)

在程序中，通过检测 GPIO中断的触发方式，即可设计一套记录按键被按压的次数的中断程序，用判断当前已经按压的次数来控制彩灯的颜色。

<iframe src="//player.bilibili.com/player.html?aid=345819290&bvid=BV1Nd4y1M7oW&cid=841776119&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

```python
from machine import Pin
from neopixel import NeoPixel
from array import array
import time
import micropython

micropython.alloc_emergency_exception_buf(100)

p_48 = Pin(48, Pin.OUT)
np = NeoPixel(p_48, 1,bpp=3, timing=1)

p0 = Pin(0,Pin.IN,Pin.PULL_UP)
trig_locks = array('B',[0])
trig_timeticks_list = array('L',[0,0])
count = array('L',[0])

def p0_irq(pin):
    if pin.value()==0 and trig_locks[0]==0:
        trig_timeticks_list[0]=time.ticks_ms()
        trig_locks[0]=1
    elif pin.value()==1 and trig_locks[0]==1:
        trig_timeticks_list[1]=time.ticks_diff(time.ticks_ms(),trig_timeticks_list[0])
        trig_locks[0]=0
        if trig_timeticks_list[1] >= 20:
            count[0] = count[0] + 1
            if count[0] > 8:
                count[0] = 0

p0.irq(handler=p0_irq,trigger= Pin.IRQ_FALLING | Pin.IRQ_RISING )

RED = (255, 0, 0)
ORANGE = (255, 100, 0)
YELLOW = (255, 255, 0)
GREEN = (0, 255, 0)
CYAN = (0, 255, 255)
BLUE = (0, 0, 255)
PURPLE = (180, 0, 255)
WHITE = (255, 255, 255)
OFF = (0, 0, 0)

color_list = [RED,ORANGE,YELLOW,GREEN,CYAN,BLUE,PURPLE,WHITE,OFF]
brightness = 0.1

while True:
    print (count)
    i = color_list[count[0]]
    color = (round(i[0]*brightness),round(i[1]*brightness),round(i[2]*brightness))
    np[0] = color
    np.write()
    time.sleep(0.1)
```

