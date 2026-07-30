# Actor cheats

从 Patch 1.4.0 开始，Mod 制作者可以在编辑器中运行测试地图时，使用 Actor cheat 在运行中即时创建和操作 Actor。这对于快速验证想法非常有用，因为你不必先配置数据或执行触发器。

这些 cheat 也可以在测试地图运行时，用于修改和检查几乎任意 Actor，对于调试运行时 Actor 问题非常有帮助。

这些 cheat 还能向 Actor 发送若干新启用的 dump 消息，例如 AnimDumpDB、AttachDump、HostedPropDump、RefDump 和 TextureDump。这些消息允许制作者在游戏运行期间检查某些 Actor 的内部状态，可作为额外的调试手段。

## Actor Cheat 概念

### Ref Names

“Ref Name” 是 “Actor Reference Name” 的简称，它唯一标识了一类系统变量，即 Actor 引用（“actor ref” 或简称 “ref”）。一个 Actor 引用能否解析为某个具体 Actor，取决于 1）该 ref name 的含义，以及 2）它所处的上下文。Ref name 可在多个 cheat 中使用，只要该 cheat 明确支持。它们是否可用，会随使用场景而变化：

- D - 在 Actor(D)ata 中
- T - 在触发器中的 Actor 相关函数里
- C - 通过 Actor (c)heats

当前可用的 ref name 如下：

| **::HoverTarget**       | C    | 鼠标指针下方的 Actor。 |
| ----------------------- | ---- | ------------------------------------------------------------ |
| **::LastCreated**       | DTC  | 在触发器中，它会解析为最近一次通过触发器函数直接成功创建的 Actor（不包括通过其他方式创建的 Actor，例如因为触发器调用而在数据中通过消息创建的 Actor）。在其他场景中，它还包括通过 Create 消息显式创建的 Actor（例如 Create SomeActor）。这样设计是为了在尽可能保持各类 LastCreated() 机制一致性的同时，也尽量符合最小惊讶原则。 |
| **::LastCreatedActual** | DTC  | 用户以任何方式最近一次成功创建的 Actor。包括通过 Create 消息创建的 Actor、Actor 请求创建的 Actor，以及系统内部创建的 Actor（例如当 CActorAction 创建爆散碎片时）。 |
| **::Main**              | DTC  | ::User scope 中的“主”Actor |
| 装饰物                  |      | CActor装饰物。 |
| Unit                    |      | CActorUnit。 |
| The rest                |      | 该 scope 中创建的第一个 Actor。 |
| **::PortraitGame**      | DTC  | 当前游戏头像窗口的主 Actor，无论当前选中了什么。 |
| **::PortraitGameSelf**  | DTC  | 当前 Actor 所在 scope 中主 Actor 的头像。适合从单位 scope 中的任意 Actor 向其头像 Actor 发送消息。如果头像对应的是当前单位之外的其他单位，则不会返回任何内容。 |
| **::Self**              | D    | 接收事件的 Actor。 |
| **::User**              | TC   | 包含最近一次 ActorFrom cheat 的结果。 |
| **::global.<RefName>**  | DTC  | 来自全局引用表的一个 Actor ref。 |
| **::scope.<RefName>**   | DTC  | 来自包含 scope 的引用表的一个 Actor ref。 |
| **::actor.<RefName>**   | DTC  | 来自包含 Actor 的引用表的一个 Actor ref。 |
| **TargetKey**           | TC   | 由 ::User scope 中的 key 所表示的 Actor。如果结果集中有多个命中，只返回第一个。 |

### Branch Ref Names

|                  |      |                                                              |
| ---------------- | ---- | ------------------------------------------------------------ |
| **::Creator**    | DTC  | ::User Actor 的创建者 |
| Hosts            |      | host Actor 是指一个 Actor 会从其继承数据的 Actor，例如朝向、挂接属性等。 |
| **::Host**       | DTC  | 主 host。用于朝向与挂接属性。 |
| **::HostImpact** | DTC  | 用于定位 beam 的命中点。 |
| **::HostLaunch** | DTC  | 用于定位 beam 的发射点。 |
| **::HostReturn** | DTC  | 触手返回时所使用的目标 host。 |
| **::Supporter**  | DTC  | 用于将一个 Actor 的生命周期绑定到某个“支撑”Actor 上（通常用于设置事件，使支撑 Actor 死亡时该 Actor 也随之死亡）。 |


### Scope Ref Names

| **::Actor**        | TC   | ::User Actor 的 scope。 |
| ------------------ | ---- | ------------------------------------------------------------ |
| **::LastCreated**  | TC   | 用户最近一次通过 cheat 或客户端代码成功创建的 scope。在数据中无意义，因为数据本身不会创建 scope。 |
| **::PortraitGame** | TC   | 游戏头像窗口的 scope。 |
| **::Selection**    | C    | 当前选中单位的 scope。即使选中了多个单位，也只返回一个 scope。 |
| **::User**         | TC   | 包含最近一次 ActorScopeFrom cheat 的结果。当该 ref 被填入新的有效 Actor scope 时，会自动设为 ::LastCreated 的值。 |

### Content Keys

Create 消息可以接受 1 到 3 个 content key。这样一来，触发器和 cheat 就能更方便地用同一份数据条目创建多种 Actor 实例，只是“内容”参数不同。例如：

`ActorCreateAt Model Hydralisk `

`ActorCreateAt Model Marine`

这两个 cheat 都会创建一个名为 “Model” 的 CActorModel。前者会用 “Hydralisk” 资源创建它，后者则用 “Marine” 资源创建。不同类型的 Actor 会按各自情况支持不同的创建参数形式。下面列出支持 content 参数的 Actor 类型，以及参数指定顺序。

#### CActorBeam ModelLink RefLaunch RefImpact

* **ModelLink** - beam 所使用的 modelData 条目名称。
* **RefLaunch** - 用于填充 beam 的 ::HostLaunch 的 ref name。
* **RefImpact** - 用于填充 beam 的 ::HostImpact 的 ref name。

#### CActorList RefName

* **RefName** - 用来填充该列表的来源 ref name。

#### CActorModel

* **ModelLink** - 模型所使用的 modelData 条目名称。
* **Variation** - 若需要，指定模型使用的变体编号（否则随机选择）。

#### CActorSound

* **SoundLink** - 要使用的声音名称。

#### CActorSplat

* **ModelLink** - splat 所使用的 modelData 条目名称。



## Actor Cheat 用法

用户可以在通过编辑器运行地图测试时，把 Actor cheat 输入到聊天栏中。

输出会写入 Alert.txt 日志，日志位于用户的 `StarCraft II/GameLogs` 目录。Alert.txt 日志的文件名前会自动附加日期与时间戳，因此现实中的文件名可能会类似于：`2011-08-08 10.30.05 Alerts.txt`。

目前暂不支持快捷写法，但未来可能会加入。



### Actor Cheat 列表

在每条命令给出的语法中，被花括号 `{}` 包围的参数表示可选参数。某些 cheat 成功执行后，会设置两个全局变量：“::User actor” 和 “::User scope”；其他 cheat 可以继续基于它们操作。用于销毁 Actor 和 scope 的 cheat 会排除那些会破坏当前活动单位和效果树的 Actor 与 scope。

#### ActorCreateAt

在指定位置创建一个 Actor。会将 ::User actor 设为该 Actor，并将 ::User scope 设为其所在 scope。

这个 cheat 适合直接在测试地图中创建一个 Actor，以便观察其属性并与其交互，而不用等地图自然运行到会生成该 Actor 的场景。坐标参数允许用户精确放置 Actor，适合战斗测试等用途。

##### Syntax

```ActorCreateAt x,y actorName {contentName} {content2Name} {content3Name}```

##### Examples

```ActorCreateAt 50,50 Model Drone ```<br/>
```ActorCreateAt 50,50 NexusSplat```

#### ActorCreateAtCursor

在鼠标指针处创建一个 Actor（以及一个容纳它的 Actor scope）。会将 ::User actor 设为该 Actor，并将 ::User scope 设为其所在 scope。

这个 cheat 适合直接在测试地图里创建 Actor，以便立即观察与交互，而不必等待地图进入某个会正常生成该 Actor 的情境。由于它直接在光标位置创建 Actor，所以用户也不需要手动输入精确坐标，就能把 Actor 放在一个容易看见的位置。

##### Syntax

```ActorCreateAtCursor actorName {contentName} {content2Name} {content3Name}```

##### Examples

```ActorCreateAtCursor Model Drone ```<br/>
```ActorCreateAtCursor NexusSplat```

#### ActorDumpAutoCreates

输出所有由于如下这类数据而被创建的 Actor 列表：

```xml
<On Terms="UnitBirth.Marine" Send="Create"/>
```

这种 Actor 创建模式称为 autocreation，也就是 Actor 会在收到某条消息时自动创建自己。这不同于下面这种创建模式：

```xml
<On Terms="ActorCreation" Send="Create SomeActor"/>
```

因为这里的 Create 消息明确指定了要创建哪个 Actor。

ActorDumpAutoCreates 可用于追踪某些事件是否在意外地创建 Actor。

##### Syntax

```ActorDumpAutoCreates```

#### ActorDumpEvents

输出 ::User Actor 所见到的所有 Actor 事件列表，不包括 autocreation 事件。

这个 cheat 可用于对地图中的所有 Actor 事件做各种文本搜索。例如，当你想看看有哪些 Actor 会响应某个特定的 Signal 事件时，它非常有用，而且不管这些 Actor 来自哪个依赖项。

##### Syntax

```ActorDumpEvents```

#### ActorDumpLeakRisks

输出所有“年龄超过指定值且有可能泄漏”的 Actor 列表。例如，用户可以检查某个 muzzle flash 模型是否已经存活了超过一分钟，因为 muzzle flash 通常不应该存在这么久。某些类型的 Actor 永远不会出现在泄漏风险列表中，因为系统会自动清理它们，因此通常不会因为错误数据而泄漏。

如果地图随着时间推移越来越卡，这个 cheat 可用于判断是否是 Actor 泄漏导致的。

##### Syntax

```ActorDumpLeakRisks age```



#### ActorDumpLive

输出整张地图上当前仍存活的 Actor 列表，并按其所在 scope 排序。

这个 cheat 很适合用来确认某些 Actor 是否实际存在，即使它们并没有出现在你预期的游戏世界位置。一个错误出现在 0,0 的 Actor，依然会显示在 live actors 列表中。

##### Syntax

```ActorDumpLive```



#### ActorFrom

根据指定的 ref name，从一个当前存活的 Actor 中设置新的 ::User actor。

这个 cheat 对把游戏世界中的各类 Actor 放入 ::User ref 非常关键，这样用户才能继续向它们发送 cheat 命令。

##### Syntax

```ActorFrom RefName```

##### Examples

```ActorFrom ::HoverTarget```<br/>
```ActorFrom ::Selection```

#### ActorFromActor

通过另一个 Actor 和一个 branch ref name，把 ::User actor 设为所引用的 Actor。

这个 cheat 可用于把游戏世界中的父 Actor 和子 Actor 放入 ::User ref，以便用户继续向它们发送 cheat 命令。它常用于对某个 Actor 的 ::Host 引用执行操作。

##### Syntax 

```ActorFromActor refName```

##### Examples

```ActorFromActor ::Host```

把 ::User actor 设为它当前所寄主的那个 Actor。

```ActorFromActor ::Creator```

把 ::User actor 设为创建它的那个 Actor。

#### ActorKillAll

销毁所有 Actor，但不包括那些属于存活单位和效果树的 Actor。

适合在测试地图中清空 Actor，以便之后单独测试某些 Actor，而不受其他 Actor 干扰。

#### Syntax: 

```ActorKillAll```

#### ActorKillClass

在光标周围指定半径内，销毁指定类别的所有 Actor。如果未指定半径，则视为无限范围。

当某类 Actor 让用户难以专注调查某个问题时，这个 cheat 可用于清空一片区域（甚至整个地图）中的该类 Actor。例如，如果你怀疑 装饰物 Actor 导致了性能问题，就可以先把它们全部清掉来验证。

#### Syntax: 

```ActorKillClass class {range}```


Examples:

```ActorKillClass Model 15 ActorKillClass Sound```

#### ActorKillLink 

在光标周围指定半径内，销毁具有指定 actor link 的所有 Actor。如果未指定半径，则视为无限范围。

当某个具体 Actor 条目的所有实例让用户难以排查问题时，这个 cheat 可用于清空某个区域（甚至整张地图）里的所有该实例。例如，如果某个范围伤害攻击创建了太多同名模型，遮挡了你正在调试的其他图形效果部分，就可以把它们全部删除。又或者，用户也可以销毁某个特定名称的全部声音，以便确认是否能听到与某个效果相关联的其他声音。

#### Syntax: 

```ActorKillLink link {range}```



##### ActorSend

向当前激活的 ::User actor 发送一条合法的用户消息。

这是使用频率最高的 Actor cheat，也是开发者（无论内部还是外部）通过 cheat 与 Actor 交互的主要方式。


#### Syntax: 

```ActorSend message```


#### Examples:

```ActorSend Destroy ActorSend SetTintColor {255,255,0}```

#### ActorSendTo

向一个系统 Actor 引用发送消息，并使用 ::User actor 来辅助解析该系统 Actor 引用。换句话说，这个命令主要用于向 branch ref name 发送消息（当然它也能作用于 ::Main ref name）。

这个 cheat 可以作为向 branch Actor 发送消息的简写方式；用户不必先通过 ActorFromActor cheat 把它们设到 ::User ref 中。


Syntax: 
ActorSendTo refName message


Examples:

ActorSendTo ::Host SetOpacity 0.5 ActorSendTo ::Main SetTintColor {255,0,0}

#### ActorScopeDumpLive

输出整张地图上所有当前存活 scope 的列表。

这个 cheat 可用于查找那些无谓占用资源、却已不再包含任何有用 Actor 的 Actor scope。


Syntax: 
ActorScopeDumpLive






**ActorScopeFrom**

这个 cheat 对于把游戏世界中的各类 scope 放入 ::User scope ref 非常关键，这样用户就能方便地找到并向该 scope 中的任意 Actor 发送消息。


Syntax: 
ActorScopeFrom scopeName


Examples:

ActorScopeFrom ::PortraitGame ActorScopeFrom ::Selection

销毁当前设置的 ::User actor 和 ::User scope。为了避免意外结果，这个命令不能销毁属于存活单位或效果的 scope。

这个 cheat 是一种高效方式，可用于销毁用户正在试验的一组或多组 Actor，因为只要销毁其所在 scope，该 scope 中的所有 Actor 都会一起被销毁。


Syntax: 
ActorScopeKill




**ActorScopeOrphan**

这个 cheat 可用于测试 ActorOrphan 消息对 ::User scope 内 Actor 的影响。


Syntax: 
ActorScopeOrphan






**ActorScopeSend**

适用于那些比较少见的情况：用户希望向某个 scope 中的所有 Actor 广播一条消息。

（顺带一提，虽然看上去这个 cheat 很适合把某个 Actor scope 中的所有模型都染成红色，但通常更好的做法，是让子 Actor 以 ::Main Actor 为 host，并继承 tintColor 属性。这样用户只需向 scope 的 ::Main Actor 发送 SetTintColor 消息，再依靠 hostedProp 继承来把颜色变化传递下去。这种方式通常更优，因为一个 scope 中可能同时包含“不应被染红”的 Actor，例如敌方命中碎片。直接广播 tintColor 消息则会把 scope 中所有模型一并染红。）


Syntax: 
ActorScopeSend message


Examples:

ActorScopeSend Destroy




**ActorUsersDump**

当用户忘记这些 ref 当前指向什么时，这个命令就很有用。


Syntax: 
ActorUsersDump




**ActorUsersFromHoverTarget**

非常适合检查和操作游戏世界中那些不属于任何可选对象的 Actor。


Syntax: 
ActorUsersFromHoverTarget




**ActorUsersFromPortraitGame**

适合检查和操作头像窗口中所包含的 Actor。


Syntax: 
ActorUsersFromPortraitGame




**ActorUsersFromSelection**

非常适合检查和操作游戏世界中那些属于可选对象的 Actor。


Syntax: 
ActorUsersFromSelection




**ActorWorldParticleFXDestroy**

可用于立刻清除世界中遮挡视线的粒子和 ribbon 特效（通常会在游戏暂停时使用），从而更仔细地检查模型或某个视觉特效的其他部分。


Syntax: 
ActorWorldParticleFXDestroy

## 





Actor Dump Messages



用户可以向 Actor 发送 dump 消息，以获得对调试很有帮助的信息。




**AliasDump**

输出当前与该 Actor 关联的所有别名。




**AnimDumpDB**

输出与该 Actor 关联模型可用的所有动画。会同时列出每个动画的持续时间，以及它是否为循环动画。




**AttachDump**

输出与该 Actor 关联模型上存在的所有挂点。同时也会输出用户指定的 attach key，以及与每个挂点关联的目标挂点体积。




**HostedPropDump**

输出指定 hostedProp 在该 Actor 上存在时的全部信息。如果 IncludeChildren 参数为 1，它还会继续输出目标 Actor 及其所有子 Actor 上该 prop 的信息。

Examples:

HostedPropDump 0 TintColor HostedPropDump 1 TeamColor

Syntax: 
HostedPropDumpAll IncludeChildren

输出该 Actor 上所有 hosted prop 的全部信息。如果 IncludeChildren 参数为 1，也会对目标 Actor 的所有子 Actor 做同样的输出。




**RefDump**

输出由 refName 指定的 Actor 的调试信息。目前它只适用于系统 ref 表中的 Actor ref，也就是格式为 ::actor.someUserRef、::scope.someUserRef 和 ::global.someUserRef 的引用。


Examples:

RefDump ::actor.someUserRef




**RefTableDump**

输出某个给定 ref 表中所有 Actor ref 的调试信息。RefTableType 参数区分大小写，期望值为 Actor、Scope 或 Global。


Examples:

RefDumpAll Actor




**TextureDump**

输出与目标 Actor 关联模型当前正在使用的所有贴图。还会标明哪些贴图与纹理槽位绑定，以及它们是否被其他动态纹理替换过。




**TextureDumpDB**

输出与目标 Actor 关联模型上，所有可供动态纹理替换使用的贴图。
