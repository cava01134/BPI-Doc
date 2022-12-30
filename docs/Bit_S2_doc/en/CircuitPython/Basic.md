# Basic function usage

## REPL simple to use

### Hello World!

1. Make sure that the development board is correctly connected in the Mu editor, refer to [Configuring the environment (Mu editor)](config_mu-editor.md).
2. The following information usually appears in the CircuitPython REPL window. The appearance of the `>>>` symbol means that we can start to enter commands to interact with it.
```
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\Auto-reload is on. Simply save files over USB to run them or enter REPL to disable.

Press any key to enter the REPL. Use CTRL-D to reload.
]0;🐍Wi-Fi: off | Done | 8.0.0-beta.0-49-g14fc4a079\]0;�Wi-Fi: off | REPL | 8.0.0-beta.0-49-g14fc4a079\
Adafruit CircuitPython 8.0.0-beta.0-49-g14fc4a079 on 2022-09-20; BPI-Bit-S2 with ESP32S3
>>>
```
3. Start typing the command on the right side of the `>>>` symbol, for example: `print("Hello World!")`.
> Note that using the English input method, Chinese characters cannot be recognized by the REPL.
```py
>>> print("Hello World!")
Hello World!
>>>
```

### REPL shortcut keys

1. Copy `ctrl + shift + c`.
2. Paste `ctrl + shift + v`.
    Use the left mouse button to drag and select the command to be copied in the REPL, press the copy shortcut key on the keyboard, and then press the paste shortcut key to copy and paste the command.
3. Soft reset `ctrl + d`.
4. Interrupt `ctrl + c`, interrupt the currently executing program, but will not restart and reset.

### View built-in modules

1. Entering `help("modules")` in the REPL will list all modules in the current CircuitPython development board.
2. After importing the module, you can use the `help()` function to view the function names or variable names available inside the module. For example, if you view the `board` module, you can see all the available pins and peripheral functions of the development board.
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
   LEDs -- board.BOOT0
   BUTTON_A -- board. BUTTON_A
   BUTTON_B -- board.BUTTON_B
   LUM1 -- board.LUM1
   LUM2 -- board.LUM2
   TEMPERATURE -- board. TEMPERATURE
   NEOPIXEL -- board.NEOPIXEL
   TX -- board.TX
   RX -- board.RX
   I2C -- <function>
   SPI -- <function>
   UART -- <function>
>>>
```

## Make a WS2812 color light blink

1. Click the **Load** button in the Mu editor, select the code.py file on the CircuitPython development board, and click **Open** to start editing code.py.

2. Enter the following code in the editor:

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
     pixels[0] = (255,0,0)
     pixels. show()
     time. sleep(0.5)
     pixels[0] = (0,255,0)
     pixels. show()
     time. sleep(0.5)
     pixels[0] = (0,0,255)
     pixels. show()
     time. sleep(0.5)
     pixels[0] = (255,255,255)
     pixels. show()
     time. sleep(0.5)
```

3. Click the **Save** button, and the edited content will be saved to the CircuitPython development board. If the code is correct, the first colored LED on the development board will flash red, green, blue and white in a cycle. Reset the development board or power it on again, and the program will start running again.
4. Use the interrupt shortcut key in the REPL to stop the program from running.
5. The code can also be directly copied and pasted into the REPL to run.

> All subsequent examples can be edited in the code.py file or copied and pasted into the REPL to run. However, after the program code in the code.py file is executed, the development board will return to the state when it is not running, and the state will not be retained, but the state will be retained when executed in the REPL.

## Use 25 WS2812 lanterns

1. Based on the code in the previous section **Make a WS2812 lantern blink**, use the for loop to light up 25 WS2812 lanterns in sequence.

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
     for i in range(25):
         pixels[i] = (255,0,0)
         pixels. show()
         time. sleep(0.1)
     for i in range(25):
         pixels[i] = (0,255,0)
         pixels. show()
         time. sleep(0.1)
     for i in range(25):
         pixels[i] = (0,0,255)
         pixels. show()
         time. sleep(0.1)
     for i in range(25):
         pixels[i] = (255,255,255)
         pixels. show()
         time. sleep(0.1)
```

2. If you want to control the colors of all lights at the same time, use `pixels.show()` to send the data to WS2812 lights after the for loop is over.

```python
import time
import board
import neopixel

pixels = neopixel.NeoPixel(board.NEOPIXEL, 25, brightness=0.1)

while 1:
     for i in range(25):
         pixels[i] = (255,0,0)
     pixels. show()
     time. sleep(0.5)
     for i in range(25):
         pixels[i] = (0,255,0)
     pixels. show()
     time. sleep(0.5)
     for i in range(25):
         pixels[i] = (0,0,255)
     pixels. show()
     time. sleep(0.5)
     for i in range(25):
         pixels[i] = (255,255,255)
     pixels. show()
     time. sleep(0.5)
```

1. The communication protocol of the WS2812 lantern adopts the single-line return-to-zero code communication method, that is, one signal line can control all the lamp beads connected in series. Each lamp bead can be regarded as an 8bit RGB pixel point. After the pixel point is powered on and reset, the DIN terminal (data receiving end) receives the data transmitted from the controller, and the 24bit data sent first is the first After each pixel point is extracted, it is sent to the data latch inside the pixel point, and the remaining data is reshaped and amplified by the internal shaping processing circuit, and then forwarded and output to the next cascaded pixel point through the DO terminal (data sending end). For the transmission of one pixel, the signal is reduced by 24 bits. ** WS2812 lanterns adopt automatic shaping and forwarding technology, so that the number of cascaded pixels is not limited, only limited by the requirements of signal transmission speed.

## Make the pin output high and low levels to control the LED

1. `board.LED` controls a single-color LED on Bit-S2. When it is high, it turns on, and when it is low, it turns off. Enter the following code in the REPL:
```py
import board
import digitalio
ledpin = digitalio.DigitalInOut(board.LED)
ledpin.direction = digitalio.Direction.OUTPUT
ledpin.value = True
```

2. Or do this:
```py
import
the board
import digitalio
ledpin = digitalio.DigitalInOut(board.LED)
ledpin.switch_to_output(value=True) # value=1
```

3. Let the LED blink at intervals of 0.5 seconds:
```py
import board
import digitalio
import time
ledpin = digitalio.DigitalInOut(board.LED)
while True:
     ledpin. switch_to_output(value=1)
     time. sleep(0.5)
     ledpin. switch_to_output(value=0)
     time. sleep(0.5)

```
4. Use the interrupt shortcut key in the REPL to stop the program from running.

5. Enter `import board;help(board)` in the REPL to list all controllable pins. `board.GP25` is exactly the same as `board.LED`.