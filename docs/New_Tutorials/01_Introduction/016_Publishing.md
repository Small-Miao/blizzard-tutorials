# 发布

虽然使用编辑器制作的项目可以在本地以单人方式游玩，但大多数创作者都希望把作品分享到线上。幸运的是，Battle.net 这一暴雪的游戏托管平台，已经扩展到支持在《星际争霸》引擎上进行开发的创作者。项目必须依靠房主手动开房分享的时代已经过去了，那是 WarCraft III 和 母巢之战 时期的做法。现在，玩家可以在无需开发者介入的情况下找到并下载游戏。这个独立运作的系统还为用户提供了额外的保护、版本控制和本地化支持。

不过，基于网络的项目分享也要求游戏开发者提前做好一些准备。这些准备主要体现在编辑器中新加入的发布系统上。你可以通过 `文件 ▶︎ Publish` 找到它，它会打开如下所示的“发布文档”窗口。

[![Publish Document Window](./resources/016_Publishing01.png)](./resources/016_Publishing01.png)
*发布文档窗口*

你首先看到的是 “Log In” 视图，请注意其中的 “Verification” 字段，它会说明当前发布状态。如你所见，此时用户无法继续，因为状态显示为 “Not Logged in to Battle.net”。若要发布内容，你需要一个绑定完整版《星际争霸 II》的 Battle.net 账号。编辑器本身会随《星际争霸 II: Starter Edition》提供，但免费用户无法在线发布。要获得该功能，你需要任意一种《星际争霸 II》正式账号，包括独立游戏版本。

如果你点击 “Log In” 按钮，系统会提示你输入账号名和密码。

![Battle.net Log In Window](./resources/016_Publishing02.png)
*Battle.net 登录窗口*

在这里输入你的账号邮箱和密码，然后选择 “Connect”。如果你的账号绑定了身份验证器，还会有另一个窗口要求你输入验证码。如果你经常发布内容，可以考虑使用 “Remember Account” 开关，这会为你省下不少时间。

登录后回到主界面时，“Verification” 应该会显示更积极的 “Ready to Publish” 提示。此时 “Next” 按钮也会变为可用。继续之前，请注意 “Publish to Regions” 的控制项。它们是一些便利选项，可让你的项目一次性发布到多个地区，而不必反复重新登录。

## 发布要求

在 “Log In” 窗口点击 “Next” 后，你会进入主发布界面 “Configure Options”。在游戏可以托管到 Battle.net 之前，必须满足一系列要求。这个界面提供了许多为上传做准备的选项，但最重要的是底部子视图中的发布要求。如果项目未满足任何要求，“Verification” 字段就会显示 “Unable to publish due to problems below”，如下图所示。

[![Publishing Options Window](./resources/016_Publishing03.png)](./resources/016_Publishing03.png)
*发布选项窗口*

这些发布要求通常只是一些上传前必须澄清的小型管理问题。下表列出了最常见的一些问题及其解决方法。后续章节还会更详细地介绍其中提到的部分选项。

| 要求 | 说明与解决方法 |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Document description has not been changed from the default text. | 项目必须修改默认说明文本。 |
|                                                                  | 返回编辑器，前往 `地图 ▶︎ 地图信息`，并修改 “Description” 字段中的文本。 |
| Locked/Unlocked Mode must be chosen.                            | 已发布地图必须设置为 Locked 或 Unlocked 之一。 |
|                                                                  | 在发布系统的 “Configure Options” 窗口中，通过 “Locked/Unlocked Mode” 字段将地图设为 Locked 或 Unlocked。 |
| The published name 'X' is note available \[Locale\].             | 在对应地区 Locale 中，发布名称 `X` 已被占用。 |
|                                                                  | 为所有地区选择一个新的发布名称，或者返回 “Publish to Locales” 选项，对有冲突的地区单独设置另一个本地化名称。但请注意，这种做法会让项目今后需要分多次发布。 |
| The requested publish name has unacceptable words.               | 项目的发布名称中包含受限词汇。 |
|                                                                  | 修改发布名称。 |

当所有发布要求都满足后，“Verification” 字段会显示 `Ready to Publish!`

## 发布选项

“Configure Options” 由若干用于为地图发布做准备的选项组成，同时也可以在上传前解决要求相关问题。具体说明如下。

| 字段 | 说明 |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Published Name | 项目的发布名称，也就是线上显示的名称。默认情况下，它会使用 “地图信息” 中设置的项目名称。你可以使用 Change Name 按钮进行修改。 |
| Locale | 选择发布所使用的本地化版本。 |
| Revision | 递增自动生成的 “Version” 字段。重大版本递增 1.0，小版本递增 0.1。Revision 与发布名称绑定，每次名称变化时，数值都会重置回默认的 0.0。 |
| Release | 设置游戏的可访问性。Private 要求上传者亲自开房，并且每位玩家都必须手动邀请；Public 则允许任何人开房。 |
| Author | 使用你的 Battle.net ID 显示作者信息。如果勾选 Use Real Name，则还会附加你的 Blizzard 账号实名。 |
| Locked/Unlocked Mode | 锁定状态决定源工程是否可获取。Unlocked 地图可被任何开发者在编辑器中下载；设置为 Locked 后，源文件将不再对公众开放。 |

在决定使用 Locked 还是 Unlocked 时务必谨慎。前者会真正把你的地图对所有开发者都保护起来，也包括你自己。如果你在开启该状态后丢失本地文件，那么项目虽然仍然在线上存在，但你将无法恢复它。将项目设为 Unlocked 可以避免这个问题，同时也方便你与其他开发者共享项目，促进协作知识的增长。

## 文件管理

当所有项目选项都配置完成，且所有要求都满足后，你可以点击 “Next” 进入 “Storage Requirements” 视图。这里会列出文件占用空间，以及项目的大小限制。

![Storage Requirements View](./resources/016_Publishing04.png)
*存储要求视图*

每个 Battle.net 账号都拥有以下存储配额：同时上传 20 个文件、总上传容量 157 MB，以及单个文件最大上传大小为地图 20 MB、模组 100 MB。这些限制在不同地区之间不共享。如果你接近任一限制，可以选择 “Manage Files” 来管理当前上传内容。这样会进入下图所示的 “Manage Published Files” 窗口。

[![Published Files Manager](./resources/016_Publishing05.png)](./resources/016_Publishing05.png)
*已发布文件管理器*

这个界面会列出当前上传到你账号的所有项目，并显示它们的 Locked 状态、Release、Version 和 Size。高亮某个项目后，你可以看到它的完整细节。任何已发布项目都可以从 Battle.net 删除。如果你需要释放空间，可以使用 “Remove From Account” 按钮。完成后，关闭窗口并在 “Check Storage Requirements” 视图中点击 “OK” 继续发布。随后你会看到一个包含 EULA 和 TOU 协议的弹窗。

[![Compliance Check](./resources/016_Publishing06.png)](./resources/016_Publishing06.png)
*合规确认*

确认你同意这些协议后，点击 “Yes” 即可发布。
