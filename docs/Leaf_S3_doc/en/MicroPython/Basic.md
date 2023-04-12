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
## PWM Single Color LED Breathing Light

### External Hardware Requirements

A LED light that can work on 3.3v.

### connection method

The GPIO13 pin is used in the routine, and the positive pole of the LED light is connected to the GPIO13 pin, and the negative pole is connected to GND.

### Code

```py
from machine import Pin, PWM
import time

PWM_LED = PWM(Pin(13))
PWM_LED.freq(1000)
PWM_LED. duty(0)

while True:
     for i in range(0,1024,1):
        PWM_LED. duty(i)
        time. sleep_ms(2)
     for i in range(1022,0,-1):
        PWM_LED. duty(i)
        time. sleep_ms(1)
    
```

# TB6612FNG module PWM drive motor

## External hardware requirements

A TB6612FNG module, a 3.3~5V DC motor.

## connection method

TB6612FNG | BPI-Leaf-S3
:---:|:---:
PWMA | 11
AIN2 | 13
AIN1 | 12
STBY | 10
VM | 5V
VCC | 3.3V
GND | GND
AO1 | Motor N pole
AO2 | Motor S pole

> The connection sequence between AO1/AO2 and the motor can be exchanged arbitrarily to change the direction of rotation.

## running result

The motor will start to rotate in one direction and gradually accelerate to the maximum speed achievable by the current current within 7 seconds, then gradually decelerate to stop within 5 seconds, then reverse the rotation and repeat the process.

<iframe width="560" height="315" src="https://www.youtube.com/embed/3WXCZ1BsPNY?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Code

```py
from machine import Pin,PWM
import time

PWM_A = PWM(Pin(11)) #Set PWM output pin
PWM_A.freq(20000) #Set PWM frequency
PWM_A. duty(0) #Set PWM duty cycle
AIN1 = Pin(12,Pin.OUT)
AIN2 = Pin(13,Pin.OUT)
STBY = Pin(10,Pin.OUT)
STBY.on() #When STBY pin is at high level, TB6612FNG starts.

def MOTOR_Forward():
     AIN1.on()
     AIN2.off()
def MOTOR_Reverse():
     AIN1.off()
     AIN2.on()

while True:
     MOTOR_Forward()
     #for cycle is used to control the PWM duty cycle change.
     #The PWM duty cycle control precision is 10bit, ie 0~1023.
     #Some motors require a certain PWM duty cycle to start.
     for i in range(350,1024,1):
        PWM_A. duty(i)
        time. sleep_ms(10)
     for i in range(1022,0,-1):
        PWM_A. duty(i)
        time. sleep_ms(5)
    
     MOTOR_Reverse()
     for i in range(350,1024,1):
        PWM_A. duty(i)
        time. sleep_ms(10)
     for i in range(1022,0,-1):
        PWM_A. duty(i)
        time. sleep_ms(5)

```
## Use ADC to detect potentiometer voltage

### External Hardware Requirements

a potentiometer.

![](https://i.imgur.com/mnuHlMR.jpg)

### ESP32-S3 ADC

The ESP32-S3 chip integrates two **ADC analog-to-digital converters**, the measurement range is 0mV-3100mV, and the resolution is 12bit, that is, 0mV-3100mV is divided into 2^12=4096 levels, and each level is a digital quantity .

The two ADC analog-to-digital converters each have 10 measurement channels, ADC1 is GPIO1 ~ 10, and ADC2 is GPIO11 ~ 20.

### connection method

GND is connected to GND, VCC is connected to 3V3, the S output terminal is connected to GPIO11 pin, and channel 1 of ADC2 is used for measurement.

GPIO1~20 pins can be used as ADC input pins.

### Code

```py
from machine import Pin,ADC
import time
adc11 = ADC(Pin(11),atten=ADC.ATTN_11DB)
#adc11 = ADC(Pin(11))
#adc11.atten(ADC.ATTN_11DB)
while True:
     read=adc11.read()
     read_u16 = adc11.read_u16()
     read_uv = adc11.read_uv()
     print("read={0},read_u16={1},read_uv={2}".format(read,read_u16,read_uv))
     time. sleep_ms(100)
```

| Attenuation value | Measurable input voltage range |
| -------- | -------- |
| ADC.ATTN_0DB | 0 mV ~ 950 mV |
| ADC.ATTN_2_5DB | 0 mV ~ 1250 mV |
| ADC.ATTN_6DB | 0 mV ~ 1750 mV |
| ADC.ATTN_11DB | 0 mV ~ 3100 mV |

  1. `ADC(*, atten)` initializes the ADC channel of a GPIO pin, and you can choose to use `atten` to set the attenuation value, which controls the measurable input voltage range of the chip. If not set, it will be the default value `atten=ADC.ATTN_0DB` or the value set last time.
  2. You can modify the attenuation value through `ADC.atten()` after initializing an ADC channel.
  3. `ADC.read()` reads the ADC and returns the read result. The ADC of the ESP32-S3 chip returns data with 12-bit precision.
  4. `ADC.read_u16()` reads the ADC and returns 16-bit data.
  5. `ADC.read_uv()` returns the calibrated input voltage in `uV` microvolts according to the characteristics of the ADC. Return values only have `mV` millivolt resolution (ie, will always be in multiples of 1000 microvolts).

The WiFi function also uses ADC2, so trying to read analog values from ADC2's measurement channels GPIO11 ~ 20 while WiFi is active will throw an exception.

It is recommended to use `ADC.read_uv()` to read the voltage value, which is a decimal constant returned after calibration according to the characteristics of the ADC analog-to-digital converter, which is more accurate than the other two reading methods. At the same time, it is also recommended to directly divide the operation when using: `ADC.read_uv()//1000` to get the data of `mV` millivolt resolution.

Directly print out `ADC.read()` or `ADC.read_u16()` to get the decimal value, you can use the `hex()` function to convert the data type into hexadecimal, for example `hex(ADC.read() )`, or use the `bin()` function to convert the data type to binary.

## Use the potentiometer to steplessly adjust the brightness of the colored lights

Building on the [Cycle Through Nine Colors] (#Cycle Through Nine Colors) section, you can use a potentiometer to control the brightness of the lights.

<iframe width="560" height="315" src="https://www.youtube.com/embed/d3tm8aYNCx8?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Code
```py
from machine import Pin,ADC
from neopixel import NeoPixel
from array import array
import time
import micropython

adc1 = ADC(Pin(1),atten=ADC.ATTN_11DB)

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

while True:
     adc1_read = adc1.read() # 12bit
     adc1_read_mv = adc1.read_uv()/1000
     adc1_read_u16 = adc1.read_u16() # 16bit
     brightness = adc1_read/4095
     i = color_list[count[0]]
     color = (round(i[0]*brightness),round(i[1]*brightness),round(i[2]*brightness))
     np[0] = color
     np. write()
     print(adc1_read,adc1_read_u16,adc1_read_mv,"mv",count[0],color)
     time. sleep(0.1)
```

## Use the ADC to measure the potentiometer to adjust the motor speed

### External Hardware Requirements

* Potentiometer x 1
* TB6612FNG motor driver module x 1
* 5v DC motor x 1
* some connecting wires

### connection method

|Potentiometer|BPI-Leaf-S3|
|---|---|
|GND|GND|
|VCC|3V3|
|S|14|

|TB6612FNG|BPI-Leaf-S3|
|---|---|
|PWMA|11|
|AIN2|13|
|AIN1|12|
|STBY|10|
|VM|5V|
|VCC|3.3V|
|GND|GND|

|TB6612FNG|Motor|
|---|---|
|AO1|Motor N pole|
|AO2|Motor S pole|

### running result

The development board will output the voltage value read by the ADC in the REPL at an interval of 100ms, in mv, and the corresponding controlled PWM duty cycle.

Adjust the potentiometer by hand to change its output voltage. The higher the voltage, the higher the PWM duty cycle output by the development board, and the faster the motor speed.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2_UeeeOBJwo?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Code
```py
from machine import Pin,ADC,PWM
import time

adc14 = ADC(Pin(14),atten=ADC.ATTN_11DB)

PWM_A = PWM(Pin(11)) #Set PWM output pin
PWM_A.freq(20000) #Set PWM frequency
PWM_A. duty(0) #Set PWM duty cycle
AIN1 = Pin(12,Pin.OUT)
AIN2 = Pin(13,Pin.OUT)
STBY = Pin(10,Pin.OUT)
AIN1.on() #MOTOR forward
AIN2.off()
STBY.on() #When STBY pin is at high level, TB6612FNG starts.

while True:
     read_mv=adc14.read_uv()//1000
     if read_mv <= 3000:
         duty_set = int(1023/3000 * read_mv)
     else:
         duty_set = 1023
     PWM_A. duty(duty_set)
     Duty_cycle = int(duty_set/1023*100)
     print("ADC_read={0}mv,Duty_cycle={1}%".format(read_mv,Duty_cycle))
     time. sleep_ms(100)

```
