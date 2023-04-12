# 基礎功能使用

>[GitHub BPI-Leaf-S3 例程](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/tree/main/Example/MicroPython-zh/02.Use_Peripherals)

## 點亮板載LED燈珠

在完成[MicroPython 運行環境設置](Environment.md)後，可以立即嘗試編程。

新建一個 main.py 腳本文件，在其中輸入以下代碼：

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

保存文件到MicroPython設備中，點擊“Run”運行按鈕，即可讓板載彩色LED燈珠閃爍。

修改 `np[0] = (25,25,25)`等號右側元組內的數據，可以改變顏色，分別對應R，G，B三色亮度等級，設定範圍是0-255，建議使用範圍0-25，亮度過高時請勿長時間直視！

[neopixel — control of WS2812 / NeoPixel LEDs — MicroPython 文檔](https://docs.micropython.org/en/latest/library/neopixel.html)

## 使彩燈循環顯示九種顏色

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

## 全彩LED燈珠循環顯示彩虹色

基於上一節，我們可以更進一步，編寫循環自動改變燈珠顏色。

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
            

np = NeoPixel(Pin(48, Pin.OUT), 1,bpp=3, timing=1)

while True:
    rainbow(num=1,level=25,delay=10)

```
此例程可適用於任意長度的ws2812燈帶。

修改 `NeoPixel(Pin(48, Pin.OUT), 1,bpp=3, timing=1)` 中第一個參數至任意想要串接燈帶的GPIO管腳。

修改 `rainbow(num=1,level=25,delay=100)` 中的num參數為燈帶上對應燈珠的數量。

當然我們也可以根據自己的想法使用for循環或while循環製作自己想要的顏色變化規律。

## 設計按鍵中斷程序,控制彩燈

BPI-Leaf-S3 有兩顆按鍵，BOOT 與 RST，RST控制芯片硬件復位，而BOOT則與GPIO0相連，其電路如下圖所示。

![](../assets/images/bpi-leaf-s3_boot_sch.png)

可見當開發板正常通電工作時，GPIO0在BOOT按鍵未按下時，串聯一顆電阻接到3.3v，此時為高電位；當BOOT按鍵按下時，GPIO0將直接接地，此時則為低電位。 ESP32-S3芯片通過檢測此GPIO管腳的電位即可判斷按鈕是否被按下。

[MicroPython GPIO中斷程序 machine.Pin.irq 文檔](https://docs.micropython.org/en/latest/library/machine.Pin.html#machine.Pin.irq)

在程序中，通過檢測 GPIO中斷的觸發方式，即可設計一套記錄按鍵被按壓的次數的中斷程序，用判斷當前已經按壓的次數來控制彩燈的顏色。

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