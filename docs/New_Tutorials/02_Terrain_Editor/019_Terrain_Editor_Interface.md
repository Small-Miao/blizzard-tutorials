# 地形编辑器界面

地形编辑器很可能是整个编辑器中内容最丰富的部分。除了作为地图设计的工作区之外，它也是编辑器中许多其他部分与地图文件本身发生交互的地方。例如，触发编辑器通过点和区域与地形编辑器沟通；数据编辑器则通过单位和其他实体与之联动；过场动画编辑器则通过镜头来配合，等等。

## 界面

[![Interface View of Uvantak's Ganymede](./resources/019_Terrain_Editor_Interface1.png)](./resources/019_Terrain_Editor_Interface1.png)
*Uvantak 的 Ganymede 界面视图*

正如你从上图中可能已经猜到的那样，地形编辑器某种意义上就像项目枢纽。其结果是，它拥有非常广泛的选项和功能，因此被划分为七个独立图层。你可以通过主工具栏中的地形栏在这些图层之间切换。那里有一组按钮，可让你直接进入各个图层，如下图所示。

![Terrain Bar](./resources/019_Terrain_Editor_Interface2.png)
*地形栏*

默认情况下，各图层彼此明确分离。一旦某个图层处于活动状态，你就只能编辑属于该图层的元素。使用地形编辑器时，始终只有一个活动图层。你可以通过查看当前被高亮的图层按钮来确认自己所在的位置。

每个图层都有自己的专属选项。这些选项会显示在主地形视图左侧的大面板中，它被称为 UI Panel。下图展示了在单位层开启时它的样子。

![UI Panel](./resources/019_Terrain_Editor_Interface3.png)
*UI 面板*

**UI 面板**会根据当前图层而变化。每个图层都拥有一个 Palette，用来承载该图层中的大多数主要控制项。关于 Palette 的更详细说明，你可以在介绍各个图层的单独文章中看到。还值得注意的是，顶部菜单标签中提供了许多按子标签划分的选项，下面会逐一介绍。

## 视图选项

这些选项用于控制编辑器中各个组件的可见性、修改当前视口属性，并提供部分镜头相关支持。你可以在 View 标签中找到它们，如下图所示。

![View Tab](./resources/019_Terrain_Editor_Interface4.png)
*视图标签*

| 操作 | 作用 |
| -------------------- | ------------------------------------------------------------------------------------------ |
| Show UI Panel | 控制 UI 面板是否可见。 |
| Show UI | 控制若干高级 UI 功能是否可见，例如帧率。 |
| Show Layer | 单独控制各图层是否可见。 |
| Show Names | 控制点、区域等元素名称是否可见。 |
| Show Tags | 控制在“地图属性”菜单中创建的标签是否可见。 |
| Show Difficulty | 控制单位难度设置是否可见。 |
| Show Background | 控制编辑器显示中的天空盒是否可见。 |
| Show Placement Grid | 控制网格组件是否可见。 |
| Show Pathing | 控制不同类型寻路信息是否可见，例如 “Unpathable Areas”。 |
| Show Terrain | 控制某些地形组件是否可见，例如菌毯、水体或地图边界。 |
| Enable Object 声音 | 开关在编辑器中放置对象时的音效。 |
| Lock Game View | 将镜头锁定为对战游戏中的标准视角设置。 |

## 渲染选项

这一部分包含一些可用于控制编辑器中高级图形元素可见性的选项。你可以在 Render 标签中找到它们，如下图所示。

![Render Tab](./resources/019_Terrain_Editor_Interface5.png)
*渲染标签*

| 操作 | 作用 |
| ------------------- | ------------------------------------------------------------------------------------- |
| Show Shader Mode | 控制不同类型着色器的显示。 |
| Show Lighting | 选择显示游戏光照，或当前正在编辑的自定义光照。 |
| Show Fog 效果 | 控制雾效是否显示。 |
| Show Particles | 控制粒子元素是否显示，包括模型、Actor 等。 |
| Show Wireframe Mode | 启用线框显示模式，将所有元素以线框形式呈现。 |

下图展示了几只刺蛇在启用线框模式时的效果。

[![Wireframe Mode](./resources/019_Terrain_Editor_Interface6.png)](./resources/019_Terrain_Editor_Interface6.png)
*线框模式*

## 图层选项

这一部分允许你控制任意时刻哪些图层处于活动状态，从而打破它们默认的彼此隔离。你可以通过 Select From 字段来激活不同的图层组合配置。这里也提供图层间的导航选项，其作用与地形栏相同。你可以在下图所示的 Layer 标签中找到这些内容。

![Layer Tab](./resources/019_Terrain_Editor_Interface7.png)
*图层标签*

## 工具

这里提供用于支持基础功能的选项，例如选择对象和设计地图。你可以通过 Tools 标签访问这些工具。

![Tools Tab](./resources/019_Terrain_Editor_Interface8.png)
*工具标签*

| 操作 | 作用 |
| ------------------- | ------------------------------------------------------------------------- |
| Selection Mode | 允许你使用鼠标选择对象。 |
| Measure Distance | 将光标切换为测距工具。 |
| Snap to Grid | 修改放置元素时的网格吸附行为。 |
| Diagonal Selection | 旋转选择轴线，使框选沿 45° 方向进行。 |
| Use Group Selection | 修改选择行为，使其只影响组内单位。 |
| Use Symmetry | 开关地图所使用的对称功能。 |

Measure Distance 会将默认鼠标光标切换为测量工具，用于测量一点到另一点之间的距离。它在估算对战地图设计中的大致距离时非常有用，也能帮助进行触发器或数据相关的计算。使用方法是先点击一次开始测量，再在目标终点处点击。下图展示了 Measure Distance 的实际效果。

[![Measuring Tool](./resources/019_Terrain_Editor_Interface9.png)](./resources/019_Terrain_Editor_Interface9.png)
*测量工具*
