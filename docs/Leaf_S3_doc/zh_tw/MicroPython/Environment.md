# Micropython 運行環境設置

Micropython的運行環境依賴於Python，所以我們在使用之前需要先安裝Python。我們這裡使用的 IDE 是 Thonny。

## 安裝Python環境

打開 [Python 官網頁面](https://www.python.org/).

對於Windows系統，下載安裝包最方便的方式是在官網首頁點擊下圖所示圖標進行下載。

![](../assets/images/Micropython_operating_env_1.png)

可以在“下載”選項卡中選擇其他操作系統或其他發行版。

建議使用 python 3.7 或更高版本。

一定要記得在開始安裝的時候勾選Add Python 3.x to PATH，這樣就可以避免手動添加到PATH中。

![](../assets/images/Micropython_operating_env_2.png)

按照安裝說明一步一步順利完成安裝。

## 安裝 Thonny IDE

以Windows PowerShell的具體操作步驟為例。

其他系統或安裝方法請參考[Thonny官網](https://thonny.org/)的說明。

右鍵單擊 Windows 開始菜單以查看 Windows PowerShell，單擊打開。

![](../assets/images/Micropython_operating_env_3.png)

我們在這里通過 pip 安裝 Thonny IDE。

Pip 是一個 Python 包管理工具。首先確認pip是不是最新版本。使用以下命令直接升級pip：

```外殼
pip 安裝 -U pip
```

使用以下命令安裝 Thonny：

```外殼
pip 安裝 thonnyapp
```

如果將來需要，可以使用以下命令升級 Thonny：

```外殼
pip 安裝 -U thonnyapp
```

可以使用 Windows 搜索或“開始”菜單欄快速找到 Thonny。

![](../assets/images/Micropython_operating_env_4.png)

## 連接開發板與電腦

通過USB數據線將開發板連接到電腦。

正確連接後板上的電源指示燈會亮起。

我們需要知道開發板是否被電腦識別，並找出連接到哪個COM口（用於串口通信，下載程序等）。

首先在桌面找到“這台電腦”，右擊，選擇“管理”，打開“設備管理器”，點擊“端口（COM和LPT）”。

一個新的 COM 端口將添加到列表中（示例圖像中的 COM21）。

![](../assets/images/Micropython_operating_env_5.png)

## 燒寫 MicroPython 固件

Leaf-S3開發板默認出廠固件為MicroPython。如果需要燒錄固件，可以參考[Micropython固件下載與燒錄](Firmware.md)。

## 配置 Thonny IDE

打開Thonny，點擊Run，點擊選擇一個解釋器：

![](../assets/images/Micropython_operating_env_9.png)

將解釋器設置為 MicroPython (ESP32)：

![](../assets/images/Micropython_operating_env_10.png)

選擇開發板的COM口：

![](../assets/images/Micropython_operating_env_11.png)

確認設置後，在shell中打開MicroPython REPL。

![](../assets/images/Micropython_operating_env_12.png)

REPL啟動並輸出信息，表示MicroPython固件燒錄成功，可以正常使用。

點擊View，勾選File，可以看到本地文件目錄和開發板上的文件目錄：

![](../assets/images/Micropython_operating_env_13.png)

![](../assets/images/Micropython_operating_env_14.png)

也可以根據需要使用其他視圖窗口。

您可以在設置中選擇自己喜歡的主題風格。

![](../assets/images/Micropython_operating_env_15.png)