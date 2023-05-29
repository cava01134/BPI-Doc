## 显示jpg图片

<iframe src="//player.bilibili.com/player.html?aid=825151478&bvid=BV1Fg4y1M79B&cid=1102157032&page=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

sst7789驱动库内有一个显示jpg格式图片的方法，这对于初次上手学习的我们非常友好。

### jpg 方法

`jpg(jpg_filename, x, y)`

在给定的 x 和 y 坐标处绘制一个 JPG 文件，坐标为图片的左上角。

此方法需要额外的 3100 字节内存用于其工作缓冲区。

### 准备合适大小的jpg文件

任选自己喜欢的图片，裁切为长320像素，宽170像素，或小于此尺寸的图片。

图片编辑工具在各种智能终端设备中和各种操作系统中都有大量可选的，可任意使用自己喜欢的工具来编辑。

这里随意推荐一个能免费使用的 Web 在线图片编辑工具，[Pixlr X](https://pixlr.com/cn/x/) 。

将裁切好的图片放入我们本地的MicroPython工作文件夹中，重命名为 `pic_1.jpg` ，上传图片到MicroPython设备中的方法参考 [在终端中使用mpbridge](#在终端中使用mpbridge) 。

这里已准备一张已裁切好尺寸的图片。

![](../assets/images/pic_1.jpg)

### jpg 方法用例

在 main.py 脚本中使用 jpg 方法。

```py
""" BPI-Centi-S3 170x320 ST7789 display """

import st7789
import tft_config
import gc

def main():
    try:
        tft = tft_config.config(rotation=1)
        tft.init()
        tft.jpg("pic_1.jpg", 0, 0)
        tft.show()
        gc.collect()

    except BaseException as err:
        err_type = err.__class__.__name__
        print('Err type:', err_type)
        from sys import print_exception
        print_exception(err)

    finally:
        tft.deinit()
        print("tft deinit")


main()

```

上传 main.py 后，将设备复位，即可在屏幕上看到图片。

我们再多准备几个合适大小的jpg文件，即可设计一个循环，像播放幻灯片一样在BPI-Centi-S3的屏幕上轮播图片了。

![](../assets/images/pic_2.jpg)
![](../assets/images/pic_3.jpg)
![](../assets/images/pic_4.jpg)
![](../assets/images/pic_5.jpg)

```py
""" BPI-Centi-S3 170x320 ST7789 display """

import st7789
import tft_config
import gc
import time

pic_list = ["pic_1.jpg", "pic_2.jpg", "pic_3.jpg", "pic_4.jpg", "pic_5.jpg"]


def main():
    try:
        tft = tft_config.config(rotation=1)
        tft.init()
        while True:
            for pic in pic_list:
                tft.jpg(pic, 0, 0)
                tft.show()
                gc.collect()
                time.sleep(1)

    except BaseException as err:
        err_type = err.__class__.__name__
        print('Err type:', err_type)
        from sys import print_exception
        print_exception(err)

    finally:
        tft.deinit()
        print("tft deinit")


main()

```
