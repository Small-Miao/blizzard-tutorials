---
title: 数据入门 1 - 简易 Firebolt
authors:
    - duckies
proofreading/readability edits:
    - zerakim
---

# 技能制作入门 1：使用数据集合制作简易 Firebolt

!!! info
    下文教程面向完全新手，默认你对编辑器几乎没有任何熟练度。

在这篇教程里，我们会完整走一遍制作一个简单投射物攻击技能的全部步骤：向敌方目标发射一枚导弹，并在命中时造成伤害。这和《星际争霸 II》中大多数默认的投射物武器原理相同。

## 最终效果

![](Assets/1SimpleFirebolt.gif)

## 简介

!!! warning
    继续之前，请先确认你的编辑器设置与下列配置一致：

**数据编辑器设置**

![](Assets/0_preferences.png)


**文档依赖项**

![](Assets/0_dependency.png)



## 第 0 步：准备 / 数据集合
作为编辑器新手，在还没有足够自信自己从零实验之前，与其完全从头创建内容，我们更适合先复制/重复利用已有数据。
为了更方便地复制和整理这些内容，我们将使用 5.0 中引入的新功能：数据集合（data collections）！

![](Assets/1-0_DataCollectionFind.png)

打开数据编辑器，点击小小的 `+` 号，然后进入 `Edit Game Data` -> `Data Collections`。

创建一个名为 "**Fire Mage – Firebolt**" 的数据集合，然后点击 “Suggest” 按钮，让编辑器自动为我们生成 ID。

![](Assets/0-1_FirstCollectionCreate.png)

### 数据集合到底是什么

使用数据集合时有一点细节需要理解。它的一个核心思路，是让我们在创建新数据时，可以自动把这些新数据分组到同一个集合里。你可以把它理解为：编辑器会自动把你做出来的东西放进一个专属文件夹里，方便整理和查找。

要让新创建的数据被识别为某个数据集合的一部分，这份数据的 ID 必须以前缀形式包含该数据集合的名称。那具体该怎么做呢？

首先，我们需要知道什么是 “ID”。编辑器里的所有数据字段同时拥有名称（name）和 ID。名称没有什么限制，它只是为了让编辑器更易读；而 ID 则是后端代码会实际使用的值。它有一些规则，比如不能包含空格，而且必须唯一。

通常当我们创建一个新元素时，会先给它取个名字，然后再让编辑器建议一个 ID。在前面的例子里，"Fire Mage – Firebolt" 自动建议出的 ID 是 "FireMageFirebolt"。

那么按逻辑来说，当我们要创建新数据，并且希望利用数据集合时，就可以用下面这种命名方式 - "**Data Collection Name @ Data Element Name**"（在这个例子中就是 "**Fire Mage – Firebolt @ DataElement**"），然后点一下 suggest 就完事了。但问题在于，当编辑器在元素 ID 中识别到集合前缀，并确认它属于这个数据集合后，它还会再自动给这个元素添加一次前缀。最后你就会得到这种怪物：

  "Fire Mage – Firebolt @ Fire Mage – Firebolt @ DataElement"

|Fire Mage – Firebolt @|Fire Mage – Firebolt @|DataElement|
| ------------- | ------------- |------------- |
|编辑器为数据集合自动生成的前缀|为了让元素被纳入数据集合而手动加上的前缀|数据元素名称|

既然编辑器会自己加前缀，那我们手动加上的那个岂不是多余？难道每个元素都要再打开一次，把这个前缀删掉吗？

这就是数据集合唯一有点麻烦的地方。给元素命名时，在自动生成元素 ID 之后，我们还得手动删掉自己加上的那段前缀。

![](Assets/1-0_NamingRouteneExample.gif)

或者，也可以在创建元素时，直接把数据集合的 ID 复制粘贴到元素 ID 里。两种方法任选你喜欢的。

---

趁现在，顺手把后面会用到的其他标签页也一起打开：

- `Edit Game Data` -> `技能`
- `Edit Game Data` -> `效果`
- `Edit Game Data` -> `单位`
- `Edit Actor Data` -> `Actor`
- `Edit Art and Sound Data` -> `按钮`

## 第 1 步：导弹

既然我们要发射一个投射物，那就得先有一个投射物。更准确地说，是复制一个现有投射物，再把它的模型改成我们想要的样子。这里我选择复制 Karass 武器所使用的投射物。进入 units 数据标签页，找到 "Weapon – Karass" 单位，右键，duplicate-unit。勾选它的 unit 和 actor，这两项通常总是成对出现；另外由于我们还会修改视觉表现，也把 model 一并勾选复制。
 
![](Assets/1-1_WeaponDuplication.png)

现在来到最关键的部分：清晰命名，避免后面头痛。把 unit 重命名为 "projectile"，给它的 ID 加上数据集合前缀（FireMageFirebolt@）；把 actor 重命名为 "Missile"，给 actor 的 ID 加上前缀；把 model 重命名为 "Model"，再给 model 的 ID 也加上前缀。我再强调一次：之后每创建一个新的数据元素，我们都会把数据集合的 ID 加进去。这个习惯一开始可能需要适应。

![](Assets/1-1_MissileCreation1.png)
*Unit*

![](Assets/1-1_MissileCreation2.png)
*Actor*

![](Assets/1-1_MissileCreation3.png)
*Model*

---

!!! warning
    命名模式非常重要。做到这一步时，你看到的结果应该与下图一致。

    ![](Assets/1-1_NamingReminder.png)

如果我们点击这个 model，就能查看模型数据。在里面双击 "**Model**" 字段，再点击 "**Browse**"。这里选择 `FrenzyMissile.m3` 作为导弹模型即可。这个地方我们只需要改这一项。

![](Assets/1-1_MissileModelSelect.png)

## 第 2 步：技能

要做出这个简单的 firebolt 技能，我们需要以下几类数据：

1. **Ability** - 保存法术的一般信息，比如射程、消耗、施法时间等杂项属性、技能可指定什么目标、自动施法属性等等。
1. **Button** - 保存图标、提示文本、玩家可见的技能名称，以及单位命令卡上显示的相关信息。
1. **效果** - 构成技能逻辑的数据条目。
1. **Actor** - 负责创建视觉和声音表现，并总体控制屏幕上显示内容的数据条目。

### 准备 Ability 与 Button

我们一步一步来。首先，在技能数据标签页中，创建一个 "**Effect - Target**" 类型的新技能。由于数据集合会负责命名，我们可以直接把这个条目命名为 "Ability"。目前它本身还没什么可设置的。

![](Assets/1-2_AbilityNaming.png)

接下来做一个按钮！在 buttons 数据标签页里创建一个 button，把热键设为 Q，图标设置成你觉得合适的图片。我觉得 `ui_tipicon_campaign_zerus03-yagdrafireball.dds` 很适合作为这个技能的图标。

![](Assets/1-2_AbilityButtonNaming.png)

按钮创建好后，把它加入技能的命令列表。回到这个技能，找到它的 "**Commands+**" 字段，双击，然后把我们做的按钮设为 "**Execute**" 命令的默认按钮。

![](Assets/1-2_AbilityCommands.png)

---

### 把技能加到单位上

虽然此时这个技能还什么都不做，但它已经可以先被加到单位上了。所以我们先做这一步，这样在后续不同开发阶段中都能直接测试表现。这里可以用 Karass（《自由之翼》里的高阶圣堂武士英雄）作为施法者。

进入该单位的 "**技能+**" 字段，点击绿色小 `+` 按钮，把我们的技能加进去。

![](Assets/2-2_AddAbilityToUnitAbilities.png)


之后进入该单位的 "**Command Card+**" 字段，然后：
1.  点击你希望放置按钮的按钮槽位
1.  点击绿色小 `+` 按钮添加按钮
1.  选择我们刚才做好的按钮
1.  在 "**Command Type**" 中选择 "Ability Command"
1.  在 "**Ability**" 中选择我们的技能。单位自身拥有的技能一般都在列表顶部，所以通常不需要特地搜索。

|![](Assets/2-2_AddAbilityToCC1.png)|![](Assets/2-2_AddAbilityToCC2.png)|![](Assets/2-2_AddAbilityToCC3.png)|
| ------------- | ------------- |------------- |

---

## 第 2-1 步：效果

好了，现在开始构建驱动这个技能的逻辑。

这个技能会做两件事：
1.  从施法者向目标发射一枚导弹。
1.  导弹命中后对目标造成伤害。

第一件事，自然就是发射导弹。进入 effects 数据标签页，创建一个新的 "**Launch Missile**" 效果。

这个效果就像名字写的一样，会按照我们提供的设置发射导弹。

目前我们能先做的是指定发射哪一个导弹。找到 "**Ammo Unit**" 字段，把它设成我们在第 1 步中复制出来的那个投射物。

|![](Assets/2-3_LaunchMissileNaming.png)|![](Assets/2-3_LaunchMissileAmmoUnit.png)|
| ------------- | ------------- |

要让导弹在命中时造成伤害，我们还需要一个 damage 效果，所以现在就来做它。

创建一个新的 "**Damage**" 效果。

将 "**Amount**" 字段设为 20，并把 "**Death**" 字段设为 "**Fire**"。这样一来，如果这个效果击杀了敌人，它就会让被击杀单位播放火焰死亡动画（如果该单位有的话）。

另外，找到 "**Response Flags+**" 字段，并勾选 "**Acquire**" 和 "**Flee**"。这样敌人就会对这次伤害作出反应，否则他们只会像木头一样站在那里挨打。
 

|![](Assets/2-3_DamageNaming.png)|![](Assets/2-3_DamageEffectFields.png)|![](Assets/2-3_DamageResponseFlags.png)|
| ------------- | ------------- |------------- |


要让发射出的导弹在命中时造成伤害，把我们刚做好的 damage 效果填入 launch missile 效果中的 "**Impact Effect**" 字段。

 ![](Assets/2-3_ImpactEffectLinking.png)

现在，两个必要效果都已经做好了，就可以把 launch missile 效果挂到技能上。进入 ability 数据，找到 "**Effect+**" 字段，把我们的 launch missile 效果放进去。
我们还要指定技能射程（在 "**Range+**" 字段中）。这里把它设为 10。

 ![](Assets/2-3_AbilityEffectLinking.png)

---

## 第 2-2 步：Actor

如果你现在测试这个技能，它的游戏逻辑已经能工作了：导弹会从施法者飞向目标，并在命中时造成 20 点伤害。但它会从施法者脚下飞出，落到敌人脚边，而且没有声音、没有爆炸，也没有任何像样的表现。

我们要借助 Actor，把这些逻辑包装成更好看的视觉效果。

|![](Assets/2-4_ActorlessLogic.gif)*游戏逻辑*|![](Assets/2-4_ActorsAdded.gif)*游戏逻辑 + Actor*|
| ------------- | ------------- |


#### 射程 Actor（小型润色，不是必须但很不错）
首先，既然我们已经设置了技能射程，那就先加一个射程 Actor。它负责在我们瞄准技能，或者把鼠标移到单位技能按钮上时，显示技能射程指示圈。

这个 Actor 既可以复制，也可以从零创建。射程 Actor 本身没太多复杂内容，所以这里我们直接新建。

我们要创建一个 "**Range**" 类型的 Actor，并把它的 parent 设为 "**Range Abil**"。这样做之后，编辑器会给我们一个 token 字段（位于屏幕底部），我们可以把对应技能填进去；在一点点辅助之下，编辑器就能自动帮我们把这个 Actor 的数据调整到适配该技能。

|![](Assets/2-4-1_RangeActorNaming.png)|![](Assets/2-4-1_RangeActorToken.png)|
| ------------- | ------------- |

填好 token ability 后，找到 "**Events+**" 字段，右键它，悬停到 "**Reset To Parent Value**" 上，然后选择 "**Range Abil**"。这样会重置该 Actor 的事件，但同时也会根据我们在 token ability 字段中指定的技能，把这些事件调整好。

![](Assets/2-4-1_RangeActorEventsReset.png)



#### 攻击 Actor

接下来，让我们的投射物像《星际争霸 II》中绝大多数其他投射物一样运作：从单位武器位置射出，命中目标身体。为此我们需要一个 "**Action**" Actor。

!!! info
    Action Actor 负责控制普通攻击过程中与视觉/声音相关的一切。简单来说，它让攻击可以正确发射、正确命中，并会考虑目标是否有护盾等情况，从而显示出打在护盾上，而不是直接落到单位本体上。

我们来复制 "Karass Attack"。它已经带有一个很不错、听起来像法术发射的音效。和之前一样，复制时只勾选我们想改的内容（已经自己做过的东西，比如 missile，就不用再复制）。

复制完成后，注意底部的 token 字段。把 "**Impact Effect**" 和 "**Launch Effect**" 替换成我们自己的效果。

|![](Assets/2-4-2_AttackActionDuplication.png)|![](Assets/2-4-2_AttackActionTokens.png)|
| ------------- | ------------- |

!!! info
    这一次在填写 token 时，Actor 的 "**Events+**" 字段会自动根据它们调整（不像前面做 range actor 时那样），所以这里不需要重置 "**Events+**" 字段。

在 attack actor 中，找到 "**Missile**" 字段，把它替换成我们自己的投射物。

![](Assets/2-4-2_AttackActionMissileSet.png)

现在 Actor 的字段就设置完了，所以接着去改我们复制出来的 model。进入 model 数据，把 "**Model**" 字段改成某种爆炸模型。
`MiraHorner_Wraith_Coop_Missile_Impact.m3` 就很合适。

这样一来，我们的导弹在行为和音效上就应该符合预期了。但使用这个技能的单位本身呢？

#### 动画 / Actor 事件
![](Assets/2-4-3_IdleCasting.gif)


如果此时 Test Document，我们会发现：虽然武器已经会从施法者手上发出，并命中目标身体，和其他常规投射物一样；但 Karass 本人却在施法时一动不动。我们需要给 Karass 的 Actor 一条指令：如果 Karass 施放我们的法术，就播放指定动画。

通常我们会直接进入单位 Actor 的 "**Events+**" 字段，在那一大串默认事件中新增事件。但这里还有一种更整洁的方法：创建一个 "**Event Macro**" Actor，把我们的事件写进去，再把这个 macro 加到单位 Actor 所使用的宏列表里。这样就不需要在其他事件中来回翻找，而且我们也始终知道，这个技能对应的 Actor 消息该去哪里看、去哪里改。


创建一个 "**Event Macro**" 类型的 Actor。

![](Assets/2-4-3_AnimationMacroNaming.png)

打开它的 "**Events+**" 字段，并添加以下内容：

对于 event：

|||
| ------------- | ------------- |
|**Msg Type:**|**Ability**|
|**Source Name:**| 选择我们的技能|
|**Sub Name:**| **Source Cast Start**|


对于 message：

|||
| ------------- | ------------- |
|**Msg. Type:**| **Animation Play**|
|**Name:**| 任意你喜欢的名称。如果你之前没接触过这个字段，就需要手动添加动画名。它们本身可以随意命名，所以最好按对你有帮助的方式来命名。我通常会把 Q 技能的动画命名为 "SpellQ"|
|**Animation Properties:**|  **Spell**, **A**|


|![](Assets/2-4-3_AnimationMacroEventsEvent.png)|![](Assets/2-4-3_AnimationMacroEventsAction.png)|
| ------------- | ------------- |

然后将 "**Time Type**" 设为 "**Time Scale**"，并将 "**Time Variant**" 设为 "1.0"。

这样动画就会以你在模型查看器中看到的原始速度播放（教程最后会提到如何查看这一点）。

![](Assets/2-4-3_AnimationMacroEventsAnimTimes.png)


接着进入单位的 Actor，找到名为 "**Macros+**" 的字段，把我们刚做的 macro 加进去。

![](Assets/2-4-3_AnimationMacroUnitAddition.png)


---


## 第 2-3 步：其他内容

### 按钮提示

给我们的按钮加一个像样的 tooltip 吧。回到 button 数据标签页中我们做好的那个按钮。先写一段通用描述："`Strikes target with a firebolt, dealing 20 damage.`"

![](Assets/2-4-4_ButtonTooltipText.png)

不过等等！我们其实可以在 tooltip 里直接引用各种数据字段，所以这里就直接从 damage 效果里读取伤害数值。

你会注意到文本窗口下方有几个按钮。点击 "**Chose Field...**"。在 "**Type**" 中选择 "**效果**"，找到我们的 damage 效果，选中伤害值后按 OK。这样，它的引用就会显示在底部的 "**Data Reference**" 下拉框旁边。点击 "**Insert:**" 按钮，就可以把这个引用插入文本中的任意位置。

|![](Assets/2-4-4_ButtonTooltipReferenceFinish.png)|![](Assets/2-4-4_ButtonTooltipReferenceEffect.png)|
| ------------- | ------------- |

---

#### 收尾润色

此时一切应该都能正常工作了。但如果仔细看，你会发现动画和技能时机并没有完全同步，导弹飞出去得太早了。解决方法很简单：进入 ability，把 "**Cast Start Time**" 设为 0.5，让第一个效果稍微延迟执行。（当然也可以通过让动画前跳的方式解决，不过那就是另一天的话题了。）

#### 总结

我们的数据工作已经完成。回到 data collections 标签页，选中这个数据集合，然后点击 "**Data Collection**" -> "**Auto Fill Data Collection**"。这样就能一览我们做出来的所有元素，也能在需要时把整套元素一起复制。更改数据集合名称时，也会同步重命名其中所有元素。

|![](Assets/2-5_CollectionAutofill.png)|![](Assets/2-5_CollectionSummary.png)|
| ------------- | ------------- |

!!! warning
    重命名数据集合时，文本中的引用不会自动更新，所以如果我们改了数据集合名称，就必须手动更新所有文本引用，例如前面按钮 tooltip 中引用 damage 效果的那一项。


|![brrrrrrrrrrr](Assets/1SimpleFirebolt.gif)|
| ------------- | 
|*Firebolt 完成！*|

# Bonus 1：制作一个更酷的 Firebolt（难度级别 - 基础）

现在我们已经有了基础版的发射-命中技能，就来让它更有意思一点吧。我们不再只发射 1 枚导弹，而是让这个技能连续发射 15 枚。

不过首先，我们需要决定一件事：是继续直接修改当前这个技能，还是重新做一个新技能？

也许未来我们还会用到原始版本的能力，或者想把它保留下来作为其他简单投射物技能的模板。

所以，更酷的 firebolt 我们就单独做成一个新技能。

制作更酷 firebolt 的第一步，就是把我们在简易 firebolt 上做过的工作先复制过来。

## 第 1 步：复制数据集合

在 data collections 标签页里，右键我们的 firebolt 数据集合，选择 "**Duplicate Data Collection...**"

 ![](Assets/P2_1_DCDuplication1.png)

编辑器会弹出一个窗口，列出将要被复制的全部元素，并为新数据集合建议一个新的 ID（此时只是 "FireMageFirebolt2"）。

 ![](Assets/P2_1_DCDuplication2.png)

点击 OK 后，我们就有了两个完全相同的数据集合。不过新的副本在 ID 末尾多了一个 "2"（通常平时看不到。我们需要双击元素查看 ID，或者按 Ctrl+D，或通过 "**View**"->"**查看原始数据**" 切换原始数据视图才能看见。）

 ![](Assets/P2_1_DCDuplication3.png)

接下来给新的数据集合改个名字，比如叫 "Fire Mage – Invoke"（当然还是像之前一样，让编辑器自动建议一个符合名称的新 ID）。

改完之后，还需要手动做几件事：
1. 把按钮名称改成 "Invoke"（因为复制数据集合时，只会修改集合名称，不会修改元素显示名称这类文本）；
1. 修复 Invoke 按钮 tooltip 中的伤害引用；
1. 把复制出来的动画 macro Actor 加到 karass 单位 Actor 上；
1. 把 Invoke 技能加给 Karass；
1. 把 Invoke 技能加到 Karass 的命令卡上。

做完这些后，就可以 Test Document，并看到我们的 Karass 已经能施放两个完全相同的技能。

!!! info
    虽然数据集合复制窗口看起来和普通复制窗口很像，但你不能取消勾选某些单独元素。如果你只想复制集合的一部分，就需要先进入数据集合的 "**Data Record**" 字段，把不需要的元素移除。复制完成后，再通过常规 autofill 把它们加回来。
    
---

## 第 2 步：额外效果

### Create Persistent

为了让我们的 "**Launch Missile**" 效果执行多次，我们要使用 "**Create Persistent**" 效果（当你想重复做某件事时，它是首选效果，比如高阶圣堂武士灵能风暴中每个 tick 的重复伤害）。

创建一个 "**Create Persistent**" 效果，并把它命名为 "**Start Persistent**"。

 ![](Assets/P2_2_CPNaming.png)

把 "**Location**" 设为 "**Target Unit**"。

把 "**Period Count**" 设为 15。

把 "**Period Durations**" 设为 0.125。

把 "**Period 效果**" 设为 "Fire Mage – Invoke @ Launch Missile"。

 ![](Assets/P2_2_CPProperties.png)

在 "**Flags+**" 中勾选 "**Channeled**" 和 "**Channeling**"。这样一来，施法者就必须专注于这个攻击，任何移动或取消技能的行为都会中断这个 persistent 效果。

 ![](Assets/P2_2_CPFlags.png)

!!! info
    即使不勾选这两个 flag，技能本身也能正常工作。如果你希望施放完技能后立刻跑路，那也完全可以不加它们。

### 技能

接着，回到技能，把它的 effect 改成我们新做的 persistent 效果。

做到这里，再次 Test Document，你就会看到技能确实已经开始按预期工作了。

---

## 第 3 步：润色 / 其他

#### 按钮

给新技能换一个更有表现力的按钮图吧。`btn-ability-tychus-odin-barrage.dds` 很合适。

然后，把我们对技能所做的改动同步反映到按钮 tooltip 中。

"`Strikes target with <d ref="Effect,FireMageInvoke@StartPersistent,PeriodCount"/> firebolts, each dealing <d ref="Effect,FireMageInvoke@Damage,Amount"/> damage.`"

#### Actor

在动画 macro 中，把 Animation Properties 改成单独的 "**Spell**"。

![](Assets/P2_3_AMAproperty.png)

### 更花哨的 Launch Missile

在我们的 launch missile 效果里，进入 "**移动器+**" 字段，并加入 "Hurricane Missile"。这样在通过这个效果发射导弹时，就会覆盖导弹默认使用的 mover。

![](Assets/P2_3_LMMover.png)
 
 
## 第 4 步：运行地图，摆上一些敌人，然后用 Firebolt 把他们轰掉。
|![](Assets/1CoolerFirebolt.gif)|
| ------------- | 
|*更酷的 Firebolt 完成！*|


* [Data Primer 1 - Simple Firebolt.SC2Map](Assets/DataPrimer1-SimpleFirebolt.SC2Map)

---

# Bonus 2：查找模型动画与挂点

前面我们给技能设置动画属性时，为什么知道要填什么？为什么 "Spell, A" 是投掷动作，而单独的 "Spell" 却会让 Karass 双手合拢？

要知道这里能用什么，我们需要在 **过场动画编辑器** 中查看模型。

回到 units 数据标签页，点击 Karass。在关联元素里我们能看到这个单位的主 Actor。点进去，找到 "**Model**" 字段。这就是对应 Model 数据元素的名称。我们也可以像从 unit 导航到 actor 那样，继续通过关联元素跳转到 model。

|![](Assets/P3_1_UnitToActorNavigation.png)|![](Assets/P3_1_ActorModelField.png)|![](Assets/P3_1_UnitToModelNavigation.png)|
| ------------- | ------------- | ------------- | 

这次目标还挺明显，不过我还是把它们之间的关联展示出来，防止你之后需要时不知道该怎么找。

进入 model 数据后，点击 model 字段，然后点击 "**View in Cutscene Editor**"。

你会看到类似下面这样的画面。

|![](Assets/P3_1_CutscneEditorDefault.png)|
| ------------- |

### 查找模型动画

右键右下角的蓝色线条，选择 "**Change Animation**"。现在你就能查看并预览这个模型的动画了。

|![](Assets/P3_1_CutscneEditorViewAnimation.png)|
| ------------- |
---

### 查找模型挂点

- "**Render**" -> "**Show Geometry**" -> "**Attachment Points**"（或者直接按 "A" 键切换）
- "**Object**" -> "**Model Data**"（或者直接按 "Shift+D"）

在 model data 窗口中选中某个挂点后，它会在过场动画编辑器中闪烁显示。

|![](Assets/P3_1_CutscneEditorViewAttachmentPoints.png)|![](Assets/P3_1_CutscneEditorViewAttachmentPointsNames.png)|
| ------------- | ------------- |

---

做到这里，我们已经迈出了制作酷炫数据内容的第一步。

多研究默认技能是怎么做的，用一种对你自己以及其他可能阅读这份数据的人都说得通的方式来命名。

大胆实验，多试多看！也可以继续阅读这个网站上的其他教程，学到更多内容。接下来只会越来越顺手。
