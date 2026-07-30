# 单位选择事件

'Unit Selection' 这一类事件在为单位增加交互方式时非常有用。这些事件会在玩家用鼠标操作某个 Unit 并导致其状态变化时触发。这里的变化共有三种，见下表。

| 事件 | 说明 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unit Selected | 当玩家第一次用鼠标点击某个 Unit 时触发，此时该单位会被选中并进入 Selected 状态。一旦单位处于这个状态，在玩家取消选择并让其回到 Basic State 之前，就不能再次触发选中。玩家选择其他游戏实体时，该单位也可能被取消选择。玩家还可以通过鼠标框选或按住 CTRL 再点击单位，一次性选择多个单位。 |
| Unit Clicked | 每当玩家用鼠标点击某个 Unit 时都会触发，可以无限次发生。对一个未选中单位的第一次点击，同时满足 Unit Clicked 与 Unit Selected 的条件。玩家一次只能点击一个单位。 |
| Unit Highlighted/UnHighlighted | 高亮与取消高亮事件取决于玩家鼠标位置是否进入或离开该 Unit 的范围。当鼠标首次进入单位范围时，会触发 Unit Highlighted，单位进入 Highlighted 状态；当鼠标离开该高亮单位范围时，会触发 Unit UnHighlighted，单位回到 Basic State。玩家一次只能高亮一个单位。 |

## 选择状态

《星际争霸 II》的 UI 会帮助你识别这些选择状态，如下图所示。图片后的说明解释了每种状态。

[![图像](./resources/048_Unit_Selection_Events1.png)](./resources/048_Unit_Selection_Events1.png)

单位基础状态 -- 单位高亮 -- 单位已选中且高亮 -- 单位已选中

最左侧图片中的单位处于 Basic 状态，既没有被 Selected，也没有被 Highlighted，因此其模型在界面上不会有任何特殊变化。左起第二张图中，鼠标进入了该单位的范围，于是单位进入 Highlighted 状态。此时会触发 Unit Highlighted 事件。鼠标停留在高亮状态的单位上时，界面会发生两项可视化变化：鼠标指针变成瞄准准星，单位脚下出现一个虚线圆圈。圆圈颜色取决于单位归属：己方单位为绿色，盟友或中立单位为黄色，敌方单位为红色。

左起第三张图中，该单位第一次被点击，因此进入 Selected 状态。这一变化会同时触发 Unit Selected 和 Unit Clicked 事件。被选中的单位在界面上会显示一个实线选中圆圈，其颜色规则与高亮圆圈一致。由于此时鼠标仍悬停在单位上，所以该单位同时处于 Highlighted 和 Selected 状态，因此两套界面效果会叠加出现：既有选中圆圈，也有高亮圆圈，鼠标光标也保持为高亮准星。

最右侧图片中，鼠标已经移出单位范围，因此触发了 Unit Unhighlighted 事件。此时单位仍被选中，仍显示实线选中圆圈；但由于不再高亮，鼠标光标已恢复为普通指针。

## 选择标志

虽然《星际争霸 II》引擎中的每个单位都可以被点击和高亮，但用于触发这些事件的数据消息默认并未开启。你需要在游戏数据中手动启用这些行为。进入数据编辑器，然后打开 + ▶︎ Edit Game Data ▶︎ 单位。找到你要监视选择事件的单位类型。进入该 Unit 后，找到 'Unit: Flags' 字段并双击，打开 'Object Values' 窗口。下图展示了一个追猎者的修改示例。

[![修改追猎者的 Unit: Flags 字段](./resources/048_Unit_Selection_Events2.png)](./resources/048_Unit_Selection_Events2.png)
*修改追猎者的 Unit: Flags 字段*

向下滚动这些单位标志，直到看到 'Cannot Be Clicked' 和 'Cannot Be Highlighted.' 这两个标志分别控制 Unit Clicked 与 Unit Highlighted/UnHighlighted 事件是否会被发送。取消勾选后，该单位类型就会把相应事件传递到触发编辑器中，其效果如下图所示。

![](./resources/048_Unit_Selection_Events3.png)
*为某单位启用 Unit Clicked 与 Highlighted/UnHighlighted 事件*

## 观察单位选择事件

打开本文附带的演示地图，里面提供了一个可供你反复试验选择事件的场景。这有助于你直观理解，单位选择事件究竟如何从玩家操作中产生。场景如下所示。

[![演示地图场景](./resources/048_Unit_Selection_Events4.png)](./resources/048_Unit_Selection_Events4.png)
*演示地图场景*

你会发现，地图中有多种单位已经通过数据编辑器配置为同时接受 Unit Clicked 与 Unit Highlighted/UnHighlighted 事件。注意，这些单位还分别属于玩家本人、盟友、敌人以及中立方。接着进入触发编辑器，看看这些单位选择事件在示例中是如何使用的。

![](./resources/048_Unit_Selection_Events5.png)
*对单位选择事件的响应输出*

这张地图在 'Map Init' 触发器中设置了一些实用的显示器、联盟控制和无敌性逻辑。除此之外，还分别为 Highlighted、UnHighlighted、Selected 和 Clicked 四种主要单位选择事件准备了对应触发器。就像上图中的 'Unit Highlighted' 示例触发器一样，这些触发器在事件发生时会向屏幕输出一条调试消息。借助这套设置，地图能够对玩家做出的每一种选择操作给出直接反馈。你可以点击编辑器中的 'Test Document' 按钮亲自体验。

下面是一些具有说明性的单位选择事件示例。

![](./resources/048_Unit_Selection_Events6.png)
*掠夺者先被高亮，随后被点击并选中*

这里，敌方掠夺者先被鼠标悬停，然后被点击。于是依次产生了 Unit Highlighted、Unit Clicked 和 Unit Selected 事件。要注意，后两者在编辑器中看起来是同时发生的，但在列表中 clicked 事件总是排在前面。你还能看到，该掠夺者对应的 UI 效果均呈红色，这表明它是敌方单位。

[![框选所有受控单位](./resources/048_Unit_Selection_Events7.png)](./resources/048_Unit_Selection_Events7.png)
*框选所有受控单位*

在下一个例子中，玩家用鼠标框选了一批单位。尽管这些单位已被框选覆盖，但仅仅框住它们并不会立即触发任何事件。

![](./resources/048_Unit_Selection_Events8.png)
*追猎者、狂热者和黑暗圣堂武士被同时选中*

当释放框选时，所有属于玩家的单位会同时被选中。每个单位都会单独触发一个 Unit Selected 事件，然后进入 Selected 状态。

[![反复点击 Lyote](./resources/048_Unit_Selection_Events9.png)](./resources/048_Unit_Selection_Events9.png)
*反复点击 Lyote*

这个例子里，一个中立的 Lyote 小动物被反复点击。这说明一个 Unit 可以无限次接受玩家点击，并且每次都会触发 Unit Clicked 事件。

## 附件

 * [048_Unit_Selection_Events.SC2Map](./maps/048_Unit_Selection_Events.SC2Map)
