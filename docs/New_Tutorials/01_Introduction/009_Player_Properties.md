# 玩家属性

玩家属性中包含许多设置游戏时必不可少的选项。你可以在任意编辑器模块中通过 `地图 ▶︎ 玩家属性` 打开它。这会启动如下所示的窗口。

[![玩家属性 Window](./resources/009_Player_Properties01.png)](./resources/009_Player_Properties01.png)
*玩家属性窗口*

## 玩家属性字段

玩家属性会处理控制权、起始位置、种族和颜色等问题。在这一点上，它和典型的 Battle.net 大厅很相似。修改这些属性会影响游戏在联机大厅中的显示方式。不过，这些选项的作用并不止于此，它们还控制着许多与非传统游戏相关的全局属性。

下表详细说明了玩家属性中的各项选项及其含义。

| 项目 | 作用 |
| ---- | ---- |
| \# | 用于排列和引用玩家的编号。 |
| Name | 多用途标识。通常会在高亮某名玩家拥有的单位时显示，但也可以用于其他场景。在联机游戏中，玩家的 Battle.net ID 总会显示为 Name 属性。由 Hostile 或 Neutral 控制的玩家所拥有的单位不会显示名称。 |
| Color | 决定玩家的队伍颜色。若已指定，则在游戏大厅中不可更改。与对战模式不同，多个玩家可以被分配相同的队伍颜色。总共有 16 种颜色可选，设置为 `(Any)` 则允许玩家在大厅中自行选择颜色。 |
| Race | 决定玩家种族，可选项为 Protoss、Terran、Zerg、Neutral 或 `(Any)`。选择 Neutral 会让玩家种族随机，而设置 `(Any)` 则允许玩家在大厅中自行选择种族。 |
| Control | 决定每个玩家槽位的控制者。User 会创建一个供人类玩家使用的槽位，即大厅中的公开位置，可由真人或 AI 电脑填入。None 表示该槽位没有玩家，这是默认值。Computer 会创建由游戏默认 AI 控制的玩家。Neutral 会创建一个与所有其他玩家均为 Neutral Alliance 的电脑玩家。Hostile 会创建一个与所有其他玩家均为 Enemy Alliance 的电脑玩家。 |
| Start Location | 决定通过 `Create Melee Starting 单位 For Each Player` 动作创建默认单位时的位置。在其他类型地图中，除非另有指定，它还控制玩家镜头的起始位置。 |
| Decal | 设置玩家支持贴花的单位所使用的默认贴花。 |
| Default Observed | 当观察者玩家未查看任何特定玩家视野时，决定其默认镜头设置。 |

需要注意的是，虽然共有 16 个玩家槽位，但《星际争霸 II》最多只支持 15 名人类玩家。至少必须有一个槽位设为 Neutral、Hostile 或 None。这也包括在线对局中的观察者。

## 自定义玩家

在对战地图中，仅通过玩家属性就可以完成很多场景定制。下方给出了一个简单的 Protoss 对抗 Zerg 主题地图设置示例。

[![Custom Game 玩家属性](./resources/009_Player_Properties02.png)](./resources/009_Player_Properties02.png)
*自定义游戏玩家属性*

## 附件

 * [009_Player_Properties_Completed.SC2Map](./maps/009_Player_Properties_Completed.SC2Map)
 * [009_Player_Properties_Start.SC2Map](./maps/009_Player_Properties_Start.SC2Map)
