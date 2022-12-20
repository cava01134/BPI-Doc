# 进阶多功能应用
## 实时动画显示双轴摇杆位置

```python
import time
import board
import busio
import analogio
import adafruit_ssd1306

def get_zero(times =500, sleep = 0.01):
    x_total = 0
    y_total = 0
    for i in range (times):
        x_axis = x_axis_pin.value
        y_axis = y_axis_pin.value
        x_total += x_axis
        y_total += y_axis
        time.sleep(sleep)
    x_zero = x_total // times
    y_zero = y_total // times
    return (x_zero,y_zero)

def get_extremum(times =500, sleep = 0.01):
    x_list = []
    y_list = []
    for i in range (times):
        x_axis = x_axis_pin.value
        y_axis = y_axis_pin.value
        x_list.append(x_axis)
        y_list.append(y_axis)
        time.sleep(sleep)
    x_extremum = (min(x_list),max(x_list))
    y_extremum = (min(y_list),max(y_list))
    return (x_extremum,y_extremum)

def get_spacing(level = 16 , zero =(32767,32767) ,x_extremum = (0,65535),y_extremum = (0,65535)):
    x_temp_1 = (zero[0] - x_extremum[0]) // level
    x_temp_2 = (x_extremum[1] - zero[0] ) // level
    y_temp_1 = (zero[1] - y_extremum[0]) // level
    y_temp_2 = (y_extremum[1] - zero[1] ) // level
    x_spacing = (x_temp_1,x_temp_2)
    y_spacing = (y_temp_1,y_temp_2)
    return (x_spacing,y_spacing)

def get_coordinates(zero = (32767,32767), x_spacing = (2048,2048),y_spacing = (2048,2048)):
    x_value = x_axis_pin.value - zero[0]
    y_value = y_axis_pin.value - zero[1]
    if x_value >= 0:
        x_axis = x_value // x_spacing[1]
    else:
        x_axis = - ((-x_value) // x_spacing[0])
    if y_value >= 0:
        y_axis = y_value // y_spacing[1]
    else:
        y_axis = - ((-y_value) // y_spacing[0])
    return (x_axis,y_axis)

i2c = busio.I2C(board.GP0, board.GP1)
display = adafruit_ssd1306.SSD1306_I2C(128, 64, i2c, addr=0x3C)
display.fill(0)
display.show()

x_axis_pin = analogio.AnalogIn(board.A0)
y_axis_pin = analogio.AnalogIn(board.A1)

display.text('Zero adjustment', 0, 28, 1, font_name='font5x8.bin', size=1)
display.show()
zero = get_zero(times =200, sleep = 0.01)
print(zero)
display.fill(0)
display.text('Extremum adjustment', 0, 28, 1, font_name='font5x8.bin', size=1)
display.show()
(x_extremum,y_extremum) = get_extremum(times = 200, sleep = 0.01)
print((x_extremum, y_extremum))
(x_spacing,y_spacing) = get_spacing(level = 32 , zero = zero, x_extremum = x_extremum,y_extremum = y_extremum)
print((x_spacing, y_spacing))
display.fill(0)
display.text('x=', 70, 16, 1, font_name='font5x8.bin', size=2)
display.text('y=', 70, 32, 1, font_name='font5x8.bin', size=2)
(x_axis,y_axis) = (0,0)
(x_axis_1,y_axis_1) = (0,0)
(x_axis_2,y_axis_2) = (0,0)
while True:
    (x_axis,y_axis) = get_coordinates(zero = zero, x_spacing = x_spacing, y_spacing = y_spacing)
    # print(x_axis,y_axis)
    if (x_axis,y_axis) == (x_axis_1,y_axis_1):
        pass
    else:
        display.text(str(x_axis_1), 90, 16, 0, font_name='font5x8.bin', size=2)
        display.text(str(y_axis_1), 90, 32, 0, font_name='font5x8.bin', size=2)
        display.fill_rect(x_axis_2-3, y_axis_2-3, 6, 6, 0)
        (x_axis_1,y_axis_1) = (x_axis,y_axis)
        (x_axis_2,y_axis_2) = (x_axis+32, -y_axis+32)
        display.fill_rect(x_axis_2-3, y_axis_2-3, 6, 6, 1)
        display.text(str(x_axis_1), 90, 16, 1, font_name='font5x8.bin', size=2)
        display.text(str(y_axis_1), 90, 32, 1, font_name='font5x8.bin', size=2)
        display.show()
```