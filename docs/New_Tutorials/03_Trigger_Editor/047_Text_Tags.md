# 文字标签

文字标签会把文本显示在游戏空间中，而不是像界面或对话框文字那样显示在屏幕空间中。它们被设计成一种视觉上悬挂在目标单位上方、并能随着摄像机运动而变化的元素，因此会自然地嵌入游戏环境中。正因为如此，文字标签常用于介于界面元素与玩法元素之间的文本内容。典型用途包括单位头顶玩家名、地点名，以及伤害数字。下图中，文字标签显示在孵化场和萃取房上方，为每个资源点提供相关的工人数量信息。

[![图像](./resources/047_Text_Tags1.png)](./resources/047_Text_Tags1.png) 显示工人数量的文字标签

## 文字标签动作

你可以在创建动作时通过进入 'Text Tags' 标签来访问文字标签动作。界面如下所示。

[![文字标签动作](./resources/047_Text_Tags2.png)](./resources/047_Text_Tags2.png)
*文字标签动作*

这些动作如下表所示。

| 动作 | 作用 |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Attach Text Tag To Unit | 以 Height Offset 的高度偏移把文字标签附着到某个 Unit 上。 |
| Attach Text Tag to Unit Attach Point | 以 X、Y 偏移把文字标签附着到某个 Unit 的一个 Attachment Point 上。注意，单位模型数据中预定义了许多可用于此目的的附着点，例如 Status Bar。 |
| Create Text Tag | 为 Players 创建一个带 Text 和 Font Size 的文字标签，并将其放置在某个 Position 与 Height Offset 上。 |
| Destroy Text Tag | 移除一个文字标签。 |
| Pause/Unpause Text Tag | 暂停或恢复发送给该文字标签的动作。 |
| Set Alignment of Text Tag | 设置文字标签的 Horizontal 对齐和 Vertical 对齐。 |
| Set Background Border of Text Tag | 设置文字标签背景边框的 Horizontal 和 Vertical 数值。 |
| Set Background 图像 of Text Tag | 为文字标签设置背景 图像 及其平铺 Type。 |
| Set Background Offset of Text Tag | 以屏幕大小百分比为单位，设置文字标签背景的 Horizontal 和 Vertical 偏移。 |
| Set Color Of Text Tag | 设置文字标签的 Color。 |
| Set Edge 图像 of Text Tag | 为文字标签某一条 Edge 设置 图像，并带有 X、Y 偏移。 |
| Set Enforce Fog Of War Of Text Tag | 设置文字标签是否使用战争迷雾。 |
| Set Faded Transparency Of Text Tag | 设置文字标签的淡化透明类型。 |
| Set Font Size Of Text Tag | 设置文字标签所用文本的 Font Size。 |
| Set Maximum Size of Text Tag | 文字标签会随着摄像机远近改变显示尺寸。此动作允许你设置其最大 Width 和 Height。 |
| Set Position Of Text Tag | 以 Height Offset 设置文字标签的位置。 |
| Set Text Alignment Of Text Tag | 设置文字标签在 Horizontal 和 Vertical 轴上的文本对齐方式。 |
| Set Text Of Text Tag | 设置文字标签的 Text。 |
| Set Time Of Text Tag | 将文字标签的 Duration 设为某个 Time。设置后，该标签会在 Duration 结束后过期。 |
| Set Visibility Type Of Text Tag | 设置文字标签的可见性类型。 |
| Show/Hide Background For Text Tag | 显示或隐藏文字标签的背景 图像。 |
| Show/Hide Text Shadow For Text Tag | 显示或隐藏文字标签的阴影。 |
| Show/Hide Text Tag | 为 Players 显示或隐藏文字标签。 |

## 使用文字标签

与对话框类似，文字标签在创建后也经常需要重新定位、移动和缩放。为支持这些操作，你应当使用 Set Variable 动作和函数标识符 Last Created Text Tag，为每个标签保存一个持久句柄。

请看下面这组文字标签动作序列。

[![文字标签动作序列](./resources/047_Text_Tags3.png)](./resources/047_Text_Tags3.png)
*文字标签动作序列*

在这组序列中，先通过 Create Text Tag 动作创建了一个文字标签。该动作确定了标签的 Text 和 Font Size，并把标签放在一个陆战队员所在的 Position 加上一定 Height Offset 处。接着，再通过 Attach Text Tag To Unit 动作把该标签附着到这个单位上，同样指定了目标 Unit 和 Height Offset。这一对动作是使用文字标签时的标准搭配。你还应注意到，附着动作会覆盖标签原本的位置，并让文字标签随着宿主单位一起移动。

[![已附着并完成样式设置的文字标签](./resources/047_Text_Tags4.png)](./resources/047_Text_Tags4.png)
*已附着并完成样式设置的文字标签*

## 附件

 * [047_Text_Tags.SC2Map](./maps/047_Text_Tags.SC2Map)
