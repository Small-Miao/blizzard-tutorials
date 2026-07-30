# 区域层

区域层允许你创建区域。区域是地图上被标记出来的一片范围，用于向其他模块传递区域信息。与点不同，区域标记的是地图上的具体形状，而不是单一坐标。你可以通过地形栏中下图所示图标切换到该图层。

![Region Layer Icon](./resources/024_Regions_Layer1.png)
*区域层图标*

## 区域面板

区域面板中包含创建区域的控制项，以及地图上现有区域的列表。当地形编辑器的区域层处于活动状态时，这个面板位于主视口左侧。它的样子如下图所示。

![Regions Palette](./resources/024_Regions_Layer2.png)
*区域面板*

区域由三种工具绘制出的形状组成：矩形、圆形和菱形。区域会在编辑器中以彩色覆盖层的方式显示，但在正常游戏过程中不可见。你还可以通过组合任意数量的基础形状来构建复合区域。组合时既可以采用正向方式，把形状面积相加；也可以采用负向方式，从已有区域中减去某一块面积。

你可以选中某个区域，并查看区域面板最下方的子视图，以确认其构成。列表会列出该区域中包含的所有基础形状，并标明它们是以 Positive 还是 Negative 方式参与组合的。

## 区域属性

复合区域是在“区域属性”窗口中构建的。你可以通过在地图视图中，或在区域面板中双击某个已有区域来打开它。下图展示了一个区域属性窗口示例。

![Composite Region Building](./resources/024_Regions_Layer3.png)
*构建复合区域*

在这里，你可以先从一个单独的正向区域开始构建复合区域。然后通过在形状列表中右键，选择 Add Circle、Add Rectangle 或 Add Diamond 来继续添加形状。任意形状都可以通过选择 Negative 选项变成挖空区域。正向区域会显示为蓝色覆盖层，负向区域则显示为红色。回到地图视图后，实际形状会以彩色覆盖层显示，而被挖空的负向区域则会表现为透明。上面的示例在地图视图中看起来如下图所示。

[![Composite Region in Editor](./resources/024_Regions_Layer4.png)](./resources/024_Regions_Layer4.png)
*编辑器中的复合区域*

## 区域演示

打开本文附带的演示地图。地图内容是在山丘顶部有一座小型要塞，稍远处则有两辆属于玩家的恶蝠车。

[![Demo Map Course](./resources/024_Regions_Layer5.png)](./resources/024_Regions_Layer5.png)
*演示地图场景*

如果你查看这张地图的触发编辑器，就会发现 `玩家 1 Captures Base` 下有一组简短动作：当某个单位进入某个区域时，要塞中的中立单位就会转移给玩家。为了实现这一点，你需要在要塞入口附近创建一个区域，并把它接入该触发器。

点击区域面板中的任意一种区域工具即可创建区域。这里最适合使用 Add Region Diamond 工具。选中它，然后在地图视图中点击并拖拽绘制区域。完成后应类似下图。

[![Creating a Region](./resources/024_Regions_Layer6.png)](./resources/024_Regions_Layer6.png)
*创建区域*

如果你不满意区域的位置，可以切换鼠标到选择模式进行调整。按下键盘上的 Space 可在普通选择指针和带加号的区域创建指针之间切换。进入选择模式后，点击某个区域即可重新激活它，然后你就可以拖拽移动它。若要调整区域大小，请按住 Ctrl 并点击区域边缘，然后拖动到新的尺寸。

接着，在区域面板中点击该区域，打开“区域属性”窗口，然后进入 `General` 标签。把名称设为 `Capture Base`。此时属性窗口应如下图所示。

![Region Properties Contents](./resources/024_Regions_Layer7.png)
*区域属性内容*

和上图一样，你可以在 `General` 标签左侧视图中编辑区域。还值得注意的是，区域的尺寸和位置也会在 `Shapes` 标签下以 Size 和 Center 的形式显示。如果你在最初放置时对区域大小拿捏不准，也可以在这里确认。

接下来，回到地形模块，选中 Add Region Rectangle 工具，再准备一个新区域。这一次请让该区域覆盖整座山顶要塞，并把它命名为 `Entire Base`。完成后大致如下图。

![Current Region Layout](./resources/024_Regions_Layer8.png)
*当前区域布局*

如果你难以同时看清整个场景，可以尝试拉远镜头，或者通过 `视图 ▶︎ Entire Map` 将镜头切换到俯视模式。在这种视角下，你可以再缩放回示例图中的角度。还请注意，你也可以像上图那样，通过在地图视图中选中某个区域来高亮显示它，例如 `Capture Base` 区域。

不过现在，`Entire Base` 区域把水晶矿和一座瓦斯气泉也包含进去了。根据触发模块中的动作，这会导致玩家获得这些资源的控制权。而资源通常应始终属于 Neutral 玩家。你可以通过复合区域来构建一个既包围整座基地、又为这些资源留出空洞的区域。

要准备这个复合区域，请先创建两个矩形区域，一个框住矿线，一个框住瓦斯气泉。然后把它们改为 Negative 区域，这样它们就会从大区域中被挖掉，从而确保资源不会被包含进区域内。完成后应如下图所示。

[![Cutout Regions Prepared](./resources/024_Regions_Layer9.png)](./resources/024_Regions_Layer9.png)
*已准备好的挖空区域*

为了让画面更易读，新区域以及 `Capture Base` 都应用了自定义颜色。你可以通过 `Region Properties ▶︎ General ▶︎ Custom Color` 来设置。现在，你可以通过选中这三个组成区域来创建复合区域，然后打开“区域属性”窗口。按住 Shift，同时点击 `Capture Base`、`Region 001` 和 `Region 002` 以将它们全部选中。然后在区域面板中右键，前往 `Edit ▶︎ Merge Selection`。

[![Collected Region Properties](./resources/024_Regions_Layer10.png)](./resources/024_Regions_Layer10.png)
*已收集的区域属性*

这个视图是你在地形编辑器中所见区域的抽象表示。在这里，你可以通过选中两个较小区域并把它们的 State 改为 Negative 来创建挖空区域。负向形状会绘制为红色，因此结果应类似下方区域属性窗口中的效果。

[![Constructed Composite Region](./resources/024_Regions_Layer11.png)](./resources/024_Regions_Layer11.png)
*构建完成的复合区域*

回到地图视图后，你应当会看到这个区域覆盖了整座要塞，但在资源周围留出了两个空洞。

[![Cutout Regions Prepared](./resources/024_Regions_Layer12.png)](./resources/024_Regions_Layer12.png)
*已准备好的挖空区域*

现在区域逻辑已经合理，你可以切换到触发编辑器，回到 `玩家 1 Capture Base` 触发器。选中 `Unit Enters/Leaves Region` 事件，双击其中的 `Region` 字段。这会打开一个“区域”窗口，里面列出了地图上的所有活动区域。请选择复合区域 `Capture Base`，然后点击 “Ok”，如下图所示。

[![Selecting Composite Region](./resources/024_Regions_Layer13.png)](./resources/024_Regions_Layer13.png)
*选择复合区域*

接着找到 `Change Owner` 动作，并使用之前相同的方法把其中的 `Region` 字段设为 `Capture Base`。

项目至此就完成了。你可以稍微回顾一下它的功能：当你把山脚下的恶蝠车移动进 `Capture Base` 区域时，触发器事件会把 `Entire Base` 区域内的所有内容转交给玩家控制。由于复合区域中设置了挖空部分，水晶矿和瓦斯气泉应继续保持中立。

如果一切工作正常，点击“测试文档”并把恶蝠车带入要塞入口后，所有权应当会像下图那样正确转移。

[![Claiming the Fort](./resources/024_Regions_Layer14.png)](./resources/024_Regions_Layer14.png)
*占领要塞*

## 附件

 * [024_Regions_Start.SC2Map](./maps/024_Regions_Start.SC2Map)
 * [024_Regions_Completed.SC2Map](./maps/024_Regions_Completed.SC2Map)
