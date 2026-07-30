# 动作定义

动作定义允许你创建自定义的动作语句序列。组装完成后，它们可以像组成自身的那些动作一样，在相同上下文中被调用。动作定义也支持参数，也就是能够改变定义行为的输入值。通过把动作组织成更高层级的结构块，你可以把那些经常重复的流程抽象成一个稳固的工具，在需要时随时调用。正确使用动作定义，不仅能节省时间、提升代码可读性，还可能带来性能优化。

## 演示动作定义

打开本文附带的演示地图并进入触发编辑器。这个项目中有一个在地图初始化时运行的触发器，它会在地图上生成三组不同位置的树和草地，并在生成模型时对演员进行一些修改。

[![生成树和草地的动作](./resources/038_Action_Definitions1.png)](./resources/038_Action_Definitions1.png)
*生成树和草地的动作*

整体逻辑其实很简单，但它由三段非常相似的流程组成，彼此之间只是在创建对象的位置上有所区别。像这种重复、臃肿的触发逻辑，通常就是提炼动作定义的好机会。在这个例子里，反复出现的模型创建选项以及对应的各种演员消息，都可以迁移进一个动作定义中。

要创建动作定义，请在触发器面板中右键，选择 新建 ▶︎ 新建动作定义。将该定义命名为 'Create Tree.' 然后按住 Shift，选中主触发器中的前五个动作，并把这些动作复制到新的动作定义中。完成后应如下所示。

[![Create Tree 动作定义](./resources/038_Action_Definitions2.png)](./resources/038_Action_Definitions2.png)
*Create Tree 动作定义*

现在，你已经把重复动作重构成了一个自定义动作定义。接下来，可以把 'Create Trees' 触发器改成调用这个动作定义三次，而不再保留当前这段臃肿代码。要在触发器中加入动作定义，需要从它的“调用位置”也就是动作列表中选取。右键点击触发器并选择 New Action，然后找到 'Create Tree' 定义，如下图所示。

[![使用新的动作定义](./resources/038_Action_Definitions3.png)](./resources/038_Action_Definitions3.png)
*使用新的动作定义*

重复添加三次，以对应它所取代的三段流程，并删除所有多余代码。结果应如下所示。

[![重新整理后的主触发器](./resources/038_Action_Definitions4.png)](./resources/038_Action_Definitions4.png)
*重新整理后的主触发器*

## 动作定义参数

虽然主触发器已经明显简化，但这里还遗漏了一点。别忘了，每一组树和草地原本都是在不同位置生成的。而按照当前写法，这个定义只会在一个位置生成树和草地，也就是 Point (20, 20)。

你可以通过参数让动作定义产生可变结果。参数值能够向动作定义传入额外信息。若想在三个不同位置创建对象，就需要一个可变化的 Point 参数。操作方式是进入 'Create Tree' 动作定义，右键点击 'Parameters' 标题并选择 新建 ▶︎ 新建参数。把参数命名为 'Location'，类型设为 Point。随后，把每个以 Point(20,20) 作为位置字段的动作，都改为使用变量 'Location'。完成后应如下所示。

[![带参数的动作定义](./resources/038_Action_Definitions5.png)](./resources/038_Action_Definitions5.png)
*带参数的动作定义*

现在返回主触发器。此时你会看到每个动作都要求为 'Location' 提供输入参数。把这些值改回树木原先生成的三个位置后，就会得到一个可正常工作的最终触发器，如下图所示。

[![image4](./resources/038_Action_Definitions6.png)](./resources/038_Action_Definitions6.png)

## 附件

 * [038_Action_Definitions_Start.SC2Map](./maps/038_Action_Definitions_Start.SC2Map)
 * [038_Action_Definitions_Completed.SC2Map](./maps/038_Action_Definitions_Completed.SC2Map)
