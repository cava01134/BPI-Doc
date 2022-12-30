# How to update CircuitPython firmware
> This method is applicable to development boards that already have tinyUF2 firmware. If the flash of the development board is erased or fails to enter UF2 mode, please refer to [How to burn tinyUF2 firmware](flash_tinyuf2.md).
1. Enter the [BPI-PicoW-S3 CircuitPython Download](https://circuitpython.org/board/bpi_picow_s3/) page.
2. Click the DOWNLOAD UF2 NOW button to download the latest released `.uf2` firmware.
3. Connect the development board to the computer via USB. If the CircuitPython firmware has been installed, a disk named `CIRCUITPY` will appear on the computer file management page. This is a disk in CircuitPython mode, and it needs to be changed to a disk in UF2 mode. The following are the specific steps.
    1. Quickly press the `RST` key once.
    2. Quickly press the `BOOT` button once when the first colored light turns purple.

4. The name of the disk in UF2 mode is `BITS2BOOT`, copy the `.uf2` firmware downloaded in step 1 to this disk, and the colored light will flash orange during the process. Do not disconnect or do anything to the board during this process.
5. After the CircuitPython firmware update is completed, it will automatically reset, and a `CIRCUITPY` disk will reappear on the computer file management page, and the specific firmware version can be viewed through the REPL.