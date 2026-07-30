# 点层

点层使用一种称为点的标记元素。点会将位置信息传递给其他模块，你可以通过地形栏中下图所示图标进入这一层。

![Points Layer Icon](./resources/023_Points_Layer1.png)
*点层图标*

## 点面板

点面板中包含创建不同类型点的工具，以及地图上当前所有活动点的列表。当点层处于活动状态时，你可以在地形编辑器左侧找到它。

[![Points Palette with Existing Point List](./resources/023_Points_Layer2.png)](./resources/023_Points_Layer2.png)
*带现有点列表的点面板*

点一共有四种类型：普通点、起始位置、声音发射器和 3D 点。你可以选择各自对应的工具，然后在地图上点击想要的位置来放置它们。这样会在该位置建立一个点，并生成一个标记。点放置后仍可通过选中并拖拽标记来移动。尽管这些标记看得见，但点本身并不占据空间，也不会在编辑器之外显示。你还可以在点面板中右键某个点，并修改其 Show in Editor 属性，以单独隐藏它的标记。点标记在编辑器中的样子如下图所示。

[![Four Types of Point Markers](./resources/023_Points_Layer3.png)](./resources/023_Points_Layer3.png)
*四种点标记*

## 点类型

[![图像](./resources/023_Points_Layer4.png)](./resources/023_Points_Layer4.png) 普通点用于在 XY 平面上标记地图中的一个具体坐标。

[![图像](./resources/023_Points_Layer5.png)](./resources/023_Points_Layer5.png) 起始位置会分配给玩家，标记他们的初始出生点。这是一种具有特殊意义的点，在编辑器多个地方都会用到。在标准对战游戏中，这些点决定基地和农民的生成位置；同时，它们也会设置对应玩家的默认初始镜头位置。

[![图像](./resources/023_Points_Layer6.png)](./resources/023_Points_Layer6.png) 声音发射器会在其所在位置播放声音。它们通常用于在地图某个距离范围内制造环境音，因此你可以把它们当作一种音频装饰物，用来营造气氛。

[![图像](./resources/023_Points_Layer7.png)](./resources/023_Points_Layer7.png) 三维点与普通点类似，但多了一个高度值，因此它们用于标记 XYZ 坐标，也就是 3D 空间中的位置。

## 点属性

点拥有一系列可在“点属性”窗口中配置的选项。你可以在地形模块中双击某个点的标记来打开此窗口；或者在点面板的列表中双击点名称。下面给出点属性窗口示例，以及它提供的选项说明。

![Point Properties](./resources/023_Points_Layer8.png)
*点属性*

| 属性 | 说明 |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name | 为点添加标签，以便在点面板中识别。 |
| Position | 点在地图中的位置，以 XY 数值表示。 |
| Rotation | 为该点存储一个可供使用的角度值。点的默认朝向为 270 度。若某个点使用了自定义标记，就会显示这个朝向。 |
| Color | 将点的标记染成指定颜色。起始位置不提供此选项。 |
| Attach To | 使用 “Choose” 选择一个单位后，点的位置会立刻设置为该单位所在位置。之后点会随着单位移动而保持附着，因此很适合用来把动态位置传递给其他模块。 |
| Model | 自定义点所使用的标记模型。 |
| Height | 仅适用于 3D 点。用于设置点位置的 Z 分量。 |
| Sound | 仅适用于声音发射器。设置该发射器播放的声音。 |

## 点的演示

打开本文附带的演示地图。你会看到一个架设在太空平台上的小型路线，其中已经放置了若干点。地图应如下图所示。

[![Demo Map Course](./resources/023_Points_Layer9.png)](./resources/023_Points_Layer9.png)
*演示地图路线*

本练习的目标，是利用现成的触发器代码生成一名机枪兵，然后让地图上的信标将机枪兵从一个平台传送到另一个平台。切换到触发编辑器，查看 `Initialization` 触发器。用于创建机枪兵的触发器已经搭好了，但它还需要一个点来决定机枪兵的生成位置。如下图所示。

![Trigger Requiring a Spawn Point](./resources/023_Points_Layer10.png)
*需要出生点的触发器*

切回地形编辑器，在点面板中点击普通点工具来创建这个点。然后在最左侧太空平台附近、靠近起始位置的地方点击，创建一个点。双击新标记打开“点属性”窗口，并将该点命名为 `Marine Spawn Location`。你应该会看到类似下图的结果。

![Labelling a Point](./resources/023_Points_Layer11.png)
*为点命名*

回到触发编辑器，将该创建触发器中的 `Point` 值设为 `Marine Spawn Location`。完成后的触发器应与下图一致。

![Point Hooked up to Trigger](./resources/023_Points_Layer12.png)
*点已连接到触发器*

现在，你的机枪兵已经准备好使用传送系统了。但如果你把注意力转回地图，会发现控制传送的那些点已经偏离了原位。这时使用 “Object Groups” 会很有帮助。你可以通过 `地图 ▶︎ Object Groups ▶︎ Points` 找到它们。

把这些点编到同一个组里，就能一次性一起移动它们。首先，需要先把它们都加入同一个组。在最左侧子视图中右键并选择 Add Group，将该组命名为 `Teleporter Points`；然后在最右侧子视图中右键并选择 Add Points。这样会打开 “Placed Objects” 窗口，你可以在其中把点加入分组。请选择 `Teleporter Point 01a`、`Teleporter Point 01b`、`Teleporter Spark Point 01a` 和 `Teleporter Spark Point 01b`，然后点击 “Ok”。操作过程如下图所示。

![Creating the Object Group](./resources/023_Points_Layer13.png)
*创建对象组*

现在请确认 `Tools ▶︎ Use Group Selection` 已经开启。这意味着只要移动组中的任意一个点，组内其他点也会一起移动。接着选中其中一个已分组的传送点，把最左侧那一组移动到太空平台最左边的信标上。这样两组点就都应与信标对齐，如下图所示。

![](./resources/023_Points_Layer14.png)
*将点对齐到传送信标*

此时运行“测试文档”，应当会在 `Marine Spawn Location` 生成一名机枪兵。之后你可以把机枪兵移动到信标上，他就会被传送到最右侧平台，再传送回来。

[![Teleportation Complete](./resources/023_Points_Layer15.png)](./resources/023_Points_Layer15.png)
*传送完成*

## 附件

 * [023_Points_Layer_Completed.SC2Map](./maps/023_Points_Layer_Completed.SC2Map)
 * [023_Points_Layer_Start.SC2Map](./maps/023_Points_Layer_Start.SC2Map)
