# 对话框面板

对话框面板是对话框项的一种分组方式。通过创建一个面板并在其上承载多个对话框项，你就可以通过修改面板本身来统一控制这些项目。你对面板做出的任何改动，都会传播到属于该面板的每个项目。因此，面板在对话框与其他对话框项之间提供了一层层级结构。它的工作方式很像单位组或玩家组，功能上相当于一个对话框分组。

## 创建对话框面板

作为示例，你可以像放置普通对话框项一样，把一个面板放进某个对话框中。

[![创建承载对话框](./resources/044_Dialog_Panels1.png)](./resources/044_Dialog_Panels1.png)
*创建承载对话框*

然后，通过选择 Create Dialog Item 动作来创建一个面板，如下图所示。

[![创建面板对话框项](./resources/044_Dialog_Panels2.png)](./resources/044_Dialog_Panels2.png)
*创建面板对话框项*

在 Create Dialog Item 的 Type 字段中选择 'Panel'。

[![选择创建类型为 Panel](./resources/044_Dialog_Panels3.png)](./resources/044_Dialog_Panels3.png)
*选择创建类型为 Panel*

你可以通过 Set Dialog Item Size to Parent 动作，把面板的属性设置为与其父对话框相同的 Width、Height 和 Position。

[![设置面板大小](./resources/044_Dialog_Panels4.png)](./resources/044_Dialog_Panels4.png)
*设置面板大小*

此时应把这个面板保存到变量中，以便为后续操作提供稳定句柄。到这一步为止，这些动作实际上已经构成了一个完整面板，其效果应如下图所示。

[![已创建的面板](./resources/044_Dialog_Panels5.png)](./resources/044_Dialog_Panels5.png)
*已创建的面板*

## 在对话框面板中创建项目

面板搭好后，你可以在其中创建一些对话框项来测试其功能。下面这个触发器更新版里，就在面板中创建、调整并定位了一对按钮。

[![在面板内创建的对话框项](./resources/044_Dialog_Panels6.png)](./resources/044_Dialog_Panels6.png)
*在面板内创建的对话框项*

这里最重要的区别在于，这些项目不是直接创建在对话框里，而是创建在面板内部。正因为如此，这些对话框项才会被“分组”到该面板中，并自动接受施加到面板上的任何改动。你还应注意，面板的变量句柄在这里非常重要，因为每创建一个内部对话框项时，都需要反复引用该面板。有时候对话框项本身不需要单独句柄，因为它们可以完全由所属对话框或面板控制；但面板本身通常都需要一个句柄。

## 修改对话框面板及其内部项目

现在一切都准备好了，可以快速演示一下对话框面板的实际价值。在下图所示的触发器更新中，Set Dialog Item Color 被用来作用于这个面板。

[![修改面板属性](./resources/044_Dialog_Panels7.png)](./resources/044_Dialog_Panels7.png)
*修改面板属性*

像这样修改面板后，系统最终会向每个对话框项发送单独的 Set Dialog Item Color 动作，这里也就是两个按钮。使用 “Test Document” 测试地图后，你会看到每个按钮都被设置成了 Yellow。效果如下图所示。

![](./resources/044_Dialog_Panels8.png)
*面板改动传播到其内部项目*

这应该能让你体会到，使用对话框面板能节省多少操作量。它们非常适合快速分组并管理大量对话框项。

## 附件

 * [044_Dialog_Panels.SC2Map](./maps/044_Dialog_Panels.SC2Map)
