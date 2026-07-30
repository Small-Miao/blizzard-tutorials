---
title: 基础技能切换系统
authors:
    - duckies
---



# 基础技能切换系统


!!! info
    本教程会尽量照顾初学者，但仍默认你对数据编辑器和触发编辑器至少有一些基础经验，因为我们需要同时使用这两个模块。触发器部分会涉及动作定义的创建，以及 catalog triggers 和数据表的使用。总共会制作 5 个触发器，而且每一个都很小。教程末尾附带了一张示例地图供参考。


## 引言


早在 5.0 补丁中，Blizzard 就给我们带来了这样一个功能：


>**动态技能支持**
>
>    Mod 制作者现在可以使用触发器为单位添加或移除技能。

我们将利用这个功能来做一件非常实用的事情。



本教程的目标如下：



**目标 1** – 通过使用触发器给单位添加技能，了解这个功能的工作方式。

**目标 2** – 创建一套简单的技能装备/卸下系统，要求配置快速、使用方便。





## 目标 1 – 测试添加技能

**我们要做什么：**

我们的目标是成功通过触发器把一个技能添加到单位身上。

**过程：**

先放置一个泽拉图到地图上。从现在开始，我们把这个单位称为“***hero***”。


我们先看看相关触发器是如何工作的。打开触发模块，删除 `Map Initialization` 触发器中的默认动作，然后新建一个动作 `**Add Ability**`。把名为 `***ArtanisVoidPsiStorm***` 的技能添加给我们的 hero。

![](SpellSwapAssets/1_InitalAbilAdd.png)

如果现在直接 `Test Document`，或者从编辑器启动地图测试，你会发现什么都没有发生。我们来看看这个 add ability 动作的说明。

>添加后的技能如果没有命令按钮，可能无法施放。你可以前往该技能的 `Command+` 字段，为其指定一个默认按钮，从而让技能自动创建命令按钮。要启用自动创建按钮，必须勾选 `Use Default Button` 和 `Create Default Button` 标记。自动创建按钮的位置可以在按钮数据中设置。



那我们就按它说的来做。进入将要赋予的这个技能 `***ArtanisVoidPsiStorm***`，打开 `**Commands+**` 字段，并按照提示勾选 `**Use Default Button**` 和 `**Create Default Button**` 标记。

![](SpellSwapAssets/1_CommandExtraFlags.png)

之后，找到与其 Execute 命令关联的按钮 `***Artanis - Psionic Super Storm***`，并找到 `**Default Button Layout+**` 字段。将 `**CardID**` 设为 `0001`，`**Column**` 设为 `1`，`**Row**` 设为 `1`。注意，列的范围是 `0` 到 `4`，行的范围是 `0` 到 `2`。根据这些规则，你很容易就能把按钮放到自己想要的位置。



现在再次启动地图。一切应该就能正常工作了。如果中途出了问题，也可以参考教程底部附带的地图。

![](SpellSwapAssets/1_AbilityAddedIngameScreen.png)


## 目标 2 - 技能拾取/切换系统

**我们要做什么：**

地图上会放置一些拾取物，它们内部存放不同技能。把鼠标悬停在拾取物上时，会显示该技能的相关信息。玩家可以用自己的 hero 右键点击它们，hero 会靠近并与之交互，从而获得其中存放的技能，同时销毁该拾取物。

受限于技术实现，所有技能都必须预先定义好在命令卡上的位置。我们会把系统设计成使用命令卡左下角前三个按钮位来容纳 3 个技能（分别视为 Q、W、E 槽位）。

如果 hero 拾取了一个技能，而该技能所属槽位已经被其他技能占用，那么系统会先卸下当前已装备的技能，并把它丢回地面，再装备新技能。

**我们将如何实现：**

要做出这样一套系统，我们需要完成 5 项任务：

**1) 数据准备。** 首先，创建一个作为拾取物的单位类型。然后还要创建一个供 hero 与拾取物交互的技能。

**2) 存储触发器。** 接着，创建一个触发器，让拾取物单位存储指定技能并显示对应信息。

**3) 技能数据准备。** 此外，还需要把相关技能配置到可用状态（勾选那些标记，定义在命令卡上的位置等）。

**4) 装备/卸下触发器。** 然后，创建一个触发器，在 hero 与拾取物交互时把技能从拾取物转移到 hero 身上。

**5) 收尾触发器。** 最后，把所有内容在交互触发器里串联起来。



所有与数据相关的部分都会比较简单，因为基本上只是改字段值。

触发器部分则主要围绕两件事展开：通过 catalog triggers 从数据里读取信息，以及通过数据表保存/读取信息。


### 第 1 部分 – 准备/数据

**目标：**

创建一个作为拾取物的单位类型，并为 hero 创建一个用于与拾取物交互的技能。

**过程：**

#### 第 1 步 – 拾取物空壳单位：

我们的目标是创建一个拾取物空壳单位（单位类型），后续通过触发器把技能引用存进这个单位里。

为此，复制 `***New Equipment***` 单位，并把副本命名为 `***Ability Pickup***`（同时清空它的技能字段，因为我们用不到）。我们计划把技能信息显示在这个拾取物单位的提示文本中，因此需要进入拾取物单位的 `**Flags+**` 字段，并取消勾选 `**No Tooltip**` 标记。此外，还要给这个单位一个未使用的碰撞标记（例如 `**Air16**`），这样多个拾取物掉在地上时就不会相互堆叠。



#### 第 2 步 – 交互技能：

现在来为 hero 与拾取物的交互制作一个技能。这个技能会包含一个 `**Set**` 效果，以及一个验证器，用来确保该技能只能对我们指定的单位类型生效。

1) 创建一个 `**Effect - Target**` 技能，并命名为 `***Pickup Interact***`。然后，为它的 `**Execute**` 命令创建一个新按钮，或者添加任意现有按钮作为 `**Default Button**`（位于 `**Commands+**` 字段）。这里我选择使用 `***Pickup Scrap Small***` 按钮。

2) 创建一个类型为 `**Unit Type**` 的验证器，并把它的 Value 字段设为 `***Ability Pickup***`（也就是刚刚复制出的那个单位）。

3) 创建一个 `**Set**` 效果，然后把我们刚创建的 `***Ability Pickup***` 验证器添加到这个效果的 Validator 字段中。

4) 在 `***Pickup Interact***` 技能中，把 Effect 字段设为刚刚创建的那个 `**Set**` 效果。然后进入 flags，勾选 `**Smart Command**`。这样单位在右键时就会自动使用该技能（而验证器会保证，只有右键点击 `***Ability Pickup***` 单位时技能才会实际执行）。

5) 把这个技能添加到泽拉图身上。因为它是一个智能技能，所以可以不把按钮放到命令卡里。



#### 第 3 步 – 触发器：

新建一个触发器，并命名为 `***Pickup Interact***`。现在我们先用它来验证整个流程已经打通，等所有内容都完成后，再回过头来重新利用这个触发器。

事件：`**Unit Uses Ability**`。在 Ability 中选择我们的 `***Pickup Interact***` 技能。在 Stage 中选择 `**Effect3 - Cast**`。

动作：`***Add Ability***` 和 `**Kill Unit**`。在 `***Add Ability***` 动作中，Ability 可以设为 `***ArtanisVoidPsiStorm***`，Unit 则设为 `**Triggering Unit**`。在 `**Kill Unit**` 动作中，把 Unit 设为 `**Triggering Ability Target Unit**`。这样一来，技能就不再是在地图初始化时授予，而是在 hero 右键点击拾取物单位时才获得。

我们来测试一下是否成功。先从 `Map Initialization` 触发器中删除 `***Add Ability***` 动作，然后把 `***Ability Pickup***` 单位放到 hero 身边，再启动地图。此时，用 hero 右键点击这个拾取物空壳单位，hero 应该会获得 Psi Storm 技能，同时该拾取物单位会被销毁。

![](SpellSwapAssets/2_1_PickupTest.gif)


### 第 2 部分 – 让拾取物单位存储技能数据

目标：

创建一个触发器，让拾取物单位存储指定技能并显示该技能信息。

![](SpellSwapAssets/TooltipPreview.png)



概览：

我们想实现两件事：

1) 当鼠标悬停在拾取物单位上时，让它显示技能信息（名称、快捷键、说明）。我们会使用 catalog triggers，从与该技能关联的按钮中读取所需文本。

2) 在单位内部存储技能标识符。这个可以通过数据表实现。

过程：

### 第 1 步 - 在鼠标悬停于拾取物上时显示信息：



首先，创建一个新的动作定义（在触发模块左侧空白区域右键 -> New -> New Action Definition）。将其命名为 `***Set Pickup AbilityValue***`。给它添加 2 个参数：`**Unit**`（命名为 `***UNIT***`）和 `**Game Link – Ability***`（命名为 `***ABILITY***`）。


然后创建一个类型为 `**Game Link – Button**` 的局部变量，并命名为 `***Button***`。


#### 第 1.1 步 – 获取 Button。

在动作中，把 `***Button***` 变量设为 `**Convert String to Game Link**`。

在 string 的值里，选择函数 `**Catalog Field Value Get**`。

在 catalog 中选择预设 `**技能**`，在 entry 中设为参数 `***ABILITY***`。在 field path for scope 中选择 `**Effect- Target**`。然后在 field 中选择 `**CmdButtonArray**`。接着把 index 设为 `0`，并在 field 中选择 `**Default Button Face**`。

>        Variable -Set Button = (Game Link((Value of 技能 ABILITY CmdButtonArray[0].DefaultButtonFace for player Any Player)))

现在，我们就能方便地访问这个技能所使用的按钮了。接着便可以通过 catalog triggers 读取该按钮上的文本信息。


不过，需要注意的是，这种方式只能用于那些只有 1 个主执行按钮的技能，例如 `**Effect - Target**`、`**Effect - Instant**`、`**Behavior**`、`**Augment**` 等。像 `**Train**`、`**Specialize**`、`**Research**` 之类的技能，则使用的是一组命令数组。

如果你想从这些技能里取出按钮信息，流程在设置 scope path 之前都一样。只是这里不要填写 `**Effect – Target**`，而应改为 `**Train**` 或其他使用命令数组的技能类型。之后，不再选择 `**CmdButtonArray**`，而是选择 `**InfoArray**`，再指定从数组中的哪个位置读取按钮链接（暂时先使用第一项，也就是 index `0`）。

在完成这些之后，我们还需要能够区分“只有一个命令的技能”和“带命令数组的技能”。幸运的是，我们可以在触发器里通过 `**Ability Matches Filters**` 这个函数来判断技能类型。创建一个 `**If Then Else**` 语句：如果技能属于某种类型，就使用第一套按钮读取逻辑；否则就使用第二套逻辑。

![](SpellSwapAssets/2_2_SetPickupValue_ClassDetect.png)


#### 第 1.2 步 – 从按钮中读取文本。


现在我们已经拿到了按钮，接下来就可以从中提取文本并整理成提示文本显示出来。

创建 4 个 `text` 类型变量，分别命名为 `***Name***`、`***Description***`、`***Hotkey***`、`***Final Text***`。

把 `***Name***` 变量设为函数 `**Convert Game Text**`。在 path 中，选择函数 `**Catalog Field Value Get**`。然后在 catalog 中选择预设 `**按钮**`，在 entry 中选择变量 `***Button***`，最后把 field 设为 `**Name**`。



对 `***Description***` 变量也做同样的设置，只是这次 catalog 的 field 要选择 `**Tooltip**`。



`***Hotkey***` 会稍有不同，因为这里要使用的是 `**Convert Game Hotkey**` 函数，而不是普通的 game text（path 仍然使用同一个 `**Catalog Field Value Get**` 函数）。



拿到这些信息之后，我们就可以按自己想要的方式组织显示内容了。我这里选择使用表达式，把收集到的内容拼接到 `***Final Text***` 变量里。

![](SpellSwapAssets/2_2_SetPickupValue_FinalFormatting.png)

为了让拾取物单位显示这些文本，我们将使用 `**Set Unit Info**` 动作。`**Info Text**` 是你点击单位后在命令卡上显示的说明文字，`**Highlight Tooltip**` 则是鼠标悬停提示。

按你喜欢的格式进行设置即可。

![](SpellSwapAssets/2_2_SetPickupValue_InfoTextAction.png)


#### 第 2 步 - 存储技能。

现在，我们要把技能标识符保存到一个数据表条目中，并以拾取物单位的 unit tag 作为该条目的名称。这样一来，只要我们还持有这个拾取物单位，就能查到它对应的数据表记录。如果你对数据表不熟悉，可以阅读这里的说明：https://s2editor-guides.readthedocs.io/New_Tutorials/03_Trigger_Editor/041_Data_Tables/ （简而言之，它就是一种可以在运行时动态创建和销毁的变量）。


创建一个新动作 `**Save Data Table Value (String)**`。在 value 中，使用函数 `**Convert Game Link to String**`，然后在 game link 中选择参数 `***ABILITY***`。在 name 中，我们可以使用单位的 tag。选择函数 `**Convert Integer To String**`，然后使用 `**Unit Tag**` 函数。对于 unit，选择参数 `***UNIT***`。

这样就完成了！

![](SpellSwapAssets/2_2_SetPickupValue_FullTrigger.png)

### 第 3 部分 – 准备技能。


正如前面所确认的，要想让技能能通过触发器被添加到 hero 身上，它必须满足以下条件：

- 打开 `**Use Default Button**` 和 `**Create Default Button**` 标记。

- 修改 `**Default Button Layout+**` 字段，明确指定这些按钮应该放在什么位置。



我们还需要一种方式来判断这个技能属于哪个槽位（Q、W 还是 E）。幸运的是，技能有一个 `**Categories+**` 字段，它的提示文本写着：

>一种对技能进行分类的方法，可用于识别某些特定技能。

非常合适！那我们就约定：Q 使用 `**User 1**`，W 使用 `**User 2**`，E 使用 `**User 3**`。


接下来，我们需要决定拿哪些技能来做测试。


不妨多试一些不同类型。

|槽位|技能类型|技能名称|
| ------------- | ------------- |------------- |
|Q 槽|Effect Target|High Templar - Psi Storm|
|Q 槽|Effect Target|Vulture - Use Spider Mines|
|Q 槽|Effect Instant|Marine - Stimpack|
|W 槽|Behavior|Ghost - Personal Cloaking|
|W 槽|Specialize|Vulture - Make Spider Mines (Hidden Build)|
|W 槽|Train|Swarm Queen Train|
|E 槽|Arm Magazine|Karax - Servitors|
|E 槽|Research|Engineering Bay - Research (Engineering Bay)|

现在，进入这些技能的数据，把它们修改为可供 hero 使用：

- 在技能的 `**Commands+**` 字段中，勾选 `**Use Default Button**` 和 `**Create Default Button**`。

- 在 `**Categories+**` 字段中，Q 槽位勾选 `**User 1**`，W 槽位勾选 `**User 2**`，E 槽位勾选 `**User 3**`。

- 在其关联按钮的 `**Default Button Layout+**` 字段中，把 Q 槽位的 `**Card ID**` 设为 `0001`，`**Row**` 设为 `2`，`**Column**` 设为 `0`；W 槽位的 `**Column**` 设为 `1`；E 槽位的 `**Column**` 设为 `2`。

- *[可选]* 你可以把按钮的 `**Hotkey**` 改成对应的 Q/W/E。

- *[可选]* 你可以在技能的 `**Editor Prefix**` 字段里加上 `Q`、`W` 或 `E`，这样在触发器中搜索它们会快很多。

- *[注意]* 某些技能（例如 stimpack）带有研究需求，所以别忘了把这些要求清掉（Requirements 可以在 `**Commands+**` 字段中移除）。

- 对于那些使用多个按钮的技能（例如幽灵的隐形，或者任何 train/research/数组型技能），你需要对每一个希望在技能被添加后自动创建的按钮都执行上述步骤（别忘了把这些按钮放在不同槽位，避免重叠）。


### 第 4 部分 – 技能装备/卸下。


装备技能的过程包括 3 个步骤：



1 – 确定该技能将使用哪个槽位；

2 – 如果该槽位上已经装备了其他技能，则先把它卸下；

3 – 把这个技能的信息保存到 hero 对应的技能槽里。


要确定槽位（第 1 步），我们可以使用一个简单循环去检查技能的 catalog 数据。

要保存当前已装备技能的信息（第 3 步），我们仍然会使用数据表。把游戏链接转换成字符串后，保存到对应路径下。数据表路径会同时使用单位的 tag 和槽位编号。这样保存后，也方便我们后续判断是否需要卸下技能。

#### 卸下技能。

概览：

我们先来创建卸下动作，因为它会在装备流程中被调用。我们将创建一个动作定义，用于把目标单位在指定技能槽中的技能卸下。

这个过程会包含以下步骤：

- 找出该单位在这个技能槽里当前持有的技能

- 从 hero 身上移除这个技能

- 清除保存该技能槽信息的数据表记录

- 创建一个拾取物单位

- 把技能植入这个拾取物单位



过程：



新建一个动作定义，命名为 `***Ability Dequip***`，并给它 2 个参数。一个 `**Unit**` 类型（命名为 `***UNIT***`），一个 `**Integer**` 类型（命名为 `***SLOT***`）。


创建一个类型为 `**Game Link – Ability**` 的局部变量，命名为 `***Ability***`。

创建一个类型为 `**String**` 的局部变量，命名为 `***DataTable Path***`。



我们会用两个已知变量来构造数据表路径名：hero 的唯一 unit tag，以及技能槽的 ID。除此之外，再插入一个 `.` 字符，用来分隔它们（便于阅读）。


现在，把 `***DataTable Path***` 变量设为一个表达式：将参数 `***UNIT***` 的 `**Unit Tag**` 与参数 `***SLOT***` 转成字符串后拼接起来，格式如下例所示。

>Variable -Set DataTable Path = {(String(SLOT)).(String((Unit tag of UNIT)))}.
![](SpellSwapAssets/2_4_DataTablePath.png)

我们把这个路径封装成一个快捷变量，便于之后在数据表相关代码里重复使用和统一修改。


接下来，从数据表中取回技能值。

把 `***Ability***` 变量设为函数 `**Convert String to Game Link**`。在 string 中选择 `**Value From Data Table (String)**`，并把路径设为变量 `***DataTable Path***`。

使用 `**Remove Ability**` 动作，把刚刚取回的技能从 hero 身上移除。

使用 `**Remove Data Table Value**` 清除对应的数据表记录，避免产生残留。

>理论上，对于这套配置来说不清除也不是绝对不行，因为我们总会用新的技能值覆盖这个路径上的旧值。但如果你打算把这个触发器用于“移除技能后暂时不立刻替换”的情况，那就绝对应该做好清理。



接着，在 hero 的位置创建一个拾取物单位。

最后，通过我们前面做好的动作 `***Set Pickup AbilityValue***`，把技能注入到新创建的空壳拾取物单位中。

![](SpellSwapAssets/2_4_AbilDequipFullTrigger.png)


#### 装备技能。

概览：

装备技能与卸下技能的动作定义很相似。我们要做的事情包括：

- 检测当前要装备的技能属于哪个槽位

- 如果 hero 在该槽位上已经装备了技能，则先卸下当前技能

- 把新技能添加到 hero 身上

- 在数据表中保存“hero 在此槽位装备了该技能”的信息

过程：

新建一个动作定义，命名为 `***Ability Equip***`。给它 2 个参数：一个 `**Unit**` 类型（命名为 `***UNIT***`），一个 `**Game Link - Ability**` 类型（命名为 `***ABILITY***`）。

创建一个类型为 `**Integer**` 的局部变量，命名为 `***Hotkey Slot***`。

创建一个类型为 `**String**` 的局部变量，命名为 `***DataTable Path***`。



第一件事，就是确定当前要装备的技能勾选了哪个 category 标记。

如果你在数据编辑器里打开 `**View**` -> `**查看原始数据**`（快捷键 `Ctrl-D`），然后去看技能上的 category 标记，就会发现这些标记本质上只是 `0` 和 `1`：`0` 表示未勾选，`1` 表示已勾选。因此，我们可以使用 catalog trigger `**Catalog Field Value Get As Integer**`，把每个标记的值按整数读出来。


![](SpellSwapAssets/2_4_CategoriesRawView.png)


所以我们可以遍历这些标记，只要某个标记返回 `1`，就把它对应的编号保存到 `***Hotkey Slot***` 变量里。

创建一个 `**Pick Each Integer**` 循环，起始值设为 `0`（对应 `**User 1**` 标记），结束值设为 `2`（对应 `**User 3**` 标记）。

然后在循环内部创建一个 `**If Then Else**` 语句。在条件中，使用 `**Catalog Field Value Get As Integer**` 检查与当前整数对应的技能 category 标记是否为 `1`。如果确实为 `1`，就把 `***Hotkey Slot***` 变量设为 `**Picked Integer**`（这样我们就记住了哪个编号的 category 被勾选）。

![](SpellSwapAssets/2_4_Equip_FlagDetectionSetup.png)

![](SpellSwapAssets/2_4_Equip_FlagDetectionLoopComplete.png)


一旦找到热键槽位 ID，我们就可以设置 `***DataTable Path***` 变量，并访问数据表，检查 hero 在这个槽位上是否已经装备了技能。

>Variable -Set DataTable Path = {(String(Hotkey Slot)).(String((Unit tag of UNIT)))}

使用一个 `**If Then Else**` 语句，并配合 `**Data Table Value Exists**` 函数来判断 `***DataTable Path***` 对应的数据表记录是否存在。如果存在，就调用 `***Ability Dequip***` 动作，把当前技能卸下并丢回地面。

![](SpellSwapAssets/2_4_Equip_DequipCondition.png)

完成这些之后，使用 `**Add Ability**` 动作添加新技能。

最后，把技能标识符保存到数据表中，以便之后知道当前槽位里装备了什么。（使用 `**Save Data Table Value (String)**`。在 value 中选择 `**Convert Game Link to String**` 函数；path 则直接使用变量 `***DataTable Path***`。）

![](SpellSwapAssets/2_4_Equip_FullTrigger.png)


### 第 5 部分 - 收尾

现在，我们可以回头修改 `***Pickup Interact***` 触发器，让它调用 `***Ability Equip***` 动作。

也可以把里面之前临时测试用的动作都删掉。

清空之后，创建一个类型为 `**String**` 的局部变量，命名为 `***DataTable Path***`，并让它引用拾取物的 unit tag。

>DataTable Path = (String((Unit tag of (Triggering ability target unit))))

接着创建一个 `**If Then Else**` 语句。如果对应数据表值存在，就装备存放在拾取物中的技能，并销毁拾取物单位。

>Ability Equip((Triggering unit),(Game Link((DataTable Path from the Global data table))))

![](SpellSwapAssets/2_5_PickupInteractFinalTrigger.png)


完成之后，你就可以在地图上放置一批空的拾取物单位。在 `Map Initialization` 触发器中给它们分别塞入技能。然后，移除泽拉图命令卡第 3 行上的默认技能按钮（避免与拾取获得的技能重叠），并给他 `10,000` 点能量，确保他施放这些技能时不会遇到资源不足的问题。运行地图，看看整套系统是如何工作的。

现在，我们已经得到了一套可用、快速且简单的技能装备/切换系统。做得不错！

* [SpellSwapTutorial.SC2Map](SpellSwapAssets/SpellSwapTutorial.SC2Map)


备注：

1) 在我的测试中，我注意到如果给单位添加 `**Brood Lord - Brood Lord Hangar**` 技能，偶尔会导致 Starcraft 崩溃；而 `**Arm Magazine**` 类型的 `**Karax - Servitors**` 则没有出现这个问题。

2) 任何单位都存在 32 个技能上限。请及时移除旧技能，确保当前已装备技能数量不超过这个限制。超过该限制会导致 Starcraft 崩溃。
