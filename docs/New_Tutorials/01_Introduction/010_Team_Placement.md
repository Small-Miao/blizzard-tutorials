# 队伍放置

队伍放置提供了一套用于组织玩家出生点的控制方式。这套组织机制从“起始位置”点开始，你可以在地形模块中放置这些点。它们标记了玩家可能出生的所有位置，但本身只是标记，并不包含任何逻辑来决定在特定情况下哪些玩家会出生在这些位置上。真正决定这一逻辑的是队伍放置：在这里，这些位置会被分组为特定排列，用来控制队伍如何以及在哪里出生。

尽管这套系统最初是为对战地图建立的，但在制作非传统游戏时它依然有用。通过起始位置对玩家分组，并根据敌对关系控制队伍出生位置的系统，适用于任何地图类型，只是在对战玩法之外可能用途有限。

队伍放置（基础） 和 队伍放置（高级） 都是玩家属性的子部分，与 玩家属性 标签一起位于同一个窗口中。你也可以分别通过 `地图 ▶︎ 队伍放置（基础）` 或 `地图 ▶︎ 队伍放置（高级）` 进入它们。

[![Team Placement Views](./resources/010_Team_Placement01.png)](./resources/010_Team_Placement01.png)
*队伍放置视图*

本文附带了一张示例演示地图。这张地图在一个正方形边缘大致等距地分布了十二个起始位置，因此可以构造出许多合理的队伍出生排列。

![Demo Map Minimap](./resources/010_Team_Placement02.png)
*演示地图小地图*

## 基础队伍放置

通过 `地图 ▶︎ 队伍放置（基础）` 打开基础队伍放置。除了当前项目的小地图外，你还会看到地图上每个起始位置的列表。你可以选择各个 Start Location，并把它们与潜在盟友关联起来，以此决定队伍。在 Basic Team Placement 中，你之后还需要通过游戏变体或触发器来在游戏里实际启用这些队伍。

## 演示基础队伍创建

你可以先在 Start Locations 子视图中选择一个起始位置，再在 Linked Allies 子视图中勾选另一个起始位置的复选框，以此创建队伍连接。以演示地图为例，请选择位于地图西北角的 `Start Location 001`，然后勾选它最近的两个相邻点 `Start Location 002` 和 `Start Location 012`。当你向某个队伍中添加盟友时，这些起始位置会变为绿色。完成这一步后，你应会得到如下结果。

[![Basic Team Setup](./resources/010_Team_Placement03.png)](./resources/010_Team_Placement03.png)
*基础队伍设置*

每个起始位置旁边的 Allies 字段会显示与之连接的盟友数量。通常在对战地图中，你会希望每个起始位置拥有相同数量的盟友，这样说明地图具备对称的排列选项。继续连接其他起始位置，把另外三个角落的起始位置分别与各自相邻位置连接起来。完成后的地图应如下图所示。注意，每一种潜在的队伍排列都拥有相同数量的盟友。

[![Fully Configured Team Placement](./resources/010_Team_Placement04.png)](./resources/010_Team_Placement04.png)
*完整配置的队伍放置*

## 高级队伍放置

通过 `地图 ▶︎ 队伍放置（高级）` 打开高级队伍放置。在小地图视图旁边，你会看到 Teams 和 Linked Enemy Teams 列表。Teams 类别允许你像在基础队伍放置中那样安排队伍，然后再把它们与敌方队伍配置配对。与基础队伍放置不同，这是一种完整方案，它既处理队伍构建，也处理可能的敌方队伍安排。完成后，只需在“游戏变体”中稍作设置，地图就基本准备好投入游戏了。

## 演示高级队伍创建

你可以点击 Teams 子视图下方的 “New” 按钮来添加一个队伍。随后会打开如下窗口。

![Team Creation Pop Up](./resources/010_Team_Placement05.png)
*队伍创建弹窗*

在这里，你可以通过选择一组起始位置来创建队伍。本例中，请选择 `Start Location 001`、`Start Location 002` 和 `Start Location 012`，以创建地图西北角的一支队伍，并将其命名为 `3 V 3 North West`。对地图的其余角落重复这一过程，并采用类似命名方式。完成后，你总共应拥有四支队伍。注意，随着队伍被添加，它也会出现在 “Linked Enemy Team” 中供选择。

接下来，你需要把两支队伍连接为敌对关系。选择一支队伍，本例使用 `3 V 3 North West`，然后在 “Linked Enemy Team” 子视图中勾选你希望作为其可能敌方排列的队伍。结果应如下图所示。

[![Team Arrangements Matched as Foes](./resources/010_Team_Placement06.png)](./resources/010_Team_Placement06.png)
*配对为敌方的队伍排列*

这套系统支持任意规模的队伍，但队伍应当与人数相等的敌方队伍进行配对。还需要注意，多个队伍之间不能共享任何起始位置。即便如此，这里依然支持非常多的配置方式。下图展示了另一种排列示例。

[![Further Team Arrangement Possibilities](./resources/010_Team_Placement07.png)](./resources/010_Team_Placement07.png)
*更多可能的队伍排列*

最后你需要知道的是，高级队伍放置比基础队伍放置功能更强。因此，如果同一张地图中两者都进行了配置，高级队伍放置会优先生效。

## 附件

 * [010_Team_Placement_Completed.SC2Map](./maps/010_Team_Placement_Completed.SC2Map)
 * [010_Team_Placement_Start.SC2Map](./maps/010_Team_Placement_Start.SC2Map)
