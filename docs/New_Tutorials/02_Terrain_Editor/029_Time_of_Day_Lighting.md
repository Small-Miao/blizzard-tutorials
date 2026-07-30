# 昼夜光照

作为一款即时战略游戏，《星际争霸》拥有许多围绕时间运作的系统。就光照而言，其中一个非常实用的设置叫作 Time of Day Lighting。这套系统允许光照随着时间流逝而变化，通过改变条件来模拟昼夜交替。除非被明确停止，否则这个循环会在整个游戏过程中不断重复。你可以用它来营造氛围和情绪，也可以把它与一组 Time of Day validator 结合，用于支持各种玩法功能。这样一来，游戏在不同光照阶段切换时，你也能相应触发不同效果。

## 创建昼夜光照

打开本文附带的演示地图。你会看到一个酒吧外景，背景中还有一片正在燃烧的残骸。切换到数据编辑器，点击绿色 `+` 打开光照标签，然后前往 `Edit Art and Sound Data ▶︎ 灯光`。在主视图中右键并选择 Add Light，新建一个光照。将它命名为 `Time of Day Lighting`，然后点击 suggest 生成 ID。接着使用下拉框把 `Light Group` 设为 `Tilesets`。整个过程应如下图所示。

[![Light Creation](./resources/029_Time_of_Day_Lighting1.png)](./resources/029_Time_of_Day_Lighting1.png)
*创建光照*

点击你新建的这组光照，然后双击 `Time of Day` 字段。如果未启用 Combine Structural Values 选项，这个字段会显示为 `Time of Day Info Array`。这会打开“对象值”窗口。

在这个窗口中，你会看到当前已有昼夜时间点的列表，也就是所有会产生光照行为的时刻。此时你通常只会看到 `00`，这是 `00:00:00` 的简写。昼夜时间使用 24 小时制，以 `HH:MM:SS` 格式记录，也就是时、分、秒。因此 `00` 就代表零点，也就是午夜。如果这里只有一个值，那么昼夜行为默认就是恒定的，也就是说游戏中的光照将保持不变。

在白色区域中右键并选择 `Add Light`，以添加一个新的昼夜时间点。下图给出了示例。

[![Adding a Time of Day Light](./resources/029_Time_of_Day_Lighting2.png)](./resources/029_Time_of_Day_Lighting2.png)
*添加昼夜光照*

点击这个新光照的 `Time` 字段第一项并输入 `6`，将其设为 `06:00:00`。这个字段遵循相同的 `HH:MM:SS` 格式。然后再添加两个新光照，分别设为 `12:00:00` 和 `18:00:00`，这样总共就有四个昼夜时间点。完成后的数组应如下图所示。

[![Four Time of Day 灯光 in an Array](./resources/029_Time_of_Day_Lighting3.png)](./resources/029_Time_of_Day_Lighting3.png)
*数组中的四个昼夜光照*

点击 “Ok” 关闭窗口，这样数据编辑器中的光照就会更新。然后再次双击 `Time of Day` 字段重新打开它。这里值得说明一点：光照窗口是一个需要频繁更新的界面。它会直接把数据推送到编辑器视图中，因此在动态渲染光照风格时非常有用。不过，任何直接向编辑器发送信息的数据或触发功能偶尔都可能“卡住”。在这种情况下，手动进行几次刷新通常很有帮助，而你刚刚做的事情正是在执行这种刷新。

现在，点击数组中的第一个光照 `00` 使其高亮，然后点击 `Modify Light` 按钮，在光照窗口中打开当前光照。

[![Light Opens in the Lighting Window](./resources/029_Time_of_Day_Lighting4.png)](./resources/029_Time_of_Day_Lighting4.png)
*在光照窗口中打开光照*

光照窗口会启动，并自动在当前光照对应的文件夹中显示所选昼夜光照。请注意，这个文件夹会按照你创建光照时选定的 Tileset 类型归类。光照窗口没有及时更新数组中所有昼夜时间点，是很常见的现象。如果该文件夹中只显示 `00:00:00`，请通过高亮 `00:00:00`，然后右键选择 `Add Light to Set` 来刷新窗口，如下图所示。

![Refreshing Light Set](./resources/029_Time_of_Day_Lighting5.png)
*刷新光照组*

刷新后，光照窗口应会在 `Time of Day Lighting` 组下显示五个条目。右键点击那个额外创建出来、用于刷新窗口的 `00:00:00` 光照，然后选择 `Remove Light from Set`。

现在，你就可以为四个昼夜光照分别配置不同属性了。游戏使用昼夜光照时，实际上是把它当作一个计划表。随着时间推进，游戏会逐渐把当前光照的各项属性过渡到数组中下一组光照的属性。这个过程会不断重复，因此只要你为一天中的各个时段制作出合理的光照，就能看到一个可辨识的昼夜循环逐渐成形。

从零开始配置光照属性往往相当耗时。更好的做法通常是复制现有光照的数据，再在其基础上修改。这里你可以使用一些 `Mar Sara` 光照作为日间和夜间的基础。进入 `Mar Sara Night Test` 文件夹并选中其中的光照，然后通过 `Edit ▶︎ Copy Light Set` 复制它的属性。

![Copying Light Set](./resources/029_Time_of_Day_Lighting6.png)
*复制光照组*

回到 `Time of Day Lighting` 组中的 `00:00:00` 光照，选中它，然后通过 `Edit ▶︎ Paste Light Set` 粘贴刚才复制的数据。下图左侧展示了这个过程，右侧则显示了粘贴后更新完成的光照。

[![Pasting Light Set -- Pasted Light](./resources/029_Time_of_Day_Lighting7.png)](./resources/029_Time_of_Day_Lighting7.png)
*粘贴光照组 -- 已粘贴的光照*

有了这组基础属性后，你现在可以继续配置 `00:00:00`。切换到它的 Key 标签，将 H (Horizontal Angle) 设为 180，V (Vertical Angle) 设为 80。把主光源移到这样一个较低、偏高的角度，有助于营造 `00:00:00` 即午夜应有的昏暗氛围。结果应如下图所示。

[![Key Lighting Being Set](./resources/029_Time_of_Day_Lighting8.png)](./resources/029_Time_of_Day_Lighting8.png)
*设置主光*

要完成这组光照，请切换到 Fill 标签，将 H 设为 45，V 设为 45；再切换到 Back 标签，将 H 设为 0，V 设为 45。然后前往 `Mar Sara Day Test` 文件夹，选中其中的 `00:00:00` 光照并复制。把它的值粘贴到 `Time of Day Lighting` 组中的 `06:00:00` 光照上。接着进入 Global 标签，把 `Light Time of Day` 设为 `6 0 0`。这是昼夜光照中最关键的字段。上一个例子之所以不用设置，是因为默认值本来就是 `0 0 0`。下图展示了该字段的设置方式。

[![Light Time of Day Being Set](./resources/029_Time_of_Day_Lighting9.png)](./resources/029_Time_of_Day_Lighting9.png)
*设置 Light Time of Day*

对于这组光照，请将 Key 的 H 设为 270，V 设为 350；Fill 的 H 设为 135，V 设为 5；Back 的 H 设为 90，V 设为 45。这样设置后，这组模拟日出的光照会形成很长的阴影，并为物体提供大量背光。

接下来，用相同方式完成最后两组光照，并使用如下参数。

对于 `12:00:00` 光照：

  - 将 `Mar Sara Day Test` 中的 `00:00:00` 光照复制到 `Time of Day Lighting` 的 `12:00:00` 光照。
  - 将 `Light Time of Day` 设为 `12 0 0`。
  - 将 Key 的 H 设为 0，V 设为 275。将 Fill 的 H 设为 225，V 设为
    60。将 Back 的 H 设为 180，V 设为 45。

这会使主光几乎直射头顶，看起来就像正午高悬的太阳。

对于 `18:00:00` 光照：

  - 将 `Mar Sara Night Test` 中的 `00:00:00` 光照复制到 `Time of Day Lighting` 的 `18:00:00` 光照。
  - 将 `Light Time of Day` 设为 `18 0 0`。
  - 将 Key 的 H 设为 90，V 设为 350。将 Fill 的 H 设为 315，V 设为
    5。  将 Back 的 H 设为 270，V 设为 45。

这会让光源在西方落下，形成与 `06:00:00` 日出相似但方向相反的长阴影效果。

关闭光照窗口并回到数据编辑器。如果还没有这样做，请高亮 `Time of Day Light` 这一组光照。

找到 `Time Per Game Loop`，并将其设为 `0 20 0`，如下图所示。

[![Setting Game Loop Time](./resources/029_Time_of_Day_Lighting10.png)](./resources/029_Time_of_Day_Lighting10.png)
*设置游戏循环时间*

这个值决定完整昼夜光照循环所需的时长。这里把周期设为较短的 20 秒，可以让你在地图测试中快速预览效果。最后，切换到 `地形类型` 标签，选中 `Agria (Jungle)`，然后把它的 `Lighting` 设为 `Time of Day Lighting`。操作如下图所示。

[![Enabling Custom Lighting in 地形类型](./resources/029_Time_of_Day_Lighting11.png)](./resources/029_Time_of_Day_Lighting11.png)
*在 地形类型 中启用自定义光照*

到这里，你的光照设置就完成了。使用“测试文档”来测试地图。你应当会看到一个被夸张加速的昼夜循环：夜景被日出打破，白昼迅速流逝，太阳西沉，场景重新陷入夜色，并不断循环。之所以会有这样的行为，是因为场景会快速穿过这四组光照配置，并以大约五秒的尺度在它们之间混合过渡。若想更深入理解昼夜设置下各组光照是如何混合的，不妨回到光照窗口中逐个检查这些单独光照。

你可以在下图中看到测试结果。

![](./resources/029_Time_of_Day_Lighting12.png)
*使用昼夜光照实现的日出到日落*

## 附件

 * [029_Time_of_Day_Lighting_Start.SC2Map](./maps/029_Time_of_Day_Lighting_Start.SC2Map)
 * [029_Time_of_Day_Lighting_Completed.SC2Map](./maps/029_Time_of_Day_Lighting_Completed.SC2Map)
