# 条件定义

条件定义用于创建自定义条件。普通条件本身已经支持一定程度的自定义，例如选择参数项以及应用不同运算符。而定义系统则允许你进一步扩展这些能力，定义可以使用编辑器中任意现有语句的自定义操作。借助它，你可以提出新的游戏状态判断方式，也可以把常用条件组织并组合起来。

[![条件定义视图](./resources/039_Condition_Definitions1.png)](./resources/039_Condition_Definitions1.png)
*条件定义视图*

| 字段 | 说明 |
| --- | --- |
| Options | 这里用于选择定义类型，本篇重点关注的是条件定义。 |
| Return Type | 决定条件返回的结果类型。Boolean 提供标准的“真或假”；Integer 则允许返回超出二元判断的更多结果。 |
| Parameters | 传递给条件的参数值。 |
| Grammar Text | 设置 GUI 中用于把条件表述成自然语言的文本。 |
| Hint Text | 设置当选中该条件时，在提示子视图中显示的说明文字。 |
| Custom Script Code | 允许使用 Galaxy Script 而不是 GUI 语句来编写该条件。 |
| Local Variables | 在该条件作用域内可用的变量。 |
| Actions | 构成该条件逻辑的动作语句。 |

## 演示条件定义

打开本文附带的演示地图，并查看地形编辑器。你会看到一组 SCV 和两个掠夺者。这些单位平均分属红色的玩家一和白色的玩家二。本练习中，你将创建一个自定义定义，用于测试单位选择事件。这个定义所提出的问题是：“这个单位是否是属于玩家一的掠夺者？”

先进入触发编辑器，在触发器面板中右键并选择 新建 ▶︎ 新建条件定义。把新定义命名为 'Unit Type Owned by Player.' 这个名字现在看起来可能有点奇怪，不过很快你就会明白。该条件定义需要两条信息来完成判断：单位类型和玩家编号。为接收这些信息，它将使用参数。进入该定义后，右键点击 'Parameters' 标题并选择 新建 ▶︎ 新建参数。重复一次。将第一个参数命名为 'Unit Type'，类型设为 --Game Link，并把 'Link type' 设为 Unit。将第二个参数命名为 'PlayerID'，类型设为 Integer。

最后，创建动作 'If Then Else.' 在 'If' 标题下创建两个条件，分别设为 Unit Type == Marauder 和 PlayerID == 1。注意要让 Unit Type 和 PlayerID 都引用你刚才创建的参数。

## Return 语句

一个条件若要生效，就必须给出某种返回结果。你在标准条件中已经见过这一点。通常它们会返回 Boolean 值，也就是真或假。在这里，要创建这样的返回值，需要使用一种特殊控制语句：Return。遇到 Return 语句时，当前这一层控制流会立即退出，在这里也就是退出条件定义。随后，它会把返回值带回条件最初被调用的位置。借助像 'If Then Else' 这样的控制语句，条件可以通过多个可能的 Return 语句来控制不同的返回结果。

在 'Then' 标题下添加一个 'Return' 动作，并把值设为 True。再在 'Else' 标题下添加一个 'Return' 动作，并把值设为 False。这样定义就完成了。它接收两个参数，并检查它们是否分别等于 Marauder 和 1。若为真，则返回 true；否则返回 false。

## 语法文本与提示文本

条件定义系统还提供了一些选项，让自定义定义更易于使用。Grammar Text 允许你设置自定义的自然语言表述方式，和多数 GUI 触发器的写法类似。注意，当前这个定义的描述是 Unit Type Owned by PlayerID (Unit Type, PlayerID)。把这样的描述直接放进触发器里会有些累赘。你可以进入 'Grammar Text'，取消勾选 'Use Default Grammar Text.' 在新的文本排列中必须保留参数，所以你可以删掉其余文字，把字段改成 'Unit Type owned by Player.' 这种格式更接近编辑器现有条件的风格，也正好解释了前面为什么取这个名字。

另一个方便选项是 'Hint Text.' 它允许你为该定义设置自定义说明，并在每次选中该定义时显示在提示子视图中。你可以选中 'Hint Text' 标题，并输入 Returns true if the Unit is a Marauder, and the Player is 1. 完成这些步骤后，定义应显示如下。

![已完成的条件定义](./resources/039_Condition_Definitions2.png)
*已完成的条件定义*

## 使用条件定义

创建一个新触发器，并命名为 'Unit Selected.' 进入该触发器后，创建一个 'Unit is Selected' 事件。接着，在 'Conditions' 上右键并选择 新建 ▶︎ 新建条件，创建一个使用该自定义定义的条件。这会打开“配置条件”窗口，其中应能看到你刚刚定义的新条件，如下图所示。

![](./resources/039_Condition_Definitions3.png)
*选择自定义条件定义*

选中这个条件并设置参数。Unit Type 应设为 Unit Type of (Triggering Unit)，而 PlayerID 应设为 Owner of (Triggering Unit)。点击 'Ok' 返回项目。最后，创建一个 'Text Message' 类型的动作，并把其 'Message' 设为 Success，结果应如下所示。

[![已完成的触发器](./resources/039_Condition_Definitions4.png)](./resources/039_Condition_Definitions4.png)
*已完成的触发器*

测试时，游戏会显示地形编辑器中看到的那组单位。若你选择的不是红色掠夺者，将不会有任何响应。若你选择玩家一的红色掠夺者，自定义条件测试就会返回 True，从而允许触发器执行其主体语句并显示消息。启动游戏并运行测试后，应会看到如下效果。

[![成功输出](./resources/039_Condition_Definitions5.png)](./resources/039_Condition_Definitions5.png)
*成功输出*

## 附件

 * [039_Condition_Definitions_Start.SC2Map](./maps/039_Condition_Definitions_Start.SC2Map)
 * [039_Condition_Definitions_Completed.SC2Map](./maps/039_Condition_Definitions_Completed.SC2Map)
