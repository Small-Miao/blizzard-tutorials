# 站点操作概览

站点操作几乎涵盖了你能施加到 Actor 上的大量修改。要真正熟练掌握它们，往往需要一些时间和经验。这是一份简短指南，介绍最常用的几类操作及其用法，帮助你更快上手。

你会注意到，这份指南与前面那些编辑器元素概览略有不同。原因在于，站点操作与编辑器中的其他元素不一样，它们的应用顺序会直接影响结果。重新排列同一组站点操作，可能会得到截然不同的表现。这也意味着，站点操作很难脱离上下文单独定义。因此，本文不会逐个拆解每种操作的字段和细节，而是通过教程练习来定义每一种操作，并演示如何应用它们。这样你就能更直观地理解：随着更多站点操作逐层叠加，宿主 Actor 会如何发生变化。

先打开本文提供的演示地图。你会在其中看到一名站在木板路上的陆战队员，如下图所示。

[![演示地图场景](./resources/067_Site_Operations_Rundown1.png)](./resources/067_Site_Operations_Rundown1.png)
*演示地图场景*

进入数据编辑器，然后切换到 Actor 标签页，开始练习的第一步。

## Site Operation (Attachment)

Attachment 站点操作会把一个 Actor 附着到某个单位或另一个 Actor 上。它通常用于制造“这些模型被连接或嫁接在一起”的视觉效果。你可以不断叠加这种嫁接，从而做出古怪的嵌合体单位，这大概也是站点操作最标志性的用途。用这种方法做出的著名例子之一，就是暴雪愚人节恶搞作品 Terratron。

![](./resources/067_Site_Operations_Rundown2.png)
*Terratron：由多个《星际争霸》模型拼接而成的机器人*

在真正理解这种操作的机制前，先明确几个术语会更有帮助。被附着的那个 Actor，也就是承载站点操作的 Actor，称为 Host。Host 被附着到的那个 Actor，则称为 Base。

创建附加型站点操作时，最重要的字段是 `Attachment Query`。你可以用它来设置要使用哪种附着点类型。下表会更详细地说明这一点。把附加型站点操作应用到 Host 上之后，Host 就会向它所连接的任意 Base 发送 `Attachment Query`。这会让 Host 通过 `Attachment Query` 指定的附着点类型实例附着到 Base 上。随后，该操作会把 Host 的位置和朝向设置为与 Base 的附着点一致。

还值得一提的是，由于附着点类型的种类有限，你会在数据编辑器中发现一整套很有用的预制附加站点操作。它们的名称通常以前缀 `SOpAttach` 开头，并以后缀描述某种附着点类型的名称，例如 `SOpAttachHead`、`SOpAttachWeapon` 和 `SOpAttachCenter`。

下表列出了附加型站点操作的字段说明。

| 字段 | 说明 |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 附着查询 | 设置附着方法，也就是指定 Host 会附着到哪个附着点上。它通过选择 `Filter Type` 来设置。这里的选项包括 Direct，即从下拉列表中直接指定附着点引用名；以及 Method，即从 Attach Methods 数据类型中指定附着点。如果指定的方法失败，你还可以提供一个 Fallback 查询。 |
| 保持位置 | 该标志会让附着位置保持在最初计算出的值上，不再变化。 |
| 保持旋转 | 该标志会让附着旋转保持在最初计算出的值上，不再变化。 |

## 使用附加操作

本文附带的地图可以用来演示附加操作，你可以直接拿那名孤零零的陆战队员练手，他不会介意。你首先需要的是作为 Host Actor 所使用的一个模型。进入数据编辑器中的 模型 标签页，在对象列表中右键并选择 `Add Model` 来创建模型。这会打开下图所示窗口。

![模型创建窗口](./resources/067_Site_Operations_Rundown3.png)
*模型创建窗口*

把模型的 `Name` 设为 `Light Model`，然后点击 `Suggest` 自动生成 ID。把 `Model Type` 设为 `Generic`，再点击 `Ok`。这样就会创建一个 Model 对象。接着定位到它的“模型”字段并双击，打开 `Object Values` 选择窗口。在这个窗口里点击 `Browse` 会打开资源浏览器，如下图所示。

[![通过资源浏览器选择模型](./resources/067_Site_Operations_Rundown4.png)](./resources/067_Site_Operations_Rundown4.png)
*通过资源浏览器选择模型*

用浏览器的搜索功能找到 `Streetlight_01.m3`，选中后点击 `Ok`。然后在选择窗口中再次点击 `Ok`，返回数据编辑器主视图。现在，这个模型资源已经与一个 Model 类型完成链接，可以在 Actor 中使用了。接下来准备 Actor：先切换到 Actor 标签页，在对象列表中右键并选择 `Add Actor`。

[![添加 Actor](./resources/067_Site_Operations_Rundown5.png)](./resources/067_Site_Operations_Rundown5.png)
*添加 Actor*

选择 `Add Actor` 后，会弹出 `Actor Properties` 创建窗口。在这里把 `Name` 设为 `Marine Light`，再点击 `Suggest` 自动生成 ID。还需要把 `Actor Type` 设为 `Model`，`Parent` 设为 `ModelAddition`。点击 `Ok` 创建这个 Actor。窗口应如下图所示。

![模型 Actor 创建窗口](./resources/067_Site_Operations_Rundown6.png)
*模型 Actor 创建窗口*

如果模型与模型 Actor 使用同一个名字，编辑器会自动帮你填好“模型”字段，也就自动建立了链接。现在该给这个 Actor 添加附加站点操作了。定位到 `Host Site Operations` 字段并双击，打开站点操作子编辑器。

[![Host Site Operation 子编辑器](./resources/067_Site_Operations_Rundown7.png)](./resources/067_Site_Operations_Rundown7.png)
*Host Site Operation 子编辑器*

点击 `Choose` 按钮后，会打开下方所示的站点操作选择窗口。

[![选择附加站点操作](./resources/067_Site_Operations_Rundown8.png)](./resources/067_Site_Operations_Rundown8.png)
*选择附加站点操作*

这里你可以直接使用一个预制好的附加操作。所有以 `SOpAttach` 为前缀的站点操作，都内置了一个 `Attachment Query`，用于定位单位模型中的某一类附着点。请选择 `SOPAttachHead` 操作，它会定位模型头部的附着点。然后点击 `Ok` 返回上一视图。

![已选择的站点操作](./resources/067_Site_Operations_Rundown9.png)
*已选择的站点操作*

该站点操作现在已经出现在 Actor 的列表中了。点击 `Ok` 完成确认。这个模型 Actor 现在包含了一个自定义路灯模型，它会被嫁接到 Base 单位的头部。剩下要做的，就是把这个 Actor 连接到它的宿主。你可以通过 Actor 事件字段来完成。进入 `Marine Light` 模型 Actor 的“事件”字段，双击打开下图所示的 Actor 事件子编辑器。

[![Actor 事件视图](./resources/067_Site_Operations_Rundown10.png)](./resources/067_Site_Operations_Rundown10.png)
*Actor 事件视图*

在白色区域中右键并选择 `Add Event`。通过下拉框，把事件的 `Msg Type` 设为 `Unit Birth`。然后把消息的 `Msg Type` 设为 `Create`，并把 `Source Name` 设为 `Marine`。设置完后应如下所示。

[![将模型 Actor 连接到单位](./resources/067_Site_Operations_Rundown11.png)](./resources/067_Site_Operations_Rundown11.png)
*将模型 Actor 连接到单位*

创建这条 Actor 事件后，`Marine Light` 模型 Actor 会在单位创建时连接到 `Marine` 单位 Actor。正如前面所说，这会让 `Marine` Actor 充当 Base，`Marine Light` 充当 Host，而 `SOpAttachHead` 则充当附加站点操作。于是，`Marine Unit` Actor 的模型就会在 `SOpAttachHead` 的 `Attachment Query` 所指定的点位上，被嫁接到 `Marine Light` Actor 的模型上。该操作的结果如下图所示。

![](./resources/067_Site_Operations_Rundown12.png)
*通过附加操作嫁接在一起的模型*

## Site Operation (Explicit Rotation)

Explicit Rotation 站点操作可以让 Host Actor 在其三个轴 `x`、`y`、`z` 上进行任意组合的旋转。它通过两个向量 `Forward` 与 `Up` 来实现。每个向量都为三维方向上的旋转提供不同的基准轴。下表拆解了这类操作的细节。

| Fiel d | 说明 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Forw ard | 为通过 X、Y、Z 输入指定的旋转操作提供一个基础旋转轴。你可以把 `Forward` 向量理解为从 Actor 身上水平向外伸出的方向。输入旋转遵循以下规则。 |
| | \-X: Right +X: Left |
| | \-Y: Forward +Y: Backwards |
| | \-Z: Down +Z: Up |
| | 这是一个单位向量，所有输入都会标准化到 1/-1 的尺度上。它的使用频率远高于另一个选项。 |
| Up | 为通过 X、Y、Z 输入指定的旋转操作提供另一个基础旋转轴。你可以把 `Up` 向量理解为从 Actor 身上垂直向上伸出的方向，像是从地面冒出来一样。输入旋转遵循以下规则。 |
| | \-X: Right +X: Left |
| | \-Y: Down +Y: Up |
| | \-Z: Forward +Z: Backwards |
| | 这也是一个单位向量，所有输入都会标准化到 1/-1 的尺度上。通常只有在模型侧躺、并且要在一定运动范围内转动时才会用到它。在这种情况下，若使用 `Forward` 向量，模型可能会翻转。 |
| Hold Posi tion | 该标志会让此操作的位置保持在最初计算出的值上。 |
| Hold Rota tion | 该标志会让此操作的旋转保持在最初计算出的值上。 |

这两个向量通常不会一起使用。混合使用两者时，最终得到的旋转会等于 `Forward` 向量旋转减去 `Up` 向量旋转，本质上相当于折中了两者的旋转。

## 使用 Explicit Rotation 操作

接下来你将使用一个 Explicit Rotation 操作，来重新调整路灯模型的朝向。回到 Actor 标签页，创建一个类型为 `Explicit Rotation` 的新 Actor，并将其命名为 `Light Offset`。流程如下图所示。

![创建 Explicit Rotation Actor](./resources/067_Site_Operations_Rundown13.png)
*创建 Explicit Rotation Actor*

要设置具体旋转，只需先选定要作为旋转基准的向量类型，再设置其 `x`、`y`、`z` 字段。可用的向量字段 `Forward` 和 `Up` 如下图所示。

[![旋转 Actor 字段](./resources/067_Site_Operations_Rundown14.png)](./resources/067_Site_Operations_Rundown14.png)
*旋转 Actor 字段*

双击 `Forward` 字段会打开如下视图。

![设置旋转站点操作](./resources/067_Site_Operations_Rundown15.png)
*设置旋转站点操作*

这里所需的旋转，是让路灯同时向左和向后各摆动一个完整单位。你可以通过把 `X` 和 `Y` 字段都设为 `1` 来实现。现在，这个操作已经完成，可以添加到模型 Actor 的站点操作列表中了。方法是进入 `Marine Light` 的 `Host Site Operations` 字段并双击打开，然后再次选择 `Choose`，在弹窗中找到新建的 `Light Offset` 操作。选中后点击 `Ok`。

![](./resources/067_Site_Operations_Rundown16.png)
*添加你自定义的 Explicit Rotation 站点操作*

它会像上图那样出现在站点操作列表中。然后点击 `Ok` 完成添加。到这里，这个操作已经生效并可供观察，不过你还应该再做一个改动。定位到 `Scale Maximum` 字段并打开，把 `X`、`Y`、`Z` 都设为 `0.25`。然后用同样的方法设置 `Scale Minimum` 字段。

[![调整路灯柱缩放](./resources/067_Site_Operations_Rundown17.png)](./resources/067_Site_Operations_Rundown17.png)
*调整路灯柱缩放*

模型创建时，缩放值会在 `Scale Maximum` 和 `Scale Minimum` 之间取值。把这两个字段设成相同数值，就能得到恒定、非随机的模型缩放。这里的数值会把模型缩放到此前大约四分之一的大小。你可以回到地形编辑器检查进度，此时应看到类似下图的结果。

[![缩放并旋转后的模型附着](./resources/067_Site_Operations_Rundown18.png)](./resources/067_Site_Operations_Rundown18.png)
*缩放并旋转后的模型附着*

注意，此时路灯的主轴已经与模型头部对齐。

## Site Operation (Local Offset)

Local Offset 站点操作可以让你沿着 `x`、`y`、`z` 三个主轴的任意方向修改模型位置。

| 字段 | 说明 |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Local Offset | 应用一个偏移向量，让 Actor 在 X、Y、Z 三个方向上移动。这个向量遵循编辑器的基础方向规则，具体如下。 |
| | \-X: Right +X: Left |
| | \-Y: Forward +Y: Backwards |
| | \-Z: Down +Z: Up |
| Hold Positi on | 该标志会让偏移位置保持在最初计算出的值上。 |
| Hold Rotati on | 该标志会让偏移旋转保持在最初计算出的值上。 |

## 使用 Local Offset 操作

现在你将使用偏移操作，对路灯附着的位置做一些微调。创建一个名为 `Light Down` 的新 Actor，类型为 `Site Operation (Local Offset)`。

![创建 Local Offset Actor](./resources/067_Site_Operations_Rundown19.png)
*创建 Local Offset Actor*

进入这个新 Actor 的 `Local Offset` 字段。你会沿负 Z 轴偏移模型，把它向下压进单位内部。双击该字段进行编辑，并将 `Z` 值设为 `-0.3`。

[![设置偏移 Actor 字段](./resources/067_Site_Operations_Rundown20.png)](./resources/067_Site_Operations_Rundown20.png)
*设置偏移 Actor 字段*

和前面的操作一样，这个站点操作也必须添加到 Base 模型的 Actor 事件中。进入 `Marine Light` 的 `Host Site Operations` 字段并双击打开子编辑器视图。然后点击 `Choose`，把 `Local Down` 偏移添加到操作列表中。此时列表里应像下图一样，包含三个不同的站点操作。

![已添加偏移操作的列表](./resources/067_Site_Operations_Rundown21.png)
*已添加偏移操作的列表*

现在正是再次提醒自己“操作顺序很重要”的好时机。改变顺序常常会导致难以预料的结果。请确认当前操作顺序与你计划的设计一致，就像上图那样。到这里，项目已经完成。一个外来模型已经被嫁接到陆战队员身上，并完成了重新定向、缩放和偏移。回到地形编辑器，花一点时间欣赏你的设计吧。

[![钱花得值](./resources/067_Site_Operations_Rundown22.png)](./resources/067_Site_Operations_Rundown22.png)
*钱花得值*

## 附件

 * [067_Site_Operations_Rundown_Completed.SC2Map](./maps/067_Site_Operations_Rundown_Completed.SC2Map)
 * [067_Site_Operations_Rundown_Start.SC2Map](./maps/067_Site_Operations_Rundown_Start.SC2Map)
