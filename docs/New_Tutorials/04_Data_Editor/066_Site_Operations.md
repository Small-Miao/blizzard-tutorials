# 站点操作

站点操作是一类用于修改宿主 Actor 物理属性的 Actor。由于这些修改会传播到与 Actor 相连的所有可视内容上，因此站点操作可以用来在游戏运行时改变模型的外观和摆放方式。

站点操作既包括平移、旋转这样最常见的功能，也能扩展到更复杂的控制。你可以用这些操作把多个对象嫁接到一起，甚至随着时间动态更新对象属性，以模拟独立运动或效果中的随机变化。还要注意，操作之间并不是互斥的；它们可以以很多方式叠加和组合。下图展示了站点操作的一些用途。

[![图像](./resources/066_Site_Operations1.png)](./resources/066_Site_Operations1.png)

通过附加操作拼装出的 Terratron -- 应用于雷神弹幕效果的随机变化操作

## 创建站点操作

作为一种 Actor，你可以在数据编辑器的 Actor 标签页中找到站点操作。通过 `+ ▶︎ Edit Actor Data ▶︎ Actor` 打开该标签页。

[![在数据中导航到 Actor](./resources/066_Site_Operations2.png)](./resources/066_Site_Operations2.png)
*在数据中导航到 Actor*

你可以像创建其他任何 Actor 一样，在 Actor 标签页中创建站点操作 Actor：在对象列表中右键，然后选择 `Add Actor`。

[![创建站点操作 Actor](./resources/066_Site_Operations3.png)](./resources/066_Site_Operations3.png)
*创建站点操作 Actor*

这会打开 `Actor Properties` 窗口。使用 `Actor Type` 下拉框，向下滚动到站点操作部分。它们都以 `Site Operation` 为前缀，并通过括号中的子类型加以区分。比如 `Site Operation (2D Rotation)` 这种 Actor 类型，就表示负责在 XY 平面内旋转的 2D Rotation 站点操作。下图展示了选择站点操作 Actor 的过程。

![站点操作 Actor 列表](./resources/066_Site_Operations4.png)
*站点操作 Actor 列表*

在这里选中任意站点操作后，点击 `Ok` 即可完成创建。

## 应用站点操作

你可以通过任意适用 Actor 的 `Host Site Operations` 字段来应用站点操作，下面展示了一个示例。

[![承载站点操作的字段](./resources/066_Site_Operations5.png)](./resources/066_Site_Operations5.png)
*承载站点操作的字段*

双击这个字段会打开 `Object Values` 窗口，其中会列出当前已挂载的站点操作，如下图所示。

[![站点操作视图](./resources/066_Site_Operations6.png)](./resources/066_Site_Operations6.png)
*站点操作视图*

在这个例子中，`SOpShadow` 和 `Terran Building Facing` 站点操作已经安装在该 Actor 上。它们分别是 Shadow 站点操作和 Explicit Rotation 站点操作的实例。要添加新的站点操作，可选择 `Choose` 字段，这会打开下方窗口。

[![选择要添加的站点操作](./resources/066_Site_Operations7.png)](./resources/066_Site_Operations7.png)
*选择要添加的站点操作*

这个窗口允许你选择一个站点操作。选定后，点击 `Ok` 返回站点操作主视图。作为本练习的一部分，请选择 `SOpHigherBy5` 操作。这样你会看到如下视图。

[![已准备添加的站点操作](./resources/066_Site_Operations8.png)](./resources/066_Site_Operations8.png)
*已准备添加的站点操作*

现在你可以点击窗口右侧的绿色 `+` 按钮，把这个站点操作添加到 Actor 上。它会进入活动站点操作列表，如下图所示。

[![已添加到 Actor 的站点操作](./resources/066_Site_Operations9.png)](./resources/066_Site_Operations9.png)
*已添加到 Actor 的站点操作*

然后点击 `Ok` 保存这个添加操作，并返回数据编辑器主视图。

## 操作顺序

需要特别注意的是，站点操作依赖顺序。以不同顺序应用相同的站点操作，效果可能天差地别，也可能看不出变化。为帮助你管理这一点，编辑器提供了一个 `Host Site Operations` 子编辑器，让你可以调整各项操作的应用顺序。

![站点操作排序控件](./resources/066_Site_Operations10.png)
*站点操作排序控件*

你可以在该窗口右侧找到改变应用顺序的控件。使用时，先从列表中选中一个站点操作，再点击上下箭头按钮，把它在操作列表中上移或下移。站点操作会按这个窗口中从上到下的顺序依次应用。

要理解应用顺序的影响，可以想想 `Local Offset` 和 `Rotator`。前者会把一个 Actor 在 3D 空间中移动一定距离，后者则会让 Actor 围绕某个点旋转。这个例子里，这两者会以不同顺序作用到一个 `Game Ball` Actor 上。下图展示的是先应用 Local Offset 的情况。

![先 Local Offset 后 Rotator 的站点操作](./resources/066_Site_Operations11.png)
*先 Local Offset 后 Rotator 的站点操作*

下一张图展示的是在 Local Offset 之前先应用 Rotator 会发生什么。

![先 Rotator 后 Local Offset 的站点操作](./resources/066_Site_Operations12.png)
*先 Rotator 后 Local Offset 的站点操作*

这里最重要的结论是：即使应用的是同一组操作，结果也并不相同。下面逐条说明这两种情况发生了什么。

  - 需要注意，Local Offset 的方向是向下，距离大约等于球体高度的 25%；Rotator 则大约是 90 度，也就是四分之一圈。
  - 先应用 Local Offset，会先把球向下偏移，但同时也会让 Rotator 的旋转轴跟着偏移。这样旋转轴就不再位于球心。结果就是，旋转不再只是旋转本身，还会改变球的位置，让它向前冲并陷入地面。球体的旋转以及 X、Z 轴位置都发生了变化。
  - 相比之下，先应用 Rotator 则会让球在原地旋转，然后再整体向下偏移进入地面。这意味着旋转轴依然保持在球心。这种情况下，球只发生了旋转以及 Z 轴位置变化。

正如上面所示，站点操作对顺序的敏感性有时会导致难以预测的结果。避免这种不确定性的最好办法，要么是事先仔细规划，要么就是多做实验，观察不同操作组合在一起时的反应。
