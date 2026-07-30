# Battle.net 信息

Battle.net 信息是定制地图在 Arcade 中展示方式的核心位置。

## 常规

[![General Tab](./resources/011_BattleNet_Info01.png)](./resources/011_BattleNet_Info01.png)
*常规标签*

项目的图标和关联网站都在 General 标签下设置。网站链接会附加在游戏 Details 页面的底部，可用于跳转到社区网站。项目图标则会频繁出现在 Arcade 的各个位置，作为项目的主要代表元素，包括 Browse 页面、Open Games 列表以及许多其他位置。在下图中，你可以看到它出现在 Overview 页面里的游戏标题旁边。

[![Arcade Overview Screen](./resources/011_BattleNet_Info02.png)](./resources/011_BattleNet_Info02.png)
*Arcade 概览页面*

## 截图

Screenshots 标签允许你选择五张宣传截图，发布到项目的 Overview 页面中。

[![Screenshots Tab](./resources/011_BattleNet_Info03.png)](./resources/011_BattleNet_Info03.png)
*截图标签*

截图既可以通过 Archive Browser 从地图当前数据中上传，也可以从本地电脑上传。每张截图必须是 800x600 px 分辨率。如果截图尺寸超过默认分辨率，系统会提示你裁剪原图到所需大小。勾选 Available In Game 选项后，截图还会额外写入游戏数据中（代价是增加发布体积）。添加后，这些截图会填充到 “Overview” 页面中的 “Screenshots” 轮播区域，如下图所示。

[![Screenshots from Overview Screen](./resources/011_BattleNet_Info04.png)](./resources/011_BattleNet_Info04.png)
*Overview 页面中的截图*

## 玩法说明

你可以在 How To Play 标签下添加一份简短说明，介绍你的游戏该怎么玩。Arcade 中的玩家会在 “How To Play” 页面看到这些内容。玩家通常会在地图加载期间，或在大厅等待时查看这个页面。因此，这给了你一个在加载画面之前传达策略和玩法信息的机会。下图展示了如何构建 How To Play 内容。

[![How to Play Tab](./resources/011_BattleNet_Info05.png)](./resources/011_BattleNet_Info05.png)
*玩法说明标签*

填写 Basic Instructions、How To Win 和 Advanced Instructions 字段后，对应内容会出现在 “How To Play” 页面的相应区域中。如果你勾选 Use Bullets 选项，那么这些说明在 Arcade 中也会保持与 How To Play 标签中相同的项目符号样式。下图展示了这些描述在 Arcade 中的显示效果。

. [![图像](./resources/011_BattleNet_Info06.png)](./resources/011_BattleNet_Info06.png)

Arcade 玩法说明页面

## 玩法说明截图

How To Play Screenshots 标签的工作方式与 Screenshots 标签相同，用于向项目 Arcade 页面中的 “How To Play” 部分添加最多五张截图。

[![How to Play Screenshots Tab](./resources/011_BattleNet_Info07.png)](./resources/011_BattleNet_Info07.png)
*玩法说明截图标签*

这些截图应为 800x600 px，并且你可以为每张截图添加说明文字以补充上下文。添加后，截图会显示在 “How to Play” 页面最右侧区域，如下图所示。

![](./resources/011_BattleNet_Info08.png)
*Arcade 玩法说明页面中的玩法说明截图*

## 教学地图

Tutorial 标签可用于为项目指定一张教学地图。设置后，玩家可以通过 “Overview” 页面中的 “Play Tutorial” 按钮单独进入这张地图。教学地图可以为玩家提供一个安全、无压力的空间，学习主地图所需的关键技巧。玩家会看到如下图所示的教学地图入口。

[![Play Tutorial Button on Overview Screen](./resources/011_BattleNet_Info09.png)](./resources/011_BattleNet_Info09.png)
*Overview 页面中的 Play Tutorial 按钮*

Tutorial 标签本身如下图所示。

[![Tutorial Tab](./resources/011_BattleNet_Info10.png)](./resources/011_BattleNet_Info10.png)
*教学标签*

教学地图通过 Tutorial Map File 关联。这里有三种可选方式。选择 Self 会把当前地图的 Tutorial Game Variant 作为教学内容。选择 Other 则允许你指定另一张地图文件，无论它保存在本地还是 Battle.net 上，都可以作为教学地图。你也可以选择 None，这会从 “Overview” 页面移除 “Play Tutorial” 按钮。

Tutorial Game Speed 选项允许你独立于主地图来设置教学地图的游戏速度。

## 补丁说明

Patch Notes 标签允许你为项目的每个版本添加补丁说明。这些内容会公开显示在 Arcade 的 “Patch Notes” 页面上。用户可以借此了解重要更新和平衡改动，同时判断地图是否仍在持续维护。

[![Patch Notes Tab](./resources/011_BattleNet_Info11.png)](./resources/011_BattleNet_Info11.png)
*补丁说明标签*

每条 “Patch Notes” 记录都包含 Version、Date 和一组 Notes。补丁总是按 Version 编号排序显示，最新版本在最上方。每组 Notes 最多可有 100 行，而每一行不能超过 140 个字符。你可以手动设置 Version 和 Date，并不要求必须与 Arcade 发布系统自动生成的值一致。

[![Arcade Patch Notes Screen](./resources/011_BattleNet_Info12.png)](./resources/011_BattleNet_Info12.png)
*Arcade 补丁说明页面*
