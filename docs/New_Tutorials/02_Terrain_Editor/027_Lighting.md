# 光照

编辑器拥有一套非常强大的光照系统，你可以用它为项目创造丰富多样的氛围、外观和感觉。下图展示了同一个场景仅通过调整光照，就能实现出完全不同的表现。

[![Varied Lighting on the Same Scene](./resources/027_Lighting1.png)](./resources/027_Lighting1.png)
*同一场景上的不同光照效果*

## 光照细节

在《星际争霸》引擎中，光照实际上是通过多种不同方式共同完成的。你可能还记得路灯、矿灯之类的光源道具，或附着在角色身上的发光效果。有些法术与技能特效甚至还能以很有趣的方式动态照亮场景。不过，本文主要关注的是地图中的全局光照效果。

一张地图在任意时刻都会使用某一种光照风格，但它也可以在游戏过程中动态切换到其他风格。这些光照风格都存放在一个名为 灯光 的大型数据类型中，并由数据编辑器直接处理。由于光照相关可用选项数量庞大，编辑器还提供了一个专门的工具，叫作 Lighting Window。这个界面把所有光照控制项以及编辑器预置光照风格汇集到一起。

[![Varied Lighting on the Same Scene](./resources/027_Lighting2.png)](./resources/027_Lighting2.png)
*同一场景上的不同光照效果*

## 光照演示

打开本文附带的演示地图。你会看到一个酒吧门面场景，它当前使用的是看起来像自然日光的默认光照。首先，你将学习如何把光照从白天切换到夜晚；然后再学习如何利用触发器缓慢恢复白天光照，从而模拟日出。

最好的起点是数据编辑器。光照是基于当前 地形类型 来应用的。地形类型 是数据中控制地表纹理、装饰物风格等内容的地貌分类。该演示地图使用的是 `Agria (Jungle)` 类型。点击 “New Tab” 的 `+` 按钮，然后前往 `Edit Terrain Data ▶︎ 地形类型` 打开 “地形类型” 标签。接着如下图所示，双击 `Lighting` 属性来设置光照。

[![Lighting Data Property](./resources/027_Lighting3.png)](./resources/027_Lighting3.png)
*光照数据属性*

如果你不确定地图当前使用的是哪种 地形类型，可以通过 `地图 ▶︎ Map 纹理` 查看。字段 `Current 纹理集` 会给出答案，如下图所示。

![Checking 地形类型](./resources/027_Lighting4.png)
*检查 地形类型*

点击 `Lighting` 字段会打开“对象值”窗口。在其中找到 `Mar Sara Night Test` 光照，选中并点击 “Ok”。

![Selecting Lighting](./resources/027_Lighting5.png)
*选择光照*

要确认你已成功修改地形光照，请打开地形模块，并选择 `Render ▶︎ Show Lighting ▶︎ Game Lighting`。主视图此时应显示出一幅明显更暗、更接近黄昏的场景。原始光照设置与新光照之间的差异如下图所示。

[![Lighting Changes](./resources/027_Lighting6.png)](./resources/027_Lighting6.png)
*光照变化*

现在切换到触发编辑器，打开 `Initialization` 触发器。这个触发器里已经包含一些动作，用于移除游戏 UI、揭示整张地图供查看，以及应用标准镜头。在 “Actions” 标题下右键，前往 `新建 ▶︎ 动作`，添加一个新的 `Wait` 动作。将它的 `Time` 字段设为 2.0。然后用同样方法再添加 `Set Lighting` 动作。将其 `Light` 字段设为 `Mar Sara Day Test`，并把 `Blend Time` 改为 6.0。完成后你应当得到如下结果。

[![Prepared Trigger](./resources/027_Lighting7.png)](./resources/027_Lighting7.png)
*已准备好的触发器*

这样地图就完成了。如果你运行测试，在短暂等待后，当前光照风格 `Mar Sara Night Test` 会缓慢过渡成 `Mara Sara Day Test`，形成类似日出般的夜转昼光照变化。启动“测试地图”功能即可看到结果。场景会慢慢变化，类似下图所示的过程。

[![Lighting Changes Simulating a Sunrise](./resources/027_Lighting8.png)](./resources/027_Lighting8.png)
*模拟日出的光照变化*

## 附件

 * [027_Lighting_Completed.SC2Map](./maps/027_Lighting_Completed.SC2Map)
 * [027_Lighting_Start.SC2Map](./maps/027_Lighting_Start.SC2Map)
