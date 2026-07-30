# 制图：美术与性能最佳实践

![Mapmaking: Best Practices in Art and Performance](./resources/mapmaking-best-practices01.jpg)

上周，我们 Team Liquid 的朋友们宣布了第 7 届 Team Liquid 地图竞赛！我们非常期待看到社区里的创作者们在接下来的天梯赛季里，为大家带来更多新鲜有趣的地图。

随着整个社区的制图热情不断升温，我们也想借这个机会，分享一些我们认为所有社区制图作者都值得了解的最佳实践。这些建议全部聚焦于地图制作的美术层面；合理运用后，不仅能让地图看起来更出色，也能让它们在玩家运行《星际争霸 II》的各种不同电脑配置上保持更好的表现。那么话不多说，我们先从 装饰物 开始，也就是制图作者可用的各种建筑、植被和装饰模型。

## 装饰物

[![img](./resources/mapmaking-best-practices02.jpg)](./resources/mapmaking-best-practices02.jpg)

**图 A：** 这个场景里有 200 棵树。虽然画面看起来不错，但对性能的压力完全没必要地偏高。很多树彼此重叠，或者被其他树挡住了。

**图 B：** 这里的树只有一半数量，但每棵树之间留出了更多空间，重叠也更少，同时整体观感依旧保留。这对性能余量较小的玩家来说非常重要。



[![img](./resources/mapmaking-best-practices03.jpg)](./resources/mapmaking-best-practices03.jpg)

**图 A：** 这些 装饰物 其实隐藏着额外成本……

**图 B：** 当地形被隐藏后，你可以看到 装饰物 被埋进地形以下的那一部分。像这样的大型 装饰物 仍然会被*完整*渲染，并带来很高的 overdraw。



[![img](./resources/mapmaking-best-practices04.jpg)](./resources/mapmaking-best-practices04.jpg)

**图 A：** 为什么 overdraw 很糟糕？这里有一棵靠近水面的巨树，而水面本来就是渲染成本较高的内容。你可以看到它在水面上的阴影。

**图 B：** 在这张图里你会发现，*即使把镜头移开*，树已经不在屏幕内了，它的阴影仍然存在。这意味着这棵树依旧在被渲染。

[![img](./resources/mapmaking-best-practices05.jpg)](./resources/mapmaking-best-practices05.jpg)

**图 A：** 把物体均匀摆放在区域内，视觉上也许更自然，但这可能遮挡单位，或者给某些单位带来不公平优势。

**图 B：** 尽量保持实际交战区域的视线和移动空间畅通，把大部分 装饰物 放在不可行走区域，或者你本来就不希望玩家前往的区域。



------

## 光照与阴影

[![img](./resources/mapmaking-best-practices06.jpg)](./resources/mapmaking-best-practices06.jpg)

在制作关卡时，应通过控制整体配色，让 装饰物、雾效和光照在色彩上彼此协调。颜色过多会分散玩家对战斗的注意力，把焦点从单位本身上拉走。



[![img](./resources/mapmaking-best-practices07.jpg)](./resources/mapmaking-best-practices07.jpg)

为关卡布光时，最高优先级应当是 **单位辨识度**。左图展示的是 MarSara 光照，这是一个不错的起点。之后你可以在此基础上继续调整设置，为关卡营造特定氛围和情绪。右图的气氛感更强，但单位依然清晰可辨。



[![img](./resources/mapmaking-best-practices08.jpg)](./resources/mapmaking-best-practices08.jpg)

地图上凡是出现 装饰物 的地方，都要记得绘制寻路。这样可以避免单位钻进模型几何体里。也包括填补一些会让跳虫之类小型单位卡住的缝隙。按 H 键即可进入寻路菜单。



[![img](./resources/mapmaking-best-practices09.jpg)](./resources/mapmaking-best-practices09.jpg)

生成植被并为关卡构建阴影，能让地图整体看起来更加完整精致。这两个功能都可以在顶部任务栏中的数据菜单里找到。按 Ctrl+Shift+L 可以构建阴影。

请记住，如果你在关卡中移动了任何 装饰物，阴影贴图就会失效，之后必须重新构建光照。

------

## 绘制与特效

[![img](./resources/mapmaking-best-practices10.jpg)](./resources/mapmaking-best-practices10.jpg)

**图 A：** 在这张图里，玩家不太容易判断地图上应该把单位往哪个方向派。

**图 B：** 这里则通过地表痕迹与 装饰物 的摆放，明确地为玩家引导了路径。这种做法有助于玩家理解前进方向，并快速定位地图中的重要区域。



[![img](./resources/mapmaking-best-practices11.jpg)](./resources/mapmaking-best-practices11.jpg)

**图 A：** 在这张图里，玩家较难判断自己什么时候正在接近更高或更低的地形层级。这是地图中需要始终清楚传达的重要信息。

**图 B：** 这里我们通过带有不同色彩层次的悬崖纹理，清楚地区分了不同高度的悬崖层级。这能帮助玩家判断自己正在接近高地还是低地。



[![img](./resources/mapmaking-best-practices12.jpg)](./resources/mapmaking-best-practices12.jpg)

**图 A：** 添加粒子效果时，请考虑的不只是你自己机器上的表现。过多烟雾会造成粒子 overdraw，*大幅*降低地图性能。

**图 B：** 在这张图里，我们依然保留了“小型被摧毁飞行器前哨”的感觉（甚至可能更强了），但性能成本明显更低。



[![img](./resources/mapmaking-best-practices13.jpg)](./resources/mapmaking-best-practices13.jpg)

**图 A：** 别忘了补上美术层面的质感！这张图里的可摧毁岩石和有机 装饰物 直接立在过于干净的沙地上，显得非常不协调。

**图 B：** 尽量在 装饰物 下方绘制符合场景语境的纹理。这张图中，同样的岩石放在岩石泥土地表上，看起来就更自然，更像真正嵌入地形之中。

------

希望这些最佳实践建议，能在你今后的地图设计中派上用场！如果你有任何问题，或者希望对某些点进一步了解，欢迎在下方评论区提问。也欢迎经常回到本页查阅这些实践经验，帮助你更快、更轻松地把地图打磨到足够让世界各地玩家游玩的程度。我们很欣赏大家对制图的热情，也希望尽一切努力帮助你做出更好的地图。如果还有其他我们能帮上的地方，也请告诉我们。谢谢！
