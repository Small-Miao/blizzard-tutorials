# 纹理集

地图地形上可用的纹理会被组织成一个个集合。从历史上看，这些集合被称为 tileset，这是 RTS 类型诞生于 2D 时代所留下的叫法。而在《星际争霸 II》编辑器中，它们现在被称为 纹理集，强调这些集合是由单独的纹理资源组成的。这个命名体系还会继续延伸：在数据编辑器里，纹理集对应的数据类型叫作 地形类型。任何玩过战役或竞技《星际争霸》的人，大概都对预制地块集的名字不陌生，比如：Bel'Shir、Shakuras 和 Char。下面展示的是其中一个纹理集在地形面板中的样子。

![Bel'Shir 纹理集](./resources/030_Texture_Sets1.png)
*Bel'Shir 纹理集*

出于性能原因，纹理集/Type 的组成受到一定限制。地形编辑器提供了强大的地表绘制能力，允许在其纹理调色板之间进行大量混合。因此，在任意时刻可用的纹理范围必须受到约束。幸运的是，现有调色板可以被重新配置，也可以完全自定义创建新的纹理集，从而为特定项目提供更丰富的纹理选择。

## 地形纹理

地形类型 由八种单独纹理组成，它们在数据中存储为 地形纹理。这个类型最初来自原始纹理文件，随后会为其注入多个字段。这样一来，纹理就会拥有与物理、光照和植被类型等相关的正确信息，从而具备在游戏中使用的资格。下表列出了一些 地形纹理 类型的重要字段。

| 属性 | 说明 |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Physics Material | 该纹理空间在发生某些事件时，会向 actor 系统报告的材质类型。例如在触发编辑器中，actor 可以依据纹理的 Physics Material 播放不同音效或使用替代模型。 |
| 装饰物 | 决定地形面板的 Generate Foliage 功能会在该纹理上生成哪类植被。 |
| Texture | 设置 地形纹理 可见的基础纹理。它决定纹理的外观，但如果该纹理还需要法线贴图，那么编辑器中的预览未必总能完全反映游戏内的最终效果。 |
| Normal Map | 设置法线贴图，它负责控制反射颜色和游戏光照数据。若缺少法线贴图，通常会导致图形错误。 |

## 创建自定义地形纹理

首先，创建一张新的 Arcade 地图，并将其默认纹理集设为 `Bel'Shir (Jungle)`。本教程中你不需要额外指定其他地图属性。然后通过 `模块 ▶︎ Data` 切换到数据编辑器。

在 Data 中，打开 terrain textures 标签，并在对象列表中右键选择 `Add 地形纹理` 来创建一个地形纹理。如下图所示。

[![Creating 地形纹理](./resources/030_Texture_Sets2.png)](./resources/030_Texture_Sets2.png)
*创建地形纹理*

这会打开 “地形纹理 Properties” 窗口。在 `Name` 中输入 `Demo Texture`，然后点击 `Suggest` 生成 ID。完成后的创建界面应如下图所示。

[![Creating 地形纹理](./resources/030_Texture_Sets3.png)](./resources/030_Texture_Sets3.png)
*创建地形纹理*

点击 “Ok” 创建该地形纹理，然后开始设置它的字段。此时，你应如下图所示，双击 `Texture` 字段来选择原始纹理。

![Creating 地形纹理](./resources/030_Texture_Sets4.png)
*创建地形纹理*

打开 `Texture` 字段后，会弹出一个带有浏览选项的“对象值”窗口。点击 `Browse` 按钮，会进入 Archive Browser，在那里你可以选择纹理。使用搜索功能快速找到 `lavasilvertoprock.dds`，然后点击 “Ok” 完成选择。

![Selecting 地形纹理](./resources/030_Texture_Sets5.png)
*选择地形纹理*

除了基础纹理外，你还需要为它提供对应的法线贴图。在 `Normal Map` 字段右侧的单元格中双击，并重复与 `Texture` 字段相同的操作。这次请找到 `texturelavasilvertoprock_nrm.dds`，然后点击 “OK” 选中它。

[![Selecting 地形纹理 Normal Map](./resources/030_Texture_Sets6.png)](./resources/030_Texture_Sets6.png)
*选择地形纹理法线贴图*

请注意，纹理名中的后缀 `_nrm` 表明这就是该纹理对应的法线贴图。不过也值得一提的是，编辑器中有些法线贴图会使用 `_normal` 或 `_norm` 结尾。如果编辑器找不到与某纹理对应的法线贴图，你应当在 `Normal Map` 字段中直接选择和 `Texture` 字段相同的纹理文件。如果完全不提供法线贴图，地图可能会出现图形问题，因为引擎会缺少某些光照和颜色信息。

## 设置自定义植被

通过地形编辑器中的 `Generate Foliage` 功能，你可以让处于 `Allow Foliage` 区域内的每种地形纹理，都生成其 `装饰物` 字段所关联的植被类型。为正在制作的纹理补上这项功能会很有教育意义。要为你的新纹理添加植被支持，请双击 `装饰物 - ` 标题右侧的单元格。

[![Foliage 装饰物 Input](./resources/030_Texture_Sets7.png)](./resources/030_Texture_Sets7.png)
*植被装饰物输入*

在刚刚打开的“对象值”窗口中，点击绿色 `+` 按钮，向该地形纹理添加一个新的植被 装饰物。这样会加入一个空白 装饰物。你可以点击 `Choose` 按钮来定义它。点击后会打开一个弹窗，允许你从项目数据中选择任意 Model 类型。操作如下图所示。

[![Foliage 装饰物 Selection](./resources/030_Texture_Sets8.png)](./resources/030_Texture_Sets8.png)
*植被装饰物选择*

请选择 `Shakuras Tree` 模型并点击 “Ok”。此时，这种地形纹理就已经支持植被生成了。每当这块纹理处在 `Allow Foliage` 区域内，激活地形编辑器中的 `Generate Foliage` 功能后，就会按照 Terrain Palette 中 Density (Per Cell) 的设置数量生成该 装饰物。

植被通常在带有一定随机性的情况下看起来会更自然，因为这样更接近真实环境。这个菜单中有几个选项可以帮助你做到这一点，其中之一就是名为 `Random Rotation` 的复选框。启用它后，每个 装饰物 模型在生成时都会随机朝向一个方向。勾选 `Random Rotation`，然后点击 “OK” 完成设置。

![Completed 装饰物 Input](./resources/030_Texture_Sets9.png)
*完成后的装饰物输入*

本文没有展开的另一个选项，是在这个 装饰物 列表中加入多种可能的植被模型，以确保最终生成时分布出不同种类。之后你还可以设置每种 装饰物 的 Probability，以调整它们的生成比例。不过在本练习中，只保留当前已选择的单一模型即可。

## 修改纹理集

现在你的 地形纹理 已经准备好了，接下来需要把它加入地图当前所使用的纹理集中。还记得你为这张地图选择的是 `Bel'Shir (Jungle)`。如前所述，这些集合在数据编辑器内部是通过 地形类型 数据类型来管理的。你当然可以完全创建一个全新的 terrain type，不过在当前示例中，只需修改当前地图所用的地块集，把 `Demo Texture` 替换掉默认八种纹理中的其中一种即可。

通过 `+ ▶︎ Edit Terrain Data ▶︎ 地形类型` 打开 地形类型 标签，然后在对象列表中找到已有类型 `Bel'Shir (jungle)`。高亮它以查看字段，然后如下图所示，双击 `Texture -- Blend` 来替换其中一项纹理。

[![Altering Bel'Shir 地形类型](./resources/030_Texture_Sets10.png)](./resources/030_Texture_Sets10.png)
*修改 Bel'Shir 地形类型*

这会打开一个标注为 `纹理 -- Blend` 的“对象值”窗口。

[![地形类型 Definition View](./resources/030_Texture_Sets11.png)](./resources/030_Texture_Sets11.png)
*地形类型 定义视图*

这个编辑器会以并排的两个列表显示全部可用 地形纹理 所组成的 `纹理库`，以及当前选中的 地形类型。中间的控制按钮允许你在纹理库和纹理集之间添加、移除、调整顺序和替换纹理。此外，你还可以使用 `预览` 按钮查看 `Demo Texture` 将基础纹理和法线贴图合并后的效果。请在 `纹理库` 中选中 `Demo Texture`，然后在 `纹理集` 中高亮 `Bel'Shir Dirt Light`，再点击 `Replace`。这样就会完成替换，如下图所示。

[![Swapped 地形纹理](./resources/030_Texture_Sets12.png)](./resources/030_Texture_Sets12.png)
*已替换的地形纹理*

在这个阶段点击 “Ok”，就会完成 地形类型 更新，并结束本练习。

## 测试自定义纹理集

回到地形编辑器，并切换到 Terrain Palette 中的纹理刷工具。你应该会在纹理集最左侧位置看到 `Demo Texture` 的缩略图预览，如下图所示。

![](./resources/030_Texture_Sets13.png)
*地形面板中的自定义纹理集*

现在这块纹理已经可以被绘制到地形上了，你可以开始自由尝试。

[![Volcanic Ash in Bel'Shir](./resources/030_Texture_Sets14.png)](./resources/030_Texture_Sets14.png)
*Bel'Shir 中的火山灰纹理*

你还可以通过先铺设一片这种纹理，再使用 `Allow Foliage` 选项标记该区域，来测试新纹理的植被能力。点击 Generate Foliage 后，结果应与下图类似。

[![Generated Shakuras Tree Foliage](./resources/030_Texture_Sets15.png)](./resources/030_Texture_Sets15.png)
*生成的 Shakuras Tree 植被*

## 附件

 * [030_Texture_Sets.SC2Map](./maps/030_Texture_Sets.SC2Map)
