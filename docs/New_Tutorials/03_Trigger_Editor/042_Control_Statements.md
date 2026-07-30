# 控制语句

控制语句是一类用于控制其他动作何时执行的动作。由于触发编辑器中的标准执行顺序是线性的，这些语句便具备了打破线性流程、创建动态系统的独特能力。你可以在创建动作时于 'General' 标签下找到控制语句，如下图所示。

[![控制语句动作](./resources/042_Control_Statements1.png)](./resources/042_Control_Statements1.png)
*控制语句动作*

## 循环

循环会创建一个重复执行的语句周期，并持续运行直到被打断。不同循环类型的差异，体现在它们如何结束，以及如何推进其内部语句。下表说明了各种循环类型。

| 循环类型 | 说明 |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Repeat | 重复执行包含其中的动作，直到达到 Count 变量指定的次数。计数会在每次完整执行完循环内容后递增。 |
| Repeat Forever | 无限循环，直到被其他控制语句打断。 |
| Pick Each Integer | 运行直到 Start 值达到 End 值。每次循环完成后该值都会递增。当前迭代值可通过 Picked Integer 函数标识符访问。 |
| Pick Each Player in Player Group | 对玩家组中的每个 Player 各执行一次循环。当前迭代的 Player 可通过 Picked Player 函数标识符访问。 |
| Pick Each Unit in Unit Group | 对单位组中的每个 Unit 各执行一次循环。当前迭代的 Unit 可通过 Picked Unit 函数标识符访问。 |
| For Each Real | 运行直到 Start 值达到 End 值。每次循环迭代后，Start 值都会按一个 Real 类型的 Increment 改变。 |
| For Each Integer | 运行直到 Start 值达到 End 值。每次循环迭代后，Start 值都会按一个 Integer 类型的 Increment 改变。 |
| For Each Player in Player Group | 对玩家组中的每个 Player 各执行一次循环。 |
| For Each UI Frame | 对指定 UI 变量中的每个框架各执行一次循环。 |
| For Each Unit in Unit Group | 对单位组中的每个 Unit 各执行一次循环。 |
| While | 只要给定的 Condition 仍为 True，就持续运行循环。 |

还有两个辅助控制语句可以改变循环行为。第一个是 Break，遇到它时会终止其所在循环。第二个是 Continue，遇到它时会跳过其下方所有动作，并直接回到循环开头。它们通常会和 If Then Else 这样的检查逻辑配合使用，以便在特定条件下控制循环行为。Repeat Forever 这种无限循环，通常就是靠这种方式最终退出。

## 等待

等待语句允许你暂停控制流。这个暂停可以是无限期的，也可以持续到某个事件发生，或直到满足特定条件。等待类型如下表所示。

| 等待类型 | 说明 |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Wait | 暂停控制流，直到经过指定的 Time。设置等待时长时还可以指定 Time Type。 |
| Wait For Timer | 暂停，直到某个 Timer 的 Elapsed 或 Remaining 时间达到指定值。 |
| Wait For Condition | 暂停，直到某个 Condition 成立。该 Condition 会按一定时间间隔被重复检查。 |
| Wait For Condition With Maximum Duration | 暂停，直到某个 Condition 成立。该 Condition 会按一定时间间隔被重复检查；此外还有一个保险用的 Max Duration，若其先耗尽，则控制流也会恢复。 |

## 使用控制语句

控制语句为打破项目逻辑默认线性流程提供了手段，因此也成为你用编辑器制作更丰富项目类型的关键工具。先来看下面这个触发器。

[![使用控制语句的触发器](./resources/042_Control_Statements2.png)](./resources/042_Control_Statements2.png)
*使用控制语句的触发器*

这个触发器中使用了多个控制语句。一个 While 循环中嵌套了 Pick Each Unit in Unit Group 循环，用于把整张地图上的单位填入一个单位组。演示地图中有六个陆战队员；当单位组被填充后，内部的 Pick Each 循环就能通过打破 While 的 Condition 来退出 While 循环。这个循环结束后，会进入一个 For Each Integer 循环。这个循环内部包含一个 1.0 秒的 Wait，以及一个输出当前计数值的文本动作。最终，这些动作会让触发器以 1 秒为间隔，在玩家屏幕上显示从 1 到 10 的计数。现在再看看另一个触发器。

[![计数结果触发器](./resources/042_Control_Statements3.png)](./resources/042_Control_Statements3.png)
*计数结果触发器*

这个触发器展示了前一个触发器计数循环中 Integer 计数值的另一种用途。当第一个触发器完成计数后，这个触发器会销毁地图上的所有陆战队员。结果如下图所示。

[![附着在单位上的文字标签效果](./resources/042_Control_Statements4.png)](./resources/042_Control_Statements4.png)
*附着在单位上的文字标签效果*

这个演示展示了控制语句如何让触发器之间基于时间而不是标准线性流程进行协作，从而实现一些简单而动态的结构。

## 附件

 * [042_Control_Statements.SC2Map](./maps/042_Control_Statements.SC2Map)
