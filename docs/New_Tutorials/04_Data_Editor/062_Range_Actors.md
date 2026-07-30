# Range Actor

Range Actor 用于在地图上绘制范围指示器。它是一种用途较为单一的可视 Actor，但在《星际争霸 II》的天梯玩法中非常常见。这类范围指示器会标出许多与玩法相关的距离，例如幽灵狙击的施法距离、侦测者侦测范围的视野半径，以及最著名的攻城坦克炮击范围。

[![攻城坦克的 Range Actor](./resources/062_Range_Actors3.png)](./resources/062_Range_Actors3.png)
*攻城坦克的 Range Actor*

严格来说，Range Actor 并不是直接绘制“范围指示器”。它实际上是在地图上绘制一圈带有重复图案的纹理，而这个纹理随后才起到范围指示的作用。这里特意强调这个区别，是为了让你明白 Range Actor 也可以用于其他目的。理解它究竟在做什么很重要，这样你才能正确地配置和运用它。

你可以在地形编辑器中通过 `视图 ▶︎ Show Terrain ▶︎ Show Range` 来设置 Range Actor 的可见性。在数据中，它位于 Actor 标签页下，类型为 `Range`，如下图所示。

[![Range Actor 列表](./resources/062_Range_Actors4.png)](./resources/062_Range_Actors4.png)
*Range Actor 列表*

## Range Actor 详情

由于继承自 Actor，Range Actor 拥有大量字段。但实际上你真正会用到的，主要只有其中最有价值的一部分。下表列出了 Range Actor 最实用的字段。

| 字段 | 说明 |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 技能 | 将该 Actor 关联到一个技能，并从该技能接收 `Range` 值。 |
| 弧度 | 设置纹理沿圆周铺设的圆弧范围，取值在 0 到 360 之间。默认情况下，指示器会绘制完整圆形，也就是 360。圆弧会从单位正前方开始，向两侧等距展开。 |
| 行为 | 将该 Actor 关联到一个行为，并从该行为接收 `Range` 值。 |
| 悬崖层级标志 | 设置 Range Actor 如何将纹理投射到其他悬崖层级。勾选 Lower、Higher 或 Equal 后，纹理就会显示到对应层级。 |
| 视野 | 将该 Actor 关联到一个单位，并从其 Sight Radius 字段接收范围值。 |
| 武器 | 将该 Actor 关联到一个武器，并从该武器接收 `Range` 值。 |
| 图标 | 设置要铺设到地图上的纹理。注意，默认的 Range Actor 全都使用 `RadarIcon1` 纹理。 |
| 图标染色 | 为 `Icon` 纹理应用染色。 |
| 图标弧长 | 设置 `Icon` 图案重复铺设时的间距。该值表示单个图标沿圆弧分布时的间隔距离。 |
| 范围标志 | 包含两个标志：Game World 和 Range Flag Minimap。前者决定该 Actor 是否显示在主游戏画面中，后者决定是否在小地图上显示一个缩放版本。 |
| 事件 | 设置 Actor 事件。Range Actor 通过事件创建和移除自身，并控制可见性行为。 |
| 范围 | 直接设置纹理铺设的半径。不要与技能、行为、视野或武器字段配合使用。 |

如上所述，有多个字段可以让你自定义 Range Actor 的外观。不过在默认数据依赖项中，这些属性大多没有被充分利用。原因在于《星际争霸 II》通常把 Range Actor 当作一种信息传达工具，因此它们的外观在不同单位之间会保持一致，以免玩家产生混淆。尽管如此，这些字段对开发者来说仍然提供了不少创造空间。

## Range Actor 消息

下表拆解了与 Range Actor 运作最相关的事件和消息。

| 消息 | 说明 |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create | 创建 Range Actor，并在地图上显示指示器。 |
| Destroy | 销毁 Range Actor，并从地图上移除指示器。 |
| RangeUpdat e | 命令 Actor 从其来源重新检查 `Range` 值。由于 `Range` 值是在 Actor 创建时设定的，当宿主属性发生变化时，这会很有用。 |
| Ability | 用于在技能处于主动选目标状态时应用该 Actor。 |
| TargetOn | |
| Ability | 用于在目标指定结束后移除该 Actor。 |
| TargetOff | |
| SelectionL ocalUpdate Start | 用于在单位被选中时应用该 Actor。 |
| SelectionL ocalUpdate Stop | 用于在单位取消选中时移除该 Actor。 |

## Range Actor 类型

作为父级使用的基础 Range Actor 一共有四种：`Range Abil`、`Range Behavior`、`Range Sight` 和 `Range Weapon`。

`Range Abil` 是专门为目标型技能提供范围指示器而设计的基础类型。它支持一个 `Ability` Token；只要与之关联，Actor 的 `Range` 字段就会被设置为该 Token 对应技能的范围值。这个 Token 同时还提供了一套 Actor 事件模板，可将 Range Actor 的创建与移除消息连接到 `Ability` Token 的选目标输入。模板如下所示。

![](./resources/062_Range_Actors5.png)
*![图像](./resources/062_Range_Actors6.png)*

![](./resources/062_Range_Actors5.png)
*![图像](./resources/062_Range_Actors6.png)*

`Range Behavior` 是为 Radar Range 或 Detect Radius 行为提供范围指示器的基础类型。它支持一个 `Behavior` Token；只要连接该 Token，Actor 的 `Range` 字段就会被设置为该 Token 的范围值。这个 Token 也带有一套 Actor 事件模板，会根据 `Behavior` Token 的开关状态来创建和移除 Actor。模板如下所示。

![](./resources/062_Range_Actors5.png)
*![图像](./resources/062_Range_Actors6.png)*

![](./resources/062_Range_Actors5.png)
*![图像](./resources/062_Range_Actors6.png)*

`Range Sight` 是为视野半径提供范围指示器的基础类型。它支持一个 `Unit` Token；只要连接该 Token，Actor 的 `Range` 字段就会被设置为该单位的数值。`Range Weapon` 则类似，不过它接收的是 `Weapon` Token，并将该 Token 的范围值赋给 Actor。需要注意的是，后两种基础类型本身并不包含 Actor 事件，因此在实际使用前，你还必须先为 Range Actor 设置创建和销毁事件。

## 演示 Range Actor

现在，打开本文提供的演示地图。场景中有一只监督者漂浮在一组悬崖上方，如下图所示。

[![演示地图场景](./resources/062_Range_Actors7.png)](./resources/062_Range_Actors7.png)
*演示地图场景*

监督者是《星际争霸 II》中以侦测能力著称的单位。它会以监督者为圆心，在一定距离内显露隐形单位。而在这张地图中，无论是平时还是选中监督者时，这个范围都没有任何显示。你可以通过添加一些 Range Actor 来改变这一点，让玩家一眼就更容易理解监督者的作用。

为此，进入数据编辑器并切换到 Actor 标签页。如果还没有打开该标签页，就通过 `+ ▶︎ Edit Actor Data ▶︎ Actor` 打开它。然后在主视图中右键，选择 `Add Actor` 创建一个新 Actor，如下图所示。

[![创建一个 Actor](./resources/062_Range_Actors8.png)](./resources/062_Range_Actors8.png)
*创建一个 Actor*

这会弹出一个窗口，用来设置新建 Actor 的细节。将新 Actor 命名为 `Overseer Sight Range`，然后点击 `Suggest` 生成 ID。把 `Actor Type` 下拉框设为 `Range`，再把 `Parent` 设为 `Range Sight`。这样既声明了它是所需的 Range Actor，又给了它一个稍后会有帮助的基础类型。创建窗口现在应如下图所示。

![图像](./resources/062_Range_Actors9.png) 已准备创建的 Range Actor

点击 `Ok` 创建 Range Actor。你会回到数据编辑器主视图。接着选中 `Overseer Sight Range` Actor，打开它的字段。由于继承了父级，你会看到字段列表底部有一个 Token。给这个 Token 填入一个 `Unit` 类型后，新建的 Range Actor 就能从现有单位中读取部分属性。请选中这个 Token 字段，如下图所示。

[![Range Actor Token 字段](./resources/062_Range_Actors10.png)](./resources/062_Range_Actors10.png)
*Range Actor Token 字段*

打开这个 Token 字段后，你就可以设置一个单位，该单位的 Sight 值会被拿来作为该 Range Actor 的 `Range`。双击该 Token 字段即可，随后会看到如下视图。

![Token 选择](./resources/062_Range_Actors11.png)
*Token 选择*

在这个 Token 弹窗中选择某个单位，就会把该单位的 Sight 作为指示器绘制距离。由于这个 Range Actor 的目标是显示监督者的视野范围，因此这里选择 `Overseer`。值得一提的是，任何视野范围相同的其他单位在这里理论上也能用，并且依然能正确表现监督者的视野；但直接填入真正的监督者，可以确保一旦该视野值因任何原因发生变化，这个 Range Actor 也会随之更新。这才是更好的设计。点击 `Ok` 后，Actor 会被更新。此时 `Sight` 字段应如下图所示。

![Token 选择](./resources/062_Range_Actors12.png)
*Token 选择*

`Sight` 字段已经自动填充。你可能也注意到，Actor 中并不会直接显示 `Range` 的值。这些值是直接链接到单位上的，所以现在 `Range` 字段相当于手动覆盖项。一般来说，不应在使用 Token 的同时再去手动设置它。

接下来，定位到“事件”字段并双击，打开 Actor 事件子编辑器。此时应该只会有一个 `ActorOrphan` 事件，它负责在该 Range Actor 在编辑器中变成孤儿时进行清理。右键点击白色区域，然后选择 `Add Event`。

[![创建 Actor 事件](./resources/062_Range_Actors13.png)](./resources/062_Range_Actors13.png)
*创建 Actor 事件*

将新事件的 `Msg Type` 设为 `Unit Birth`，并把 `Source Name` 设为 `Overseer`。然后把它的消息设为 `Create`。如下图所示。

[![设置创建事件](./resources/062_Range_Actors14.png)](./resources/062_Range_Actors14.png)
*设置创建事件*

这个事件与消息的组合，会在监督者创建时一并创建这个 Range Actor。这意味着监督者出现时，范围指示器就会始终显示。接着再创建一个事件，把它的 `Msg Type` 设为 `Unit Death`，同时把 `Source Name` 也设为 `Overseer`，消息设为 `Destroy`。这样在监督者死亡时，范围指示器就会被销毁并移除。完成后的 Actor 事件应如下所示。

[![完成的 Actor 事件](./resources/062_Range_Actors15.png)](./resources/062_Range_Actors15.png)
*完成的 Actor 事件*

点击 `Ok` 保存 Actor 事件，然后回到数据编辑器主视图。到这一步，范围指示器实际上已经可以工作了，但你还可以做一个小调整来提升可见性。选中 `Icon Arc Length` 字段，将其值设为 `1.125`。这相当于把基础值减半，从而让圆周上显示的范围图标数量翻倍。

如果你现在测试这个 Range Actor，结果应该已经正确。不过，为了更清楚地展示这类 Actor 的效果，下一步我们会再做一个第二范围指示器，并把它也附到监督者身上。你可以先选中 `Overseer Sight Range` Actor，再右键选择 `Duplicate Actor`，快速完成这件事。界面如下所示。

[![复制 Range Actor](./resources/062_Range_Actors16.png)](./resources/062_Range_Actors16.png)
*复制 Range Actor*

这会弹出 `Duplicate Actor` 窗口，其中只有一个 `Overseer Sight Range` 条目。

![复制窗口](./resources/062_Range_Actors17.png)
*复制窗口*

复制 Actor 可能会非常混乱，因为它常常会连带复制许多与之关联的 Actor。不过这里的 Range Actor 是一个独立、单一用途的 Actor，所以可以放心复制。确认窗口中勾选了 `Overseer Sight Range`，然后点击 `Ok`。

编辑器中会生成一个名为 `Overseer Sight Range Copy` 的副本。选中这个新 Actor 并双击，修改它的属性。在弹出的 `Actor Properties` 窗口中，把名称改为 `Overseer Sight Facing`，然后点击 `Suggest` 生成 ID。确认窗口中的值与下图一致后，点击 `Ok`。

![复制窗口](./resources/062_Range_Actors18.png)
*复制窗口*

选中 `Overseer Sight Facing` Actor，找到它的 `Arc` 字段并把值改为 `90`。这样该 Actor 就只会在监督者前方投射四分之一圆，更强调单位当前朝向。接着选中 `Sight` 字段，右键并依次选择 `Reset to Parent Value ▶︎ [Core.SCMod] CActorRange`。如下图所示。

[![重置 Actor 的 Sight 字段](./resources/062_Range_Actors19.png)](./resources/062_Range_Actors19.png)
*重置 Actor 的 Sight 字段*

这样一来，这个表示朝向的 Actor 的范围就与监督者 Token 解绑了。现在请创建一个新值：选中 `Range` 字段，将其设为 `8`。这样你就能把它与 `Overseer Sight Range` 指示器区分开来；否则两个指示器会在同一 `Range` 上绘制，彼此完全重叠。

最后一步，选中 `Icon Tint` 字段并双击，打开 `Object Values` 窗口。点击彩色框打开取色器，把颜色设为黄色，也就是 `R255 G255 B0`，然后点击 `Ok`。Alpha 保持 `255` 不变，再点击一次 `Ok` 完成图标染色。

[![选择图标染色](./resources/062_Range_Actors20.png)](./resources/062_Range_Actors20.png)
*选择图标染色*

现在，请对照下方确认这两个构建完成的 Actor 字段。左边是 `Overseer Sight Range`，右边是 `Overseer Sight Facing`。

![](./resources/062_Range_Actors21.png)
*Overseer Sight Range 字段 -- Overseer Sight Facing 字段*

至此地图已经完成。如你所见，监督者现在拥有两个独立的 Range Actor：一个用白色在其视野范围处绘制指示器，另一个用黄色绘制一个表示其朝向的指示器。测试地图时就能看到这两个 Actor 实际运作。使用 `Test Document` 进行测试，应会得到类似下图的结果。

[![自定义监督者范围指示器](./resources/062_Range_Actors22.png)](./resources/062_Range_Actors22.png)
*自定义监督者范围指示器*

## 附件

 * [062_Range_Actor_Completed.SC2Map](./maps/062_Range_Actors_Completed.SC2Map)
 * [062_Range_Actor_Start.SC2Map](./maps/062_Range_Actors_Start.SC2Map)
