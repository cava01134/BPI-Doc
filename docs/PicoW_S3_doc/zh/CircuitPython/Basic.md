# 基础功能使用

## 编辑 code.py 使彩灯闪烁

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

