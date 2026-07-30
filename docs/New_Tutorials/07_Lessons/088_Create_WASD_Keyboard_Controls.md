# 创建 WASD 键盘控制

使用键盘来控制游戏角色移动，是一种非常经典的操作方案。其中最知名的例子就是 WASD 键盘控制，也就是把 `W`、`A`、`S` 和 `D` 四个键分别绑定到上、左、右、下四个方向。这种方案常被采用，因为它符合人体工学、几乎人人可用，并且一只手即可操作，另一只手还能继续使用鼠标。

虽然《星际争霸》本身使用鼠标加键盘的控制方式，但它并不会把键盘直接绑定到移动上。不过，对于使用编辑器制作的自定义项目来说，这完全可以实现。学习如何制作 WASD 键盘控制，不仅很有教育意义，在动作、冒险和模拟类游戏里也有大量实际用途。

## 按键 UI 事件

键盘控制系统必须有某种方式，把玩家输入传递到游戏逻辑中。UI 事件通常很适合承担这个职责，而在这里你将使用 `Key Pressed` 事件。`KeyPressDOWN` 和 `KeyPressUP` 这两个触发器分别创建了对 WASD 四个按键的响应事件。`KeyPressDOWN` 会在这些键被按下时运行，而 `KeyPressUP` 会在这些键被释放时运行。下图展示了这些触发器的组成方式。

[![按键 UI 事件触发器](./resources/088_Create_WASD_Keyboard_Controls7.png)](./resources/088_Create_WASD_Keyboard_Controls7.png)
*按键 UI 事件触发器*

通过把每个按键独立处理，这套系统就能支持硬件允许的任意按键同时按下与释放组合。因此，像同时按下 `W` 和 `A` 这样的组合，也能正确地让角色同时向上和向左移动。

## 按键存储数组

为了跟踪所有并发按键状态，这套设计需要一个数组来存储。`KEYPRESS` 数组的类型是 `Boolean`，大小设为 `3`。考虑到数组索引从 `0` 开始，这实际上提供了 `4` 个位置，足够整个 WASD 控制系统使用。使用布尔数组的好处在于，每个数组值都能表示某个按键当前是否被按下。按键与索引的对应关系如下。

W == Index 0

A == Index 1

S == Index 2

D == Index 3

值为 `True` 表示已按下，值为 `False` 表示未按下。数组中的所有位置默认都设为 `False`。数组本身如下图所示。

[![按键存储数组](./resources/088_Create_WASD_Keyboard_Controls8.png)](./resources/088_Create_WASD_Keyboard_Controls8.png)
*按键存储数组*

## Switch 动作

正如你已经看到的，这两个按键触发器都会对四个 WASD 按键中的任意一个做出响应。这样一来，设计就不必依赖八个独立事件来监视键盘输入。但这也意味着你需要使用一些控制语句来正确解析输入。可以用 switch 语句来解决这个问题，为四个不同的按键值分别提供一个 case。`KeyPressUP` 触发器中使用的 switch 语句如下图所示。

![KeyPressUP Switch](./resources/088_Create_WASD_Keyboard_Controls9.png)
*KeyPressUP Switch*

在每个触发器中，switch 语句都包含一个“设置变量”动作，用于把对应 case 的按键所关联的数组索引设为某个值。对于 `KeyPressUP` 触发器，这些值会被设为 `True`，表示该键当前处于按下状态。相反，在 `KeyPressDOWN"` 触发器中，这些值会被设为 `False`，表示该键已经释放。

## 按键检测循环

为了把持续不断的按键输入流送入移动逻辑中，这个设计使用了一个名为 `KeyPress Detect` 的循环。这个循环本身较长，下面按原样列出，并附带注释说明。

  - General -- While (Conditions) are true, do (Actions) // While 循环允许持续移动

<!-- -->

  - Conditions

<!-- -->

  - (WASD Unit is alive) == True // 检查受控单位是否仍然存活

<!-- -->

  - Actions

<!-- -->

  - General -- If (Conditions) then do multiple (Actions) // 触发一系列输入检查

<!-- -->

  - If Then Else

<!-- -->

  - General -- Else If // 这些检查会寻找某一种特定的

<!-- -->

  - Else If // 输入情况

<!-- -->

  - KEYPRESS\[0\] == False
  - KEYPRESS\[1\] == False
  - KEYPRESS\[2\] == False // 这里表示四个键
  - KEYPRESS\[3\] == False // 全部都没有按下

<!-- -->

  - Then

<!-- -->

  - Unit -- Order WASD Unit to (Stop)(Replace Existing Orders) // 停止移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[0\] == True
  - KEYPRESS\[1\] == True // W 和 A 键被按下

<!-- -->

  - Then

<!-- -->

  - Execute Move(145.0, 1.0) // 将单位向东北移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[0\] == True // W 和 D 键被按下
  - KEYPRESS\[3\] == True

<!-- -->

  - Then

<!-- -->

  - Execute Move(45.0, 1.0) // 将单位向西北移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[2\] == True // S 和 A 键被按下
  - KEYPRESS\[1\] == True

<!-- -->

  - Then

<!-- -->

  - Execute Move(45.0, -1.0) // 将单位向东南移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[2\] == True // S 和 D 键被按下
  - KEYPRESS\[3\] == True

<!-- -->

  - Then

<!-- -->

  - Execute Move(145.0, -1.0) // 将单位向西南移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[0\] == True // W 键被按下

<!-- -->

  - Then

<!-- -->

  - Execute Move(90.0, 1.0) // 将单位向北移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[1\] == True // A 键被按下

<!-- -->

  - Then

<!-- -->

  - Execute Move(180.0, 1.0) // 将单位向西移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[2\] == True // S 键被按下

<!-- -->

  - Then

<!-- -->

  - Execute Move(90.0, -1.0) // 将单位向南移动

<!-- -->

  - General -- Else If

<!-- -->

  - Else If

<!-- -->

  - KEYPRESS\[3\] == True // D 键被按下

<!-- -->

  - Then

<!-- -->

  - Execute Move(0.0, -1.0) // 将单位向东移动

<!-- -->

  - General -- Wait 0.0 Real Time Seconds // 为测试输出创建一个停顿

## 移动单位

`KeyPress Detect` 会把一组指令传给 `Execute Move` 动作，由它来处理受控单位的移动。这些指令包括一个 `Angle` 和一个 `Offset`，它们会被送入 `Order Targeting Point` 命令。这个命令使用的是 `Move` 技能，并把单位派往一个“带极坐标偏移的点”。由于命令把单位当前的位置再加上这个偏移作为目标点，因此它实际上会让单位沿 `Angle` 指定的方向、按 `Offset` 的大小进行移动。

当 `Offset` 设为负值时，它实际上表示让单位朝相反方向移动。例如，当 `S` 和 `D` 键被按下时，会使用 `145.0` 的角度和 `-1.0` 的偏移，因此单位会向西南方向移动。而当 `W` 和 `A` 键被按下时，角度仍是 `145.0`，但偏移为 `1.0`，于是单位会向东北方向移动。`Execute Move` 动作定义如下图所示。

[![Execute Move 动作定义](./resources/088_Create_WASD_Keyboard_Controls10.png)](./resources/088_Create_WASD_Keyboard_Controls10.png)
*Execute Move 动作定义*

## 把系统连接起来

在本演示中，移动系统会在地图开始时初始化。这并不是硬性要求，但却是最常见的场景。控制方案很少会在游戏进行过程中发生变化，不过你仍然可以把初始化动作放到别的位置。`近战初始化` 触发器如下图所示。

![近战初始化 触发器](./resources/088_Create_WASD_Keyboard_Controls11.png)
*近战初始化 触发器*

这个触发器会把一名预先放置在地图上的单位赋值给 `WASD Unit` 变量，使其成为这套移动系统的受控单位。随后，`KeyPress Detect` 循环开始运行，并立即开始监听玩家输入。只要你在制作持续运行的 UI 事件系统，就应密切关注大量循环与事件检查带来的性能影响。如果你打算把这样的系统用于联机环境，务必认真测试，确保延迟不会成为问题。

## 测试控制

启动地图后，你就可以用 WASD 键控制陆战队员移动了。

[![WASD 键盘控制](./resources/088_Create_WASD_Keyboard_Controls12.png)](./resources/088_Create_WASD_Keyboard_Controls12.png)
*WASD 键盘控制*

## 附件

 * [088_Create_WASD_Keyboard_Controls.SC2Map](./maps/088_Create_WASD_Keyboard_Controls.SC2Map)
