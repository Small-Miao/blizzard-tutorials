# 事件宏

和任何逻辑系统一样，Actor 事件里也有许多会反复执行的常见流程。在触发器编辑器中，这个问题通常通过把重复任务抽成定义，再在需要之处统一复用来解决。这样能为你节省大量时间和精力。事件宏则是 Actor 事件中实现相同目的的方案。

宏是一种携带事件集合的 Actor。你可以把宏链接到任意数量 Actor 的“宏”字段中。以这种方式承载宏，会让宏中的事件与宿主 Actor 的事件合并。通过共享这个宏，它就能成为一套通用事件的来源，相当于一个可复用定义。下图展示了事件宏的典型视图。

[![事件宏列表](./resources/065_Event_Macros1.png)](./resources/065_Event_Macros1.png)
*事件宏列表*

事件宏能让处理 Actor 事件变得更容易。把高频出现的事件拆分出去，有助于保持项目整洁。事件宏还能避免错误悄悄混入项目，因为你只需要更新这一处定义，而不必在一大批 Actor 之间来回逐个修改。

## 事件宏详情

事件宏本身并没有太多可配置元素。虽然它看起来似乎有很多字段，但大多数都是因为它作为 Actor 所继承而来的默认项。当前最值得关注的字段是“事件”。查看宏的“事件”字段，会打开如下所示的 Actor 事件子编辑器。

[![事件宏列表](./resources/065_Event_Macros2.png)](./resources/065_Event_Macros2.png)
*事件宏事件*

在宏“事件”字段中定义的元素，构成了它的核心定义。宏被挂载后，这个定义里的全部事件、术语和消息都会转移到宿主 Actor 的事件中。值得注意的是，这些事件实际上并不会直接显示在宿主的“事件”字段里。

## 演示事件宏

打开本文提供的演示地图。场景中有一组建筑和几名幽灵，站在一个陨坑中，如下图所示。

[![演示地图场景](./resources/065_Event_Macros3.png)](./resources/065_Event_Macros3.png)
*演示地图场景*

在这个练习中，你要修改地图，让所有正在受攻击的对象都播放一种特殊的受伤动画。场景中的每个单位都会得到这个新动画。借助事件宏，你只需写一份定义，就能很快把这套新行为推送到四种单位类型上。

先进入数据编辑器中的 Actor 标签页。如果该标签页不可见，可通过 `+ ▶︎ Edit Actor Data ▶︎ Actor` 打开。然后在主视图中右键选择 `Add Actor` 创建一个新 Actor。会弹出如下窗口。

![创建事件宏](./resources/065_Event_Macros4.png)
*创建事件宏*

将 Actor 命名为 `Damage Response Macro`，然后点击 `Suggest` 生成 ID。把 Actor 类型设为 `Event Macro`，再点击 `Ok`。随后你会看到如下视图。

[![创建事件宏](./resources/065_Event_Macros5.png)](./resources/065_Event_Macros5.png)
*创建事件宏*

在 Actor 标签页中选中新建的宏，定位到它的“事件”字段并双击打开。这会启动 Actor 事件子编辑器。右键点击白色区域，选择 `Add Event`，如下图所示。

[![添加 Actor 事件](./resources/065_Event_Macros6.png)](./resources/065_Event_Macros6.png)
*添加 Actor 事件*

这样会创建一个事件，图标是旗帜；同时还会创建一条 Actor 消息，图标是场记板。你可以选中这个视图中的任意元素，然后通过最右侧面板的 `Msg Type` 来修改它的类型。现在请选中 `ActionDamage` 事件，并用 `Msg Type` 下拉框把它改成 `Unit Damaged` 事件。

[![配置 Actor 事件](./resources/065_Event_Macros7.png)](./resources/065_Event_Macros7.png)
*配置 Actor 事件*

选中 `ActorDamagePhysics` 消息，并使用下拉框把它改为 `Set Tint Color`。这样会展开一系列子选项，让你自定义染色效果。把 `Color` 设为黄色，也就是 `R255 G255 B128`，把 `HDR Multiplier` 设为 `4.0`，再把 `Blend Duration` 设为 `0.04`。这条事件与消息结合后，会让 Actor 在受到攻击时快速变成一种很亮的黄色。最后，把消息的 `Label` 设为 `DamageResponseColor`，方便之后引用。你的事件列表应如下图所示。

[![完成的染色事件](./resources/065_Event_Macros8.png)](./resources/065_Event_Macros8.png)
*完成的染色事件*

再添加一个类型为 `Unit Damaged Event` 的事件，并把它的消息设为 `Timer Set`。将计时器的 `Duration Base` 设为 `0.12`，再把它的 `Name` 设为 `Damage Response Timer`。

[![完成的计时器事件](./resources/065_Event_Macros9.png)](./resources/065_Event_Macros9.png)
*完成的计时器事件*

最后再添加一个事件，并把它设为 `Timer Expired` 类型。选中该事件，右键并选择 `Add Term`。使用下拉框把术语设为 `TimerName`，再将其 `Name` 字段设为 `DamageResponseTimer`。然后把这个事件的消息设为 `Clear Tint Color`，把消息属性 `Blend Out Duration` 设为 `0.08`，`Label` 设为 `DamageResponseColor`。完成后的列表应如下图所示。

[![完成的 Actor 事件](./resources/065_Event_Macros10.png)](./resources/065_Event_Macros10.png)
*完成的 Actor 事件*

现在，这个宏应会产生如下行为：当对象受伤时，Actor 会变黄并启动一个计时器；计时器到时后，会被另一条事件检测到，并通过事件术语进行确认；随后触发 Actor 恢复正常颜色。颜色快速亮起又熄灭，就形成了每次攻击触发一次的闪光效果。这个闪烁持续时间大致等于计时器长度。你现在可以把这个宏安装到任意 Actor 上，把这些效果赋予它。点击 `Ok` 保存事件并返回数据编辑器主视图。

在 Actor 标签页中找到 `Bunker` Actor，选中它的“宏”字段。双击后会打开 `Object Values` 窗口，里面会列出该 Actor 当前已挂载的宏。点击绿色 `+` 把新宏加入列表。这会打开 `Object Value (Array)` 窗口，你可以在其中选择要添加的宏。点击 `Choose`，即可看到当前项目中所有 `Event Macro` Actor 的列表。选择 `Damage Response Macro`，然后依次确认这三个窗口。界面如下图所示。

[![为 Actor 添加宏](./resources/065_Event_Macros11.png)](./resources/065_Event_Macros11.png)
*为 Actor 添加宏*

现在对 `Pylon`、`Forge` 和 `Ghost` Actor 重复这一流程，练习就完成了。游戏已经把这个事件宏，也就是被攻击时闪光的行为，附加到了演示场景中的每个单位上。现在你可以测试地图，命令幽灵攻击任意目标，即可看到效果生效。

[![宏照亮受伤反馈](./resources/065_Event_Macros12.png)](./resources/065_Event_Macros12.png)
*宏照亮受伤反馈*

## 附件

 * [065_Event_Macros_Start.SC2Map](./maps/065_Event_Macros_Start.SC2Map)
 * [065_Event_Macros_Completed.SC2Map](./maps/065_Event_Macros_Completed.SC2Map)
