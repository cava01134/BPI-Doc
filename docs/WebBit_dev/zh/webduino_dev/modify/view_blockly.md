# 查看范例积木

进入积木设计器，可以使用国内源 [Blockly Developer Tools](http://walkline.wang/blockly/blockfactory/) 如下图。

![](../../assets/webduino_dev/modify/images/blockly_tools.png)

使用该模板项目 [webduino-blockly-template](https://github.com/BPI-STEAM/webduino-blockly-template) ，并将它的代码通过 download 或 clone 得到它。

![](../../assets/webduino_dev/modify/images/demos_library.png)

将文件里的 demo/library.xml 导回积木设计器，参考已有积木，重新设计出属于你的积木。

![](../../assets/webduino_dev/modify/images/select_library.png)

可以看到有如下积木类型。

![](../../assets/webduino_dev/modify/images/blockly_list.png)

让我们点开一个 itpk_answer 看看都是如何定义的。

![](../../assets/webduino_dev/modify/images/itpk_answer.png)

可以看到，左侧是积木编辑工具，右侧则是生成的结果：

- 积木外观样式预览（Preview）
- 积木外观定义代码（Block Definition）
- 代码生成函数桩（Generator stub）

![](../../assets/webduino_dev/modify/images/blockly_result.png)

现在知道有这些东西就行，回头就会用上，至少知道这些代码都是使用该工具生成的。

> 欲读懂内部工作机制，请学习[HTML5](https://www.runoob.com/html/html5-intro.html)和[ECMAScript 6](https://www.runoob.com/w3cnote/es6-concise-tutorial.html)。