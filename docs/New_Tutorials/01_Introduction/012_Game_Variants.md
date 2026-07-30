# 游戏变体

设置游戏变体可以让开发者指定玩家在地图 Battle.net 大厅中可用的选项。结合游戏属性，游戏变体控制着你的项目以不同模式呈现时的差异。这些变体只能在地图文档中设置，不能在模组文件中设置。你可以在编辑器任意位置通过 `地图 ▶︎ 游戏变体` 进行配置。下图展示了入口位置。

[![Navigating to 游戏变体](./resources/012_Game_Variants01.png)](./resources/012_Game_Variants01.png)
*进入游戏变体*

选择 游戏变体 后，你会看到如下窗口。

[![游戏变体 Window](./resources/012_Game_Variants02.png)](./resources/012_Game_Variants02.png)
*游戏变体窗口*

请注意最左侧名为 “Variants” 的边栏。这个列表列出了游戏中所有可用模式，并且无论你切换到哪个标签视图，它都会始终保持可见。你可以在这个边栏中右键并选择 “Add” 或 “Add Standard” 来创建新的变体。

如果你正在制作一个新项目，那么在开始编辑变体前，需要先取消使用默认设置，即取消勾选变体窗口左下角的 Use Default Variants 选项。高亮边栏中的某个变体后，你就可以在三个标签页中修改它的设置：Game Type、Game Attributes 和 Player Attributes。后文会进一步说明这些标签页中的设置。

## 变体与大厅视图

游戏变体用于定义 Arcade 大厅房主可选的配置项。玩家可以在典型大厅的 “Settings” 面板中访问这些选项，如下图所示。

[![Default Lobby](./resources/012_Game_Variants03.png)](./resources/012_Game_Variants03.png)
*默认大厅*

## 游戏类型

[![Game Type Tab](./resources/012_Game_Variants04.png)](./resources/012_Game_Variants04.png)
*游戏类型标签*

变体的基础设置在 Game Type 标签中完成。Mode 定义了一般意义上人们所说的“变体”，而 Category 则提供分类方式。这些内容会体现在游戏大厅中玩家看到的 “Category” 和 “Mode” 字段里。游戏的 Genre 设置位于变体窗口顶部下拉框中，用来定义地图在 Arcade 中归属的主要搜索类别。

## 游戏属性

[![Game Attributes Tab](./resources/012_Game_Variants05.png)](./resources/012_Game_Variants05.png)
*游戏属性标签*

Game Attributes 标签决定了典型大厅 “Settings” 区域中大多数可配置数值。这里的五项属性都可以设置默认值。某些情况下，还可以通过 Removed Values 选项把部分数值从可选列表中移除。各项属性说明如下表所示。

| 属性 | 说明 |
| ---- | ---- |
| Game Duration | 游戏在经过多长时间后结束。通常这里会设为 Infinite，由游戏自身决定何时结束。 |
| Game Privacy | 决定比赛历史和开局顺序可见性的选项。它主要用于高水平竞技环境，便于玩家保留某些对局信息。标准设置 Normal 会在对局结束后显示比赛历史与开局顺序。 |
| Game Speed | 决定引擎运行的具体游戏速度。标准值为 Faster。 |
| Lobby Delay | 设置游戏从大厅启动后播放的倒计时。标准值为 10 秒。 |
| Locked Alliances | 设置同盟锁定状态。若同盟未锁定，玩家可在游戏内自行协商谁是盟友、谁是敌人。默认情况下该项会显示出来，但处于 Locked。它也可以设为 Hidden，此时同盟会被锁定，但不会显示这些同盟关系。 |

要理解这些属性如何在大厅中生效，理解 Access 的概念非常重要。每个属性都有一个 Access，用于决定它是否可以在游戏大厅中被修改。如果属性设为 Unlocked，就可以在大厅中更改；Locked 的属性在大厅中可见但不可更改；Hidden 的属性既不可见也不可修改。任何可在大厅中配置的属性都只能由大厅房主设置。

## 玩家属性

[![Player Attributes Tab](./resources/012_Game_Variants06.png)](./resources/012_Game_Variants06.png)
*玩家属性标签*

Player Attributes 标签是 玩家属性 的更深入版本。你可以在这里为每个单独玩家设置属性，包括种族、颜色、让分以及其他标准对战选项。

与 Game Attributes 标签类似，这里也可以为每项属性设置 Access，不过在这里它是以“逐玩家”的方式处理的。
