# 动态寻路阻挡器

动态寻路阻挡器允许你创建可在游戏过程中开启和关闭的寻路区域。之所以能做到这一点，是因为它们本质上其实是单位，因此可以通过触发器动作来显示或隐藏。隐藏单位时，它的 footprint 会被暂停，其对应的寻路效果也会随之消失；当阻挡器再次显示出来时，寻路规则也会恢复。

这种可动态切换的寻路有不少用途，其中最常见的是：在某个条件满足，或者某种类似任务的操作完成之前，限制玩家进入地图的某些区域。动态寻路阻挡器在地形编辑器中还会显示它们的 footprint，因此与其他临时封路方案相比，它们往往更容易操作。

## 创建动态寻路阻挡器

你可以在单位面板中搜索 `Dynamic`，找到预制的动态寻路阻挡器。

![Dynamic Pathing Blockers](./resources/031_Dynamic_Pathing_Blockers1.png)
*动态寻路阻挡器*

每种阻挡器的名称通常会标明它的 footprint 尺寸、形状以及阻挡的寻路类型。不要把动态寻路阻挡器与寻路层中的 dynamic pathing fill 选项混淆。前者是可以在游戏过程中改变状态的寻路区域，而后者则是在编辑器中把某片区域填充为 “No Pathing”。

你也可以在创建单位时使用模板 `PATHINGBLOCKER` 从零开始制作动态寻路阻挡器，如下图所示。

![Dynamic Pathing Blockers](./resources/031_Dynamic_Pathing_Blockers2.png)
*动态寻路阻挡器*

创建完成后，你需要把阻挡器的 `Footprint` 字段设为想要使用的 Footprint 对象。它还需要连接到一个 unit actor，才能使用标准模型。一个常见阻挡器的数据蓝图如下图所示。

![Dynamic Pathing Blocker Data Composition](./resources/031_Dynamic_Pathing_Blockers3.png)
*动态寻路阻挡器的数据构成*

## 放置动态寻路阻挡器

在放置动态寻路阻挡器时，放置网格会特别有帮助。你可以通过 `视图 ▶︎ Show Placement Grid` 打开它，并勾选其中所有选项。

[![Placement Grid View Options](./resources/031_Dynamic_Pathing_Blockers4.png)](./resources/031_Dynamic_Pathing_Blockers4.png)
*放置网格视图选项*

在这个演示地图中，动态放置阻挡器被用来模拟一种能量门。地图使用了 `Protoss Energy Line (Blue)` 装饰物，并配合若干悬崖立面，来为阻挡器提供视觉表现。下图展示了这一结构。

[![Energy Gate Site](./resources/031_Dynamic_Pathing_Blockers5.png)](./resources/031_Dynamic_Pathing_Blockers5.png)
*能量门位置*

由于地图结构的限制，机枪兵只能通过一条宽度为八个单位的通道穿过这道门。这正是使用动态寻路阻挡器的理想场景，因此地图中放置了四个 `Dynamic Pathing Blocker 2x2` 来填满这个缺口。

![](./resources/031_Dynamic_Pathing_Blockers6.png)
*带有动态寻路阻挡器的能量门*

既然这条路是由动态阻挡器封住的，你就可以通过开关它们来实现门或闸门的功能。

## 使用动态寻路阻挡器

由于动态寻路阻挡器本质上是单位，只有当它们在地图上处于激活状态时，才会应用自己的 footprint。你可以使用触发器动作 `Show/Hide Unit` 来改变阻挡器状态，从而按需开启或关闭它们。其他所有可以以单位为目标的动作，也都能在不同程度上用于动态寻路阻挡器；常见的有 `Kill Unit`、`Create 单位` 和 `Move Unit Instantly`。

在这个演示练习中，动态寻路阻挡器会在地图初始化时被加入到一个组里。之后又添加了如下触发器。

[![Dynamic Pathing Blocker Toggling Trigger](./resources/031_Dynamic_Pathing_Blockers7.png)](./resources/031_Dynamic_Pathing_Blockers7.png)
*切换动态寻路阻挡器的触发器*

这个触发器会每五秒改变一次动态寻路阻挡器的状态。当变量 `Line Hidden` 设为 False 时，能量门会通过 SetOpacity actor 消息渐显出来。与此同时，同一段语句块还会通过 `Show Unit` 动作启用动态寻路阻挡器。关闭状态下的门如下图所示。

[![Path Blocked by Closed Gate](./resources/031_Dynamic_Pathing_Blockers8.png)](./resources/031_Dynamic_Pathing_Blockers8.png)
*关闭大门阻断路径*

当该变量切换为 True 时，透明度和阻挡器状态会分别通过 SetOpacity 和 `Hide Unit` 被切换。这会把大门及阻挡器一并从地图中移除，从而允许单位通过。

[![Path Revealed by Open Gate](./resources/031_Dynamic_Pathing_Blockers9.png)](./resources/031_Dynamic_Pathing_Blockers9.png)
*开启大门后显露出的通路*

## 附件

 * [031_Dynamic_Pathing_Blockers.SC2Map](./maps/031_Dynamic_Pathing_Blockers.SC2Map)
