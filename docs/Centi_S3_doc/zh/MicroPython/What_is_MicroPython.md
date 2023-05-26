## 什么是 MicroPython？

![](../assets/images/Mircopython.png)

[MicroPython](https://micropython.org/)是Python 3编程语言的精益高效实现，包括 Python 标准库的一小部分，并且经过优化，可在微控制器和有限的硬件资源中运行。

由 [Damien P. George](https://dpgeorge.net/) 于 2013年 众筹开源。

它与使用C程序开发微控制器最明显的差异性，就是验证代码时无需漫长的编译。

使用串口通信软件，通过REPL(read-eval-print-loop)输入命令来控制微控制器，和Python的REPL一样。

也可使用一些工具将 python 脚本文件上传到微控制器内运行。

它对Python 3 的实现，包括了支持多线程的 _thread 库，编写并发代码的 asyncio 库。

它尽可能与普通Python兼容，允许您轻松地将代码从桌面端移植到微控制器。

同时它还具备一些特定用于微控制器的库，以便充分利用微控制器芯片内的硬件功能，例如定时器，硬件中断，WiFi等，这取决于具体的硬件。

在具备上述特性的同时，它的硬件开销很少，最低只需 256k 的代码空间和 16k 的 RAM 即可运行。

如果你了解Python，很大程度上你就已经了解MicroPython了。

在另一方面，你深入地学习MicroPython，也能提升你对Python的理解。
