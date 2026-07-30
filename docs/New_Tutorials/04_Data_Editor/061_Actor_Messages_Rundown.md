# Actor 消息概览

几乎每一种数据类型背后，都有某种对应的 Actor 消息来提供其逻辑后端。这也意味着 Actor 消息的种类既丰富又繁多。本文会拆解其中许多消息类型，并给出一些使用建议。

开始之前，你需要知道：Actor 的消息既可以直接在可展开的“事件”字段中查看，也可以在 Actor 事件子编辑器中以结构化方式查看。本文每一项也都会附带该消息类型在子编辑器中的视图，不过基础字段视图有时同样值得关注。你可以在下图中看到一个例子。

[![Actor 事件字段](./resources/061_Actor_Messages_Rundown1.png)](./resources/061_Actor_Messages_Rundown1.png)
*Actor 事件字段*

在这里，你能看到 Actor 消息的直接 ID，例如 `UnitBirth` 或 `ActorCreation`，而不是子编辑器选择面板里那种结构化名称，如 `Unit Birth` 或 `Actor Creation`。这些 ID 在触发器编辑器或脚本中使用 Actor 消息时可能很重要，因此下文每个条目开头都会列出对应 ID。

虽然本文可以作为参考，但你也应当明白，想要深入了解完整的 Actor 消息库，最好的办法仍然是亲自到子编辑器里逐个创建并检查。你可以前往某个 Actor 的“事件”字段，双击打开子编辑器，然后在白色区域中右键选择“添加事件”，这样就会创建一组事件与消息配对。

[![Actor 事件创建](./resources/061_Actor_Messages_Rundown2.png)](./resources/061_Actor_Messages_Rundown2.png)
*Actor 事件创建*

下面会依次概览多种 Actor 消息类型。

## 播放动画

[![播放动画消息](./resources/061_Actor_Messages_Rundown3.png)](./resources/061_Actor_Messages_Rundown3.png)
*播放动画消息*

AnimPlay

播放指定动画。这是控制动画最核心的消息，其选项主要用于控制动画的物理表现方式。

| 字段 | 说明 |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 名称 | 为该消息设置引用标签。 |
| 动画属性 | 从下拉列表中选择动画类型。所有动画都绑定到一组预设状态，你可以在过场动画编辑器中查看和研究它们。 |
| 标志 | |
| 资源驱动循环 | 与 Play Forever 类似，但会使用动画内部定义得更明确的循环行为。 |
| 完全匹配 | 会让该动画连续播放时彼此无缝衔接，不会出现卡顿。 |
| 不循环 | 当前动画只播放一遍。 |
| 永久播放 | 持续循环该动画。 |
| 随机起始偏移 | 从一个随机帧开始播放动画。 |

Blend In 会在两组不同动画之间进行动态插值，这里设置的是从当前动画过渡到此动画所花费的时间。Blend Out 设置从此动画过渡到新动画所花费的时间。Time Variant 设置用于指定 Time Variant 的时间缩放类型。Time Type 设置用于指定 Time Variant 的时间尺度类型。Blend In Start Offset 设置混合过程开始时的偏移时间。Priority Override 设置此动画在混合时相对于其他动画优先级的优先级。

## 动画括号开始

[![动画括号开始消息](./resources/061_Actor_Messages_Rundown4.png)](./resources/061_Actor_Messages_Rundown4.png)
*动画括号开始消息*

AnimBracketStart

动画括号可以看作三段式动画序列的容器。这三个阶段分别是 Opening、Content 和 Closing。

| 字段 | 说明 |
| ------------------- | ---------------------------------------------------------------------- |
| 名称 | 为该消息设置引用标签。 |
| Opening | 设置第一阶段动画。 |
| Content | 设置第二阶段动画，在 Opening 之后播放。 |
| Closing | 设置最后阶段动画，在 Content 之后播放。 |
| 标志 | |
| Closing Full | 确保 Closing 动画在任何情况下都能播放完成。 |
| 即时 | 跳过 Opening 动画，直接从 Content 开始。 |
| 不循环 | 让 Content 动画只播放一次。 |
| 永久播放 | 在收到手动停止前不断重播 Opening 动画。 |
| 随机起始偏移 | 为动画的起始时间应用随机偏移。 |

Time Variant 设置用于指定 Time Variant 的时间缩放类型。Time Type 设置用于指定 Time Variant 的时间尺度类型。

## 销毁

[![销毁消息选项](./resources/061_Actor_Messages_Rundown5.png)](./resources/061_Actor_Messages_Rundown5.png)
*销毁消息选项*

Destroy

销毁该 Actor，使其不再接收后续更新，并移除任何可视组件。可用类型有两种：Immediate 和 Normal。Immediate 会立刻销毁 Actor 及其附属部分；如果对象是单位，则不会播放死亡动画。Normal 会移除 Actor 及其附属部分，但允许死亡动画播放完毕，并让粒子自然淡出。

## 开始发光

[![开始发光消息](./resources/061_Actor_Messages_Rundown6.png)](./resources/061_Actor_Messages_Rundown6.png)
*开始发光消息*

GlowStart

为 Actor 的模型施加一种脉冲式发光效果。可以通过 `Glow Stop` 消息停止。这个消息没有子选项。

## 设置光环颜色

[![设置光环颜色消息](./resources/061_Actor_Messages_Rundown7.png)](./resources/061_Actor_Messages_Rundown7.png)
*设置光环颜色消息*

HaloSetColor

光环会在模型周围添加一圈发光轮廓，通常用于让单位与背景形成对比，或为了特定目的突出显示该单位。这个消息用于设置光环颜色，而 `Halo Start` 和 `Halo Stop` 则分别控制光环的添加与移除。

## 替换模型

[![替换模型消息](./resources/061_Actor_Messages_Rundown8.png)](./resources/061_Actor_Messages_Rundown8.png)
*替换模型消息*

ModelSwap

将 Actor 的模型设置为 `Model` 所指定的值。这会替换当前已经选中的模型，同时也支持选择该模型的具体 Variation。

## 设置不透明度

[![设置不透明度消息](./resources/061_Actor_Messages_Rundown9.png)](./resources/061_Actor_Messages_Rundown9.png)
*设置不透明度消息*

SetOpacity

改变 Actor 的不透明度，并将该变化传播到任何关联的可视资源，例如模型。

| 字段 | 说明 |
| ----------------- | ---------------------------------------------------------------------------------------------------------- |
| 不透明度 | 设置应用后的不透明度级别，其中 0.0 为默认状态，1.0 为完全透明。 |
| 混入时长 | 设置不透明度变化施加所经历的时间。默认是瞬时应用。 |
| 标签 | 为该消息设置引用标签。 |

## 设置染色颜色

[![设置染色颜色消息](./resources/061_Actor_Messages_Rundown10.png)](./resources/061_Actor_Messages_Rundown10.png)
*设置染色颜色消息*

SetTintColor

为 Actor 应用染色，并将颜色变化传播到任何连接的可视资源，例如模型。

| 字段 | 说明 |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 颜色 | 设置作为染色应用的颜色。点击颜色框会打开取色器窗口。 |
| HDR 倍率 | 设置通过 HDR 光照产生的亮度增幅。 |
| 混入时长 | 设置颜色应用所经历的时间。默认是瞬时应用。 |
| 混合类型 | 从不同混合方式中选择。One Shot 会向该颜色混合一次，Bounce 会先混入颜色再退回，Cycle 则会反复混入并退出该颜色。 |
| 标签 | 为该消息设置引用标签。 |
| 优先级 | 设置该染色消息相对于类似消息的优先级。 |
