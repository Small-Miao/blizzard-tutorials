# 数学函数

编辑器内置了一套标准数学函数，可在触发编辑器中的各种工作里使用。只要你在填写数值字段，就可以通过选择 'Function' 来源并按 'Math' 标题筛选来访问它们，如下图所示。

[![数学函数选择](./resources/045_Math_Functions4.png)](./resources/045_Math_Functions4.png)
*数学函数选择*

## 类型转换

从上图可以看到，大多数数学函数同时提供 Real 和 Integer 两个版本。这里的版本指的是函数最终输出的类型，而这些函数的输入类型则不一定相同。下面通过一个例子详细说明。

![函数版本](./resources/045_Math_Functions5.png)
*函数版本*

在这个例子中，Arithmetic 数学函数列出了两个版本：输出整数的 Arithmetic(Integer) 和输出实数的 Arithmetic(Real)。注意，这里正在设置的是一个 Real 字段，但列表中同时出现了 Integer 和 Real 两类函数。这是因为所有 Integer 函数都带有到 Real 的隐式类型转换，因此在这种场景下也可以被选用。这种类型转换只是简单地为整数结果补上一个小数部分 0。例如 Arithmetic (Integer) 的 1 + 1 会得到 Integer 2，然后被隐式转换成 Real 2.0。

Arithmetic 的 Integer 版和 Real 版都要求输入类型与输出类型一致。不过，也有一些带版本的数学函数只接受 Real 输入，但仍会输出对应名称中的 Integer 或 Real 类型。Log2 就是一个例子。它为了保证足够精度，只接受 Real 数值输入，但 Log2 Real 和 Log2 Integer 两个版本仍分别输出对应类型。

对于这类需要先提供 Integer 输入的情况，编辑器也提供了显式类型转换。你可以在填写 Real 字段时，进入 'Function' 来源，然后选择 'Conversion' 标签找到它，如下图所示。

[![Integer 到 Real 的转换函数](./resources/045_Math_Functions6.png)](./resources/045_Math_Functions6.png)
*Integer 到 Real 的转换函数*

显式整数转换与隐式转换的工作方式相同，都是给结果补上小数部分 0。这个函数也适合在那些只提供 Real 版本的数学函数中输入整数值，本文后面会讲到这类函数。若需要反方向的转换，在填写 Integer 字段时还可以使用显式的 Real 到 Integer 转换函数。

[![Real 到 Integer 的转换函数](./resources/045_Math_Functions7.png)](./resources/045_Math_Functions7.png)
*Real 到 Integer 的转换函数*

'Convert Real to Integer' 会把 Real 四舍五入到最近的整数 Integer；所有以 .5 结尾的 Real 值都会向下舍入。

## 共享数学函数

现有数学函数中，有 16 个是共享运算，它们同时提供输出为 Real 和 Integer 的版本，如下表所示。本文不会对这些数学概念做完整讲解，但会给部分函数附上示例。除非特别说明，每个函数都接收与自身版本相对应的 Real 或 Integer 输入。

| 函数 | 说明 |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Absolute Value | 返回一个数的绝对值，也就是不带符号的大小。 |
|  | Absolute Value (-2.0) = 2.0 |
|  | Absolute Value (4) = 4 |
| Arithmetic | 创建一个由两个参数组成的表达式，并使用四种基本运算符之一：加、减、除、乘。该表达式的结果会被填入字段中。这是非常常用的函数；若要扩展到更多参数，可以在某一项中继续嵌套 Arithmetic，或使用 Arithmetic Multiple。 |
| Arithmetic Multiple | 对任意数量的参数执行一次同类型的基本运算。与 Arithmetic 一样，可用的运算包括加、减、除和乘。下表后面的内容会进一步介绍 Arithmetic 的使用。 |
| Ceiling | 向上取整到下一个整数。只接受 Real 输入。 |
|  | Ceiling Real (2.1) = 3.0 Real |
|  | Ceiling Real (-3.2) = -3.0 Real |
|  | Ceiling Integer (5.1) = 6 Integer |
| Clamp | 将某个值限制在给定的上下边界之间。超过上限时会被设为上限，低于下限时会被设为下限。 |
|  | Clamp 40.0 between 1.0 and 10.0 = 10.0 |
|  | Clamp -5 between 1 and 10 = 1 |
| Floor | 向下取整到前一个整数。只接受 Real 输入。 |
|  | Floor Real (2.1) = 2.0 Real |
|  | Floor Real (-3.2) = -4.0 Real |
|  | Floor Integer (5.1) = 5 Integer |
| Log2 | 返回该数的以 2 为底对数。也就是 2 需要取多少次幂才能得到这个数。只接受 Real 输入。 |
|  | Log2 Real (40.0) = 5.3 Real |
|  | Log2 Integer (40.0) = 5 Integer |
| Maximum | 返回两个数中较大的那个。 |
|  | Maximum (10, 15) = 15 |
|  | Maximum (20.0, 18.0) = 20.0 |
| Minimum | 返回两个数中较小的那个。 |
|  | Minimum (10, 15) = 10 |
|  | Minimum (20.0, 18.0) = 18.0 |
| Modulo | 返回整数除法后的余数。只接受 Integer 输入。 |
|  | Modulo (9, 6) = 3 |
| Power | 将数值提升到指定 Power 幂次。只接受 Real 输入。 |
|  | Power Real (8.0, 2.0) = 64.0 Real |
|  | Power Integer (8.0, 2.0) = 64 Integer |
| Random Real | 返回指定范围内的随机实数。 |
| Random Integer | 返回指定范围内的随机整数。 |
|  | Random real between 0 and 100.0 = 42.5 |
|  | Random integer between 25 and 50 = 26 |
| Round | 将数值舍入到最近的整数。所有以 .5 结尾的数都会向下舍入。只接受 Real 输入。 |
|  | Round Real (3.2) = 3.0 Real |
|  | Round Integer (2.5) = 2 Integer |
| Square Root | 返回该值的平方根。只接受 Real 输入。 |
|  | Square Root Real (43.0) = 6.6 Real |
|  | Square Root Integer(43.0) = 7 Integer |
| Trunc | 移除数值的小数部分。只接受 Real 输入。 |
|  | Trunc Real (3.5) = 3.0 Real |
|  | Trunc Real (2.6) = 2.0 Real |
|  | Trunc Integer(2.6) = 2 Integer |

## Real 数学函数

此外还有 9 个只输出 Real 类型的数学函数。这些函数大多偏重三角运算，因此提供精度较低的 Integer 版本意义不大。当然，你仍然可以通过 'Convert Real to Integer' 函数来生成 Integer 输出。还要注意，其中有些函数会把输入看作角度或百分比之类的数值类型，但这主要只是建模意义上的区分；它们输入和输出的实际数据类型始终都是 Real。下表列出了这些 Real 数学函数。

| Function | Description |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| Arccosine | 返回该数余弦函数的反函数值。 |
| Arcsine | 返回该数正弦函数的反函数值。 |
| Arctangent From Deltas | 接收 X 和 Y 的增量，返回这两个数比值的反正切。 |
| Arctangent From Value | 返回给定 Value 的反正切。 |
| Cosine | 将输入视为角度，返回其余弦值。 |
| Sine | 将输入视为角度，返回其正弦值。 |
| Tangent | 将输入视为角度，返回其正切值。 |
| Random Angle | 返回一个 0 到 360.0 之间的随机 Real。 |
| Random Percent | 返回一个 0 到 100.0 之间的随机 Real。 |

## 变量动作

凡是使用数值字段的动作、事件或条件，都可以使用数学函数。不过，数学函数最常见的用途之一，是配合非常实用的变量动作。下面这三个动作负责变量的修改与初始化。你可以在创建动作时进入 'Variable' 标签找到它们，如下图所示。

[![变量动作](./resources/045_Math_Functions8.png)](./resources/045_Math_Functions8.png)
*变量动作*

其中最核心的是 Set Variable 动作，它能够构建一个表达式，把任意类型的变量设为某个值，如下图所示。

[![Set Variable 动作](./resources/045_Math_Functions9.png)](./resources/045_Math_Functions9.png)
*Set Variable 动作*

变量被设定的值可以是直接指定的字面值，也可以来自另一个变量、预设或函数。最后一种情况就支持数学函数，而这些元素组合在一起后，就能以非常强大的方式修改游戏变量。下面的例子就使用 Set Variable 动作，通过一串数学函数为一个数值型 Real 字段赋值。

[![使用数学函数的 Set Variable](./resources/045_Math_Functions10.png)](./resources/045_Math_Functions10.png)
*使用数学函数的 Set Variable*

Modify Variable (Integer) 和 Modify Variable (Real) 可以看作 Set Variable 的同类动作，但用途更加专门。它们允许你用一种近似内置 Arithmetic 函数的方式来修改数值变量，每种数值类型各有一个版本。正因为形式更固定，这两个动作不如 Set Variable 那样灵活，但在原型搭建或简单变量修改中会更快。下图展示了它们的示例。

[![Modify Variable 动作](./resources/045_Math_Functions11.png)](./resources/045_Math_Functions11.png)
*Modify Variable 动作*

和共享数学函数一样，Modify Variable (Integer) 在必要时也会使用从 Real 到 Integer 的隐式转换。

## 演示数学函数

接下来，把注意力转到本文附带的演示地图，看看数学函数在实战中的表现。打开地图后，你会看到如下场景。

[![演示地图场景](./resources/045_Math_Functions12.png)](./resources/045_Math_Functions12.png)
*演示地图场景*

这里的陆战队员面前有几个有意思的选择。每个信标都连接着一个触发动作，会把某个数学函数应用到游戏变量上，而这个变量就是该陆战队员的生命值。你可以查看区域层，确认每个信标都对应一个大小合适的圆形区域。接着进入触发编辑器继续查看。

[![触发编辑器视图](./resources/045_Math_Functions13.png)](./resources/045_Math_Functions13.png)
*触发编辑器视图*

从逻辑上看，这张地图设置了一个初始化触发器，用于配置摄像机和文字标签等效果，并把这个陆战队员存入一个单位变量，以供后续使用。之后，有两个独立触发器分别响应 'Unit Enters Region' 事件。在上图所示触发器中，进入 HP Addition 区域会执行一个使用数学函数的动作。更具体的写法如下。

![](./resources/045_Math_Functions14.png)
*带数学函数的 Set Unit Property 动作*

当陆战队员进入区域时，会通过一个 Arithmetic 数学函数把其最大生命值增加 5。测试地图后，结果如下图所示。

[![陆战队员生命值增加 5](./resources/045_Math_Functions15.png)](./resources/045_Math_Functions15.png)
*陆战队员生命值增加 5*

数学函数已经直接对游戏变量产生了变化。如果你愿意，可以继续把陆战队员移动进这个信标区域以重复效果。若你再去看 Unit Subtracts Life Region 触发器，会看到下面的内容。

[![Unit Subtracts Life 触发器](./resources/045_Math_Functions16.png)](./resources/045_Math_Functions16.png)
*Unit Subtracts Life 触发器*

这个触发器同样使用了 Arithmetic 函数，但这一次执行的是减法，用来降低陆战队员的生命值。为避免生命值被减到 0，又额外串接了一个 Maximum (Real) 数学函数，确保结果不会低于安全值。它们的效果如下图测试所示。

![陆战队员生命值降低到 5](./resources/045_Math_Functions17.png)
*陆战队员生命值降低到 5*

反复进入该区域后，触发器最终会触发 Maximum 函数，把内部 Arithmetic 运算的输出钳制在 5。

## 附件

 * [045_Math_Functions.SC2Map](./maps/045_Math_Functions.SC2Map)
