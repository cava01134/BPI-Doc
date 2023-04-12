# Basic use case

>[GitHub BPI-Leaf-S3 routines](https://github.com/BPI-STEAM/BPI-Leaf-S3-Doc/tree/main/Example/MicroPython-zh/02.Use_Peripherals)

## Light up the onboard LED lamp bead

After completing [MicroPython runtime environment settings](Environment.md), you can try programming immediately.

Create a new main.py script file and enter the following code in it:

```py
from machine import Pin
from neopixel import NeoPixel
import time
pin_48 = Pin(48)
np = NeoPixel(pin_48, 1, bpp=3, timing=1)
while True:
     np[0] = (25,25,25)
     np. write()
     time. sleep_ms(250)
     np[0] = (0,0,0)
     np. write()
     time. sleep_ms(250)
```

Save the file to the MicroPython device, click the "Run" button to make the onboard colored LED flash.

Modify the data in the tuple on the right side of `np[0] = (25,25,25)` to change the color, which corresponds to the brightness levels of R, G, and B respectively. The setting range is 0-255, and the recommended range is 0-25. When the brightness is too high, please do not look directly at it for a long time!

[neopixel — control of WS2812 / NeoPixel LEDs — MicroPython documentation](https://docs.micropython.org/en/latest/library/neopixel.html)

## Make the lights cycle through nine colors

```py
from machine import Pin
from neopixel import NeoPixel
import time

pin_48 = Pin(48, Pin.OUT)
np = NeoPixel(pin_48, 1, bpp=3, timing=1)

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
         np. write()
         time. sleep(1)

```

## Full-color LED lamp beads cycle display rainbow colors

Based on the previous section, we can go one step further and write a loop to automatically change the color of the lamp bead.

```py
from machine import Pin
from neopixel import NeoPixel
import time

def rainbow(num=1,level=25,delay=100):
     def write_all(num,delay,red,green,blue):
         for j in range (num):
             np[j] = (red,green,blue)
         np. write()
         time. sleep_ms(delay)
    
     red,green,blue = level,0,0
    
     rainbow_step_list2 = [(0,1,0),(-1,0,0),(0,0,1),(0,-1,0),(1,0,0),(0,0, -1)]
    
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
This routine can be applied to ws2812 light strips of any length.

Modify the first parameter in `NeoPixel(Pin(48, Pin.OUT), 1,bpp=3, timing=1)` to any GPIO pin that you want to connect the light strips in series.

Modify the num parameter in `rainbow(num=1,level=25,delay=100)` to be the number of corresponding lamp beads on the light strip.

Of course, we can also use for loop or while loop to make the color change pattern we want according to our own ideas.

## Design button interrupt program to control colored lights

BPI-Leaf-S3 has two buttons, BOOT and RST, RST controls the hardware reset of the chip, and BOOT is connected to GPIO0, the circuit is shown in the figure below.

![](../assets/images/bpi-leaf-s3_boot_sch.png)

It can be seen that when the development board is powered on and working normally, when the BOOT button is not pressed, GPIO0 is connected to 3.3v with a resistor in series, and it is at a high potential at this time. When the BOOT button is pressed, GPIO0 will be directly grounded, and at this time it is a low potential. The ESP32-S3 chip can determine whether the button is pressed by detecting the potential of this GPIO pin.

[MicroPython GPIO interrupt program machine.Pin.irq documentation](https://docs.micropython.org/en/latest/library/machine.Pin.html#machine.Pin.irq)

In the program, by detecting the trigger mode of the GPIO interrupt, you can design a set of interrupt program that records the number of times the button is pressed, and control the color of the colored light by judging the number of times that the button has been pressed.

<iframe width="560" height="315" src="https://www.youtube.com/embed/PQ2x4PayFPc?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

```python
from machine import Pin
from neopixel import NeoPixel
from array import array
import time
import micropython

micropython.alloc_emergency_exception_buf(100)

p_48 = Pin(48, Pin.OUT)
np = NeoPixel(p_48, 1, bpp=3, timing=1)

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

p0.irq(handler=p0_irq, trigger= Pin.IRQ_FALLING | Pin.IRQ_RISING )

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
     np. write()
     time. sleep(0.1)
```