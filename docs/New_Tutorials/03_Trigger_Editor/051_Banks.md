# Bank

Bank 是编辑器用来存储信息并在不同项目之间复用这些信息的方式。下图展示了一个示例 Bank 文件。

[![自定义游戏 Bank 文件](./resources/051_Banks1.png)](./resources/051_Banks1.png)
*自定义游戏 Bank 文件*

这个 Bank 存储了某种自定义游戏中玩家的个人统计数据。注意这里的结构。一个 Bank 首先按分区分层，然后再细分为键值对。把值存入 Bank 时，它们会写入目标分区，并以各自关联的键保存，除此之外没有特定顺序。

## Bank 动作

处理 Bank 的控件可以在创建动作时于 `Bank` 标签下找到。如下图所示，后面还附有一个表格来拆解这些控件的作用。

[![Bank 动作](./resources/051_Banks2.png)](./resources/051_Banks2.png)
*Bank 动作*

| 动作 | 效果 |
| ---- | ---- |
| 预加载 Bank | 为指定玩家预加载并同步一个 Bank。 |
| 创建 Bank 分区 | 创建一个 Bank 分区，之后就可以向其中写入键值对。 |
| 保存 Bank | 保存一个 Bank，确保对文件所做的全部更改会在游戏结束后保留下来。 |
| 存储数据 | 将某个值作为键保存到 Bank 的某个分区中。可存储的类型包括 Boolean、Integer、Real、Point、String、Text 和 Unit。 |
| 重新加载 Bank | 重新加载一个 Bank，撤销两次保存之间发生的任何更改。 |
| 打开 Bank | 打开一个 Bank 以便使用和修改。 |
| 恢复单位 | 创建一个先前通过 `Store Unit` 动作存入的单位。该单位会从 Bank 某个分区中的某个键恢复，并在指定 Point、归属于某个 Player、朝向指定 Angle 创建出来。 |
| 等待 Bank | 这是一个等待控制语句，会一直暂停到指定 Bank 的“已重新加载”条件为 True。 |
| 设置 Bank 选项 | 将 Bank 的签名选项设置为启用或禁用。签名提供了一种加密方式，使玩家无法随意修改 Bank。 |
| 移除 Bank 键 | 从 Bank 的某个分区中移除一个键及其关联的值。 |
| 移除 Bank 分区 | 从 Bank 中移除一个分区以及其中包含的所有键值对。 |

## 查找本地 Bank 存储

你可以在下面所示的 Windows 路径中找到本地 Bank 存储。

  - Libraries
      - Documents
          - StarCraft II
              - StarCraftPlayer.ID@\#
                  - Banks
                      - ID Code
                          - Bank Files

这一过程如下方图片序列所示。

[![访问本地 Bank 存储](./resources/051_Banks3.png)](./resources/051_Banks3.png)
*访问本地 Bank 存储*
