# Galaxy 脚本

GalaxyScript 是 SC2 编辑器使用的编程语言。虽然它的大部分内容都隐藏在触发编辑器的 GUI 层之后，但你在触发器里做的所有工作，最终都会沉淀成 GalaxyScript，并由编辑器自动生成到你的 `MapScript.galaxy` 中。

很多人认为既然已经有触发编辑器，就根本不该直接接触 GalaxyScript。但实际上，项目中有些部分确实值得用 GalaxyScript 来写。本指南会带你了解：什么时候应该在项目中引入 GalaxyScript，以及具体该怎么做。

本教程假定你已经具备传统编程或触发编辑器方面的扎实基础。它也默认你已经把地图保存为 `.SC2Components` 格式（实际上几乎没有理由不这么做），并且正在使用 Talv 的 VSCode 扩展。

[![例如，这段 GUI 会编译成下方代码](./resources/058_GalaxyScript1.png)](./resources/058_GalaxyScript1.png)
*例如，这段 GUI 会编译成下方代码*

```c
void gf_SayHello () {
    // Automatic Variable Declarations
    playergroup auto50B0D547_g;
    int auto50B0D547_var;

    // Implementation
    auto50B0D547_g = PlayerGroupAll();
    auto50B0D547_var = -1;
    while (true) {
        auto50B0D547_var = PlayerGroupNextPlayer(auto50B0D547_g, auto50B0D547_var);
        if (auto50B0D547_var < 0) { break; }
        UIDisplayMessage(PlayerGroupSingle(auto50B0D547_var), c_messageAreaSubtitle, (StringExternal("Param/Value/E3C00EB1") + PlayerName(auto50B0D547_var)));
    }
}
```

# 为什么要用 Galaxy 脚本？

*“但 GUI 里不是也能做 GalaxyScript 能做的一切吗？那我们为什么还要用它？”*

并不是。有些事情只能在 GalaxyScript 里完成，或者在 GUI 里做起来会痛苦得要命。除此之外，还有一些别的理由，下面逐条说明。

### 1. 你可以复制粘贴

每个人在编辑器里都遇到过这种情况：你发现自己接下来要做大量复制粘贴，或者比如说，想把一个 `Int` 改成 `Real`。可一旦你尝试在 GUI 编辑器里改这个东西，它往往会把整条语句都重写掉。复制粘贴本身也要慢得多，特别是在每次只想改 1 到 2 个字符的时候。你不得不回到*所有*相关语句里，再一次次穿过那一层层窗口迷宫，体验并不好。Galaxy 通过允许你像编辑普通代码那样直接修改脚本，一下子解决了这些问题。它还意味着：当你只是在修改一些小地方，比如变量类型时，已有代码不会被覆盖重写。

### 2. 编辑器的类型安全有点过头了

编辑器里的很多东西，归根到底其实就是 `Strings` 或 `Ints`。例如，`Dialogs` 和 `Dialog Items` 本质上都只是 `Integers`。`Unit Types`（或者说，实际上 90% 的数据引用）本质上也只是 `Strings`。很多时候，编辑器会强迫你在它内建的众多类型之间反复转换。处理 Game Link 时，这尤其让人恼火，因为它们常常会拖慢游戏，甚至让游戏卡顿。就我个人而言，`Create Dialog Item From Template` 在我尝试选取条目时，有 50% 的概率会直接把编辑器搞崩；可我明明只是已经在布局里定义好了这些东西，只想用一个字符串而已。

摆脱编辑器类型约束的感觉非常自由，但能力越大，责任越大。不过，好的一面是：当你用 Galaxy 编写代码时，调试输出也更容易读，因为它真的会给出脚本里的函数名，这让查错和修错都简单得多。

### 3. 有些事情你在 Galaxy 脚本里能做，但在 GUI 里根本做不到

像动态创建触发器这样的东西（见本文末尾示例），就是触发编辑器完全做不到的。你只能通过脚本来实现。

---

不过，类型安全以及编辑器的其他特性，本身也可能非常有益。Galaxy 和布局/UI 一样，本质上都是工具。随着使用经验增加，你会逐渐知道什么时候适合用 Galaxy，什么时候不适合。通常情况下，我最后写出的代码大约会有 75% 放在触发器里，25% 放在 Galaxy 中。这个比例会因人而异，也取决于你对代码的熟悉程度；你最终会找到一个最适合自己的平衡点。

# 入门

先从一些基础例子开始，帮助你熟悉使用 Galaxy 的工作流。学会这一套之后，其他内容基本都会顺理成章。
第一件事是 [把这个网站加入书签](https://mapster.talv.space/galaxy/reference)，因为接下来它会非常有帮助。

## 第 1 步：创建你的第一个脚本

在文件资源管理器中打开你的地图目录，然后创建一个名为 `scripts` 的文件夹。我们会把所有 galaxy 文件都放在这里。

创建一个名为 `UI.galaxy` 的文件。前面提到过，`Create Dialog Item From Template` 这个函数很喜欢把我的编辑器搞崩。那我们就自己写一个包装函数，好让我们可以直接通过字符串来创建 Dialog Item。

在这个文件中，复制粘贴（或手写）下面这段代码。

```cpp
int CreateDialogItemFromTemplate(int dialog, int type, string template)
{
    return DialogControlCreateFromTemplate(dialog, type, template);
}
```

接下来，我们需要把它导入编辑器，才能真正使用。先创建一个 `Custom Script Object (Ctrl+Alt+T)`，命名为 `imports`。这是你唯一一次需要在编辑器里手写的 Custom Script。然后在里面加入下面这一行：

```c
include "scripts/UI"
```

[![它应该看起来像这样](./resources/058_GalaxyScript2.png)](./resources/058_GalaxyScript2.png)
*它应该看起来像这样*

Custom Script 只是在生成时把脚本追加到 `MapScript.galaxy` 的顶部。（如果你写过 C/C++，它就有点像 `#include <stdio.h>`。）这样我们就能在“主”脚本里使用这个文件中的函数，以及它后面出现的其他脚本中的函数。

现在，让我们创建一个 `native function`，这样才能在编辑器里真正调用刚才写的脚本。新建一个函数（Ctrl+F），并让它与脚本中的函数同名：`CreateDialogItemFromTemplate`。打开函数选项，然后勾选 `Native Function`。由于它返回的是一个 Dialog Item，因此把返回值设置为 `Dialog Item`。接着，我们来创建参数。

- **Dialog** 是一个 int，但你应该记得 Dialog 本质上只是整数，所以它的类型应设为 Dialog。
- **Type** 是一个预设，也就是 Dialog Item 的类型。
- **Template** 是一个 string。

[![它应该看起来像这样](./resources/058_GalaxyScript3.png)](./resources/058_GalaxyScript3.png)
*这应该就是你的最终结果。*

接下来测试一下。我先创建一个对话框项目，再通过我们自己的函数在其中创建一个按钮。

[![它应该看起来像这样](./resources/058_GalaxyScript4.png)](./resources/058_GalaxyScript4.png)
*这不是好习惯。不要这样写你的触发器。*

然后启动测试地图，接着 --

[![它应该看起来像这样](./resources/058_GalaxyScript5.png)](./resources/058_GalaxyScript5.png)
*完成。*

到这里就结束了。你已经写出了自己的第一个 Galaxy 脚本。现在，如果你还想继续写更多函数或动作，你已经知道怎么做了。

老实说，上面这个例子用 GUI 也同样很容易实现，不过它更多是为了演示流程，而不是展示 Galaxy 的真正威力。至于什么时候该写脚本，什么时候该用 GUI，我有几条经验规则。

- 只编写自包含函数。引用全局变量*确实*可以做到（只要在变量名前加上 `gv_` 前缀），但一般来说不建议这么做。这个规则唯一的例外是常量，不过这应该很直观。
- 所有 catalog 相关工作都在 Galaxy 里做。GUI 提供的函数很烦，而用字符串工作也远比用 game link 轻松。
- 那些恼人的类型转换，或者经常重复出现的逻辑（循环不适合的情况），都应该放到 Galaxy 里处理。
- 触发器创建始终在 GUI 中完成。

正如前面说过的，每个人写代码的风格都不同，所以这些建议也许适合你，也许不适合。你还是需要自己去尝试并逐步摸索。

# 提示

Galaxy 在语言设计上有一些比较奇怪的地方。

- 所有变量都必须在函数开头声明。你不能先调用别的函数，再去声明变量。
- `var++` 不能用。应该改成 `var+=1`。
- 只要某个函数所在文件已经被包含进 MapScript，那么即使*当前*文件没有额外写 include 语句，你也照样可以引用另一个 Galaxy 文件中的函数。
- `for` 循环里可以出现 null 值。
- 如果你需要创建一个异步/独立线程函数，就为它创建一个触发器。

# 示例

下面这些是我整理出来的一些通用工具/示例动作，用来展示在 galaxy 中完成不同任务的写法。所有变量名都尽量做到了见名知意。凡是以 `c_` 开头的内容，都是预设。

## 遍历玩家组

```c
void DisplayMessageToPlayerGroup(string message, playergroup group)
{
    int player;

    for(player = 1; player <= CONST_MAX_PLAYERS; player+=1)
    {
        if(!PlayerGroupHasPlayer(group, player)){ continue; }
        UIDisplayMessage(PlayerGroupSingle(player), c_messageAreaChat, StringToText(message));
    }
}
```

## 动态函数注册

```c
void CreateButtonAndRegisterToTrigger()
{
    // Create the Dialog Item
    int dialogitem = libNtve_gf_CreateDialogItemButton(
        CONST_DIALOG,
        300,
        75,
        c_anchorCenter,
        0,
        0,
        StringToText(""),
        StringToText("Click Me!"),
        ""
    );

    // Add it to global trigger TRIGGER_VARIABLE
    TriggerAddEventDialogControl(
        TRIGGER_VARIABLE,
        c_playerAny,
        dialogitem,
        c_triggerControlEventTypeClick
    );
}
```

## 创建触发器 / 创建异步函数（在独立线程中运行）

```c
trigger MyGlobalTrigger;

// You can name these variables whatever you want; testConds/runActions is just the standard.
bool MyTrigger(bool testConds, bool runActions)
{
    // Echoes the chat message back to the player
    UIDisplayMessage(PlayerGroupSingle(EventPlayer()), c_messageAreaChat, StringToText(EventChatMessage()));

    return true;
}
void MyTrigger_Init()
{
    // Use the name of the function you want to execute as an argument
    MyGlobalTrigger = TriggerCreate("MyTrigger");
    TriggerAddEventChatMessage(MyGlobalTrigger, c_playerAny, "echo", false);
}
```

## 作用域

```c
// This function cannot be accessed outside of this file
static bool ThisIsTrue()
{
    return true;
}


// This function can
bool TrueIsTrue()
{
    if(!ThisIsTrue())
    {
        return false;
    }
    else
    {
        return true;
    }
}
```
