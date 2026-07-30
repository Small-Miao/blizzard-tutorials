# 点击响应行为

点击响应行为会在玩家点击其宿主单位时触发一个效果。这是一种相当冷门的行为类型，在《星际争霸 II》的数据依赖项中几乎只出现过一次，就是每张地图地表风格里的本地野生生物，也就是小动物。如果你之前没注意过，《星际争霸》以及整个 --Craft 系列里的小动物，在被反复点击后都会表现出某种特殊反应。如果你还没体验过，现在就去试试，也绝对不算浪费时间。

[![点击一只小动物 30 次](./resources/070_Click_Response_Behavior1.png)](./resources/070_Click_Response_Behavior1.png)
*点击一只小动物 30 次*

虽然它缺少传统用途，但点击响应行为展现了玩家输入与数据效果之间一种很有意思的直接关系，值得研究。

## 演示点击响应行为

打开本文提供的演示地图。你会看到一个小型虫族基地，外面飘着一群异龙，如下图所示。

[![演示地图场景](./resources/070_Click_Response_Behavior2.png)](./resources/070_Click_Response_Behavior2.png)
*演示地图场景*

这个场景已经配置为允许玩家以免费且加速的方式从孵化场中生成异龙。你将利用这一点，添加一个自定义点击响应行为，使异龙在连续被点击若干次后自毁。

首先进入数据编辑器并切换到 行为 标签页。如果该标签页尚未打开，可通过 `+ ▶︎ Edit Game Data ▶︎ 行为` 打开。然后在行为主窗口中右键，选择 `Add Behavior`。如下图所示。

[![创建行为](./resources/070_Click_Response_Behavior3.png)](./resources/070_Click_Response_Behavior3.png)
*创建行为*

在行为创建窗口中，将名称填写为 `Mutalisk Destroy`，然后点击 `Suggest` 生成 ID。接着把 `Behavior Type` 设为 `Click Response`。完成后的创建窗口应如下所示。

![点击响应行为创建](./resources/070_Click_Response_Behavior4.png)
*点击响应行为创建*

点击 `Ok` 完成创建，并返回数据编辑器主界面。

## 设置行为字段

创建好行为后，选中它来编辑字段。首先定位到 `Count` 字段，用来设置触发该行为所需的点击次数。双击该字段，打开 `Object Values` 窗口，并把 `Count` 值设为 `5`，如下图所示。

[![设置行为点击次数](./resources/070_Click_Response_Behavior5.png)](./resources/070_Click_Response_Behavior5.png)
*设置行为点击次数*

接着定位到 `Count Delay` 字段。双击该字段，打开另一个 `Object Values` 窗口，并将其值设为 `0.5`。这会在每次点击之间设置一个计时器；如果超出这个时限，点击计数就会被重置。如果你希望该行为在正常使用中不容易被误触发，这会非常有用。

[![设置行为点击延迟](./resources/070_Click_Response_Behavior6.png)](./resources/070_Click_Response_Behavior6.png)
*设置行为点击延迟*

现在定位到 `Count Effect` 字段，选中并双击打开编辑窗口。这个字段用于设置点击达到次数后要触发的效果。之后你可以回到这里做更多实验，不过现在先选择 `Suicide`。这会让单位在被反复点击后移除自身。

[![设置点击响应效果](./resources/070_Click_Response_Behavior7.png)](./resources/070_Click_Response_Behavior7.png)
*设置点击响应效果*

现在点击 `Ok` 完成这个行为的构建。完成后的字段应如下图所示。

![完成的行为字段](./resources/070_Click_Response_Behavior8.png)
*完成的行为字段*

## 应用点击响应行为

这个行为现在已经完全准备好了，可以接入玩法。此类行为有一个很方便的特点，就是可以直接塞进单位里，因此设置起来相当简单。这也很合理，因为它提供的是一个直接响应玩家输入的效果，所以最自然的挂载位置就是单位本身。你可以切换到 单位 标签页，或通过 `+ ▶︎ Edit Game Data ▶︎ 单位` 打开它。然后选中 `Mutalisk`，并定位到它的 `行为` 字段。

[![异龙行为字段](./resources/070_Click_Response_Behavior9.png)](./resources/070_Click_Response_Behavior9.png)
*异龙行为字段*

双击 `行为` 字段，打开 `Object Values` 窗口。在 `行为` 框内右键并选择 `Add Value`。这样就会向单位添加一个空白行为，如下图所示。

[![添加异龙行为](./resources/070_Click_Response_Behavior10.png)](./resources/070_Click_Response_Behavior10.png)
*添加异龙行为*

创建完成后，选中新值，并通过窗口搜索功能或滚动条找到 `Mutalisk Destroy` 行为。

[![已设置 Mutalisk Destroy 行为](./resources/070_Click_Response_Behavior11.png)](./resources/070_Click_Response_Behavior11.png)
*已设置 Mutalisk Destroy 行为*

点击 `Ok` 选择该行为。此时你还需要对单位做最后一项修改。默认情况下，单位并不会把玩家点击当作一种数据事件来响应。它们在游戏视图中当然可以被点击和操作，但在数据层面上，这个信号默认不会产生事件。为修正这一点，请选中 `Mutalisk`，定位到它的 `Flags` 字段。双击打开后，找到 `Cannot Be Clicked` 标志并取消勾选。

[![允许异龙被点击](./resources/070_Click_Response_Behavior12.png)](./resources/070_Click_Response_Behavior12.png)
*允许异龙被点击*

点击 `Ok` 返回数据编辑器主视图。地图现在已经完成，你可以运行 `Test Document`。如果你在地图中查看一只异龙，会发现它现在带有一个行为状态，如下图所示。

[![带有 Mutalisk Destroy 行为的异龙](./resources/070_Click_Response_Behavior13.png)](./resources/070_Click_Response_Behavior13.png)
*带有 Mutalisk Destroy 行为的异龙*

连续点击这只异龙五次，就能看到这个行为的设计效果。

[![异龙点击响应效果](./resources/070_Click_Response_Behavior14.png)](./resources/070_Click_Response_Behavior14.png)
*异龙点击响应效果*

## 附件

 * [070_Click_Response_Behavior_Start.SC2Map](./maps/070_Click_Response_Behavior_Start.SC2Map)
 * [070_Click_Response_Behavior_Completed.SC2Map](./maps/070_Click_Response_Behavior_Completed.SC2Map)
