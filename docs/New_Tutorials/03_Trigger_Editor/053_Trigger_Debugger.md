# 触发调试器

对于高级用户，编辑器提供了触发调试器。它能让你以更细致的方式分析性能。

## 启动调试器

在运行时，你可以启动调试器，监视测试地图期间被调用的地图脚本。它是一个很有用的辅助工具，能够分类显示各种低效之处和错误。进入调试器主要有三种方式。

  - 在 `文件 ▶︎ Preferences ▶︎ Test Document` 中启用编辑器设置 `Show Trigger Debugging Window`。
  - 在测试地图时，于聊天框输入作弊命令 `trigdebug`。
  - 在触发编辑器中使用原生动作 `Debug -- Open or Close the Debugging Window`。

最后一种方式最稳妥，因为把它放进触发器后，可以在同一会话中动态启用、停止并重复使用。更关键的是，这是唯一支持在线多人测试的方法。

无论你要以哪种方式使用调试器，游戏客户端都必须设置为 `Windowed` 模式。如果你使用上面列表中的第一种方法，还需要先把 `Test Document ▶︎ Game Display Mode` 设为 `Windowed`，编辑器才会显示调试窗口。下图高亮了这一设置。

[![image0](./resources/053_Trigger_Debugger1.png)](./resources/053_Trigger_Debugger1.png)

通过设置启用调试器

## 配置调试器

开始一个新项目时，你通常需要先调整默认调试器设置，以便更好地投入使用。启动调试器后，在最下方的子视图上右键。此时会出现若干配置选项。把所有以 `Show` 开头的选项都取消勾选，只保留 `Show Errors` 和 `Show User Output`。接着切换到 `Variables` 标签，在最上方视图内右键并取消勾选 `Show Constants`。在默认设置下，调试器会输出海量诊断数据，反而可能明显拖慢性能。按这里描述的方式配置后，可以把调试器本身带来的性能影响降到最低。

## 调试器的组成部分

`Trigger Debugging` 窗口会以九个按标签页分组的视图来展示当前项目。不同时间点上，各类信息的实用性会有所不同，因此各部分的大致用途可以归纳如下。

| 调试部分 | 分析部分 |
| -------- | -------- |
| Variables | Variables |
| Threads | Threads |
| Script Code | Trigger Profiling |
| Triggers | Function Profiling |
|  | Triggers |
|  | Activity |

从功能上说，每个标签页里都是一张按列分隔的数据表。点击表头即可按该字段排序；再次点击则可在升序和降序之间切换。你还应留意每个标签页底部的状态栏。这里会显示一些高层项目属性：`Game Time`，即测试开始后的已过时间；`A.I. Time`，即测试中 AI 代码已运行的总时间；以及 `Memory -- Code, Globals`，它表示当前编辑器项目已占用的硬编码最大可用内存百分比。

## Variables 标签

![Variables 标签](./resources/053_Trigger_Debugger2.png)
*Variables 标签*

Variables 标签会显示当前正在使用的所有全局变量。全局变量以占用内存大而著称，因此按 `Total Memory Size` 排序，对于找出项目中最吃内存的部分非常有帮助。你也会在这里看到自定义类型，包括记录和数组。在游戏进行的任意时刻，都能从高层视角查看变量值，这本身就是很有用的第一步调试手段。下表说明了该标签中各字段的含义。

| 字段 | 说明 |
| ---- | ---- |
| Variable | 变量的 Galaxy 脚本名称。 |
| Type | 变量的 Galaxy 类型，常见标准类型包括 int、bool、string、struct 和 array。 |
| Total Memory Size | 元素占用的内存大小，单位为字节。复杂数据类型会显示其完整内存分配量；数组的内存大小等于元素数量乘以基础元素大小；struct 的内存大小则等于各基础元素大小之和。 |
| Value | 变量的当前值。复杂数据类型会显示 `Double Click to Expand` 选项；点击后会展开其中包含的各个变量。 |

## Triggers 标签

Triggers 标签会列出游戏中已经发生过的每一个触发器，并附带它们的使用情况明细。这个明细能帮助你了解触发器被激活的次数、条件失败的频率，以及它们的运行耗时。该视图还提供了一项非常独特且实用的功能。右键某一项后，你可以选择 `Run Trigger` 来执行一次该触发器的调试运行。如果触发器主体中需要外部事件参数，这种方式可能不会成功，但你可以通过设置默认值来提前规避。

[![Triggers 标签](./resources/053_Trigger_Debugger3.png)](./resources/053_Trigger_Debugger3.png)
*Triggers 标签*

| 字段 | 说明 |
| ---- | ---- |
| Trigger | 触发器函数的 Galaxy 脚本名称。 |
| Events | 触发器被触发的次数。 |
| Failed Conditions | 触发器条件失败的次数，因此不会执行触发器主体语句。 |
| Run | 触发器主体语句实际运行的次数。通常等于 `(Events -- Failed Conditions)`，但使用 `Run Trigger` 选项可能会让这个值失真。 |
| Average Fail Time | 触发器条件失败所花费的平均时间，单位 ms。 |
| Average Run Time | 触发器执行所花费的平均时间，单位 ms。 |
| Total Time | 触发器总共占用的时间，单位 ms。这个值大致可以估算为 `(Average Fail Time * Failed Conditions) + (Average Run Time * Run)`。 |

## Threads 标签

在 Threads 标签下，你会看到所有活动中的触发器线程。凡是使用了等待控制语句的内容都会出现在这里，例如触发器本身，或处于多线程范式中的动作定义。由于这类过程通常较复杂且更吃性能，这个标签对于正确构建这些系统非常重要。在这里右键某一行并选择 `View Script`，会跳转到 Script Code 标签，并自动滚动到所选函数的第一行。

[![Threads 标签](./resources/053_Trigger_Debugger4.png)](./resources/053_Trigger_Debugger4.png)
*Threads 标签*

| 字段 | 说明 |
| ---- | ---- |
| Thread | 对于普通触发器，这里会显示触发器函数的脚本名。使用动作定义多线程时，这里会显示隐式生成的触发器函数名。它们通常很容易识别，因为会带有 `auto` 前缀，以及显示 Thread ID 的数字后缀。 |
| Time | 该线程已处于活动状态的总时间，单位 ms。 |
| Waiting For | 显示该线程当前正在执行的 `Wait` 时间类型及其时长（秒）。共有两种类型：Real 和 Game，分别表示真实时间和游戏时间。区分这一点有助于判断当前到底是哪一个 `Wait` 语句在控制该触发器。 |

## Queue 标签

[![Queue 标签](./resources/053_Trigger_Debugger5.png)](./resources/053_Trigger_Debugger5.png)
*Queue 标签*

Queue 标签会列出当前正在使用动作队列的所有触发器。使用队列的动作包括特殊的 Galaxy 脚本函数 `TriggerQueueEnter()` 和 `TriggerQueueExit()`。只有在触发器正在等待其队列动作被处理时，它们才会显示在这里。由于这个标签的用途非常专门，所以使用频率通常较低，这也解释了它没有更多附加字段。

## Trigger Profiling 标签

[![Trigger Profiling 标签](./resources/053_Trigger_Debugger6.png)](./resources/053_Trigger_Debugger6.png)
*Trigger Profiling 标签*

在 Trigger Profiling 标签中，你会看到一个更聚焦的 Triggers 标签版本，专门用于性能分析和监视。这里列出的只有触发器，以及用于多线程的动作定义。与直接显示运行时间不同，这个标签使用 `Self Only Time` 和 `Self Only + Children Time` 来分析性能。它们能让你以更细致的粒度检查代码性能，定义如下。

| 术语 | 定义 |
| ---- | ---- |
| Self-Only Time | 原生 Galaxy 语言特性的执行时间。它衡量运行基础操作所需的时间，例如修改变量、运行控制结构或复制参数。不包含子函数调用时间，即使是原生 Galaxy 函数也不算在内；但它包含函数调用本身的开销。 |
| Self+Children Time | 触发器包含所有子函数和调用在内的总执行时间。这里的 Children Time 指的是各自 Self Time 内部发生的子函数调用时间；该项就是在前者 Self-Only Time 的基础上再加上这些时间。 |

与 Trigger 标签类似，你可以右键某一行并选择 `View Script`，把所选触发器带到 Script Code 标签，并滚动到该函数的第一行代码。这里还有几个很有用的选项。右键主窗口后，你可以选择 `Show Natives`，让视图为基础函数调用到的每个原生函数都创建一条子项。你还可以启用 `Show SubCalls`，在分析器视图中显示完整的调用栈。

下面说明 Trigger Profiling 标签中的各个字段。需要注意的是，其中一些字段采用了前文提到的另一套时间体系。

| 字段 | 说明 |
| ---- | ---- |
| Script Call Identifier | 基础函数的 Galaxy 脚本名称。如果启用了 `Show Natives`，这里也会显示某个特定原生函数的名称。 |
| Run | 触发器主体语句运行的次数。与 Triggers 标签中的同名字段一致。 |
| Average Self-Only Time | 触发器的平均执行时间，单位 ms。 |
| Average Self+Children Time | 触发器包含子调用在内的平均执行时间，单位 ms。 |
| Worst Self-Only Time | 触发器单次最长执行时间，单位 ms。这个字段对于查找可能造成延迟尖峰的来源非常有用。 |
| Worst Self+Children Time | 触发器包含子调用在内的单次最长执行时间，单位 ms。 |
| Total Self-Only Time | 触发器仅自身部分的总执行时间，单位 ms。 |
| Total Self+Children Time | 触发器包含子调用在内的总执行时间，单位 ms。 |

## Function Profiling 标签

[![Function Profiling 标签](./resources/053_Trigger_Debugger7.png)](./resources/053_Trigger_Debugger7.png)
*Function Profiling 标签*

Function Profiling 标签是 Trigger Profiling 标签在函数层面的对应物。它能让你进一步了解当前测试会话中各函数的性能表现。该部分 `Run Times` 中展示的时间是函数层级的时间，等同于前文所述的 `Self-Only` 时间。这个区别一开始可能会让人有些困惑，但在高强度性能分析中，它对于横向比较各元素的总体性能很有价值。

| 字段 | 说明 |
| ---- | ---- |
| Function Name | 函数的 Galaxy 脚本名称。 |
| Run | 函数被调用的次数。 |
| Average Run Time | 函数的平均执行时间，单位 ms。 |
| Worst Run Time | 函数单次最长执行时间，单位 ms。 |
| Total Time Run Time | 函数总执行时间，单位 ms。请注意，这些数值属于函数层级的 Run Time，本质上等同于 Self-Only 时间形式。 |

## Activity 标签

[![Activity 标签](./resources/053_Trigger_Debugger8.png)](./resources/053_Trigger_Debugger8.png)
*Activity 标签*

在 Activity 标签中，你会看到当前会话期间游戏活动的数据可视化。这里的 `Activity` 指的是下列三项属性的组合。

| 所描述的属性 | 标记颜色 |
| ------------ | -------- |
| Fired Events | Green |
| Checked Conditions | Yellow |
| Executed Triggers | Red |

图表会把每项属性绘制在一个“执行次数 vs 时间（ms）”的坐标图上，纵轴是执行次数，横轴是时间。你要注意，既然执行触发器必然要先检查条件，那么红线必然是黄线的子集，始终处于黄线覆盖的范围之内。总体来说，这个标签本身对性能的影响较小。实际上，你可以通过观察绿线是否突然大幅波动，来识别延迟尖峰。你也可以粗略判断任意时刻的总活动量或引擎负载，从而更直接地感知哪些代码部分值得进一步调查。

## Script Code 标签

从本质上说，Script Code 标签显示的是当前已加载项目的脚本代码。不过，它最大的优势在于自己就是一个完整的脚本调试器，支持设置断点、查看局部变量以及逐行单步执行代码。因此，在整个触发调试器中，这一部分拥有最核心也最实用的调试功能。你可以在错误发生的当下、就在对应位置找到它们，而不是靠输出信息和直觉去追踪问题。两种方法各有价值，但最严谨的排错工作应当在这里完成。

[![Script Code 标签](./resources/053_Trigger_Debugger9.png)](./resources/053_Trigger_Debugger9.png)
*Script Code 标签*

## 设置断点

要在运行时调试脚本，你首先必须设置断点。断点相当于调试器开始分析代码的入口。一旦代码执行流程命中断点，游戏就会暂停，调试器也会开始显示数据。你可以在脚本代码窗口中右键并选择 `Add/Remove Breakpoint` 来添加断点。要移除断点，只需选中已经带有断点标记的代码位置，再重复同样的操作。

## 进一步的调试注意事项

必须注意，这种技术在项目代码规模扩大后会面临什么问题。当项目达到一定体量时，手动查找目标代码行会变得很繁琐。对此，你可以切换到 Triggers 标签或 Trigger Profiling 标签，按名称排序后找到想调试的函数，再右键对应行并使用 `View Script`。如前所述，这会把你带回 Script Code 标签，并滚动到该函数的第一行代码。

另一种方式是在 Galaxy 地图脚本中直接设置断点：只需在目标位置写入关键字 `Breakpoint`。当执行流程遇到这个关键字时，触发调试器就会打开并跳转到断点所在位置。目前 GUI 还不支持这种方式，因此你需要使用宏或自定义脚本元素。

你还需要注意，Script Code 标签一次只能显示单个库的内容。这意味着如果项目使用了多个库，你就必须根据需要在不同库之间切换显示。切换的方法是对目标库中的某个触发器使用 `View Script` 命令。

## 分析数据

当断点设置完成，并且在游戏执行流程中实际命中后，游戏会暂停，并把焦点切到调试器。这时，主视图 Script Code 标签会显示额外信息，如下图所示。

![](./resources/053_Trigger_Debugger10.png)
*在 Script Code 标签中命中断点*

你可以使用每个子视图顶部的下拉菜单切换显示的数据，以便解析当前系统状态。下表列出了可用选项。

| 选项 | 说明 |
| ---- | ---- |
| None | 隐藏所选视图面板。 |
| Globals | 所有全局变量的列表，类似于 Variables 标签。 |
| Locals | 每个局部变量的信息，包括事件参数。 |
| Watch | 包含通过实用的 `Add Watch` 选项标记后的变量列表。 |
| Callstack | 当前调用栈，也就是函数调用层级，按自底向上的顺序显示。 |
| Breakpoints | 当前已设置的全部断点列表。 |

在实际使用中，`Locals` 选项的实用性远远高于其他选项。它允许你展开当前正在执行函数中的每一个值，便于查看。通过研究这些局部变量和参数，你可以跟踪某个特定函数的控制流程，并确认它的值是否落在预期范围内。
