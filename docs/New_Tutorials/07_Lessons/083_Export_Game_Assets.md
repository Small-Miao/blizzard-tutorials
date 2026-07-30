# 导出游戏资源

正如你可以通过导入器把自定义资源带入编辑器一样，标准资源也可以从项目中导出为常见文件类型。不同类型的资源需要使用略有区别的方法来提取。本文会介绍这些方法。

从编辑器中导出，常见用途之一是更仔细地查看标准资源。你可以把它们从编辑器中取出，放进其他软件里修改、重组，再重新导入使用。如果你遗失了项目中的某些源文件，了解导出方法也很有价值，因为你可以从基础项目中重新导出相关文件作为恢复手段。对于各类资源，也有一些常见文件格式值得了解，列表如下。

| 类型 | 文件格式 |
| ------------------ | ----------------------- |
| 纹理 | .dds |
| 模型 | .m3 |
| 音效 | .wav |
| UI 布局 | .SC2Layout, .xml |
| 触发器库 | .SC2Lib |
| 光照 | .SC2Lighting |
| 组件文件夹 | .SC2Components, Various |

## 打开归档浏览器

你可能已经在处理各种数据或触发器操作时用过归档浏览器查找文件。它也是从项目中导出内容的主要入口。若要将它用于导出，你需要直接打开浏览器，而不是在某个编辑提示框中调用它。你可以在编辑器任意位置通过 `窗口` ▶︎ `控制台` 打开它，如下图所示。

[![打开控制台](./resources/083_Export_Game_Assets1.png)](./resources/083_Export_Game_Assets1.png)
*打开控制台*

启动控制台后，输入命令 `browse`，然后点击“Enter”按钮，如下图所示。

![启动资源浏览器的命令](./resources/083_Export_Game_Assets2.png)
*启动资源浏览器的命令*

这会打开归档浏览器。你将看到当前项目中所有资源的结构化文件夹视图。

[![归档浏览器视图](./resources/083_Export_Game_Assets3.png)](./resources/083_Export_Game_Assets3.png)
*归档浏览器视图*

## 从归档浏览器导出

你可以在归档浏览器中右键某个资源并选择“导出文件”来导出它。

[![归档浏览器视图](./resources/083_Export_Game_Assets4.png)](./resources/083_Export_Game_Assets4.png)
*归档浏览器视图*

## 导出 UI 布局

你也可以通过归档浏览器导出 UI 布局。只需在 `UI` 文件夹中找到它们，选中后点击“导出文件”即可。

[![归档浏览器视图](./resources/083_Export_Game_Assets5.png)](./resources/083_Export_Game_Assets5.png)
*归档浏览器视图*

另一种做法是直接从 UI 编辑器中提取它们。找到对应的 `.SC2Layout` 文件，选中其中的 XML 数据，再按 `Ctrl+C` 复制。之后便可把数据粘贴到文本编辑器或纯文本文件中留作后用。

[![选中用于导出的 XML](./resources/083_Export_Game_Assets6.png)](./resources/083_Export_Game_Assets6.png)
*选中用于导出的 XML*

## 导出触发器库

你可以在触发编辑器中使用库面板导出触发器库。选中目标库，右键并选择“导出库”即可。

[![导出触发器库](./resources/083_Export_Game_Assets7.png)](./resources/083_Export_Game_Assets7.png)
*导出触发器库*

## 导出光照

你可以在光照窗口中导出光照文件。选中要导出的光照配置，然后右键选择“导出光照”。

[![导出光照](./resources/083_Export_Game_Assets8.png)](./resources/083_Export_Game_Assets8.png)
*导出光照*

## 导出组件文件夹

地图项目还可以通过导出流程拆分为组件文件夹。这种格式由脚本、组件列表、图像文件和原始数据组成，适合在编辑器外做诊断和分析。要导出它，请打开要保存为组件文件夹的地图，并前往 `文件` ▶︎ `另存为`。将“保存类型”字段改为 `.SC2Components`，然后点击“保存”。

[![另存为《星际争霸 II》组件文件夹](./resources/083_Export_Game_Assets9.png)](./resources/083_Export_Game_Assets9.png)
*另存为《星际争霸 II》组件文件夹*

完成后，会生成一个带有所选文件名、并附加 `.SC2Map` 后缀的文件夹。该文件夹中会包含一组固定结构的数据组件和资源，外观大致如下图所示。

[![《星际争霸 II》组件文件夹](./resources/083_Export_Game_Assets10.png)](./resources/083_Export_Game_Assets10.png)
*《星际争霸 II》组件文件夹*
