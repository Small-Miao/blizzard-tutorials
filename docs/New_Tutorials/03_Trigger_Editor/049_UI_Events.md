# UI 事件

《星际争霸 II》究竟如何响应玩家输入，有时会让人觉得有些不透明、难以触及。尽管编辑器已经提供了大量控制能力，但在硬件、操作系统和底层引擎共同作用下，仍有许多因素不容易直接观察。为了减轻这种“封闭感”，引擎提供了若干方式，让你能够介入“玩家输入电脑”与“游戏作出反应”之间的过程。在触发层面，重点就是一组响应玩家输入的事件，统称为 UI 事件。你可以在创建事件时按 'UI' 标签筛选看到它们，如下图所示。

[![UI 事件列表](./resources/049_UI_Events1.png)](./resources/049_UI_Events1.png)
*UI 事件列表*

下表对这些事件进行了说明。

| 事件 | 说明 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Abort Mission | 当某个 Player 在游戏菜单中点击 'Abort Mission' 按钮时触发。 |
| Button Pressed | 当某个 Player 点击命令卡中的某个 Button 时触发。该按钮可通过 Button Pressed 函数识别。这是一个性能开销较大的事件。 |
| Custom Dialog Dismissed | 当某个 Player 以某个 Result 关闭对话框时触发，结果可以是 Yes、No 或 Any。这里可用的标识函数包括用于结果的 Custom Dialog Result，以及用于玩家的 Triggering Player。 |
| Game Credits Finished | 当某个 Player 的游戏结尾字幕滚动结束时触发。 |
| Game Menu Item Selected | 当某个 Player 选择某个游戏菜单项时触发，例如 Save Button、Quit Button、Abort Mission 或 Achievements Button。这里可用的标识函数包括 Game Menu Selected 和 Triggering Player。 |
| Hotkey Pressed | 当某个 Player 将某个 Hotkey 置于特定状态时触发，状态可以是 Down 或 Up。Hotkey 包括多种工具类快捷键，每个都绑定到单个键位，例如 Army Select、Idle Worker 和 Warp-in。这里可用的标识函数包括用于 Hotkey 的 Hotkey Pressed，以及用于玩家的 Triggering Player。注意，如果处理该快捷键按下的代码已经存在，那么该快捷键就不会再触发此事件。这是一个性能开销较大的事件。 |
| Key Pressed | 当某个 Player 将某个 Key 置于特定状态时触发，状态可以是 Down 或 Up，并且支持 Shift、Control、Alt 等修饰键。每次按键都可通过 Key Pressed、Shift Key Pressed、Control Key Pressed 和 Alt Key Pressed 等函数来识别。这是一个性能开销较大的事件。 |
| Mouse Clicked | 当某个 Player 将某个 Mouse Button 置于特定状态时触发，状态可以是 Down 或 Up。可用鼠标键包括 Left、Right 和 Middle。点击本身可通过 Mouse Clicked Button 识别，而 Mouse Clicked UI Pos X/Y 与 Mouse Clicked World Pos X/Y/Z 则可分别识别屏幕空间或游戏空间中的坐标。这是一个性能开销较大的事件。 |
| Mouse Moved | 当某个 Player 移动鼠标时触发。Mouse Clicked UI Pos X/Y 与 Mouse Clicked World Pos X/Y/Z 这些函数可分别识别其在屏幕空间或游戏空间中的坐标。这是一个性能开销较大的事件。 |
| Mouse Wheel | 当某个 Player 滚动鼠标滚轮时触发。Mouse Wheel Moved 函数会返回该移动的幅度。这是一个性能开销较大的事件。 |
| Resources Requested | 当某个 Player 通过团队资源菜单请求资源时触发。 |
| Resources Traded | 当某个 Player 通过团队资源菜单向某个 Recipient Player 转移资源时触发。 |
| Target Mode Updated | 当某个 Player 将某个 Ability Command 的目标模式切换到某个 State 时触发，状态可以是 On、Off 或 Any。 |

## 性能注意事项

监视玩家输入可能会让某些 UI 事件在极短时间内触发非常多次。这主要体现在 Button Pressed、Hotkey Pressed、Key Pressed、Mouse Clicked、Mouse Moved 和 Mouse Wheel 这些事件上。因此，只有在确有必要时才应使用它们。如果必须使用，一种避免性能拖慢的技巧，是给负责监视的触发器配上一个条件，在不需要它时把事件逻辑封住，直到再次需要为止。下图就是一个示例。

![输入事件节流](./resources/049_UI_Events2.png)
*输入事件节流*

## 观察 UI 事件

每个 UI 事件都会响应某种玩家输入，并通常提供一些标识函数，帮助你描述这次输入以便进一步使用。对于鼠标事件尤其如此。鼠标事件是 UI 事件的一个子集，提供了大量标识函数来描述玩家的鼠标移动和鼠标按键操作。你可以在事件创建界面中搜索 'Mouse' 来查看它们，如下图所示。

[![鼠标 UI 事件](./resources/049_UI_Events3.png)](./resources/049_UI_Events3.png)
*鼠标 UI 事件*

在本文附带的演示地图中，你可以找到一些样例触发器，展示 UI 事件通常是如何使用的。打开地图并进入触发编辑器后，会看到如下界面。

[![地图触发编辑器视图](./resources/049_UI_Events4.png)](./resources/049_UI_Events4.png)
*地图触发编辑器视图*

地图中只有一个陆战队员，它会在初始化时被识别并存入变量。其余触发器则会利用玩家输入，构建一套把这个陆战队员直接绑定到鼠标上的控制方案。这当然与《星际争霸 II》传统 RTS 中可选择并控制大量单位的方式差别很大。为支持这套控制方式，这里还禁用了鼠标拖框选择，也就是 'box-selecting'。如果你查看 'mouseMove'、'mouseDown' 和 'mouseUP' 触发器，就会看到如下内容。

[![鼠标 UI 触发器](./resources/049_UI_Events5.png)](./resources/049_UI_Events5.png)
*鼠标 UI 触发器*

这套单位直接控制系统由两个 Mouse Clicked 事件 'mouseDOWN' 与 'mouseUP'，以及一个 Mouse Moved 事件 'mouseMove' 共同构成。其原理是：'mouseMove' 触发器持续监视鼠标位置，并通过 Mouse Moved World Pos X 与 Mouse Moved World Pos Y 两个标识函数记录该位置；当玩家点击鼠标时，'mouseDown' 事件会触发，并进入一个 While 循环，持续向该单位下达移动命令，目标点始终是鼠标上一次记录下来的位置。

只要玩家一直按住鼠标键，这个过程就会持续进行，于是单位会在这两个触发器的协同作用下持续跟随鼠标移动。当鼠标按键松开，也就是发生 'clicked up' 时，'mouseDown' 里的 While 循环会被打断，移动随之停止。测试地图后，你应当会看到类似下图的效果。

[![直接控制陆战队员](./resources/049_UI_Events6.png)](./resources/049_UI_Events6.png)
*直接控制陆战队员*

这正是你可能想介入玩家输入并使用 UI 事件的一个典型理由。这里，它被用来给玩家提供一种全新的游戏控制方式，而且体验往往很有趣。不过，要让这种移动保持平滑，就必须依赖大量事件。如果你打开触发调试器，就能更直观地看到这一点。

[![直接控制陆战队员](./resources/049_UI_Events7.png)](./resources/049_UI_Events7.png)
*直接控制陆战队员*

你会看到，仅仅几秒钟的持续鼠标监视，就导致触发了 250+ 次事件。这个数字足以让你对这类操作为何会在性能上变得非常昂贵形成直观印象。

## 附件

 * [049_UI_Events.SC2Map](./maps/049_UI_Events.SC2Map)
