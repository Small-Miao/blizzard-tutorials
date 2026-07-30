# 创建地图

任何项目的第一步都是创建一个新文档，也就是地图或模组。创建文档时，你会看到一组选项，帮助你快速完成项目配置。这些初始配置日后都可以在编辑器内部修改。

## 地图创建选项

启动编辑器后，前往任意编辑器界面左上角的文件标签。

[![File Menu](./resources/005_Creating_a_Map01.png)](./resources/005_Creating_a_Map01.png)
*文件菜单*

这会打开“新建文档”窗口，你可以在其中选择想创建的文档类型。

[![Choosing Document Type](./resources/005_Creating_a_Map02.png)](./resources/005_Creating_a_Map02.png)
*选择文档类型*

在本示例中，点击 “Arcade 地图” 左侧按钮选中它，然后点击 “Next” 继续。接下来你会进入依赖项界面，如下图所示。

[![Choosing 依赖项](./resources/005_Creating_a_Map03.png)](./resources/005_Creating_a_Map03.png)
*选择依赖项*

依赖项用于说明项目将从哪些模组文件获取资源。你始终可以进入 “Custom” 分类并保持不选任何内容，这样就会得到一个完全空白、没有任何模组依赖项的项目。在本示例中，你将使用庞大的《星际争霸》资源库，也就是通常所说的“标准依赖项”。能够访问标准依赖项，是使用编辑器最重要的优势之一。

若要导入最新一版《星际争霸》资源，请选择 “虫群之心” 标准依赖项，然后点击 “Next” 继续。这会带你进入地图配置界面，如下图所示。

## 地图配置

[![Example Generation Options](./resources/005_Creating_a_Map04.png)](./resources/005_Creating_a_Map04.png)
*示例生成选项*

地图配置界面是你为初始地形外观做出基础决定的地方。各项选项的作用如下表所示。

| 属性 | 作用 |
| ---- | ---- |
| Dimensions (Width x Height) | 设置地图初始完整尺寸。之后你可以在 Map Bounds 选项中再次修改。尺寸范围为 32 到 256，步进为 8。 |
| Playable Size | 地图中单位实际可寻路的区域大小。该数值会根据地图四周一段写死的不可玩缓冲区进行修正。在某些极小尺寸下不会使用这一缓冲区。 |
| Size Description | 对地图尺寸的基础描述。可选项包括 Tiny、Small、Medium、Huge 和 Epic。 |
| Use Terrain | 取消勾选会禁用地形生成。虽然有些情况下你可能需要无地形地图，但它在功能上与模组非常接近。 |
| 纹理集 | 选择地图的 “地形类型”，也就是构建地表时使用的八纹理调色板。它还会影响菌毯视觉、悬崖类型、光照和环境音效组。 |
| Initial Texture | 生成时，整张地形都会先刷成当前地块集中的这一种纹理。 |
| Base Height | 所有地形都会以此默认高度生成。如果未勾选 Add Random Height，地形将呈现为平滑表面。 |
| Add Random Height | 在 Base Height 的基础上制造随机起伏，幅度由 Strength 和 Variability 滑块决定。这有助于生成更自然的地形底稿。 |

按照上方“示例生成指南”中的设置，你应当得到一张如下图所示的地图。这张地图已经适合作为项目起点。此时通常最好先保存文件。

[![Newly Generated Char Rock](./resources/005_Creating_a_Map05.png)](./resources/005_Creating_a_Map05.png)
*新生成的 Char Rock*
