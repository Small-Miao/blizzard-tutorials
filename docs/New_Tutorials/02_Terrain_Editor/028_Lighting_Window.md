# 光照窗口

光照窗口是一个用于在编辑器中修改和预览光照的界面。你可以用它来创建全新的光照方案，也可以直接利用游戏中已有的任意光照资源。如下图所示。

[![Lighting Window and its Use](./resources/028_Lighting_Window1.png)](./resources/028_Lighting_Window1.png)
*光照窗口及其用法*

你可以在编辑器任意位置通过 `Window ▶︎ Lighting` 打开光照窗口。游戏中的所有光照设置都集中在这里，并按 UI Lighting、Tileset Lighting、Portrait Lighting 和 Cinematics Lighting 等类别分组。在这里，你可以浏览和修改这些光照组，甚至可以在项目之间导入和导出光照。要这样做，请前往 `文件 ▶︎ Export Light` 或 `文件 ▶︎ Import Light`。在编辑器之外，这些文件会以 `.SC2Lighting` 格式保存。

由于光照窗口中的参数数量极多，修改光照可能会显得有些令人望而生畏。这些参数被分为十个类别：Global、Tone Mapping、Colorization、Variations、SSAO、Terrain、Regions、Key、Fill 和 Back。下面会分别介绍。

理解这些属性最简单的方法，是利用光照窗口在编辑器主视图中实时预览光照效果，因为每次更改都会动态更新。你可以先打开光照窗口，再切换到地形编辑器。然后通过 `Render ▶︎ Show Lighting ▶︎ Game Lighting` 让编辑器显示游戏光照。这样无论你选择哪一种光照，或对其进行何种修改，编辑器视图都会立刻响应，方便你不断测试和实验。

下面各节会逐一介绍光照标签中的各项功能，给出属性说明并展示部分效果预览。每张预览图的最左侧都是基础光照方案，即 `Agria (Jungle)` 光照。随后展示的各项变化会在图片说明中标明。请注意，这些变化彼此之间并不会叠加，每一项都是在基础光照上单独做出的调整。

## 全局

[![Global Lighting](./resources/028_Lighting_Window2.png)](./resources/028_Lighting_Window2.png)
*全局光照*

| 属性 | 说明 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ambient Color | 定义作用于所有表面的全局环境光颜色。 |
| Specular Multiplier | 设置反光表面反射出的光强度。 |
| Emissive Multiplier | 设置自发光纹理发出的光强度。 |
| Light Time of Day | 设置光照的时间值。它会在“Time of Day”光照中使用，通过一系列光照来模拟行星运转时发生的变化。 |
| Current Test Time | 设置编辑器预览中的当前时间。将其设为与 Light Time of Day 相同，可测试当前光照。 |

[![图像](./resources/028_Lighting_Window3.png)](./resources/028_Lighting_Window3.png)

基础 -- 红色环境光 -- 绿色环境光 -- 蓝色环境光 -- 高镜面反射 -- 高自发光 -- 低自发光

## 色调映射

[![Tone Mapping](./resources/028_Lighting_Window4.png)](./resources/028_Lighting_Window4.png)
*色调映射*

| 属性 | 说明 |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Exposure | 设置施加到场景中的整体光照量。 |
| Bloom Threshold | 设置开始应用泛光的最小亮度值。泛光是一种从明亮物体边缘向外溢出的光效。夜景中常见较低泛光阈值。 |
| Ambient Multiplier | 设置环境光强度。该项目前在编辑器中未启用。 |
| Diffusive Multiplier | 设置物体产生的白光量。 |
| White Point | 当 Method 设为 Reinhard 时，设置应用到场景中的亮度量。 |
| Method | 改变色调映射的应用方式。每种方法都是不同的算法，会影响亮度和对比度。 |

[![图像](./resources/028_Lighting_Window5.png)](./resources/028_Lighting_Window5.png)

基础 -- 高曝光 -- 低泛光阈值 -- 高漫反射倍率 -- 极高漫反射倍率 -- Reinhard 映射下的高白点 -- Linear 色调映射

## 颜色调整

[![Colorization](./resources/028_Lighting_Window6.png)](./resources/028_Lighting_Window6.png)
*颜色调整*

| 属性 | 说明 |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input Low | 控制场景暗部的对比度。必须始终低于 Input High。 |
| Input High | 控制场景高光部分的对比度。必须始终高于 Input Low。 |
| Input Gamma | 控制场景中间调的对比度。 |
| Output Low | 调整场景暗部，同时保留色彩。必须始终低于 Input High。 |
| Output High | 调整高光亮度，同时保留色彩。必须始终高于 Input Low。 |
| Brightness | 设置场景中反射光的强度。 |
| Contrast | 设置暗部与高光差异的强调程度。 |
| Hue | Hue 会整体偏移场景中所有对象的色相。在关闭 Colorize 时，这一效果会更容易分辨。 |
| Saturation | 设置场景中的色彩强度。 |
| Lightness | 改变场景中的整体亮度。 |
| Colorize | 调整场景中色彩的强度和对比度。关闭它后，Hue、Saturation 和 Lightness 对光照的贡献方式也会发生变化。 |
| Correct Gamma | 启用后，会根据显示器的 gamma 设置修正场景颜色。 |

[![图像](./resources/028_Lighting_Window7.png)](./resources/028_Lighting_Window7.png)

基础 -- 提高 Input Low -- 降低 Input High -- 高 Input Gamma -- 提高 Output Low -- 提高 Output High -- 高对比度

[![图像](./resources/028_Lighting_Window8.png)](./resources/028_Lighting_Window8.png)

基础 -- 高对比度 -- 关闭 Colorize 后降低 Hue -- 关闭 Colorize 后提高 Hue -- 低饱和度 -- 提高亮度 -- 关闭 Correct Gamma

## 变化

Variations 是一组简单滤镜，可用于影响场景整体光照。选择任意 variation 控件后，对应属性就会被添加到该光照中。

[![Lighting Variations](./resources/028_Lighting_Window9.png)](./resources/028_Lighting_Window9.png)
*光照变化*

| 属性 | 说明 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Region | 设置该变化作用于场景的暗部、中间调还是高光部分。 |
| Sensitivity | 控制所应用滤镜的强度。这些效果是可叠加的；Red 5 相当于应用五次 Red 1。 |
| Color Settings | 应用选中的颜色。可选项包括 Red、Cyan、Magenta、Green、Blue 和 Yellow。 |
| Light Settings | 为场景增加亮度或暗度。这些效果彼此会相互抵消。 |
| Less Saturation & More Saturation | 对场景施加更多或更少饱和度，改变色彩强调程度。这些效果同样会彼此抵消。 |

[![图像](./resources/028_Lighting_Window10.png)](./resources/028_Lighting_Window10.png)

基础 -- 青色中间调 -- 紫色暗部 -- 绿色高光 -- 更暗的蓝色暗部 -- 更亮的黄色中间调 -- 更高饱和度

## SSAO

[![Ambient Occlusion](./resources/028_Lighting_Window11.png)](./resources/028_Lighting_Window11.png)
*环境光遮蔽*

屏幕空间环境光遮蔽（SSAO）是一种根据对象与镜头距离来进行阴影处理的渲染技术。这些设置目前尚未启用。

## 地形

[![Terrain Lighting](./resources/028_Lighting_Window12.png)](./resources/028_Lighting_Window12.png)
*地形光照*

| 属性 | 说明 |
| ----------------------------- | ---------------------------------------------------------------------- |
| Terrain Specular Exponent | 设置地形高光反射的基础强度。 |
| Terrain Diffuse Multiplier | 设置地形整体光反射强度。 |
| Terrain Specular Multiplier | 设置地形高光反射强度。 |
| Creep Specular Exponent | 设置菌毯反光的基础强度。 |
| Creep HDR Diffuse Multiplier | 设置菌毯整体光反射强度。 |
| Creep HDR Specular Multiplier | 设置菌毯高光反射强度。 |
| Creep HDR Emissive Multiplier | 设置菌毯自发光纹理所产生的光量。 |

[![图像](./resources/028_Lighting_Window13.png)](./resources/028_Lighting_Window13.png)

基础 -- 提高 Terrain Specular Exponent -- 提高 Terrain Diffuse Exponent -- 提高 Terrain Specular Multiplier -- 降低 Creep Specular Exponent -- 提高 Creep HDR Diffuse Multiplier -- 提高 Creep HDR Specular Multiplier

## 区域

[![Region Lighting](./resources/028_Lighting_Window14.png)](./resources/028_Lighting_Window14.png)
*区域光照*

Regions 定义的是仅作用于地图某一片区域的光照设置。你最多可以在地形模块的地形层中，使用光照刷绘制四个不同区域。每个区域都能分别配置 Key、Back、Fill 和环境光设置。一旦配置完成，区域内的光照就会覆盖全局光照。你可以在地形模块的地形层中，把这些光照区域应用到地形上。

[![Base -- Painted Regions](./resources/028_Lighting_Window15.png)](./resources/028_Lighting_Window15.png)
*基础 -- 已绘制区域*

## 主光、补光与背光

这三个相关类别共同组成了三点布光模型，这是一种常见于多种媒体中的传统布光方法。Key 是主光源，它照亮场景的主要部分，也是决定整体外观、色彩和阴影风格的核心。Fill 是辅助光源，通常会从主光源一侧斜打进来，为物体补足体积感，减弱单个阴影的影响，让画面更柔和。

而 Back light 顾名思义，是从物体背后或场景另一侧照射过来的光。通常 Back lighting 用来为物体边缘制造一圈类似轮廓光的效果，使其从背景中脱离出来。你可以在光照窗口中的三个相似标签里找到这些光照类型。下图展示了其中之一。

[![Key Lighting](./resources/028_Lighting_Window16.png)](./resources/028_Lighting_Window16.png)
*主光*

| 属性 | 说明 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Diffuse Color | 设置光照整体漫反射的颜色。 |
| Diffuse Multiplier | 设置光照整体漫反射的强度。 |
| Specular Color | 设置光照直接反射的颜色。 |
| Specular Multiplier | 设置光照直接反射的强度。 |
| Direction | 使用 H 和 V 控件设置光照方向。H 即 Horizontal angle，对应方位角：North 为 0，East 为 90，South 为 180，West 为 270。V 即 Vertical angle，对应光源高度角。目标正上方为 90，正下方为 270。请注意，若光源位于地平线以下，它将不会照亮地形纹理。 |

## 自定义一组光照

打开本文附带的演示地图。你会看到文中部分示例图片所使用的场景：一个黑色电影风格的酒吧门面。遗憾的是，干净明亮的白天光照破坏了气氛。你可以用光照界面来解决这个问题。要开始探索光照自定义，请通过 `Window ▶︎ Lighting Window` 打开光照窗口。然后选择预制光照组 `Custom Night Light`，再点击 `00:00:00` 打开该光照。

[![Starting Light](./resources/028_Lighting_Window17.png)](./resources/028_Lighting_Window17.png)
*起始光照*

接下来，你将对光照进行一系列调整。它们会以步骤形式列出，并在下方的图条中展示对应效果。与前面示例图不同，这里的效果是会累积叠加的。请对照这张样例图观察每一步的结果，从而感受不同光照属性的影响。完成练习后，这个场景应当会更具黑色电影气质。

1.  在 Global 中，将 Ambient Color 设为 R86、G79、B147。将 Specular Multiplier 设为 0.6，并把 Emissive Multiplier 设为 1.1。
2.  在 Tone Mapping 中，将 Exposure 从 1.5 降到 0.6。
3.  将 Bloom Threshold 从 1.0 降到 0.1。把 Diffuse Multiplier 从 1.0 提高到 1.4。
4.  在 Colorization 中，将 Input Low 从 0 提高到 0.05。将 Input High 从 1 降低到 0.9。
5.  将 Colorization 从 0.3 提高到 0.5。
6.  在 Terrain 中，将 Terrain Diffuse Multiplier 从 1.0 降到 0。将 Terrain Specular Multiplier 从 3.75 降到 0。

![](./resources/028_Lighting_Window18.png)
*基础 -- 步骤 1 -- 步骤 2 -- 步骤 3 -- 步骤 4 -- 步骤 5 -- 步骤 6*

## 附件

 * [028_Lighting_Window_Start.SC2Map](./maps/028_Lighting_Window_Start.SC2Map)
 * [028_Lighting_Window_Completed.SC2Map](./maps/028_Lighting_Window_Completed.SC2Map)
