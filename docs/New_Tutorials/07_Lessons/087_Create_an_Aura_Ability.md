# 创建一个光环技能

光环是一类游戏中很常见的技能。光环通常由单个单位向外发散，在一定作用范围内为友方单位赋予有益效果。就主题表现而言，光环常常用于体现某个英雄单位的存在能够鼓舞并强化周围同伴。你可以在下图中看到一个光环效果示例。

[![《魔兽争霸 III》的耐久光环](./resources/087_Create_an_Aura_Ability1.png)](./resources/087_Create_an_Aura_Ability1.png)
*《魔兽争霸 III》的耐久光环*

在《魔兽争霸 III》中，牛头人酋长的“耐久光环”会为附近友军提供移动速度和攻击速度加成。虽然这是光环的一种经典用法，但类似概念在不同游戏中还有很多变体。把这个概念抽象后，大致可以归纳为以下几点。

  - 光环是被动、不可主动施放的技能，由某个来源单位持有。
  - 光环会对来源单位周围一定范围内的单位产生变化。

这个抽象定义更贴近光环的本质：它并不一定只对应正面效果，也不一定只会出现在英雄单位身上。基于这一点，光环技能的设计可以非常多样。例如，举着火炬的单位可以拥有燃烧光环，对附近敌人施加持续伤害。这类光环甚至还可以设置为叠加，多个火炬单位聚在一起时效果更强。又或者，地图上可以有一件被诅咒的神器，一枚不断脉动、散发缩小光环的戒指，会让靠近它的单位体型变小。任何试图靠近并偷走宝物的盗贼，很快就会发现自己小到根本搬不动它。

## 设计光环

光环显然能为游戏增加很多有趣的玩法元素。学会在编辑器中创建一个光环，是一项非常值得练习的技能。幸运的是，只要事先理清光环的关键特征，实现起来其实会很快。

这里首先要注意的一点是，光环有多种实现方式。本文将介绍一种基于数据的做法，它相对简单直观。不过，这种实现方式和前面提到的“技能”定义存在一个表面上的差异。在这个例子里，光环实际上并不会按照《星际争霸 II》编辑器里严格意义上的 `Ability` 来实现。在编辑器中，`Ability` 通常指通过命令卡界面访问、会在游戏中触发某种行为的内容，例如陆战队员的“兴奋剂包”或狂热者的“冲锋”。虽然光环也可以用那类方式制作，但本文中的光环会使用两个行为和两个效果来完成。

行为会直接改变单位的属性，并挂载在单位本身上，这和前文给出的光环定义非常契合。你将使用一个行为把光环赋予来源单位，而它又会把第二个、负责改动属性的行为传递给每一个受到光环影响的单位。那一对效果则分别用于查找光环接收者，以及把改变属性的行为施加到这些接收者身上。

像这样先把光环机制讲清楚，有助于形成整体设计思路。这个设计会在后文中继续说明和展开，不过现在先把结构列出来，便于你随时参照。文章开头提到的那些创意变体，很多都可以在这个通用框架上实现。

  - 单位 -- 持有光环
      - 行为（增益） -- 赋予来源单位光环
          - 效果（搜索区域） -- 查找单位周围的光环接收者
              - 效果（应用行为） -- 将增益施加到找到的接收者身上
                  - 行为（增益） -- 对接收者施加光环的属性变化

要设计一个具体的光环实现，你需要选定一个来源单位，以及一个具体的增益行为。为了演示，本例将制作一个速度光环，使所有接收者的移动速度翻倍。这个光环会赋予陆战队员。设计方案如下。

  - 陆战队员 -- 持有速度光环
      - 速度光环 -- 赋予陆战队员速度光环
          - 速度光环搜索区域效果 -- 查找陆战队员周围的友方单位
              - 速度光环应用行为效果 -- 将速度增益施加给友方单位
                  - 速度增益 -- 使单位速度翻倍

要开始搭建这套设计，请打开本教程附带的演示地图。地图应如下图所示。

[![地图准备](./resources/087_Create_an_Aura_Ability2.png)](./resources/087_Create_an_Aura_Ability2.png)
*地图准备*

接下来切换到数据编辑器，开始构建光环逻辑。

## 搜索区域效果

需要注意的是，你不必严格按照设计方案列出的顺序来制作光环。因此，此处最合适的起点是“搜索区域”效果，因为它负责查找光环持有者周围的友军。新建一个效果，并将其属性设置为下图所示的内容。

![搜索区域效果创建](./resources/087_Create_an_Aura_Ability3.png)
*搜索区域效果创建*

然后选中新建的效果，打开 `Search: Search Filters` 字段。将 `Self` 过滤器设为 `Excluded`，如下图所示。

[![搜索过滤器](./resources/087_Create_an_Aura_Ability4.png)](./resources/087_Create_an_Aura_Ability4.png)
*搜索过滤器*

这样就能避免该效果把施法者或光环持有单位本身也算进去。接着，把 `Impact Location -- Value` 设为 `Target Unit`，把 `Target: Launch Location -- Value` 设为 `Caster Unit`。前者规定半径范围内要找的是哪类单位，后者则规定搜索从光环持有者的位置开始。最后设置实际的搜索范围和大小。将 `Search: Areas -- Radius` 改为 `3`，再把 `Search: Areas -- Arc` 改为 `360`。这样一来，搜索区域就是一个以光环持有者为中心、向任意方向延伸三格地图单位的完整圆形。该效果的字段应如下图所示。

[![搜索区域效果字段](./resources/087_Create_an_Aura_Ability5.png)](./resources/087_Create_an_Aura_Ability5.png)
*搜索区域效果字段*

你会注意到，这里的 `Effect` 字段还没有设置。它将用于对光环接收者启动“应用行为”效果。

## 增益行为 -- 陆战队员速度光环

接下来，创建作为光环来源的行为。新建一个行为，并将其属性设置为下列内容。

![光环行为创建](./resources/087_Create_an_Aura_Ability6.png)
*光环行为创建*

选中该行为，并将其 `Movement: Modification - Movement Speed Multiplier` 字段设为 `2`。这会对光环持有单位本身施加速度增益，在本例中也就是陆战队员。这样速度光环的来源单位在与受益单位一同行动时，自己也能跟得上。然后设置 `Time Scale Source -- Value` 字段为 `Global`。时间缩放决定引擎如何计算时间。单个单位可以使用自己的时间缩放，但在这里，这个行为应基于全局时间，也就是标准时间运行。

按照设计思路，光环会先通过搜索区域效果查找接收者，再开始施加变化。这一步通过把 `Effect - Periodic` 字段设为上一步创建的 `Search Area - Marine Speed Aura` 效果来完成。如下图所示。

[![选择光环的搜索效果](./resources/087_Create_an_Aura_Ability7.png)](./resources/087_Create_an_Aura_Ability7.png)
*选择光环的搜索效果*

接着，将 `Period` 设为 `0.0625`，这样 `Effect -- Periodic` 以及由它触发的搜索效果就会以每秒 16 次的频率执行。完成后的光环行为字段应如下图所示。

[![陆战队员速度光环字段](./resources/087_Create_an_Aura_Ability8.png)](./resources/087_Create_an_Aura_Ability8.png)
*陆战队员速度光环字段*

## 增益行为 -- 速度增益

这个部分是负责给光环接收者提供速度提升的行为。它同样是一个增益类型，其创建窗口应如下所示。

![速度增益行为创建](./resources/087_Create_an_Aura_Ability9.png)
*速度增益行为创建*

创建完成后，将 `Modification -Movement Speed Multiplier` 字段改为 `2`。这样，任何持有该行为的单位都会获得速度加成。由于光环范围内的每个单位都会分别得到这个行为，整个搜索区域内的加速效果就会由这个速度增益行为分发出去。

同时，把 `Duration` 字段设为 `0.2`。这表示该修改在单位身上的持续时间，也就是说，如果某个单位离开了光环范围，它的加速效果会在 `0.2` 秒后消失。最后一步，与前一个行为一样，把 `Time Scale Source -- Value` 设为 `Global`。完成后的字段应与下图一致。

[![陆战队员速度增益字段](./resources/087_Create_an_Aura_Ability10.png)](./resources/087_Create_an_Aura_Ability10.png)
*陆战队员速度增益字段*

## 应用行为效果

最后一个组成部分，是把速度增益施加到已找到友方单位身上的效果。这是一个“应用行为”效果，其创建界面应如下图所示。

![速度增益行为创建](./resources/087_Create_an_Aura_Ability11.png)
*速度增益行为创建*

这里唯一需要设置的字段是 `Effect: Behavior`。将它设为前面创建的 `Marine Speed Buff`。

[![设置要应用的行为](./resources/087_Create_an_Aura_Ability12.png)](./resources/087_Create_an_Aura_Ability12.png)
*设置要应用的行为*

现在，每当 `Search Area -- Marine Speed Aura` 效果找到一个单位时，都会调用这个效果。随后它会把 `Marine Speed Buff` 行为施加给这些单位，从而提高它们的速度。到这一步，效果的完整字段应如下图所示。

[![应用行为效果字段](./resources/087_Create_an_Aura_Ability13.png)](./resources/087_Create_an_Aura_Ability13.png)
*应用行为效果字段*

## 连接这些数据

先分别构建各个独立组件，是制作大型数据对象的一种高效流程，但这样也会暂时留下一些连接上的空缺。现在就来把它们补齐。回到 `Search Area -- Marine Speed Aura` 效果，打开它的 `Areas -- Effect` 字段，并将其设为 `Apply Behavior -- Marine Speed Aura` 效果。

![](./resources/087_Create_an_Aura_Ability14.png)
*将应用行为效果连接到搜索效果*

这样一来，搜索效果就会对每个找到的友方单位启动“应用行为”效果。最后，你还需要把这个光环设置到它的来源单位，也就是陆战队员身上。由于本文采用的光环实现方式，这一步很简单。切换到单位标签页中的 `Marine`，打开它的 `行为 -- Behavior` 字段。在弹窗中点击绿色 `+` 为单位添加一个行为，选择 `Marine Speed Aura`，然后在两个窗口中都点击 `Ok`。如下图所示。

[![将光环添加到来源单位](./resources/087_Create_an_Aura_Ability15.png)](./resources/087_Create_an_Aura_Ability15.png)
*将光环添加到来源单位*

## 测试光环

现在，你的光环已经完整构建完毕。你可以查看数据导航器进行确认，它会很直观地展示整个光环的数据结构。

[![光环数据结构](./resources/087_Create_an_Aura_Ability16.png)](./resources/087_Create_an_Aura_Ability16.png)
*光环数据结构*

通过这个可视化结构，你可以看出以下连接关系。

  - 单位（陆战队员） -- 通过它的 `行为 -- Behavior` 字段持有 `Marine Speed Aura` 行为

  - 行为（Marine Speed Aura） -- 通过它的 `Effect -- Periodic` 字段施加 `Search Area` 效果

  - 效果（Search Area - Marine Speed Aura） -- 通过 `Areas -- Effect` 字段施加 `Apply Behavior`

  - 效果（Apply Behavior - Marine Speed Aura） -- 通过它的 `Behavior` 字段创建 `Marine Speed Buff`

  - 行为（Marine Speed Buff） -- 光环的最终落点
    
    > 这与本文开头给出的设计结构完全吻合。测试该项目后，你应当会看到光环被创建在陆战队员宿主身上，并作用于其周围大约两格半径的圆形区域内的单位。所有受影响的单位都应以接近正常速度两倍的速度移动。同时，它们也会显示一个增益图标，如下所示。

[![应用行为效果字段](./resources/087_Create_an_Aura_Ability17.png)](./resources/087_Create_an_Aura_Ability17.png)
*应用行为效果字段*

## 附件

 * [087_Create_an_Aura_Ability.SC2Map](./maps/087_Create_an_Aura_Ability.SC2Map)
