# 地图属性

地图属性会影响非常广泛的设置，涵盖从 Battle.net 上游戏大厅的表现方式，到“战争迷雾”的运作方式等各个方面。你可以在任意编辑器模块中通过 `地图 ▶︎ 地图信息` 进入汇总后的地图属性。

![Map Tab](./resources/008_Map_Properties01.png)
*地图标签*

需要注意的是，“地图属性”实际上指的是一组由八个选项卡组成的设置集合，用来控制地图层面的高级决策。无论你从 Map 标签下进入哪一个子项，最终都会打开同一个“地图属性”窗口。下面会分别介绍这些选项卡。

  
地图信息 --------

地图信息属性用于设置地图的基本信息与管理性细节。

![Info Tab](./resources/008_Map_Properties02.png)
*信息标签*

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 89%" />
</colgroup>
<thead>
<tr class="header">
<th>属性</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>名称</td>
<td>地图名称，会显示在 Battle.net、Arcade 以及发布界面等位置。</td>
</tr>
<tr class="even">
<td>建议玩家数</td>
<td>用于标注地图大致适合多少名玩家的内部说明。它与自定义游戏和 Arcade 中 “Game Info” 与 “地图信息” 页面里显示的 Suggested Players 数值无关，后者会根据大厅信息自动生成。</td>
</tr>
<tr class="odd">
<td>描述</td>
<td>用于描述地图内容的区域。这里也常被用来填写作者署名，或任何可能对受众有价值的一般性说明。</td>
</tr>
<tr class="even">
<td>预览图像</td>
<td>这张图片会在多个场景中显示。</td>
</tr>
<tr class="odd">
<td></td>
<td><ul>
<li>作为 Arcade “Browse” 页面中的主缩略图。</li>
<li>在 Battle.net “Custom Games” 区域中高亮某个游戏时显示。</li>
<li>在发布界面中显示。</li>
<li>在默认地图设置下作为地图预览画面显示。</li>
<li>若未选择截图，则显示在 Arcade 的 “Game Info” 页面中。</li>
</ul></td>
</tr>
<tr class="even">
<td></td>
<td>它可以设置为地图图像、自定义图像或隐藏。最后一种会使上述所有场景都不显示图片。</td>
</tr>
<tr class="odd">
<td>游戏小地图图像</td>
<td>决定小地图所使用的图片。可选项包括地图图像和自定义图像。通常默认应使用地图图像，但为了支持非传统玩法也提供了自定义选项。</td>
</tr>
<tr class="even">
<td>小地图分辨率</td>
<td>决定小地图质量等级，具体如下：Normal: 256 x 248，High: 512 x 496，Ultra: 1024 x 992。</td>
</tr>
<tr class="odd">
<td>语言环境</td>
<td>选择当前输入信息所对应的本地化版本。你还会看到一个 “Copy to All Locales” 按钮，用于把当前视图中的信息复制到其他所有现有语言环境。</td>
</tr>
</tbody>
</table>

## 地图选项

地图选项是一组偏向玩法与视觉效果的设置，用于修改游戏表现。其中也包含一些会影响地图创建决策的管理类选项。

![Options Tab](./resources/008_Map_Properties03.png)
*选项标签*

| 属性 | 说明 |
| ---- | ---- |
| 未探索区域 | 设置地图使用的“战争迷雾”类型。可选项包括灰色遮罩、黑色遮罩和黑色遮罩（缩小半径）。此外，“修改”标签会把你带到数据模块，以便进行更深层级的自定义。 |
| 最低飞行高度 | 设置飞行单位下降时允许达到的最低高度。 |
| 静态阴影强度 | 设置静态映射阴影的不透明度，用于兼容较低图形设置。 |
| “游戏选项”标记 | Disable Observers：禁止在线对局中的观察者。 |
| | Disable Replay Recording：关闭地图对局中的录像保存。 |
| | Disable Trigger Preloading：在触发器中被显式引用的数据可以预加载，以牺牲触发器加载时间来换取更短的初始载入时间。 |
| | Hide Errors During Online Games：防止 Battle.net 中显示任何地图调试或错误信息。 |
| | Hide Errors During Test Document：防止在使用“测试文档”功能时显示任何地图调试或错误信息。 |
| | Show Game Start Countdown：在地图加载完成后显示动画倒计时。 |
| | Stagger Periodic Trigger Events：自动错开周期性触发器，以尽量优化性能并避免触发器排队。 |
| | Use Horizontal Field of View：将游戏默认的垂直视野改为水平视野。对第三人称或第一人称射击等自定义项目很有用。 |
| 发布选项 | 将地图类别设置为 Melee/Custom Map 或 Arcade 地图。这个区分会决定地图出现在 Battle.net 的 “Custom Games” 还是 Arcade 区域。Automatically Add Multiplayer Data 允许 Melee/Custom Map 在游戏启动时自动添加所需的多人依赖项。Hide Battle.net Lobby 则允许 Arcade 地图 玩家在等待游戏开始时最小化大厅。 |

## 地图边界

地图边界允许你动态修改地图中可游玩区域和镜头区域的大小限制。地图四边的箭头按钮可以沿对应轴线调整尺寸。

![边界标签页](./resources/008_Map_Properties04.png)
*边界标签*

| 属性 | 说明 |
| ---- | ---- |
| 修改镜头边界 | 锁定或解锁调整尺寸时对镜头区域的影响。 |
| 修改地图边界 | 锁定或解锁调整尺寸时对可游玩地图区域的影响。 |
| 重置为默认值 | 将所有已配置边界恢复为默认设置。 |
| (Map Size) Description | 对地图尺寸的基础自动描述。可选项包括：Tiny、Small、Medium、Huge 和 Epic。 |
| Playable (Map Size) | 当前地图尺寸，不包含四周强制保留的不可玩缓冲区。 |
| Full (Map Size) | 当前地图完整尺寸，包含缓冲区。 |

## 地图对称

![Symmetry Tab](./resources/008_Map_Properties05.png)
*对称标签*

地图对称用于设置地图中的对称控制。这在制作对战地图地形时尤其有用，因为对称通常是竞技玩法中的必要条件。

## 地图纹理

![纹理标签页](./resources/008_Map_Properties06.png)
*纹理标签*

每张地图都会使用一种特定的 纹理集，它是由八种纹理组成的调色板，用于在地图地表上绘制地形。你可以在 Map 纹理 标签中选择这些调色板。需要注意的是，悬崖样式也是该调色板的一部分，也可以在这里配置。

## 地图标签

地图标签可用于为地形对象创建 Difficulty Tags，例如单位、装饰物、区域和点，从而方便你在之后进行筛选查看。

![Tags Tab](./resources/008_Map_Properties07.png)
*标签页*

## 地图加载画面

地图加载画面标签允许你配置玩家在地图加载过程中看到的引导界面。

[![Load Screen Tab](./resources/008_Map_Properties08.png)](./resources/008_Map_Properties08.png)
*加载画面标签*
