

# 二维码扫描

Web:AI 具备二维码扫描功能，能够通过开发板上的镜头侦测二维码，并可将二维码所记录的信息显示在屏幕上。

## 照相画面

「拍摄照片」积木可以使用镜头拍摄一次画面，配合「无限循环」积木就可以达成相机取景器的效果。

![](../../assets/images/upload_e5f0e28421cf299a02c56d80badc8485.png)

另外也可以使用「变量」积木替拍摄照片命名，通过命名来做出更多变化。

![](../../assets/images/upload_2630972dadc38129eb09e64e7d262658.png)

> 以上两种积木组合方式执行后会达到相同的结果，差别在于若是要做出更多应用变化，就需要搭配「变量」积木的命名。

### 画面上画文字

Web:AI 能够在屏幕画面或图片上显示文字，这时就需要搭配「图片上画文字」积木。

> 请特别注意，「图片上画文字」积木需要放在「LCD 显示图片」积木之前！

![](../../assets/images/upload_d4fc3f4eca1a16281c9aade6bb111715.png)

## 读取图片中的 QRcode(二维码)

「读取图片的 QRcode」积木能够读取图片上的 QRcode 信息，并通过「LCD 屏幕」积木显示出来。

![](../../assets/images/upload_fffaf7ffa609a3acfc3f5e733a88c047.png)


## 示例：QRcode 扫描

1. 使用「变量」积木将拍摄照片命名为「画面」。

    ![](../../assets/images/upload_2faf10807186f50299cd8d55f2d87f65.png)

2. 使用「图片上画文字」积木，填入「画面」及「读取图片的 QRcode」积木，代表在画面上显示 QRcode 的信息。

    ![](../../assets/images/upload_0c662efb60008d34d600fcfcc2e91587.png)

3. 放入「LCD 显示图片」积木，设定为「画面」。

    ![](../../assets/images/upload_247a402fccd1886ecd12846df20cd917.png)

4. 前面步骤完成后代表能够扫描一次 QRcode，为了能够不断扫描，在最外层放入「无限循环」积木。

    ![](../../assets/images/upload_bf87cd1a75b15fbf75bc21d972fb1b25.png)

5. 完成后按下执行，使用 Web:AI 的镜头扫描 QRcode，就可以看到屏幕显示 QRcode 信息了。

   ![](../../assets/images/upload_d393959c360f2933fa83169f9791d172.png)

    ![](../../assets/images/upload_dc36ceb0e810940ce6371095d09c5a09.png)